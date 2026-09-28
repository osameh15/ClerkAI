# ADR-022: Frontend = Nuxt 4 + TypeScript + PWA

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09-28 |
| Deciders | Technical Leadership, Frontend Lead |
| Blueprint | BP §6, §20, §21 |

## Decision

The web client is a **Nuxt 4** application written in **TypeScript**, installable as a **PWA**, responsive and mobile-first, using IndexedDB for offline-capable workflows (Site Report). It talks to the backend only through the versioned REST API via the generated Web SDK. Module cards, forms and navigation are API-driven.

## Consequences

Service worker, upload queue and sync live in the client ([pwa-architecture](../../pwa/pwa-architecture.md)). Rendering mode: SPA for the authenticated app (no server-side access to tenant data from the Nuxt server) unless a later ADR justifies SSR for specific public pages (Store listing).

## Related

[frontend-architecture](../../frontend/frontend-architecture.md)
