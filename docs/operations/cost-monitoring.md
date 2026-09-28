# Cost Monitoring

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Founder |
| Blueprint | BP §5.4, §22.1, §27 (AI cost risk: "Metering, routing, dedup, quotas, margin dashboard") |

## Dashboards

| Dashboard | Content |
|---|---|
| AI cost | Cost by provider/model/profile/module per hour/day; cost per job distribution; fallback share |
| Margin | Revenue vs cost by plan, module, org (daily) |
| Infrastructure | Cloud spend by service (compute, storage, egress, DB) |
| Storage | Growth by category; retention effectiveness |
| Anomalies | Orgs above baseline; runaway jobs |

## Controls

Budgets and alerts on cloud accounts; AI provider spending limits at provider level (defence in depth); weekly cost review; monthly unit-economics report ([unit-economics](../billing/unit-economics.md)).
