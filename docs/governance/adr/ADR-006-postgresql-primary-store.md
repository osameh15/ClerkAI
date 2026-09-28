# ADR-006: Primary Database = PostgreSQL

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §6, §7.2, §28.1 |

## Context

Clerk needs strong transactional guarantees (numbering sequences, approvals, outbox), row-level security for tenant isolation, JSONB extension points for module-specific data, and vector search for early retrieval.

## Decision

**PostgreSQL** (version ≥ 16) is the transactional source of truth for all business data.

Data rules (BP §7.2):
- UUIDv7 primary IDs ([ADR-018](ADR-018-uuidv7-identifiers.md)); high-volume events may use internal BIGINT plus external UUID.
- Every tenant-scoped table has an explicit `organization_id`, even if inferable.
- No large binaries in PostgreSQL; metadata and object references only.
- JSONB for metadata and extension points; important queryable fields are normalised columns.
- RLS on sensitive tables ([ADR-013](ADR-013-multi-tenant-isolation.md)).
- `pgvector` extension for embeddings ([ADR-019](ADR-019-pgvector-initial.md)).

## Consequences

- One database cluster initially, with one schema per bounded context allowed (`identity`, `org`, …) or table prefixes — see [schema-conventions](../../data/schema-conventions.md).
- Backup/PITR is the basis for RPO ≤ 15 minutes (BP §17.1).

## Alternatives Considered

MySQL (weaker RLS/JSONB), document DB (weak relational integrity for approvals/billing).

## Related

[data-model-overview](../../data/data-model-overview.md), [schema-conventions](../../data/schema-conventions.md)
