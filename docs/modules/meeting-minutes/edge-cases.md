# Meeting Minutes — Edge Cases

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |

| Case | Expected behaviour |
|---|---|
| Silent/empty recording | Transcript empty with `no_speech`; if no other sources, job fails `SOURCE_PROCESSING_FAILED` (retryable only with new source); if notes exist, proceed from notes |
| Very poor audio | Low confidences → more review items; banner suggesting better source; never invent content |
| Overlapping speakers | Diarization confidence low → segments attributed "Unknown speaker"; actions owners from those segments → review |
| Participant list missing | Speakers remain labels; owners stored as text if named in speech; review items for owners |
| Same name for two members | Owner candidates presented in review; no auto-pick |
| Agenda contradicts spoken decision | Both kept; `CONTRADICTS`; review item; spoken decision preferred by default config after user confirmation |
| Relative deadlines ("next Tuesday") | Resolved against meeting date; original text preserved; review if meeting date unknown |
| Deadline in the past | Review item |
| Decision later reversed in same meeting | Later statement supersedes; earlier becomes proposal/discussion with `SUPERSEDES` relation |
| Meeting in multiple recordings | Timeline merge by position/capture time; gaps allowed |
| Handwritten notes only | Proceeds; confidence from OCR; recommended review of all decisions |
| Mixed languages | Per-segment routing; artifact language per config (default dominant language) |
| Meeting exceeds limits | Upload blocked with clear limit message (`UPLOAD_TOO_LARGE`/limits) |
| Recording notice not confirmed | Submit blocked (`VALIDATION_FAILED` on job input) where policy requires |
| Confidential content instructions inside documents ("ignore previous instructions") | Treated as data; no behavioural change ([prompt-injection](../../security/prompt-injection.md)) |
| Module disabled mid-processing | Job continues to completion |
| Provider outage | Bounded wait then retryable failure; partial knowledge not published as final |
