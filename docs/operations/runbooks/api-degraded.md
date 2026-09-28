# Runbook: API Degraded

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |

**Symptoms:** elevated 5xx, p95 latency > 1 s, SSE disconnect storms, users cannot load Home/jobs; outbox lag growing.

**Detection:** SLO burn alerts; LB health; synthetic checks; outbox lag alert.

**Immediate actions:**
1. Declare incident (SEV2 if widespread).
2. Check recent deploys/flag changes → rollback if correlated ([rollback](../rollback.md)).
3. Scale API replicas if CPU/memory saturated.

**Diagnosis:**
- Traces: which route templates/dependencies are slow (DB, Redis, storage presign, Temporal start)?
- DB: connection pool exhaustion, slow queries, locks ([database-degraded](database-degraded.md)).
- Redis down → rate limiter/cache fallbacks active? ([redis-degraded](redis-degraded.md)).
- Dispatcher leadership lost → outbox lag.
- Abuse: request spikes by IP/org/API key.

**Mitigation:** rollback; enable read-only mode if DB write path failing; tighten edge rate limits; block abusive keys; disable non-critical features via flags (exports, embeddings); restart dispatcher / force leader handover.

**Recovery:** return to normal limits; drain backlog; verify SSE reconnect.

**Validation:** SLO metrics back within target for 30 min; synthetic job E2E passes.

**Escalation:** Tech Lead after 30 min unresolved; Security if abuse suspected.

**Post-incident:** postmortem (SEV1/2), add alert/test gaps.
