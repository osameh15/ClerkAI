# State Management

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §20, §21 |

## State Categories

| Category | Tool | Examples | Persistence |
|---|---|---|---|
| Server state | Query cache layer (composables around Web SDK with SWR semantics) | jobs, artifacts, modules | Memory; selected lists cached in IndexedDB for offline read |
| Session/org context | Pinia store | user, active org, permissions/entitlement hints | Memory (+ last org in localStorage) |
| Offline queue | Pinia + IndexedDB repositories | local visits, captures, pending uploads | IndexedDB ([indexeddb-model](../pwa/indexeddb-model.md)) |
| UI state | Component local / URL query | filters, open panels | URL |
| Editor draft buffer | Editor-local + IndexedDB autosave | unsaved edits | IndexedDB until saved |

## Rules

- Server is source of truth; client caches keyed by org ID; org switch clears org-scoped caches.
- Mutations use idempotency keys and optimistic updates only where safe (for example review item resolution), always reconciled with server response.
- No sensitive data in localStorage; IndexedDB data cleared on sign-out (with unsynced-data warning).
