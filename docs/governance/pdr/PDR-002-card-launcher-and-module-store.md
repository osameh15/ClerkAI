# PDR-002: Home = Module Card Launcher; Module Store Is Top-Level Navigation

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Product / Design |
| Blueprint | BP §4, §20, §28.2, App. A |

## Decision

1. The Home screen is a **Card View Module Launcher** of installed and enabled modules the user may use, plus Recent Jobs and **Quick Clerk** (source-first entry).
2. The **Module Store** is a primary navigation destination from v1 (Discover, Category, Search, Installed, Updates). It is a platform service, not a list of cards: registry, installation, dependency resolution, versioning, configuration, entitlement and permission registration.
3. Cards and Store listings are **API-driven** (`GET /v1/modules`, `GET /v1/store/modules`); no module card is hard-coded in the frontend.

## Consequences

- Cards reflect state: enabled, entitlement-locked (upgrade CTA), permission-hidden, disabled, suspended.
- Store exists in v1 even with only `clerk-official` modules (Marketplace stage M0).

## Related

[module-launcher](../../frontend/module-launcher.md), [module-store-ui](../../frontend/module-store-ui.md), [module-store](../../modules/module-store.md)
