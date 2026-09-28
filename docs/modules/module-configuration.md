# Module Configuration

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §4, App. A (Modules/Manage: config), App. B (`PATCH /v1/modules/{key}/configuration`) |

## Model

- Manifest `configuration.schema` (JSON Schema) + `uiSchema` + `defaults` + allowed `scopes` (`organization`, `team`, `project`).
- `module_configurations` stores immutable versions per installation; the installation points to the active one. Team/project overrides are stored as scoped configuration versions (resolved most-specific first).
- Jobs pin the effective configuration version at submit.

## Rules

- Validation against schema on every change (`MODULE_CONFIGURATION_INVALID` with JSON Pointer details).
- Platform-enforced floors: organizations can make review stricter but not disable module-declared blocking review for evidence-required high-risk types.
- Secrets are never part of module configuration (no API keys in config); integrations use platform integrations.
- Changes audited with redacted diff; `module.configuration.changed` emitted.
- Configuration UI is generated from schema + uiSchema; hybrid modules may provide a `settings.configuration` extension.
