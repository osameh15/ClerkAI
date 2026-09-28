# Navigation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product Design + Frontend |
| Blueprint | BP §20 |

## App Shell

| Element | Desktop | Mobile |
|---|---|---|
| Primary nav | Left sidebar: Home, Inbox, Jobs, Artifacts, Actions, Projects, Module Store | Bottom bar: Home, Inbox, Create (+), Actions, More |
| Org switcher | Top-left; switching sets `Clerk-Organization-Id` and clears org-scoped caches | Sheet from avatar |
| Settings | Sidebar footer (permission-filtered) | "More" |
| Sync status | Header indicator (offline/pending uploads count) | Persistent chip |
| Notifications | Bell with unread count | Same |
| Review/approvals badges | Counts on Jobs/Approvals | Same |

## Rules

- Nav items hidden when the user lacks view permission; Module Store always visible (discovery), install buttons gated.
- Deep links include org context (`?org=<id>`) resolved against memberships; mismatch → org chooser.
- Back navigation preserves list filters (URL query state).
- RTL mirrors layout direction; icons with direction semantics mirrored.
