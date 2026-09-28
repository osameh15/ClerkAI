# ADR-014: Observability = OpenTelemetry

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership, DevOps |
| Blueprint | BP §6, §17, §28.1 |

## Decision

All services instrument traces, metrics and logs with **OpenTelemetry** SDKs and export via an OTel Collector. The backend is replaceable ([OD-012](../open-decisions.md#od-012)). Correlation identifiers (`request_id`, `correlation_id`, `job_id`, `workflow_id`/`run_id`, `ai_execution_id`, `artifact_id`/version) are propagated as span attributes and structured log fields. Trace context crosses HTTP, Temporal (interceptors) and events (envelope `traceparent` in metadata).

## Consequences

Sensitive-data logging rules apply ([logging](../../operations/logging.md)). Audit is not implemented via telemetry ([glossary](../glossary.md#disambiguation)).

## Related

[opentelemetry](../../operations/opentelemetry.md), [observability](../../operations/observability.md)
