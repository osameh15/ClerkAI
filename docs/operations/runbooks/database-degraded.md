# Runbook: PostgreSQL Degraded

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |

**Symptoms:** API 503 `SYSTEM_UNAVAILABLE`, timeouts, workers failing DB activities, replication lag.

**Detection:** DB health/latency alerts, connection saturation, replication lag, error logs with SQLSTATE.

**Immediate actions:** declare incident; check HA status (failover in progress?); stop non-essential batch jobs (retention sweeps, backfills, re-embedding).

**Diagnosis:** `pg_stat_activity` (long transactions, locks), `pg_stat_statements` (regressed queries), disk/IO, autovacuum, connection pool settings, recent migrations (locks from DDL).

**Mitigation:** terminate runaway queries; roll back offending release/migration; enable read-only mode; scale instance; add missing index concurrently (with review); pause worker queues that hammer DB (Temporal worker scale-down — workflows wait durably).

**Recovery:** resume workers gradually; reconciler for queued jobs; verify outbox catch-up.

**Validation:** latency normal; no failed job spikes; outbox lag < 60 s; RLS/tenant tests smoke on production read-only probe.

**Escalation:** Tech Lead; managed DB provider support if infra-level.

**Post-incident:** postmortem; query regression tests; capacity review.
