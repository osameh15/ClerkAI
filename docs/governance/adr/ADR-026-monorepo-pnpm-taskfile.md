# ADR-026: Monorepo Tooling = pnpm Workspace + Taskfile

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership |
| Blueprint | BP §6.1 (proposed monorepo), App. F (make/task commands) |
| Closes | [OD-024](../open-decisions.md#od-024) |

## Decision

- **pnpm workspaces** own JavaScript/TypeScript dependency management (`apps/web`, `packages/web-sdk`, `packages/module-spec` JS tooling, `packages/shared-schemas`). One `pnpm-lock.yaml` at the repository root.
- **Go** dependencies are managed by Go modules/workspace (`go.work`).
- **Taskfile** (`Taskfile.yml`, go-task) is the repository-wide orchestration layer for developer and CI-equivalent commands across Go, Nuxt, documentation validation, schemas, module validation, infrastructure (Docker Compose) and database migrations.

## Rules

1. CI invokes the same Taskfile tasks developers run locally (for example `task lint`, `task test`, `task test:integration`, `task docs:validate`, `task schemas:validate`, `task modules:validate`, `task openapi:validate`, `task db:migrate`, `task up`), so "works locally" equals "works in CI".
2. **No Nx, Turborepo, Bazel or other monorepo orchestrator is introduced during Sprint 0** unless a concrete, documented requirement shows Taskfile + pnpm is insufficient; such a change requires a new ADR.
3. Task definitions stay thin: they call standard tools (`go`, `pnpm`, `sqlc`, `goose`, validators); no business logic in Taskfile.
4. Tool versions are pinned (Go per [ADR-027](ADR-027-go-runtime-baseline.md); `packageManager` field for pnpm; tool versions recorded in the repo).

## Alternatives Considered

Make (portable but awkward on Windows and for cross-language task graphs); Nx/Turborepo (JS-centric caching, unnecessary at current scale); Bazel (high cost for a 3–5 person team).
