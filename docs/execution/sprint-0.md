# Sprint 0 — Executable Foundation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §29.3, App. F (Sprint 0) |

## Goal

Create the executable foundation and core contracts. **Sprint 0 must not start with the Meeting module** (BP §29.3). No business module features.

## Day-1 Decisions (must close in the first 2 days)

**Closed 2026-09-28:** OD-001 → chi v5 ([ADR-023](../governance/adr/ADR-023-http-router-chi.md)) · OD-002 → sqlc + pgx/v5 ([ADR-024](../governance/adr/ADR-024-sql-access-sqlc-pgx.md)) · OD-003 → goose v3 ([ADR-025](../governance/adr/ADR-025-migrations-goose.md)) · OD-024 → pnpm workspace + Taskfile ([ADR-026](../governance/adr/ADR-026-monorepo-pnpm-taskfile.md)) · Go 1.27.x baseline ([ADR-027](../governance/adr/ADR-027-go-runtime-baseline.md)). No Nx/Turborepo/Bazel in Sprint 0.

## Deliverables and Acceptance Criteria

| # | Area (BP App. F priority) | Deliverable | Acceptance criteria |
|---|---|---|---|
| S0-01 | Repository (P0) | Monorepo per [container-architecture](../architecture/container-architecture.md#monorepo-mapping-bp-61): `apps/web`, `apps/api`, `apps/worker`, `internal/`, `modules/platform/hello`, `packages/{web-sdk,module-spec,shared-schemas}`, `migrations/`, `deployments/`, `docs/` | `task setup && task test` succeeds on a clean machine (Linux/macOS/Windows WSL); README quick start |
| S0-02 | Go service bootstrap (P0) | `apps/api` and `apps/worker` binaries with config loading/validation, health endpoints, graceful shutdown, kernel packages (`platform/tenant`, `ids`, `errors`, `db`, `outbox`, `idempotency`, `authz`, `auditlog`, `telemetry`) | `/health` returns ok; invalid config fails fast; import-boundary lint configured and green |
| S0-03 | Nuxt 4 bootstrap (P0) | `apps/web` with TS strict, i18n fa/en + RTL, app shell, PWA manifest + service worker skeleton, API wrapper | App builds; RTL/LTR toggle works; Lighthouse PWA installable check passes |
| S0-04 | Local Docker environment (P0) | Compose: PostgreSQL 16 + pgvector, Redis, MinIO, Temporal + UI, Mailpit, OTel Collector + Jaeger, mock AI provider | `task up` brings all services healthy < 3 min; documented ports |
| S0-05 | Configuration strategy (P0) | Typed config, `.env.example`, secret handling policy, mock-provider guard | No secrets in repo (secret scan green); production config refuses mock provider |
| S0-06 | CI foundation (P0) | Pipeline: format, lint, static analysis, Go/TS unit tests, secret scan, dependency scan, container build | PR shows all checks; failing lint blocks merge |
| S0-07 | Migration tooling (P0) | goose v3 SQL migrations run via `task db:migrate`; migrations 000–005 ([migration-order](../data/migration-order.md)); CI migration test | Fresh apply + re-apply idempotency; destructive-change lint in place |
| S0-08 | Identity foundation (P0) | `Authenticator` interface + local/dev provider; User entity; sessions (hashed tokens, cookie) | Dev login works locally; provider disabled in non-local envs by guard |
| S0-09 | Organization / Tenant context (P0) | Organization, Membership, TenantContext resolution (`Clerk-Organization-Id`), personal workspace creation | Requests without membership → 403; tenant context available in use cases |
| S0-10 | Access foundation + RLS PoC (P0) | Permission catalogue seed, system roles, permission evaluator; RLS on `organizations`, `memberships` + one domain table; `SET LOCAL app.org_id` Tx manager | RLS test: no context → 0 rows; org A cannot read org B; `clerk_app` NOBYPASSRLS asserted in CI |
| S0-11 | Module specification (P0) | `packages/module-spec`: manifest JSON Schema (from [module-manifest.schema.json](../api/schemas/module-manifest.schema.json)), validator CLI (L1–L4 minimum) | All four doc manifests + Hello manifest validate in CI |
| S0-12 | Module Registry skeleton (P0) | Migrations 008–011 skeleton tables; register Hello Module version from package | `platform.hello@0.1.0` stored as published version with manifest hash |
| S0-13 | Source abstraction + storage (P0) | `BlobStore` port (MinIO adapter), Source/SourceFile domain skeleton | Presign PUT/GET scoped to one key works against MinIO |
| S0-14 | Basic upload vertical slice (P0) | `POST /v1/uploads` + `/complete` prototype (single part), Source row, outbox event | Upload of a text/image file creates Source with verified SHA-256 |
| S0-15 | Job abstraction (P1) | Job aggregate + state machine (domain only, table-driven tests) | All transitions in [job-state-machine](../jobs/job-state-machine.md) covered by unit tests |
| S0-16 | Temporal (P1) | Hello workflow + activity with retry and cancellation test; deterministic workflow IDs | Test: activity fails twice then succeeds; cancellation handled; replay test harness in CI |
| S0-17 | OpenAPI (P1) | OpenAPI base (from docs), validation in CI, server types generation, Web SDK thin wrapper; endpoints `/health`, `/me`, `/organizations`, `/modules` skeleton (BP App. F) | OpenAPI validates; contract test for implemented endpoints |
| S0-18 | Observability (P1) | OTel traces/metrics/logs; request_id/correlation_id middleware; trace across API → Temporal | One trace shows API request → workflow → activity with correlation_id |
| S0-19 | Testing infrastructure (P1) | Testcontainers helpers; fixtures; tenant-isolation test helpers; mock AI provider | Integration test suite runs in CI < 10 min |
| S0-20 | Hello/Example declarative module (P0) | `modules/platform/hello`: manifest, job input schema, knowledge subtype `hello.note`, artifact schema `note_summary`, workflow `ingest.require → ai.extract (mock) → artifact.generate` | Package builds and validates; used in Sprint 1 |
| S0-21 | Docs (P1) | ADR/PDR/BDR templates + contribution guide (exist in `/docs/governance`); Day-1 ODs closed as ADRs | ADR-023..027 accepted (done at Documentation Gate) |

## Out of Scope

Real AI providers, real modules, billing provider, production hosting, PDF/DOCX engines (only prototypes if time allows).

## Sprint 0 Definition of Done

All P0 items accepted; P1 items accepted or explicitly carried into Sprint 1 with owner; CI green on main; local stack documented; no open Critical security findings.
