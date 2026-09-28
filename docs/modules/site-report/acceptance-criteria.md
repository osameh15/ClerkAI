# Site Report — Acceptance Criteria

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + QA |
| Blueprint | App. D, §21 |

| ID | Criterion |
|---|---|
| SR-AC-01 | An inspector can create a visit and capture photos, voice and notes with no network; captures persist across app restarts. |
| SR-AC-02 | Each capture shows Local / Pending / Uploading / Synced status; failures show retry. |
| SR-AC-03 | Local files are deleted only after server confirmation and checksum verification. |
| SR-AC-04 | After sync and submit, observations are generated with location, category, severity, issue, corrective action, responsible party, deadline and evidence. |
| SR-AC-05 | A voice note captured in the same observation group as a photo is attached to the correct observation (App. D "Observation linkage"). |
| SR-AC-06 | Critical-severity observations always create review items. |
| SR-AC-07 | Deadlines derived from severity defaults are labelled as defaults, not stated deadlines. |
| SR-AC-08 | A second visit in the same project shows previous open items and allows before/after capture; an open item can become Resolved (with new evidence) or remain open/Overdue (App. D). |
| SR-AC-09 | "Resolved" is never set without human confirmation and after-evidence. |
| SR-AC-10 | Default approval flow Engineer → PM → Consultant works with project roles; Final allocates `SR-…` number. |
| SR-AC-11 | Final report creates Action Items for new corrective actions; overdue flag appears after deadline. |
| SR-AC-12 | Photo-log PDF renders before/after pairs without layout breakage (golden tests). |
| SR-AC-13 | Guests with project grants can view final reports but not drafts (configurable). |
| SR-AC-14 | Evaluation suites `site-core` and `site-reinspection` pass. |
| SR-AC-15 | 200 photos + 30 voice notes visit syncs on a 3G-profile network with resume after interruption (load/resilience test). |
