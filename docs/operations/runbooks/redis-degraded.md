# Runbook: Redis Degraded

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |

**Symptoms:** slower API (cache misses), SSE live updates missing (clients polling), rate limiter in local fallback mode.

**Detection:** Redis health alert; increased DB load; SSE `fallback: poll` events.

**Immediate actions:** confirm fallbacks active (no data loss expected — Redis is ephemeral, [ADR-020](../../governance/adr/ADR-020-redis-ephemeral-state.md)); watch DB load.

**Diagnosis:** memory exhaustion/eviction, network, failover, key explosion (unbounded keys).

**Mitigation:** failover/restart; increase memory; flush offending namespace if key explosion (never needed for correctness); temporarily tighten API rate limits to protect DB.

**Recovery:** caches warm naturally; run quota counter reconciliation from PostgreSQL.

**Validation:** hit rate recovers; SSE live; quotas consistent.

**Escalation:** SRE lead after 30 min.

**Post-incident:** key namespace review; capacity.
