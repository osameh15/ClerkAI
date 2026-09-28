# BDR-001: Platform Subscription + Included Usage + Modules/Add-ons + Overage

| Field | Value |
|---|---|
| Status | Accepted (prices Provisional) |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Founder |
| Blueprint | BP §5, §5.1, §5.2, §28.3 |

## Decision

Revenue model: **Platform Subscription + Seats + Included Usage + Module Entitlements/Add-ons + Overage.** Plans (Personal, Team, Business, Enterprise) are commercial packaging of entitlements, quotas and prices captured in immutable PlanVersions. BP §5.1 prices are **Validation Pricing / Provisional**, to be revised after 4–6 weeks of real Cost-per-Job data and customer interviews.

## Consequences

- Subscription, seats, add-ons and overage are separate billing concepts ([billing-architecture](../../billing/billing-architecture.md)).
- Plan names never appear in domain code; features map to Entitlement keys.

## Related

[pricing-model](../../billing/pricing-model.md), [plan-model](../../billing/plan-model.md)
