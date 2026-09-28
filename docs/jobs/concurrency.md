# Job Concurrency

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (limits Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain |
| Blueprint | BP §22.1 (quotas), §14.1 (abuse) |

## Controls

| Control | Scope | Mechanism |
|---|---|---|
| Concurrent processing jobs | Organization (`jobs.concurrent` entitlement) | Checked at submit; if exceeded, job stays `queued` and the workflow waits on a semaphore (Temporal-based, per org) rather than failing |
| Parallel sources in a job | Job | Child workflows per source, bounded fan-out (default 8) |
| AI concurrency | Provider/model | Redis token buckets + per-worker semaphores |
| Fairness | Across organizations | Per-org semaphore prevents one tenant from monopolising `ai`/`media` queues; weighted by plan |
| State races | Job row | `SELECT … FOR UPDATE` for transitions ([concurrency-strategy](../architecture/concurrency-strategy.md)) |

## User Experience

A job waiting for organization capacity shows "Queued — waiting for capacity" with position if known.
