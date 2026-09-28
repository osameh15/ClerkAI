# Module Installation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4, EPIC-05, G5 ("Four modules install as packages") |

## Installation Lifecycle

```mermaid
sequenceDiagram
  autonumber
  participant A as Admin (UI)
  participant API as clerk-api
  participant DB as PostgreSQL
  participant T as ModuleInstallWorkflow
  participant ACC as Access
  participant COM as Commerce
  participant TPL as Templates
  participant WF as Workflow
  A->>API: POST /v1/modules/{key}/install (Idempotency-Key, version?, config?)
  API->>COM: entitlement module.{key} + required entitlements + modules.installed.max
  API->>DB: tx: module_operations(pending), outbox, audit
  API->>T: start (module-op/{operation_id})
  API-->>A: 202 ModuleOperation
  T->>T: resolve version (latest compatible or requested)
  T->>T: check compatibility.platform + capabilities available
  T->>T: resolve dependencies (install required deps first, same workflow)
  T->>DB: validate initial configuration vs schema (defaults merged)
  T->>ACC: register module permissions + default role mappings
  T->>TPL: register default templates (org-visible) + numbering sequences
  T->>WF: register approval workflow definitions (module scope)
  T->>DB: tx: module_installations(enabled) + module_configurations(v1) + operation succeeded + outbox module.installed + audit
  Note over T: any failure → compensate registered artifacts, operation failed, no installation row
```

## Rules

- Idempotent by operation (and by `Idempotency-Key`); a second install on an existing non-uninstalled installation → `409 MODULE_ALREADY_INSTALLED`.
- Only one in-flight operation per (org, module) (`MODULE_OPERATION_IN_PROGRESS`).
- Registration steps are idempotent upserts keyed by (org, module, version, item key).
- Assignments (teams/projects) optional at install; default = organization-wide.
- Installing never grants permissions beyond manifest `defaultRoles` mapping to system roles; custom roles are untouched.
- Emits `module.installed`; the launcher shows the card immediately for users with `<key>.create`.

## Failure Modes

| Failure | Result |
|---|---|
| Not entitled | 402 before operation starts |
| Incompatible platform/capability missing | Operation failed `MODULE_VERSION_INCOMPATIBLE` |
| Dependency unsatisfiable | Operation failed `MODULE_DEPENDENCY_UNSATISFIED` with details |
| Registration failure (transient) | Retried; persistent → compensation + failed |
