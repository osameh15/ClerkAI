# Module Launcher (Home)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product Design |
| Blueprint | BP §20, [PDR-002](../governance/pdr/PDR-002-card-launcher-and-module-store.md), App. A (Home/Create) |

## Data Source

`GET /v1/modules?view=launcher` → `ModuleCard[]` (name, icon, accent token, order, labels, `launch_state`, ui_mode). **No hard-coded cards.**

## Card States

| `launch_state` | Rendering | Action |
|---|---|---|
| `available` | Normal card | Opens `/m/:key/new` |
| `entitlement_required` | Card with lock + "Upgrade" | Opens upgrade/contact-admin sheet (402 semantics) |
| `permission_denied` | Hidden by default (optionally greyed with "Ask your admin") | — |
| `disabled` / `suspended` | Visible to admins only with status badge | Link to module management |
| `not_installed` | Shown in "Suggested" row for admins | Link to Store detail |

## Other Home Sections

- **Quick Clerk**: drop zone / capture buttons → creates Sources in Inbox; then suggests modules that accept those kinds.
- **Recent jobs**: last 10 with status chips; items needing review first.
- **Empty state (new org)**: guidance to install modules (admins) or ask admin (members).

## Performance

Cards cached per org (stale-while-revalidate); offline: last cached cards shown with offline badge; Site Report card remains usable offline.
