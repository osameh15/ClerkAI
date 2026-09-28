# Load Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (targets Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §19 (Load: Upload concurrency, SSE, job queue, DB hot paths) |

| Scenario | Profile (beta sizing ×3 headroom, provisional) | Pass criteria |
|---|---|---|
| Upload concurrency | 200 concurrent multipart uploads (100 MB–1 GB), 3G/4G network profiles | > 99% completion; API p95 within SLO |
| SSE | 2,000 concurrent streams with reconnect churn | No leaks; events delivered < 2 s p95 |
| Job queue | Burst of 500 submissions across 50 orgs | Fair scheduling (per-org limits respected); no stuck runs |
| DB hot paths | Mixed read/write at 300 rps | p95 queries < 50 ms; no lock contention alarms |
| AI gateway | Provider rate-limit saturation simulation (mock) | Back-pressure works; no runaway retries; budgets enforced |
| Offline sync burst | 50 devices syncing 200 photos each | Completion without duplicate sources |

Run before G13 and before G14; results feed [capacity-planning](../operations/capacity-planning.md).
