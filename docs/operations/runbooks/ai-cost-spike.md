# Runbook: Unexpected AI Cost Spike

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Founder |

**Symptoms:** hourly AI cost > 3× baseline; budget alerts; provider spend alerts.

**Detection:** cost anomaly alert (global or org), provider billing alerts.

**Immediate actions:** identify scope (global vs one org vs one module/prompt/route); if global and unexplained, enable cost-safety flag (routes restricted to cost ceiling, non-essential AI features paused such as embeddings/on-demand generation).

**Diagnosis:** retry/repair loops (schema invalid → repeated attempts), fallback to expensive route, new prompt version with larger context, runaway job (huge source), abuse (scripted API usage), pricing change at provider, missing dedup/cache.

**Mitigation:** roll back prompt/route; lower attempt caps; suspend abusive API keys/org (audited); enforce hard budget for the org; cancel runaway jobs.

**Recovery:** reconcile usage events; customer credits for platform-caused overcharges ([billing-failure-cases](../../billing/billing-failure-cases.md)).

**Validation:** cost back to baseline; margins dashboard normal.

**Escalation:** Founder for customer-impacting suspensions; Security if abuse.

**Post-incident:** add cost regression test to gates; tune anomaly thresholds.
