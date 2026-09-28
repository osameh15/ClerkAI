# ADR-002: Modular Monolith with Extractable Services

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §0, §6, §7, §28.1 |

## Context

Clerk has ~19 bounded contexts (BP §7) but a 3–5 person team. Microservices would impose distributed transactions, network failure modes and deployment overhead before product validation. A big-ball-of-mud monolith would make later scaling a rewrite.

## Decision

Build a **Go Modular Monolith** with explicit, enforced bounded-context boundaries. API and workers share one domain codebase but run as **separate processes/deployments**:

- `apps/api` — HTTP API, SSE, webhooks ingress.
- `apps/worker` — Temporal workers (may be split into pools: audio, vision, document, ai, export).
- Optional `apps/dispatcher` — outbox dispatcher (may run inside worker initially).

Each context in `internal/<context>` exposes a public Go API package; other contexts use only that package. Persistence is owned per context.

## Consequences

- **Positive:** in-process calls, single transaction scope where a use case legitimately spans one context, one deploy pipeline, cheap refactoring.
- **Negative:** boundary erosion risk. Mitigation: import-rule linting, table-ownership lint, architecture review ([dependency-rules](../../architecture/dependency-rules.md)).
- **Extraction path:** a context becomes a service by replacing its in-process API package with a client implementing the same interface and moving its tables to a dedicated schema/database. Worker pools are already separate processes. See [future-service-extraction](../../architecture/future-service-extraction.md).

## Alternatives Considered

- **Microservices from day one:** rejected — premature operational cost (BP §0).
- **Unstructured monolith:** rejected — violates AP-11 (scale by extraction).

## Compliance / Enforcement

- CI import-boundary check (tool per [OD-024](../open-decisions.md#od-024) / `go-arch-lint` or custom `go list` script).
- Each table has exactly one owning context ([entity-catalog](../../data/entity-catalog.md)); cross-context SQL joins are forbidden.

## Related

[bounded-contexts](../../architecture/bounded-contexts.md), [dependency-rules](../../architecture/dependency-rules.md), [ADR-007](ADR-007-event-architecture.md)
