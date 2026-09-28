# Module Store

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4 ("not merely a list of cards… registry, installation, dependency resolution, versioning, configuration, entitlement, and permission registration"), §20, App. A, [PDR-002](../governance/pdr/PDR-002-card-launcher-and-module-store.md) |

## Functions

| Function | Behaviour | Doc |
|---|---|---|
| Discovery | Browse by category, search, featured; localised listing | [module-store-ui](../frontend/module-store-ui.md) |
| Catalog | Listing: name, publisher (verified badge), inputs, outputs, pricing/entitlement, versions, changelog, permissions requested | [listings](../marketplace/listings.md) |
| Installation | Asynchronous workflow | [module-installation](module-installation.md) |
| Dependency resolution | Required/optional deps resolved and installed or reported | [module-dependencies](module-dependencies.md) |
| Entitlement validation | `module.<key>` + required entitlements; upgrade CTA if missing | [module-entitlements](module-entitlements.md) |
| Version compatibility | Platform compatibility + dependency ranges | [compatibility-policy](compatibility-policy.md) |
| Permission registration | Module permissions added to catalogue + default role mapping | [module-permissions](module-permissions.md) |
| Workflow registration | Approval workflow definitions registered for the org (module scope) | [workflow-architecture](../architecture/workflow-architecture.md) |
| Template registration | Default templates made available to the org | [template-engine](../artifacts/template-engine.md) |
| Configuration validation | Initial config validated against schema | [module-configuration](module-configuration.md) |
| Enable / Disable | Status changes | [module-disable-uninstall](module-disable-uninstall.md) |
| Upgrade | Compatible version upgrade with migrations | [module-versioning](module-versioning.md), [module-migrations](module-migrations.md) |
| Deprecation | Listing badge; upgrade guidance; policy may restrict new jobs | [module-lifecycle](module-lifecycle.md) |
| Suspension | Platform/billing/security suspension | [module-lifecycle](module-lifecycle.md) |
| Uninstall | Logical removal; data retained per policy | [module-disable-uninstall](module-disable-uninstall.md) |
| Data retention after uninstall | Artifacts/Knowledge follow retention policy; not deleted by uninstall | [module-disable-uninstall](module-disable-uninstall.md) |
| Version pinning | Installation `version_policy = pinned` | [module-versioning](module-versioning.md) |
| Auto-update policy | `auto_patch` (default) / `auto_minor` / `pinned`; majors always manual | [module-versioning](module-versioning.md) |

## Views

- **Discover:** all public listings visible to the org (entitlement status shown, not hidden).
- **Installed:** installations with status, version, update availability.
- **Updates:** installations with Upgrade Available/Required.

## API

`GET /v1/store/modules`, `GET /v1/store/modules/{key}`, `POST /v1/modules/{key}/install|enable|disable|upgrade|uninstall`, `GET|PATCH /v1/modules/{key}/configuration`, `GET /v1/modules` (launcher).

## Marketplace Stage

v1 = **M0**: internal store, Clerk Official only ([marketplace-roadmap](../marketplace/marketplace-roadmap.md)).
