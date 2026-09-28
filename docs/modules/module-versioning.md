# Module Versioning

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §1.1 (Version everything), §4.1 (Upgrade Available/Required), §8 (module versioning independent of API) |

## SemVer for Modules

| Bump | Examples |
|---|---|
| MAJOR | Breaking artifact schema change; removed input slot; removed permission; configuration schema breaking change; knowledge type removal |
| MINOR | New artifact type, new optional slot, new knowledge subtype, new configuration option, prompt MINOR bumps |
| PATCH | Prompt wording fixes, template fixes, translation fixes |

## Version Policies (per installation)

| Policy | Behaviour |
|---|---|
| `auto_patch` (default) | Patch versions applied automatically after publication (non-disruptive, in-flight jobs keep pins) |
| `auto_minor` | Minor + patch automatic |
| `pinned` | No automatic changes; admins upgrade manually |

MAJOR upgrades are always manual and may require migrations ([module-migrations](module-migrations.md)).

## Pinning Semantics

- Installation pins a version; **jobs pin at submit** (the version active on the installation at that moment).
- Upgrading an installation never changes running jobs or existing artifacts' schema refs.
- Upgrade Required: when a version becomes `deprecated` with a sunset date or `blocked`, installations show Upgrade Required with `upgrade_required_by`; after the date new jobs are blocked (policy), existing data remains readable.

## Upgrade Flow

`POST /v1/modules/{key}/upgrade` → ModuleInstallWorkflow(operation=upgrade): compatibility & dependency check → configuration migration (transform old config to new schema; fail if impossible, requiring admin input) → register new permissions/templates/workflows (old ones retained for history) → switch `module_version_id` → `module.upgraded`.
