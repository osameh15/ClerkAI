# Module Store UI

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product Design |
| Blueprint | BP §4, §20, App. A (Module Store, Module Detail), EPIC-15 |

## Views

| View | Content |
|---|---|
| Discover | Featured, categories, search (`q`), listings with publisher badge, entitlement status |
| Installed | Installations with status, version, update availability |
| Updates | Upgrade Available / Required with deadlines |
| Module detail | Description, screenshots, publisher (verified), inputs (source kinds), outputs (artifact types), permissions requested, pricing/entitlement, versions + changelog, dependencies, Install/Enable/Upgrade actions |

## Install UX

1. Install button (requires `modules.install`; otherwise "Request from admin").
2. Pre-check: entitlement → if missing, upgrade path.
3. Dialog: version (default latest compatible), version policy, assignments (org/teams/projects), initial configuration (schema form with defaults).
4. Progress: operation status (polling `ModuleOperation`), then success with "Open module" CTA.

## States

Loading skeletons; error with retry; offline read-only (cached catalog); permission-denied for install; entitlement upgrade CTA; "Disable is not uninstall" explanation in management screens ([module-disable-uninstall](../modules/module-disable-uninstall.md)).
