# Architecture Decision Records

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §28.1, §26.1 |

## Purpose

ADRs record architecturally significant decisions, their context and consequences. ADR-001 … ADR-016 are the initial records defined in BP §28.1 and keep the Blueprint's numbering. ADR-017+ are refinements added during documentation.

## Index

| ID | Title | Status | Source |
|---|---|---|---|
| [ADR-001](ADR-001-backend-go.md) | Core backend language = Go | Accepted | BP §28.1 |
| [ADR-002](ADR-002-modular-monolith.md) | Modular Monolith with extractable services | Accepted | BP §28.1 |
| [ADR-003](ADR-003-temporal-workflows.md) | Durable workflows = Temporal | Accepted | BP §28.1 |
| [ADR-004](ADR-004-rest-openapi.md) | Public API = REST + OpenAPI | Accepted | BP §28.1 |
| [ADR-005](ADR-005-direct-object-storage-upload.md) | Direct-to-object-storage uploads | Accepted | BP §28.1 |
| [ADR-006](ADR-006-postgresql-primary-store.md) | Primary database = PostgreSQL | Accepted | BP §28.1 |
| [ADR-007](ADR-007-event-architecture.md) | Transactional Outbox + Event Bus abstraction | Accepted | BP §28.1 |
| [ADR-008](ADR-008-declarative-first-modules.md) | Module runtime = declarative-first | Accepted | BP §28.1 |
| [ADR-009](ADR-009-schema-driven-module-ui.md) | Module UI = schema-driven + optional custom/hybrid | Accepted | BP §28.1 |
| [ADR-010](ADR-010-structured-ai-output.md) | AI output = structured + schema-validated | Accepted | BP §28.1 |
| [ADR-011](ADR-011-evidence-first.md) | Evidence = first-class entity | Accepted | BP §28.1 |
| [ADR-012](ADR-012-command-idempotency.md) | Critical commands = idempotent | Accepted | BP §28.1 |
| [ADR-013](ADR-013-multi-tenant-isolation.md) | Tenant isolation = application guard + RLS | Accepted | BP §28.1 |
| [ADR-014](ADR-014-opentelemetry.md) | Observability = OpenTelemetry | Accepted | BP §28.1 |
| [ADR-015](ADR-015-s3-compatible-storage.md) | Binary storage = S3-compatible | Accepted | BP §28.1 |
| [ADR-016](ADR-016-structured-artifact-content.md) | Artifact content = structured JSON + renderer | Accepted | BP §28.1 |
| [ADR-017](ADR-017-provider-neutral-ai.md) | Provider-neutral AI Gateway with capability profiles | Accepted | BP §1.1, §11.1 |
| [ADR-018](ADR-018-uuidv7-identifiers.md) | UUIDv7 identifiers | Accepted | BP §7.2 |
| [ADR-019](ADR-019-pgvector-initial.md) | pgvector for initial embeddings | Accepted | BP §6 |
| [ADR-020](ADR-020-redis-ephemeral-state.md) | Redis for cache, rate limits, short-lived state only | Accepted | BP §6 |
| [ADR-021](ADR-021-sse-progress.md) | SSE for processing updates; WebSocket only when needed | Accepted | BP §6, §8 |
| [ADR-022](ADR-022-nuxt-pwa-frontend.md) | Frontend = Nuxt 4 + TypeScript + PWA | Accepted | BP §6 |
| [ADR-023](ADR-023-http-router-chi.md) | Go HTTP router = go-chi/chi v5 (transport only) | Accepted | Closes OD-001 |
| [ADR-024](ADR-024-sql-access-sqlc-pgx.md) | SQL access = sqlc + pgx/v5 (generated types stay in adapters) | Accepted | Closes OD-002 |
| [ADR-025](ADR-025-migrations-goose.md) | Migrations = pressly/goose v3 (SQL by default) | Accepted | Closes OD-003 |
| [ADR-026](ADR-026-monorepo-pnpm-taskfile.md) | Monorepo tooling = pnpm workspace + Taskfile | Accepted | Closes OD-024 |
| [ADR-027](ADR-027-go-runtime-baseline.md) | Go runtime baseline = Go 1.27.x | Accepted | Leadership decision |

## Mapping from Documentation Brief Numbering

The documentation brief proposed different numbers. The Blueprint numbering is authoritative ([source-of-truth-policy](../source-of-truth-policy.md)).

| Brief file name | Actual record |
|---|---|
| ADR-006-event-architecture | [ADR-007](ADR-007-event-architecture.md) |
| ADR-007-declarative-first-modules | [ADR-008](ADR-008-declarative-first-modules.md) |
| ADR-008-schema-driven-module-ui | [ADR-009](ADR-009-schema-driven-module-ui.md) |
| ADR-009-structured-ai-output | [ADR-010](ADR-010-structured-ai-output.md) |
| ADR-010-evidence-first | [ADR-011](ADR-011-evidence-first.md) |
| ADR-011-command-idempotency | [ADR-012](ADR-012-command-idempotency.md) |
| ADR-012-multi-tenant-isolation | [ADR-013](ADR-013-multi-tenant-isolation.md) |
| ADR-013-provider-neutral-ai | [ADR-017](ADR-017-provider-neutral-ai.md) |
| ADR-014-postgresql-primary-store | [ADR-006](ADR-006-postgresql-primary-store.md) |

## Template

```markdown
# ADR-NNN: <Title>

| Field | Value |
|---|---|
| Status | Proposed | Accepted | Deprecated | Superseded by ADR-XXX |
| Date | YYYY-MM-DD |
| Deciders | <roles> |
| Blueprint | BP §x |
| Supersedes | — |

## Context
## Decision
## Consequences (positive / negative / follow-ups)
## Alternatives Considered
## Compliance / Enforcement
## Related
```

## Rules

- Numbers are never reused. Superseded records stay in place with status `Superseded by ADR-XXX`.
- A change to an Accepted ADR is a new ADR.
- Proposing to replace a fixed technology (Go, Nuxt, PostgreSQL, Temporal, Redis, S3, REST/OpenAPI) requires a new ADR that explains why the current decision is insufficient, with evidence.
