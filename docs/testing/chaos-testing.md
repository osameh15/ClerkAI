# Chaos / Resilience Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §19 (Chaos/Resilience: Provider failure, worker crash, Temporal retry, storage timeout) |

| Experiment | Expected outcome (from [failure-model](../architecture/failure-model.md)) |
|---|---|
| AI provider returns 5xx/429 for 10 min | Fallback routes used; no unvalidated output; jobs complete or fail retryably |
| Kill worker pods mid-activity | Activities resume elsewhere; no lost jobs |
| Temporal frontend unavailable 5 min | Submissions queued; reconciler restarts workflows |
| Object storage latency/timeout injection | Retries with backoff; uploads resume; no corrupted sources |
| Redis down | Degraded but correct operation |
| PostgreSQL failover | Brief 503s; no data loss; outbox resumes |
| Dispatcher leader killed | Standby takes over; outbox lag recovers |
| Network partition to scanner | Sources stay `scanning`; never bypass |

Game days in staging before G13 and quarterly after launch; results logged with follow-ups.
