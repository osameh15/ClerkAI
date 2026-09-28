# Feature Gates

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce + Frontend |
| Blueprint | BP §13, §20.2 (EntitlementGate, PermissionGate) |

## Definition

A **feature gate** is a named point in the product where an entitlement check applies (for example `export:docx`, `artifact:minutes:approval`). Modules declare their gates in the manifest (`entitlements.features[].gates`); platform gates are listed in the entitlement catalogue.

## Three Different Gates

| Gate | Checks | Backed by | UI component | Failure UX |
|---|---|---|---|---|
| EntitlementGate | Organization capability | Entitlements | `EntitlementGate` | Upgrade prompt / contact admin |
| PermissionGate | User permission | RBAC + policy | `PermissionGate` | Hidden or disabled with explanation |
| Feature flag | Rollout/kill switch | Feature flags | none (invisible) | Feature absent |

Feature flags never substitute for entitlements ([feature-flag-strategy](../feature-flags/feature-flag-strategy.md)).

## Client Hints

`GET /v1/me` and `GET /v1/modules` return effective permission and entitlement hints so the UI can render gates without extra round-trips. The server re-checks every operation.
