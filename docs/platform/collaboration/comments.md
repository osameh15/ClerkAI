# Comments and Mentions

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline (v1 minimal) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Collaboration domain |
| Blueprint | BP §7 (Collaboration: Comment, Mention, Share), §7.3 (`comments`) |

## Scope (v1)

- Threaded comments on Artifacts (optionally anchored to a content path), Review Items and Approval Requests.
- Mentions of organization members trigger notifications.
- Body: sanitised Markdown subset (bold, italic, lists, links https-only); no HTML, no images inline.
- Edits keep `edited_at`; deletion tombstones content.

## Rules

- Visibility follows the subject's visibility (resource policy); mentioning someone who cannot see the subject does not grant access and is rejected with a validation error.
- Comments are Confidential data; covered by retention category of their subject.
- Comment API endpoints are **deferred** in the initial OpenAPI (not in BP App. B); added with EPIC-14 UX. Data model exists (migration 030).

## Future

Real-time presence and live co-editing (WebSocket) — Future ([ADR-021](../../governance/adr/ADR-021-sse-progress.md)).
