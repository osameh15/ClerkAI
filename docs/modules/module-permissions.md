# Module Permissions

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + Access |
| Blueprint | BP §4 (permission registration), §13.1 |

## Rules

1. Module permissions are namespaced by module key: `<module-key>.<action>` ([permission-namespaces](../platform/access/permission-namespaces.md)).
2. Declared in the manifest with localised descriptions and `defaultRoles`.
3. Registered at install/upgrade into the permission catalogue (`permissions.source = module`) and mapped to system roles per `defaultRoles`.
4. Enforcement is generic: platform operations on module resources check the platform permission **and** the module permission of the same verb (`jobs.create` + `<key>.create`; `artifacts.approve` + `<key>.approve`; `artifacts.edit` + `<key>.edit`; view operations `<key>.view`).
5. Additional module-specific permissions (for example `business.meeting-minutes.view-transcript`) are enforced via declared **field/feature obligations** interpreted by platform components (artifact type visibility, editor section visibility) — not by module code.
6. Removing a permission requires a MAJOR module version; on upgrade, removed permissions are deactivated, not deleted.
