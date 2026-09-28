# Module Entitlements

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + Commerce |
| Blueprint | BP §4, §5, App. C, [BDR-003](../governance/bdr/BDR-003-module-entitlements.md) |

## Declarations

| Manifest field | Meaning |
|---|---|
| `entitlements.required[]` | Must be held to install and to create jobs (typically `module.<key>`) |
| `entitlements.features[]` | Entitlement key → module feature gates (for example `workflow.approval` → `artifact:minutes:approval`) |
| `entitlements.meters[]` | Usage meters the module consumes (for pricing previews in the Store) |

## Checks

| Operation | Entitlement check |
|---|---|
| Install / enable / upgrade | `required[]`, `modules.installed.max` |
| Create/submit job | `required[]` (+ `limited` quota semantics) + quota reservations for meters |
| Use gated feature | Feature key |
| Existing data read | **No entitlement required** (historical access never depends on current plan) |

## Loss of Entitlement

On `entitlement.changed` removing `module.<key>`: installations → `suspended` (reason `billing`); in-flight jobs complete; new jobs blocked with upgrade CTA; data readable.

## Packs

Add-ons like `pack.construction` grant multiple module entitlements through `organization_entitlements` rows with `source=addon` ([organization-overrides](../platform/entitlements/organization-overrides.md)).
