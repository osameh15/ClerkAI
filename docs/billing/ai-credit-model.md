# AI Credit Model

| Field | Value |
|---|---|
| Status | Accepted (concept) / Provisional (rates) |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Founder + Commerce |
| Blueprint | BP §5 ("Clerk should expose AI Credits or Processing Minutes while the backend records real token, audio, vision, storage, and compute usage"), [BDR-002](../governance/bdr/BDR-002-cost-aware-pricing.md) |

## Concept

An **AI Credit** is a stable customer-facing unit of AI work, decoupled from provider tokens and prices. Conversion: `credits = Σ(raw_quantity × rate_card[meter])` using the subscription's PlanVersion rate card at the time of consumption.

## Design Goals

- Predictable for customers (a typical 60-minute Persian meeting ≈ N credits, shown in the Store as guidance).
- Stable when providers change prices (Clerk absorbs or reprices via new PlanVersion, not per-call volatility).
- Margin-protecting: rate cards set so that expected provider cost per credit ≤ target share of credit revenue ([unit-economics](unit-economics.md)).

## Estimates Before Submit

Job submit shows an estimate (credits and minutes) from source durations/pages and module historical averages; quota reservation uses the estimate; final charge uses actual usage.

## TBD

Rates and included amounts per plan: [OD-021](../governance/open-decisions.md#od-021).
