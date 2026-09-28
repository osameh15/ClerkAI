# Frontend Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §19, §29.2 (UX states) |

| Suite | Scope |
|---|---|
| Component tests | Every catalogue component in all states ([component-catalog](../frontend/component-catalog.md)), fa (RTL) + en (LTR) |
| Visual regression | Story snapshots for key components and screens (RTL/LTR, mobile/desktop) |
| Schema form | JSON Schema + x-ui rendering, validation messages, conditional visibility |
| Stores/composables | Org switching clears caches; SSE reconnect; error mapping |
| Offline | IndexedDB repositories, upload queue state machine, sync engine with mocked network interruptions |
| Accessibility | Automated a11y checks (axe) on key screens; keyboard navigation tests for review queue/editor |
| Security | No `v-html` on content (lint rule); CSP compliance in E2E |
