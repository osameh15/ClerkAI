# Frontend Error Handling and UX States

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product Design |
| Blueprint | BP §29.2 ("User-facing UX includes empty, loading, error, and access-denied states") |

## Required States per Screen

| State | Pattern |
|---|---|
| Loading | Skeletons matching layout; no spinners-only for > 300 ms content |
| Empty | Explanation + primary CTA (for example "Install a module", "Capture your first observation") |
| Error (retryable) | Inline message + Retry; request ID for support |
| Error (non-retryable) | Clear explanation + next step |
| Permission-denied (403) | "You don't have access" + ask-admin guidance |
| Not found / no access (404) | Neutral not-found page (no existence disclosure) |
| Entitlement upgrade (402) | Upgrade/ask-admin sheet with the specific feature named |
| Quota exceeded (402 BILLING_QUOTA_EXCEEDED) | Usage meter + options (upgrade, overage, wait for reset) |
| Offline | Offline banner; read-only cached data; capture queue available where supported |
| Conflict (409/412) | Refresh/merge UI |
| Rate limited (429) | Auto-retry with countdown |
| Maintenance/read-only (503) | Banner; disable mutations |

## Error Mapping

`ClerkApiError.code` → localised message catalogue (fa/en) keyed by error code ([error-catalog](../api/error-catalog.md)); unknown codes fall back by HTTP status.

## Happy-Path Is Not Enough

Every Epic's Definition of Done includes screenshots/stories of the states above ([definition-of-done](../execution/definition-of-done.md)).
