# BDR-002: Pricing Is Cost-Aware; Tokens Are Not the UX Billing Unit

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Founder |
| Blueprint | BP §5, §5.4, §22, §28.3 |

## Decision

- Every consumption first produces an exact **UsageEvent** with raw internal units (seconds, pages, images, tokens, byte-hours, counts) and attributed provider cost.
- A **Pricing Engine** converts raw usage into customer-facing units (AI Credits, Processing Minutes, Pages, Storage) using rate cards in the PlanVersion. Pricing changes never require worker changes.
- Tokens are never presented as the billing unit in the product.
- Cost guardrails: soft/hard quotas, organization AI budgets, routing cost ceilings, dedup/reuse, deterministic-extraction caching, per-plan upload limits, anomaly detection (BP §22.1).

## Related

[usage-metering](../../billing/usage-metering.md), [ai-credit-model](../../billing/ai-credit-model.md), [quotas](../../billing/quotas.md)
