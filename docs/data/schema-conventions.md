# Schema Conventions

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §7.2, App. E |

## Naming

| Object | Convention | Example |
|---|---|---|
| Tables | plural `snake_case` | `artifact_versions` |
| Columns | `snake_case`; FKs `<entity>_id`; timestamps `<verb>_at`; booleans `is_`/`has_` | `submitted_at`, `is_internal` |
| Primary key | `id` | |
| Unique constraints | `uq_<table>__<cols>` | `uq_modules__key` |
| Foreign keys | `fk_<table>__<col>` | `fk_jobs__module_version_id` |
| Indexes | `ix_<table>__<cols>` | `ix_jobs__organization_id__status` |
| Check constraints | `ck_<table>__<rule>` | `ck_jobs__status` |
| RLS policies | `rls_<table>__tenant` | |
| Enum-like columns | `TEXT` + `CHECK (col IN (...))` (easier to evolve than PostgreSQL `ENUM`) | `status` |

## Schemas

v1 uses one PostgreSQL schema per bounded context (`identity`, `org`, `access`, `commerce`, `marketplace`, `module`, `project`, `source`, `job`, `processing`, `ai`, `evaluation`, `knowledge`, `artifact`, `template`, `workflow`, `action`, `collab`, `notify`, `integration`, `audit`, `privacy`, `platform`). This makes ownership visible, simplifies grants and prepares extraction. Documents refer to tables without schema prefix for readability.

## Types

| Concept | Type |
|---|---|
| IDs | `UUID` (UUIDv7 app-generated) |
| Timestamps | `TIMESTAMPTZ` (UTC); never `TIMESTAMP` without zone |
| Business dates (deadlines) | `DATE` + original text + calendar ([OD-026](../governance/open-decisions.md#od-026)) |
| Money | `BIGINT` minor units or micros (`cost_micros`), with `currency CHAR(3)` |
| Quantities (usage) | `NUMERIC(20,6)` |
| Confidence | `NUMERIC(4,3)` in [0,1] with CHECK |
| Email | `CITEXT` |
| Flexible metadata | `JSONB` (must have a JSON Schema reference when it carries module data) |
| Text search | `TSVECTOR` generated columns where search is required ([OD-031](../governance/open-decisions.md#od-031)) |
| Embeddings | `vector(n)` |

## Required Columns

- Tenant tables: `organization_id UUID NOT NULL` as the **first** column of composite indexes.
- Mutable tables: `created_at`, `updated_at` (trigger-maintained), `row_version BIGINT NOT NULL DEFAULT 1` where optimistic concurrency applies.
- Actor columns: `created_by UUID` references `users.id` or is null for system; for AI/human distinction use `origin`.

## JSONB Rules

- JSONB is for extension points (module-specific attributes, settings, manifests), not for fields queried in `WHERE`/`JOIN`/`ORDER BY` on hot paths.
- Module-defined JSONB (`knowledge_entities.attributes`, `artifact_versions.content`, `jobs.input`) always stores a `schema_ref` column naming the JSON Schema (`<module-key>/<schema-name>@<version>`).
- GIN indexes only with proven query requirements (BP App. E).

## Foreign Keys and Delete Behaviour

- All FKs explicit (BP App. E). Default `ON DELETE RESTRICT`. Historical data deletion is performed by retention/deletion workflows in dependency order, not by cascades — except pure child rows of the same aggregate (`team_members`, `role_permissions`) which may `CASCADE`.
- Cross-tenant integrity: child tables referencing tenant parents use composite FKs `(organization_id, parent_id) REFERENCES parent(organization_id, id)` where the parent has `UNIQUE(organization_id, id)` ([tenant-data-isolation](tenant-data-isolation.md)).

## Related Documents

[entity-catalog](entity-catalog.md), [indexing-strategy](indexing-strategy.md), [migration-strategy](migration-strategy.md)
