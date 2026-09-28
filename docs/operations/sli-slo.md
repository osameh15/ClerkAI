# SLIs and SLOs

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | **Provisional** (no production data yet) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Product |
| Blueprint | BP §17.1 |

> These targets are **provisional**. They are not validated against production data and will be recalibrated after the Private Beta (G13). BP §17.1 values are marked (BP).

| SLI | Definition | Initial target | Window |
|---|---|---|---|
| API availability (BP) | Successful (non-5xx) responses / valid requests, excluding client errors | 99.9% at Paid Launch | 30 days |
| API latency (BP) | p95 latency of non-processing endpoints | < 500 ms | 30 days |
| Upload init success (BP) | Successful `POST /uploads` / attempts (excluding 4xx policy rejections) | > 99.5% | 30 days |
| Upload completion success | Completed sessions / sessions with all bytes transferred | > 99% | 30 days |
| Workflow completion reliability (BP) | Jobs reaching terminal non-platform-failed state / submitted jobs with valid input (incl. retry/fallback) | > 99% | 30 days |
| Platform-caused artifact generation error (BP) | Jobs failed due to platform/provider errors / submitted jobs | < 1% | 30 days |
| Job processing latency | Time-to-reviewable-draft p90 per module (for example Meeting 60-min audio) | Module-specific, set after beta (placeholder: < 20 min) | 7 days |
| AI provider dependency | Successful gateway calls (after fallback) / calls | > 99.5% | 7 days |
| Event delivery | Outbox events delivered to consumers within 60 s | > 99.9% | 7 days |
| Webhook delivery | Deliveries succeeding within 24 h (excluding customer endpoint 4xx) | > 99% | 30 days |
| Error rate | 5xx / requests | < 0.5% | 1 day |
| RPO (BP) | Max data loss window for production metadata | ≤ 15 min | — |
| RTO (BP) | Time to restore service after disaster | ≤ 4 h at launch; reduced later | — |

## Error Budgets

Error budget policy applies from Paid Launch: when a budget is exhausted, feature releases pause in the affected area except reliability fixes.
