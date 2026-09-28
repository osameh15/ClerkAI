# Overage

| Field | Value |
|---|---|
| Status | Accepted (mechanism) / Provisional (prices) |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce |
| Blueprint | BP §5 (Overage), §5.2 (AI/Processing overage) |

## Mechanism

- If `overage.allowed = true`, usage beyond included quotas continues and is billed per PlanVersion overage prices at period end (invoice line items per customer unit).
- Organization budgets can cap overage (hard stop).
- Personal plan default: no overage (hard limit) — provisional.
- Overage shown in Billing & Usage screen with projected end-of-period cost.
- Overage calculations are reproducible from usage aggregates and PlanVersion; invoices store a snapshot of lines.
