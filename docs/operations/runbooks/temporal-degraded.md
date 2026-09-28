# Runbook: Temporal Degraded

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |

**Symptoms:** jobs stuck in `queued`, schedule-to-start latency high, workflow start errors in API logs.

**Detection:** Temporal task latency alerts; reconciler reports many unstarted runs.

**Immediate actions:** declare incident; confirm API still accepts commands (state persisted in PostgreSQL; FG-1); communicate "processing delayed".

**Diagnosis:** Temporal server health (frontend/history/matching), its persistence DB, worker pollers connected?, task queue backlogs per queue, recent worker deploy with non-determinism errors (replay failures).

**Mitigation:** scale workers for backlogged queue; roll back worker deploy on non-determinism; restart Temporal services; if persistence lost → restore Temporal DB and run reconciler to recreate workflows from PostgreSQL run/step state.

**Recovery:** reconciler starts/re-signals workflows with deterministic IDs (duplicate-safe).

**Validation:** backlog drained; jobs progressing; no duplicate AIExecutions beyond retries.

**Escalation:** Tech Lead immediately for non-determinism errors.

**Post-incident:** add replay test for the failing history.
