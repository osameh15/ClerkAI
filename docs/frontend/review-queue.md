# Review Queue UI

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend + Product Design |
| Blueprint | BP §1.1 (Human-in-the-loop), App. A (Review Queue: low-confidence fields, evidence, confirm/edit/reject) |

Domain: [review](../platform/collaboration/review.md).

## Views

- **Per job** (`/jobs/:id/review`): items ordered by severity then document order; progress "3 of 12 resolved; 2 blocking remaining".
- **Global** (`/review`): items across jobs assigned to me / in my scope; filters by module, reason, severity.

## Item Card (ReviewItem component)

Reason label + explanation; current value (or "not stated"); candidates (for example owner options, OCR alternatives); evidence preview inline (audio snippet player, image crop with bbox, text excerpt); actions **Confirm** (Enter), **Edit** (E), **Reject** (R), **Defer** (D); comment field.

## Specialised Review (hybrid)

`review.speaker-resolver` (Meeting: play samples per speaker label, assign participant), `review.observation-board` (Site: drag photos/voice to observations, set severity/location), `review.handwriting-spans` (Handwriting: page image with highlighted spans, type-over).

## States

Empty ("Nothing to review"), all resolved (CTA to artifact), permission-limited (read-only view), offline (queue disabled with notice), conflict (item resolved by someone else — refresh).
