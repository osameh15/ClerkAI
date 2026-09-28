# Migration Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (tool Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §18.1, §29.2 |

## Rules (BP §18)

1. **Forward-compatible migrations** with a documented rollback or roll-forward plan.
2. **No breaking schema change deployed together with the code** that needs it — use **expand/contract**:
   - *Expand:* add nullable column/table/index (concurrently); deploy code writing both; backfill in batches.
   - *Migrate reads:* deploy code reading the new structure.
   - *Contract:* remove old column in a later release after verification.
3. Migrations are plain SQL, versioned, reviewed ("DB changes require migration review", BP §26.1), and run by a dedicated job with `clerk_migrator` before application rollout.
4. Every migration runs in CI against an empty database **and** against a snapshot of the previous schema with representative data (migration test step in the pipeline).
5. `CREATE INDEX CONCURRENTLY` for indexes on populated tables (outside a transaction).
6. Long backfills run as batched background jobs (Temporal activity), never inside the migration.
7. RLS policies, grants and triggers are created in the same migration as their table.
8. Destructive statements (`DROP`, type narrowing) require a `-- contract:` annotation referencing the expand release; CI lint blocks unannotated destructive changes.

Tooling: `pressly/goose v3`, SQL migrations by default; Go migrations only for genuine programmatic data transformation ([ADR-025](../governance/adr/ADR-025-migrations-goose.md), closes OD-003).

## Rollback

- Code rollback must work against the expanded schema (because contract happens later).
- Schema rollback scripts exist for expand steps; data-destructive contract steps are roll-forward only and require a verified backup.

## Module Data Migrations

Module schema changes do not create tables; they version JSON Schemas and provide data transforms executed by the platform ([module-migrations](../modules/module-migrations.md)).

## Related Documents

[migration-order](migration-order.md), [deployment](../operations/deployment.md), [rollback](../operations/rollback.md), [database-testing](../testing/database-testing.md)
