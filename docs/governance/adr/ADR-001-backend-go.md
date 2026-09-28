# ADR-001: Core Backend Language = Go

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §6, §28.1 |

## Context

Clerk's backend must run a REST API, long-running Temporal workers, media/document processing orchestration, and an AI Gateway, with a small team (3–5 people, BP §26) and strong operational simplicity.

## Decision

The core backend (API process and worker processes) is written in **Go**, as one module-aware codebase ([ADR-002](ADR-002-modular-monolith.md)).

## Consequences

- **Positive:** static binaries, low memory footprint, first-class Temporal Go SDK, strong concurrency primitives, simple deployment, good OpenTelemetry support.
- **Negative:** fewer mature libraries for DOCX/PDF generation and some ML tooling than Python/JVM ecosystems. Mitigation: isolate such concerns behind ports; renderers or specialised processors may run as sandboxed sidecar processes in another language without changing domain code ([OD-008](../open-decisions.md#od-008), [OD-009](../open-decisions.md#od-009)).
- **Follow-ups:** router ([OD-001](../open-decisions.md#od-001)), SQL library ([OD-002](../open-decisions.md#od-002)), migration tooling ([OD-003](../open-decisions.md#od-003)).

## Alternatives Considered

- **TypeScript/Node:** shared language with frontend, but weaker for CPU-heavy orchestration and long-running workers.
- **Python:** best ML ecosystem; weaker typing guarantees and concurrency for API/workers. Python remains acceptable for *offline evaluation tooling* ([OD-033](../open-decisions.md#od-033)) and isolated processors.
- **Kotlin/JVM:** capable, heavier runtime and ops footprint.

## Compliance / Enforcement

`apps/api` and `apps/worker` are Go. Non-Go components require an ADR and must communicate over a defined port (HTTP/gRPC/CLI with sandboxing), never share the database schema.

## Related

[ADR-002](ADR-002-modular-monolith.md), [go-backend layout in component-architecture](../../architecture/component-architecture.md)
