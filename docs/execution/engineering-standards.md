# Engineering Standards

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §6, §26.1 |

## Go

- Go 1.27.x, latest approved patch ([ADR-027](../governance/adr/ADR-027-go-runtime-baseline.md)), with modules/workspace; HTTP via chi only in transport adapters ([ADR-023](../governance/adr/ADR-023-http-router-chi.md)); SQL via sqlc + pgx/v5 only in postgres adapters, generated types never exposed as domain entities ([ADR-024](../governance/adr/ADR-024-sql-access-sqlc-pgx.md)); repository commands via Taskfile, JS dependencies via pnpm ([ADR-026](../governance/adr/ADR-026-monorepo-pnpm-taskfile.md)); `gofmt`, `goimports`, `golangci-lint` (errcheck, govet, staticcheck, gosec, revive, depguard for boundaries).
- Layering per [component-architecture](../architecture/component-architecture.md); no global state; constructors for dependencies; `context.Context` first parameter; `tenant.Context` explicit for tenant data.
- Errors: wrap with `%w`; typed domain errors mapped once per transport; no panics across boundaries.
- SQL: explicit, reviewed, parameterised; tenant filter present even with RLS.
- Concurrency: bounded goroutines; `errgroup` for fan-out; no shared mutable state without synchronisation.
- Logging: `slog` structured; redaction rules ([logging](../operations/logging.md)).
- Time: UTC; injected clock.

## TypeScript / Nuxt

- `strict` TS; ESLint + Prettier; no `any` without justification; Vue Composition API; no `v-html` for content; i18n keys for all user text; RTL-safe CSS (logical properties).

## Data and Contracts

- OpenAPI-first; JSON Schema 2020-12; SemVer for versioned contracts; migrations expand/contract.

## AI Assets

- Prompts in registry/package files, never inline in Go; every prompt change evaluated.

## Documentation

- Canonical-owner rule ([documentation-conventions](../governance/documentation-conventions.md)); ADR for architectural changes.
