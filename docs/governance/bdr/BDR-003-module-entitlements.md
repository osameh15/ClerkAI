# BDR-003: Module Entitlement Is Separate from User Permission

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Founder, Technical Leadership |
| Blueprint | BP §1.1, §13, §28.3, App. C |

## Decision

- **Entitlement:** the organization's right to a capability (module, feature, quota) from Plan, add-on, contract or override.
- **Permission:** the user's operational right via Role and resource policy.
- An action is allowed only if both pass (and resource policy allows). Example: a Member with `artifacts.export` cannot export DOCX if the organization is not entitled to `export.docx`; an entitled organization's Viewer still cannot export without the Permission.
- Modules declare both the permissions they register and the entitlement keys they require.

## Related

[authorization-model](../../platform/access/authorization-model.md), [entitlement-model](../../platform/entitlements/entitlement-model.md), [module-entitlements](../../modules/module-entitlements.md)
