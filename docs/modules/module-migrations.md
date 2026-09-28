# Module Migrations

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §7.2 (no new Core tables per module), §4.1 (Upgrade Required) |

## Principle

Modules never create database tables. "Migration" for modules means **declarative data transforms** between schema versions, executed by the platform.

## Transform Types

| Target | When | Mechanism |
|---|---|---|
| Configuration | MAJOR/MINOR config schema change | `migrations/config/<from>-<to>.jsonata` (or equivalent declarative mapping), executed at upgrade; failure → admin input required |
| Knowledge attributes | Subtype schema change | Optional; existing items keep old `schema_ref` (readable); transform only if new jobs need to reference old items (cross-job modules) |
| Artifact content | Artifact schema MAJOR change | **Not migrated in place.** Existing versions keep their schema ref; editing an old artifact may offer "upgrade to new format" creating a new version via declared transform |
| Templates | Template changes | New template versions; old versions retained for re-rendering history |

## Rules

- Transforms are pure, deterministic, tested with fixtures in the module package.
- The registry keeps all schemas referenced by any stored data.
