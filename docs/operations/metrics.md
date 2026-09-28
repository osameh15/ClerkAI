# Metrics

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §17, §2.1 |

## Technical Metrics (RED/USE)

| Area | Metrics |
|---|---|
| API | Requests by route template/status, latency histogram, in-flight, SSE connections |
| Workers | Activity executions by type/status, duration, schedule-to-start latency, queue backlog |
| Temporal | Workflow starts/completions/failures, task latency |
| Outbox | Lag (oldest unpublished age), publish rate, consumer lag, dead letters |
| DB | Connections, query latency (by statement fingerprint), locks, replication lag |
| Redis | Memory, hit rate, latency |
| Storage | Request errors, latency, bytes |
| Uploads | Session creations, completion success rate, checksum failures, scan verdicts |
| AI | See [ai-observability](../ai/ai-observability.md) |

## Business/Product Metrics

Jobs by module/status, time-to-artifact, review item counts, correction/acceptance rates, evidence coverage, module activation, WAO, cost per job/module/org ([product-kpis](../product/product-kpis.md)).

## Cardinality Rules

No unbounded labels (no job IDs or org IDs on high-frequency metrics; use exemplars/traces). Organization-level metrics computed from DB aggregates, not metric labels.
