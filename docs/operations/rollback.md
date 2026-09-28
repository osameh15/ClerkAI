# Rollback

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | DevOps / SRE |
| Blueprint | BP §16.1 (Rollback paths tested), §18 (rollback/roll-forward plans), §29.2 |

| Change type | Rollback mechanism | Time target |
|---|---|---|
| Application code | Redeploy previous image (schema is expand-compatible) | < 15 min |
| Feature | Disable feature flag | < 5 min |
| AI model/profile route | Revert route / disable canary flag | < 5 min |
| Prompt version | Re-activate previous version | < 5 min |
| Module version | Installations pinned/auto-update rollback: republish previous as recommended; `yank` bad version | < 30 min |
| Migration (expand) | Rollback script or leave (compatible) | Per plan |
| Migration (contract) | Roll-forward only; restore from backup as last resort | — |
| Frontend | Re-point static assets to previous build; SW kill-switch | < 15 min |

## Triggers

Automatic: canary error rate or latency SLO breach, schema-validity drop for AI changes. Manual: on-call judgement. Every rollback produces an incident or change record.

## Testing

Rollback drills per quarter in staging (deploy N → N+1 → N) including worker replay.
