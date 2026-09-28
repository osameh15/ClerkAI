# Data Model Overview

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) / Data |
| Blueprint | BP §6, §7, §7.2, App. E |

## Purpose

Entry point to the data documentation.

## Stores and What They Hold

| Store | Holds | Authority | Doc |
|---|---|---|---|
| PostgreSQL | All business entities, metadata, structured content (artifacts, knowledge), audit, usage, outbox | **System of record** | [entity-catalog](entity-catalog.md) |
| pgvector (in PostgreSQL) | Chunk embeddings | Derived, rebuildable | [vector-storage](vector-storage.md) |
| Object storage | Original files, renditions, large extraction payloads, AI raw payloads (optional), exports, evaluation assets | Authoritative for bytes; metadata in PostgreSQL | [object-storage-layout](object-storage-layout.md) |
| Redis | Caches, counters, locks, pub/sub | Ephemeral | [redis-usage](redis-usage.md) |
| Temporal persistence | Workflow execution history | Execution state only | [temporal-workflows](../jobs/temporal-workflows.md) |
| Browser IndexedDB | Offline drafts, captured blobs pending upload | Client-local until synced | [indexeddb-model](../pwa/indexeddb-model.md) |

## Data Rules (BP §7.2)

1. UUIDv7 primary IDs; high-volume events may use BIGINT + external UUID → [identifier-strategy](identifier-strategy.md).
2. Every tenant-scoped table has explicit `organization_id` → [tenant-data-isolation](tenant-data-isolation.md).
3. Original Sources are immutable; replacement creates a new Source → [immutable-data-policy](immutable-data-policy.md).
4. Artifacts are never overwritten; every edit creates an ArtifactVersion → [versioning-strategy](versioning-strategy.md).
5. AuditEvent and UsageEvent are append-only → [immutable-data-policy](immutable-data-policy.md).
6. No large binaries in PostgreSQL → [object-storage-layout](object-storage-layout.md).
7. JSONB for metadata/extension points; queryable fields normalised → [schema-conventions](schema-conventions.md).
8. RLS on sensitive tables as a second line of defence → [tenant-data-isolation](tenant-data-isolation.md).

## Document Map

[erd](erd.md) · [entity-catalog](entity-catalog.md) · [schema-conventions](schema-conventions.md) · [identifier-strategy](identifier-strategy.md) · [tenant-data-isolation](tenant-data-isolation.md) · [immutable-data-policy](immutable-data-policy.md) · [soft-delete-policy](soft-delete-policy.md) · [versioning-strategy](versioning-strategy.md) · [indexing-strategy](indexing-strategy.md) · [partitioning-strategy](partitioning-strategy.md) · [migration-strategy](migration-strategy.md) · [migration-order](migration-order.md) · [retention-model](retention-model.md) · [object-storage-layout](object-storage-layout.md) · [redis-usage](redis-usage.md) · [vector-storage](vector-storage.md) · [backup-data-model](backup-data-model.md)
