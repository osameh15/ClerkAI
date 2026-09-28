# Module Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + QA |
| Blueprint | BP §18.1 (module validation), §19 (Contract: Module manifests), G5, G11 |

Author guide: [testing-modules](../modules/sdk/testing-modules.md).

## Platform-Level Module Tests

| Test | Purpose |
|---|---|
| Hello Module E2E | Proves platform genericity: installs from manifest without Core change; job → workflow → artifact with permission/entitlement/audit/usage (BP §29.3) |
| Install/disable/enable/upgrade/uninstall matrix | Lifecycle behaviour per [module-lifecycle](../modules/module-lifecycle.md) incl. "disable ≠ uninstall" data retention |
| Dependency resolution | Required/optional deps, conflicts, cycles |
| Permission registration | Module permissions appear in catalogue, mapped to default roles, deactivated on uninstall |
| Compatibility | Incompatible platform range rejected |
| Pinning | Running jobs unaffected by upgrade; new jobs use new version |
| Four launch modules | Each passes manifest validation, workflow dry-run with mock AI, mapping/template golden tests, and module E2E |

## G5 Exit Evidence

All four launch module packages install successfully as packages in a clean organization (BP §24).
