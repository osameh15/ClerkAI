# Runbook: AI Provider Outage

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + SRE |

**Symptoms:** AI steps failing/retrying, circuit breakers open, jobs stuck in processing or failing with `AI_PROVIDER_UNAVAILABLE`, schema-invalid spikes (degraded provider).

**Detection:** provider error-rate alerts; circuit-breaker state; provider status page.

**Immediate actions:** confirm fallback routes are carrying traffic; if a profile has no healthy route, enable degraded-mode banner; declare SEV2 if user-visible.

**Diagnosis:** which provider/model/region; error class (auth vs capacity vs 5xx); did a credential rotate? did provider deprecate the pinned model?

**Mitigation:** (1) confirm/force fallback; (2) temporarily add an evaluated alternative route via emergency model change (change record retroactive within 24 h; only routes with prior passing evaluation); (3) pause new submissions for affected modules if quality would drop unacceptably; (4) increase bounded wait for queued work.

**Recovery:** close circuits after probes; retry failed jobs in bulk (authorized admin action) — idempotency prevents duplicates.

**Validation:** gateway success > 99.5%; schema validity normal; backlog cleared.

**Escalation:** provider support; Product for customer comms.

**Post-incident:** review fallback coverage per profile; add a second route where missing.
