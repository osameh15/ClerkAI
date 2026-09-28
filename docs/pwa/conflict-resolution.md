# Conflict Resolution

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (v1 rules) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §21 ("conflict handling belong[s] in the data flow") |

## v1 Conflict Surface (small by design)

Offline operations in v1 are **append-mostly**: creating visits and adding captures. Conflicts are therefore limited.

| Situation | Rule |
|---|---|
| Same local job synced twice | Idempotency key returns the same server job |
| Job metadata edited offline and online elsewhere | Server `ETag` check on PATCH; on 412 → show both values, user chooses (field-level) |
| Capture added offline to a job already submitted online by another device | Server rejects attach to non-draft (`JOB_INVALID_TRANSITION`) → PWA offers "create follow-up visit" or "add to Inbox" |
| Job deleted/cancelled on server while device offline | Captures moved to local "Unassigned" → user can upload to Inbox or new visit |
| Action item resolved offline (future) | Server wins on status conflicts with prompt |
| Module disabled/entitlement lost while offline | Sync of captures to Inbox still allowed; job creation fails with clear message |

## Principles

Never discard user-captured data automatically; prefer creating additional server objects (Inbox Sources) over overwriting.
