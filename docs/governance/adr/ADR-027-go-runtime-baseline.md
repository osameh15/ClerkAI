# ADR-027: Go Runtime Baseline = Go 1.27.x

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership |
| Blueprint | BP §6 (Backend Core: Go), [ADR-001](ADR-001-backend-go.md) |

## Decision

Pin the Go toolchain to the **1.27.x** line. CI and development environments use the **latest approved patch release** of Go 1.27.

## Rules

1. `go.mod` / `go.work` declare `go 1.27` and a `toolchain go1.27.<patch>` directive set to the approved patch.
2. The approved patch is recorded in one place (the `toolchain` directive) and consumed by CI images, dev containers and Taskfile checks (`task doctor` verifies the local version).
3. Patch upgrades (security/bug fixes) are applied promptly via a normal PR once CI passes; minor-line upgrades (1.28+) require a new ADR or an amendment note after compatibility testing (Temporal SDK, pgx, sqlc, chi, OTel).
4. Container build images use the same pinned toolchain.
