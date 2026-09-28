# Business Principles

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline (numbers Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product / Founder |
| Blueprint | BP §2, §5, §22, §23, §28.3 |

## Purpose

Business rules that shape the architecture: how Clerk is sold, metered, and protected against variable AI cost.

## Principles

| ID | Principle | Technical consequence | Decision |
|---|---|---|---|
| BP-01 | **Organization-centric accounts.** B2B-first; Personal Workspace is a one-person Organization. | Billing, entitlements and data all hang off `organization_id`. | [PDR-008](pdr/PDR-008-personal-workspace-as-organization.md) |
| BP-02 | **Revenue model** = Platform Subscription + Seats + Included Usage + Module Entitlements/Add-ons + Overage. | Plan, seat, quota, add-on and overage are separate data concepts. | [BDR-001](bdr/BDR-001-platform-subscription-model.md) |
| BP-03 | **Tokens are never the customer-facing unit.** Customers see AI Credits, Processing Minutes, Pages, Storage. Backend records real provider usage and cost. | Usage Events record raw units + cost; a Pricing Engine converts to billable units. | [BDR-002](bdr/BDR-002-cost-aware-pricing.md) |
| BP-04 | **Entitlement ≠ Permission.** | Two independent evaluators; both required. | [BDR-003](bdr/BDR-003-module-entitlements.md) |
| BP-05 | **Plans map to entitlements, never to code.** | No plan names in domain code; code checks entitlement keys. | [plan-entitlements](../platform/entitlements/plan-entitlements.md) |
| BP-06 | **Every Job has attributable cost.** | Usage Events carry `job_id`, `module_key`, `artifact_id` where applicable. | [cost-attribution](../billing/cost-attribution.md) |
| BP-07 | **Gross margin guardrail.** Direct AI + storage + compute + delivery cost should preferably stay below 20–25% of self-service account revenue (initial target). | Margin dashboards, budget guardrails, routing cost ceilings. | [unit-economics](../billing/unit-economics.md) |
| BP-08 | **Validation pricing is provisional.** BP §5.1 prices are hypotheses to revise after 4–6 weeks of real Cost-per-Job data and interviews. | Prices live in versioned PlanVersions, never in code. | [pricing-model](../billing/pricing-model.md) |
| BP-09 | **Local pricing.** Iran and regional markets priced in local currency by purchasing power, not USD conversion; reviewed periodically under inflation/FX volatility. | Multi-currency PlanVersions; billing provider per region ([OD-010](open-decisions.md#od-010)). | [pricing-model](../billing/pricing-model.md) |
| BP-10 | **Marketplace exists in the model from day one**; third-party execution comes later. | Publisher/Listing/Version/pricing metadata tables in v1. | [BDR-004](bdr/BDR-004-marketplace-future-revenue.md) |
| BP-11 | **Enterprise revenue** may include setup, integration and SLA fees. | Contract-based entitlements and organization overrides. | [BDR-005](bdr/BDR-005-enterprise-services-revenue.md) |
| BP-12 | **Don't feature-complete Enterprise/Marketplace before demand.** Design contracts fully; implement on signal. | Architecture-ready vs v1-implemented is tracked per feature. | [v1-scope](../product/v1-scope.md) |

## Revenue Streams (BP §5.2)

Base platform subscription; additional seats; AI/processing overage; specialist Module Packs (for example Construction Pack); standalone premium modules; enterprise setup/migration/integration services; API and automation volume; marketplace revenue share (later); limited professional services for organization-specific templates and workflows.

## Related Documents

- [billing/billing-architecture.md](../billing/billing-architecture.md)
- [billing/unit-economics.md](../billing/unit-economics.md)
