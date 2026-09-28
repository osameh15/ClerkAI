# Runbook: Stuck Workflow

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Processing domain |

**Symptoms:** job in `queued`/`processing` without step progress beyond expected time; user reports "stuck".

**Detection:** stuck-run alert (no step update > 2 h, excluding `needs_review`/approval waits); reconciler reports.

**Immediate actions:** look up job → run → `temporal_workflow_id` (admin tool); check workflow state in Temporal UI.

**Diagnosis:**
| Finding | Cause |
|---|---|
| No workflow exists | Post-commit start failed → reconciler should start it |
| Workflow waiting on signal unexpectedly | Missed signal (review/approval) → PostgreSQL state says resolved |
| Activity retrying repeatedly | Poison input, provider issue, bug |
| Non-determinism error | Incompatible worker deploy |
| Waiting for org concurrency semaphore | Org at `jobs.concurrent` limit (expected) |

**Mitigation:** start/re-signal from PostgreSQL state (idempotent); fix/rollback worker; mark poison source extraction failed; cancel and retry job (new run) for the user.

**Recovery/Validation:** job reaches terminal state; user notified.

**Escalation:** Tech Lead for non-determinism or systemic cases.

**Post-incident:** add failing case to tests; improve progress/timeout configuration.
