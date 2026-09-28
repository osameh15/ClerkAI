# Redis Usage

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §6 |

## Principle

Redis is ephemeral ([ADR-020](../governance/adr/ADR-020-redis-ephemeral-state.md)). Every use has a defined behaviour when Redis is unavailable.

## Key Namespaces

| Purpose | Key pattern | TTL | On Redis loss |
|---|---|---|---|
| Entitlement cache | `t:{org}:ent:v{n}` | 5 min + event invalidation | Read from PostgreSQL |
| Permission cache (per membership) | `t:{org}:perm:{membership}:v{n}` | 5 min + event invalidation | Compute from PostgreSQL |
| Module catalog cache | `p:store:catalog:{locale}` | 10 min | PostgreSQL |
| Feature flags | `p:ff:{key}` | 1 min | Defaults in code/config |
| Rate limits | `rl:{scope}:{id}:{window}` (scope: ip, user, org, apikey) | window | Local in-memory conservative limiter |
| Quota counters (soft) | `t:{org}:quota:{meter}:{period}` | period | Fall back to PostgreSQL `quota_counters` (slower) |
| AI provider limiter | `p:ai:rl:{provider}:{model}` | seconds | Local limiter per worker |
| SSE fan-out | channel `t:{org}:job:{job_id}` | pub/sub | Clients fall back to polling |
| Short locks | `lock:{purpose}:{id}` | ≤ 60 s | PostgreSQL advisory locks for correctness-critical cases |
| Upload progress (optional) | `t:{org}:upload:{id}` | 1 h | Not shown |

## Rules

- Tenant-owned data keys **must** start with `t:{organization_id}:`; cached values also include `organization_id` which is verified on read.
- No customer content (transcripts, artifact content) in Redis.
- Serialization: JSON or msgpack with schema version in key (`v{n}`) to allow deploys without flush.
- Invalidation via events (`entitlement.changed`, `access.role.changed`, `module.*`).
