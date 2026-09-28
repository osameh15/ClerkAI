# Performance Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (targets Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Tech Lead |
| Blueprint | BP §17.1 (API p95 < 500 ms), §19 |

| Area | Test | Target (provisional) |
|---|---|---|
| API hot paths | Home cards, job list, artifact get/patch, review list | p95 < 500 ms at expected beta load |
| Upload init/complete | Session creation, completion verification | p95 < 300 ms / < 2 s |
| Rendering | 20-page report PDF | p95 < 10 s |
| Ingestion | 60-min audio end-to-end transcription (provider-bound) | Tracked; module time-to-artifact SLO set after beta |
| Frontend | LCP on 4G mid-range phone for Home | < 2.5 s |
| Editor | Load 50-page artifact | Interactive < 2 s |

Performance budgets are checked in CI for frontend bundles (size limits) and in staging for backend before releases.
