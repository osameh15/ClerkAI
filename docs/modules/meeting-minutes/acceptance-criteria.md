# Meeting Minutes — Acceptance Criteria

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + QA |
| Blueprint | App. D, §29.2 |

| ID | Criterion (Given / When / Then) |
|---|---|
| MM-AC-01 | Given an installed, enabled module and a member with `create`, when they record or upload a meeting and submit, then a Job is queued with pinned versions and processing starts without further input. |
| MM-AC-02 | Adding sources to a draft never starts processing (no auto-submit). |
| MM-AC-03 | Minutes list decisions, proposals, discussion points, unresolved topics, actions, risks and open questions in separate sections. |
| MM-AC-04 | Every decision and action in the draft has ≥ 1 openable evidence link, or an explicit warning/review item. |
| MM-AC-05 | An ambiguous speaker/owner produces a Review Item rather than a guess (BP App. D). |
| MM-AC-06 | Actions without a stated deadline show "no deadline stated" (not an invented date). |
| MM-AC-07 | Jalali dates are displayed as spoken and normalised; exported documents use the org calendar setting. |
| MM-AC-08 | Resolving review items regenerates affected sections as a new version (origin `system`); prior versions remain. |
| MM-AC-09 | Editing the minutes requires `If-Match`; concurrent edits produce a conflict UI, never silent overwrite. |
| MM-AC-10 | With approval required, Final is impossible until all stages approve; document number is allocated at Final. |
| MM-AC-11 | On Final, Action Items are created (per config) with owner, deadline, link to minutes and evidence. |
| MM-AC-12 | PDF and DOCX exports of golden samples render in Persian RTL without clipping (golden tests). |
| MM-AC-13 | Executive summary and follow-up can be generated later from the same Knowledge without re-transcription or extra ASR cost. |
| MM-AC-14 | Transcript is hidden for users lacking `view-transcript`. |
| MM-AC-15 | Evaluation suite `meeting-core` passes all blocking thresholds. |
| MM-AC-16 | Worker crash during transcription does not lose the job; job completes (G7). |
| MM-AC-17 | Usage events and audit events are recorded for AI generation, edits, approvals and exports. |
