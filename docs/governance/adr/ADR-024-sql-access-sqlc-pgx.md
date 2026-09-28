# ADR-024: SQL Access = sqlc + pgx/v5

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership |
| Blueprint | BP §6, §7.2 |
| Closes | [OD-002](../open-decisions.md#od-002) |

## Context

Repositories must make tenant filters reviewable, run `SET LOCAL app.org_id` inside transactions for RLS ([ADR-013](ADR-013-multi-tenant-isolation.md)), and support PostgreSQL features (JSONB, arrays, `FOR UPDATE SKIP LOCKED`, pgvector).

## Decision

- **Runtime driver:** `github.com/jackc/pgx/v5` (with `pgxpool`) is the PostgreSQL driver.
- **Query layer:** **sqlc** generates type-safe Go code from hand-written SQL, targeting pgx/v5 (`sql_package: pgx/v5`).

## Constraints

1. **sqlc-generated types are not domain entities.** Generated code lives in `internal/<context>/adapters/postgres/sqlcgen` (or equivalent) and is used only inside that adapter. Repositories map generated row types ↔ domain types explicitly.
2. `domain`, `app` and `ports` packages must not import pgx or sqlc-generated packages (import-boundary lint).
3. Each context's SQL files reference only tables that context owns (table-ownership lint, [dependency-rules](../../architecture/dependency-rules.md)).
4. Transactions go through the platform Tx manager, which sets `app.org_id`/actor variables with `SET LOCAL` before executing sqlc queries on the `pgx.Tx`.
5. Tenant-owned queries include an explicit `organization_id` predicate even though RLS exists (defence in depth).
6. Dynamic queries that sqlc cannot express (for example complex filter builders) are written by hand with pgx in the same adapter, parameterised, and reviewed.

## Consequences

- Positive: SQL remains explicit and reviewable; compile-time type safety; no ORM hiding tenant filters.
- Negative: mapping boilerplate between generated and domain types — accepted as the cost of keeping domain free of persistence types.

## Alternatives Considered

GORM/ent (obscure SQL and tenant filtering); `database/sql` + `lib/pq` (weaker PostgreSQL feature support).
