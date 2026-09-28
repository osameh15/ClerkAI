# Tracing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Tech Lead |
| Blueprint | BP §17 |

## Propagation

| Hop | Mechanism |
|---|---|
| Browser → API | `traceparent` from web client (optional; server starts trace otherwise) |
| API → Temporal → activities | Temporal OTel interceptors (context propagator) |
| Outbox → consumers | Envelope `trace.traceparent`; consumers create linked spans (span links) |
| Workers → AI providers | Client spans; no payload attributes |
| Workers → object storage/DB/Redis | Instrumented clients |

## Sampling

Head sampling 10% for normal traffic (provisional), 100% for errors (tail sampling at collector), 100% for jobs flagged for debugging (flag), always-on for evaluation runs.

## Span Attributes

Standard IDs from [observability](observability.md); never content. Long workflows produce many spans — workflow-level root span per run with activity spans as children; very long human waits are not a single open span (spans end at signal wait; resumed with links).
