# Permissions and Entitlements UI

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product Design |
| Blueprint | BP §13, §20.2 (EntitlementGate, PermissionGate), App. A (Roles & Permissions) |

## Gates

| Component | Input | Denied rendering |
|---|---|---|
| `PermissionGate` | permission key (+ resource) | Hide (navigation) or disabled control with tooltip "You don't have permission — ask an admin" |
| `EntitlementGate` | entitlement key | Upgrade prompt for Owners/billing managers; "Available on higher plans — ask your admin" for others |

Server responses override hints: `403` → permission-denied state; `402` → entitlement-upgrade state ([api/authorization](../api/authorization.md)).

## Roles & Permissions Screen

Role list (system + custom), permission matrix grid grouped by namespace (platform + installed modules), create/edit custom role (entitlement-gated), assignment management with scope pickers (org/team/project), audit link.

## Permission-Denied States (full-page)

Direct links to inaccessible resources show 404-style "not found or no access" (no existence disclosure) with a link home.
