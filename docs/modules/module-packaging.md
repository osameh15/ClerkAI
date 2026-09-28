# Module Packaging

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4 (package layout), §6.1 (`modules/` in monorepo) |

## Source Layout

Official modules live in the monorepo under `modules/<category>/<name>/` with the structure in [module-platform](module-platform.md). `packages/module-spec` provides the manifest schema and the validator CLI.

## Build

`module-spec build modules/business/meeting-minutes` →
1. Validate ([module-validation](module-validation.md)).
2. Resolve and embed referenced files; compute per-file SHA-256.
3. Produce `package.tar.zst` + `package.lock.json` (file list, hashes, manifest hash, builder version).
4. Upload to `platform/modules/{key}/{version}/` and register a `draft` ModuleVersion.

## Integrity

- `module_versions.package_sha256` and `manifest_sha256` stored; the worker verifies hashes when loading definitions.
- Third-party packages (future) must be signed by the publisher key registered in Marketplace, and counter-signed by Clerk after review ([trust-and-verification](../marketplace/trust-and-verification.md)).

## Size Limits

Package ≤ 20 MB (templates/fonts/assets); evaluation datasets are referenced, not bundled.
