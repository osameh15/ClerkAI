# Quotas and Cost Guardrails

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (limits Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce + AI Engineer |
| Blueprint | BP §22.1 |

## Guardrails (BP §22.1) and Realisation

| Guardrail | Realisation |
|---|---|
| Per-plan soft and hard quotas | Quota entitlements (`ai.credits.monthly`, `processing.minutes.monthly`, `pages.monthly`, `storage.bytes`); soft = notify at 80/100%; hard = block unless `overage.allowed` |
| Organization monthly AI budgets and alert thresholds | Org setting (Owner/billing.manage): budget in credits; alerts at 50/80/100%; hard stop option |
| Model routing with a cost ceiling | Route `max_cost_micros` + request `maxCostCredits` ([model-routing](../ai/model-routing.md)) |
| Deduplication and reuse of Source processing | [deduplication](../ingestion/deduplication.md), [extraction-cache](../ingestion/extraction-cache.md) |
| Caching deterministic extractions | Extraction cache keyed by pipeline version/options |
| Max duration/pages per upload by plan | `upload.max_bytes`, `upload.max_duration_seconds`, module limits |
| Anomaly detection for spikes | Hourly cost vs trailing baseline per org and global ([ai-observability](../ai/ai-observability.md)) |

## Reservation Protocol

1. At submit: estimate → reserve in Redis counter (and PostgreSQL `quota_counters.reserved` for hard-limit meters).
2. During processing: actual usage events decrement reservation and increment used.
3. At completion/cancel: release remainder.
4. Reconciliation job fixes drift from Redis loss.

## Failure UX

`402 BILLING_QUOTA_EXCEEDED` with meter and upgrade/overage options; in-progress jobs are not interrupted by crossing soft limits; hard limit mid-job → job continues to completion if reservation covered it, else pauses at next AI step with clear status (provisional policy).
