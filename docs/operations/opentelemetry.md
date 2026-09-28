# OpenTelemetry

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE |
| Blueprint | BP §17, [ADR-014](../governance/adr/ADR-014-opentelemetry.md), R9 |

## Setup

- Go services: OTel SDK (traces, metrics, logs bridge), OTLP exporter to the collector; resource attributes `service.name` (`clerk-api`, `clerk-worker-ai`, …), `service.version`, `deployment.environment`.
- Nuxt: browser tracing for API calls (optional, sampled) and Web Vitals metrics to a first-party endpoint.
- OTel Collector: batching, tail sampling, attribute redaction processor (defence in depth: drops attributes matching sensitive keys), routing to backend ([OD-012](../governance/open-decisions.md#od-012)).
- Semantic conventions: HTTP, DB, messaging; custom namespace `clerk.*` (`clerk.job_id`, `clerk.ai_execution_id`, `clerk.module_key`, `clerk.organization_id`).

## Local

Collector + Jaeger (or Grafana LGTM) in Docker Compose; Sprint 0 deliverable: trace from API request through Temporal hello workflow with correlation ID visible (BP App. F).
