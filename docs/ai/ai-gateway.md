# AI Gateway

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11.1, [ADR-017](../governance/adr/ADR-017-provider-neutral-ai.md) |

## Principle

Modules request **capabilities**, not vendors.

```json
{
  "capability": "structured_extraction",
  "profile": "high_quality_extraction",
  "quality": "high",
  "language": "fa",
  "modalities": ["text"],
  "prompt": "meeting.action-extraction@^1.0.0",
  "schema": "business.meeting-minutes/ai/meeting-actions@1.0.0",
  "inputs": { "chunks": ["C1", "C2", "…"], "participants": ["…"] },
  "limits": { "max_cost_credits": 2.5, "timeout_ms": 90000 }
}
```

## Facade

```go
type Gateway interface {
    Execute(ctx context.Context, tc tenant.Context, req CapabilityRequest) (Result, error)
}
type Result struct {
    Output         json.RawMessage   // validated against schema
    Citations      []ChunkCitation   // mapped to chunk IDs
    ExecutionIDs   []ids.AIExecutionID // all attempts
    FinalExecution ids.AIExecutionID
    Usage          UsageSummary
    Confidence     *float64          // if schema provides
}
```

Called from Temporal activities only (never from HTTP handlers synchronously, except tiny interactive calls that may be added later with explicit ADR).

## Execution Steps

1. **Resolve** task → profile → prompt version (range resolved to the active PromptVersion compatible with the profile).
2. **Policy:** tenant data usage policy — allowed providers/regions, payload retention, "private AI" constraints; module `dataPolicy` ([enterprise-data-controls](../privacy/enterprise-data-controls.md)).
3. **Budget:** reserve estimated credits against organization AI budget/quota; hard limit → `AI_BUDGET_EXCEEDED` (unless overage allowed).
4. **Route:** ordered candidate routes filtered by policy, language, modality, availability (circuit breakers), cost ceiling ([model-routing](model-routing.md)).
5. **Render prompt:** system instructions + schema + delimited untrusted content with chunk handles ([prompt-injection](../security/prompt-injection.md)).
6. **Call provider** via adapter with timeout; capture usage.
7. **Validate:** JSON parse → schema → citation check → sanitise ([schema-validation](schema-validation.md)).
8. **Repair/fallback** within budget ([fallback-strategy](fallback-strategy.md)).
9. **Record** AIExecution per attempt + UsageEvents; release unused budget reservation.
10. **Return** validated result (or typed error).

## Routing Inputs (BP §11.1)

Quality · Language · Modality · Cost · Latency · Availability · Tenant policy (and region, brief §30).

## Non-Functional

- Per-provider rate limiting and circuit breaking; concurrency caps per org (fairness).
- Provider credentials are loaded only in the `ai` worker pool from the secret manager, per provider ([secrets-management](../security/secrets-management.md)).
- No prompt or payload logging by default; optional encrypted payload storage per policy (≤ 30 days) for debugging/evaluation.

## Errors

`AI_PROVIDER_UNAVAILABLE`, `AI_OUTPUT_INVALID`, `AI_POLICY_BLOCKED`, `AI_BUDGET_EXCEEDED`, `AI_LANGUAGE_UNSUPPORTED`, `AI_CONTENT_REJECTED` ([error-catalog](../api/error-catalog.md)).
