# Fallback Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §8.3 ("Model fallback creates a separate AIExecution record for every attempt"), §16.1 ("Provider outage, fallback policy, and degraded mode are defined") |

## Attempt Loop (per gateway call)

| Step | Condition | Action | AIExecution |
|---|---|---|---|
| 1 | — | Call route R1 | #1 |
| 2 | Transient error | Retry R1 (max 2 more, backoff) | #2, #3 |
| 3 | Output JSON malformed / schema-invalid | **Repair** on R1: resend with validator errors, ask for corrected JSON only (max 1) | #n (`attempt_kind=repair`) |
| 4 | Still invalid, capacity/auth errors, or circuit open | Fallback to R2 | #n+1 (`fallback_of_execution_id` = first) |
| 5 | All routes exhausted | Fail with typed error | — |

Budget caps: max 5 attempts and max cost multiplier 2.5× of the primary estimate per call (provisional).

## Degraded Mode

- **Circuit breaker** per (provider, model): opens after error-rate threshold (for example 50% over 1 min with ≥ 20 calls) or auth errors; half-open probes every 60 s.
- When a profile has no available route: workflows wait (bounded, default 30 min) then fail the step `AI_PROVIDER_UNAVAILABLE` (retryable). New submissions show a status banner ("AI processing delayed").
- Fallback to a lower-quality route is allowed only if that route is part of the profile (evaluated) — never ad-hoc.

## Safety Constraint

Content rejections (`content_rejected`) do not trigger fallback to a different provider by default (avoid policy shopping); modules may opt in per task with Product/Security approval.
