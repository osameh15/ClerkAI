# ADR-025: Database Migrations = pressly/goose v3

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership |
| Blueprint | BP §18 (forward-compatible migrations, expand/contract), App. E |
| Closes | [OD-003](../open-decisions.md#od-003) |

## Decision

Use **`github.com/pressly/goose/v3`** for PostgreSQL schema migrations, executed by the dedicated migrator job with the `clerk_migrator` role ([tenant-data-isolation](../../data/tenant-data-isolation.md)).

## Rules

1. **SQL migrations by default.** Schema changes (tables, indexes, constraints, RLS policies, grants, triggers) are plain `.sql` goose migrations in `migrations/`, sequentially numbered per [migration-order](../../data/migration-order.md).
2. **Go migrations only when a migration genuinely requires programmatic data transformation** that SQL cannot express safely (for example re-encoding JSONB with application logic). Go migrations must be idempotent, batched, and reviewed; long backfills still run as background jobs, not inside migrations ([migration-strategy](../../data/migration-strategy.md)).
3. `CREATE INDEX CONCURRENTLY` and other non-transactional statements use goose's `-- +goose NO TRANSACTION` annotation.
4. Every migration defines `-- +goose Down` for expand steps; destructive contract steps are roll-forward only and carry the `-- contract:` annotation required by CI lint.
5. CI runs: apply all migrations to an empty database, apply from the previous release snapshot, and the destructive-change lint.
6. Migrations are invoked through Taskfile (`task db:migrate`, `task db:status`) — see [ADR-026](ADR-026-monorepo-pnpm-taskfile.md).

## Alternatives Considered

golang-migrate (no Go migrations), Atlas (declarative diffing and linting; stronger tooling but heavier; its lint ideas are covered by the CI destructive-change lint).
