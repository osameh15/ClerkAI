# Failure Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) + SRE |
| Blueprint | BP §8.3, §16.1, §17, §19 (Chaos/Resilience), App. D |

## Purpose

Enumerates failure classes, the designed behaviour for each, and the guarantees the platform keeps under failure.

## Guarantees Under Failure

| ID | Guarantee |
|---|---|
| FG-1 | A submitted Job is never lost: it completes, fails with a reason, or is cancelled. |
| FG-2 | No committed business change is lost (PostgreSQL durability, RPO ≤ 15 min for disaster scenarios). |
| FG-3 | No duplicate side effects from retries (idempotency layers). |
| FG-4 | Tenant isolation holds under partial failure (RLS independent of application state). |
| FG-5 | Loss of Redis never loses data. |
| FG-6 | AI provider outage degrades (fallback / wait / clear failure), never produces unvalidated output. |

## Failure Catalogue

| Failure | Detection | Designed behaviour | User-visible effect | Runbook |
|---|---|---|---|---|
| API instance crash | LB health checks | Other instances serve; SSE clients reconnect with `Last-Event-ID` | Brief reconnect | [api-degraded](../operations/runbooks/api-degraded.md) |
| Worker crash mid-activity | Temporal heartbeat/start-to-close timeout | Activity retried on another worker; idempotent | Slight delay | [stuck-workflow](../operations/runbooks/stuck-workflow.md) |
| Temporal unavailable | Client errors, health | API accepts commands, commits state; reconciler starts/signals workflows when Temporal recovers | Processing delayed; status `queued` | [temporal-degraded](../operations/runbooks/temporal-degraded.md) |
| Temporal start fails after DB commit | Reconciler scan: `queued` jobs without workflow for > 2 min | Start with deterministic ID (duplicate-safe) | None / delay | [stuck-workflow](../operations/runbooks/stuck-workflow.md) |
| PostgreSQL primary loss | HA monitoring | Failover to replica; writes fail fast during failover (503 `SYSTEM_UNAVAILABLE`, retryable) | Temporary errors | [database-degraded](../operations/runbooks/database-degraded.md) |
| Redis loss | Health | Fallback: caches bypassed, rate limits local/in-memory conservative, SSE falls back to polling hint | Slower, polling | [redis-degraded](../operations/runbooks/redis-degraded.md) |
| Object storage unavailable | Health, errors | Upload session creation returns 503; processing activities retry with backoff | Uploads paused | [object-storage-degraded](../operations/runbooks/object-storage-degraded.md) |
| AI provider outage / 5xx / rate limit | Gateway error rates, circuit breaker | Retry → fallback route → wait (bounded) → step failure with `AI_PROVIDER_UNAVAILABLE`; job `failed` retryable | Delay or retryable failure | [ai-provider-outage](../operations/runbooks/ai-provider-outage.md) |
| Malformed/schema-invalid AI output | Validator | Repair attempt → fallback model → failure; each an AIExecution | Possibly review items | [structured-output](../ai/structured-output.md) |
| AI cost spike | Budget/anomaly detection | Soft limit alert; hard limit blocks new AI work for org | Quota message | [ai-cost-spike](../operations/runbooks/ai-cost-spike.md) |
| Poison source (parser crash/timeouts) | Activity failures after N attempts | Extraction `failed` with reason; source `partially_processed`; job proceeds if extraction optional else fails | Clear error on source | [failed-upload](../operations/runbooks/failed-upload.md) |
| Malware detected | Scanner | Quarantine; no processing; notify uploader/admin; audit | Source quarantined | [tenant-security-incident](../operations/runbooks/tenant-security-incident.md) (if targeted) |
| Outbox dispatcher stalled | Outbox lag metric | Standby acquires leadership; alert | Delayed notifications | [api-degraded](../operations/runbooks/api-degraded.md) |
| Webhook endpoint failing | Delivery failures | Exponential retry up to 24 h, then disable endpoint + notify | Integration paused | [webhooks](../integrations/webhooks.md) |
| Clock skew | NTP monitoring | Time comparisons use DB time for business rules | — | — |

## Retry Policy Defaults

Defined per activity class in [retries](../jobs/retries.md). Non-retryable errors (validation, authorization, schema-invalid after repair budget, quarantined source) fail fast.

## Degraded Modes

| Mode | Trigger | Behaviour |
|---|---|---|
| AI degraded | Primary provider circuit open | Route to fallback profile; if none, pause new AI steps and queue Jobs (bounded wait, default 30 min) before failing |
| Read-only mode | DB failover / maintenance | Mutating endpoints return `503 SYSTEM_READ_ONLY`; reads continue from replica |
| Upload paused | Object storage outage | Offline PWA queue retains captures; server rejects new sessions with retry hint |

## Chaos / Resilience Tests

Provider failure, worker crash, Temporal retry, storage timeout (BP §19) — see [chaos-testing](../testing/chaos-testing.md).
