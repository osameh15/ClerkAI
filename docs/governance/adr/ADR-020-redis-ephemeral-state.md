# ADR-020: Redis for Cache, Rate Limits and Short-Lived State Only

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership |
| Blueprint | BP §6 |

## Decision

Redis is used for: caching (entitlements, permissions, feature flags, module catalog), rate-limit counters, SSE fan-out (pub/sub), short-lived locks, and ephemeral progress. **Redis is never the only copy of business data.** Loss of Redis degrades performance and realtime updates but must not lose or corrupt data. All keys are namespaced and tenant-prefixed where tenant-owned.

## Related

[redis-usage](../../data/redis-usage.md), [redis-degraded runbook](../../operations/runbooks/redis-degraded.md)
