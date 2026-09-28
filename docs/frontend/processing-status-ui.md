# Processing Status UI

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend |
| Blueprint | BP §8 (SSE, frontend must not know internal stages), App. A (Processing: stage timeline, approximate progress, cancel, error/retry), §20.2 (ProcessingTimeline) |

## ProcessingTimeline

- Stages from the module workflow (localised labels) — not internal steps.
- Per-stage status: pending / running / done / waiting (review) / failed / skipped.
- Approximate progress bar; ETA shown only when module historical data supports it.
- Live via SSE (`useJobStream`), fallback to polling every 5 s; reconnect with `Last-Event-ID`.

## Actions

Cancel (confirmation; explains what is kept), Retry (on failed, if retryable), Go to review (on needs_review), Open artifact (on completed).

## Error Display

Maps failure codes to human messages and next steps (for example `SOURCE_QUARANTINED` → "A file was blocked for security reasons"; `AI_PROVIDER_UNAVAILABLE` → "Processing is delayed; you can retry"). Never shows raw technical details; shows request ID for support.

## Background Behaviour

Users can leave the page; notifications (in-app/email) announce completion/review needed.
