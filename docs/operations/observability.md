# Observability

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (backend Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Tech Lead |
| Blueprint | BP §17, [ADR-014](../governance/adr/ADR-014-opentelemetry.md) |

## Correlatable Identifiers (BP §17)

| Identifier | Purpose | Propagation |
|---|---|---|
| `request_id` | Every API request | Response header, logs, error body |
| `correlation_id` | Full end-to-end business operation | Header → Temporal memo/context → events → AI executions |
| `job_id` | Job lifecycle | Span attribute, logs |
| `workflow_id` / `run_id` | Temporal execution | Temporal interceptors |
| `ai_execution_id` | Every model call or fallback attempt | Span attribute |
| `artifact_id` / version | Final output and provenance | Span attribute |
| `organization_id` | Tenant | Attribute where safe (internal backends only) |

## Trace Example

```mermaid
sequenceDiagram
  participant U as User "Generate"
  participant A as API request span
  participant J as Job submit
  participant T as Temporal workflow span
  participant AI as AI Execution spans
  participant AR as Artifact span
  U->>A: POST /jobs/{id}/submit (traceparent)
  A->>J: use case (job_id)
  J->>T: start workflow (trace context propagated)
  T->>AI: activities → ai.execute (ai_execution_id)
  AI-->>T: results
  T->>AR: artifact.generate (artifact_id, version)
```

## Pillars

[logging](logging.md) · [metrics](metrics.md) · [tracing](tracing.md) · [opentelemetry](opentelemetry.md) · [sli-slo](sli-slo.md) · [alerting](alerting.md)

## Sensitive Data

No customer content, secrets, tokens or full prompts in any telemetry ([logging](logging.md)).
