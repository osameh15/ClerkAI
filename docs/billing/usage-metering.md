# Usage Metering

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce domain |
| Blueprint | BP §22, §8.2 (`usage.recorded`), App. F (Usage event in vertical slice) |

## UsageEvent (canonical)

| Field | Description |
|---|---|
| `event_id` | UUIDv7 |
| `organization_id` | Tenant |
| `meter` | From [usage-event-catalog](usage-event-catalog.md) |
| `quantity`, `unit` | Raw internal units |
| `occurred_at` | When consumption happened |
| Attribution | `job_id`, `processing_run_id`, `ai_execution_id`, `source_id`, `artifact_id`, `module_key`, `module_version`, `actor_id` |
| Cost | `provider_key`, `model_key`, `cost_micros`, `cost_currency`, `estimated` |
| `idempotency_key` | Unique per emission (for example `ai_execution:{id}:llm.input`) |
| `corrects_event_id` | For compensating events |

## Rules

- Recorded through the `UsageRecorder` port in the same transaction as the consuming record (AIExecution, extraction, export) — append-only.
- Aggregated asynchronously into `usage_aggregates` (daily/monthly) and Redis quota counters.
- Never edited; corrections are compensating events.
- `usage.recorded` events drive anomaly detection and near-real-time quota counters.
