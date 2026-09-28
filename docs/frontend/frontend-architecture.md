# Frontend Architecture

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (UI kit Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §6, §8, §20, §21, [ADR-022](../governance/adr/ADR-022-nuxt-pwa-frontend.md), [ADR-009](../governance/adr/ADR-009-schema-driven-module-ui.md) |

## Stack

Nuxt 4 · TypeScript (strict) · Vue 3 Composition API · PWA (service worker, manifest) · IndexedDB (offline) · generated Web SDK from OpenAPI ([OD-025](../governance/open-decisions.md#od-025)) · UI kit/editor per [OD-014](../governance/open-decisions.md#od-014) · i18n (fa/en, RTL/LTR) · Pinia for client state.

## Structure

```text
apps/web/
  app/
    pages/                 # route-level screens (see information-architecture)
    layouts/               # app shell (nav, org switcher, sync status)
    components/
      platform/            # ModuleCard, SourceUploader, ProcessingTimeline, ArtifactEditor, EvidenceViewer, ...
      schema-form/         # JSON Schema + x-ui renderer
      extensions/          # registered module extension components (hybrid/custom)
    composables/           # useApi, useJobStream (SSE), useEntitlements, usePermissions, useOffline
    stores/                # Pinia: session, org context, offline queue, preferences
    services/
      api/                 # Web SDK wrapper (auth headers, org header, idempotency keys, ETags, error mapping)
      offline/             # IndexedDB repositories, sync engine
    plugins/               # i18n, telemetry, service worker registration
  public/                  # icons, manifest
packages/web-sdk/          # generated OpenAPI client
```

## Principles

1. **API-driven everything**: module cards, forms, stages, permissions/entitlement hints come from the API; no module-specific code outside `components/extensions/` ([module-ui-contract](../modules/module-ui-contract.md)).
2. **Server is authoritative**: UI gates are hints; every action handles 401/402/403/404/409/412/422.
3. **SPA for the authenticated app**; no SSR access to tenant data from the Nuxt server; public Store pages may be SSR/SSG later.
4. **Designed states**: empty, loading, error, permission-denied, entitlement-upgrade, offline for every screen ([error-handling](error-handling.md)).
5. **RTL-first**: logical CSS properties, bidi-safe components, Jalali date support.
6. **Accessibility**: WCAG 2.2 AA target; keyboard-first review/editor.
7. **Security**: strict CSP, no `v-html` for content, safe links, CSRF header ([output-validation](../security/output-validation.md)).

## Documents

[information-architecture](information-architecture.md) · [navigation](navigation.md) · [module-launcher](module-launcher.md) · [module-store-ui](module-store-ui.md) · [universal-input-ui](universal-input-ui.md) · [artifact-editor](artifact-editor.md) · [review-queue](review-queue.md) · [evidence-ui](evidence-ui.md) · [processing-status-ui](processing-status-ui.md) · [permissions-ui](permissions-ui.md) · [billing-ui](billing-ui.md) · [responsive-design](responsive-design.md) · [design-system](design-system.md) · [component-catalog](component-catalog.md) · [state-management](state-management.md) · [api-client](api-client.md) · [error-handling](error-handling.md)
