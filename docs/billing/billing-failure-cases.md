# Billing Failure Cases

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (policies Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce + Product |
| Blueprint | BP §5, §22 |

| Case | Policy |
|---|---|
| Job fails due to platform/provider error | Customer not charged credits for the failed run (usage recorded with `billable=false`); cost absorbed and tracked |
| Job fails due to invalid input (quarantined, unsupported) | No AI charge beyond work actually done before detection (usually none) |
| Retry after platform failure | Charged once (successful run) |
| Retry after user-caused failure | Normal charging |
| Job cancelled by user mid-way | Charged for completed work |
| Duplicate submission | Idempotency prevents double charge |
| Payment webhook arrives twice | Dedup by provider event ID |
| Payment failure | `past_due` → grace → blocked new jobs; data retained |
| Plan downgrade below current usage (seats, storage) | Downgrade scheduled; admins prompted to reduce; no data deletion |
| Usage recorded late (worker crash) | Idempotent emission ensures eventual recording; invoices close with a lag window (for example 48 h) |
| Pricing error discovered | Compensating usage events / credits; audit |
| FX volatility (local currency) | New PlanVersions per currency; existing periods unchanged |
