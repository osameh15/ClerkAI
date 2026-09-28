# Modules

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §3, §4, §23 |

## Map

| Area | Documents |
|---|---|
| Platform concept | [module-platform](module-platform.md), [module-runtime-contract](module-runtime-contract.md), [module-ui-contract](module-ui-contract.md) |
| Registry & Store | [module-registry](module-registry.md), [module-store](module-store.md), [module-packaging](module-packaging.md), [module-validation](module-validation.md) |
| Lifecycle | [module-lifecycle](module-lifecycle.md), [module-installation](module-installation.md), [module-disable-uninstall](module-disable-uninstall.md), [module-versioning](module-versioning.md), [module-dependencies](module-dependencies.md), [module-migrations](module-migrations.md), [compatibility-policy](compatibility-policy.md) |
| Governance | [module-configuration](module-configuration.md), [module-permissions](module-permissions.md), [module-entitlements](module-entitlements.md), [module-security](module-security.md), [module-telemetry](module-telemetry.md) |
| SDK (authors) | [module-authoring-guide](sdk/module-authoring-guide.md), [manifest-reference](sdk/manifest-reference.md), [schema-reference](sdk/schema-reference.md), [workflow-reference](sdk/workflow-reference.md), [prompt-reference](sdk/prompt-reference.md), [evaluation-reference](sdk/evaluation-reference.md), [testing-modules](sdk/testing-modules.md), [publishing-modules](sdk/publishing-modules.md) |
| Contract | [module-manifest.schema.json](../api/schemas/module-manifest.schema.json) |
| Example manifests | [meeting-minutes](manifests/meeting-minutes.example.yaml), [site-report](manifests/site-report.example.yaml), [handwriting-document](manifests/handwriting-document.example.yaml), [lecture-summary](manifests/lecture-summary.example.yaml) |

## Launch Modules

| Module | Spec |
|---|---|
| `business.meeting-minutes` | [meeting-minutes/](meeting-minutes/README.md) |
| `construction.site-report` | [site-report/](site-report/README.md) |
| `general.handwriting-document` | [handwriting-document/](handwriting-document/README.md) |
| `education.lecture-summary` | [lecture-summary/](lecture-summary/README.md) |

All four manifests validate against the same manifest schema (CI check). None requires a Platform Core change.
