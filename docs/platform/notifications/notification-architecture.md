# Notification Architecture

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Notifications domain |
| Blueprint | BP §7 (Platform: Notification), §7.3 (`notifications`) |

## Design

- Notifications are produced by **event consumers** (never called synchronously by domain code).
- Channels (v1): in-app (stored in `notifications`, delivered via polling/SSE on the shell) and email (transactional provider behind a `Mailer` port; Mailpit locally).
- Preferences per membership × type × channel; security notifications (new session, MFA change, API key created) cannot be disabled.

## Notification Types (v1)

| Type | Trigger event | Default recipients |
|---|---|---|
| `job.completed` | `job.processing.completed` | Job creator |
| `job.review_required` | `job.review.required` | Job creator, assigned reviewers |
| `job.failed` | `job.failed` | Job creator |
| `approval.requested` | `approval.requested` | Approvers |
| `approval.decided` | `approval.decided` | Submitter |
| `action.assigned` | `action.created`/`updated` (owner changed) | Owner |
| `action.overdue` | `action.overdue` | Owner, job creator |
| `module.installed` / `module.suspended` | module events | Admins |
| `quota.threshold` | `quota.threshold.reached` | Owner, billing delegates |
| `security.*` | identity/integration events | Affected user / Owner |
| `export.ready` | `artifact.export.completed` | Requester |

## Rules

- Payloads contain IDs and short labels; no customer content in emails beyond titles the user already sees (for example artifact title). Email links point into the app (authentication required).
- Localised by recipient locale.
- Idempotent per (event_id, recipient, channel).
- Digests (batching) — Future.
