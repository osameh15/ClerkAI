# Plan Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce domain |
| Blueprint | BP §5, §7.3 (plans, plan_versions) |

## Plan and PlanVersion

- `plans`: stable identity (`personal`, `team`, `business`, `enterprise`, private enterprise plans).
- `plan_versions`: immutable once `active`:

```json
{
  "plan": "team",
  "version": 3,
  "currency": "USD",
  "prices": { "monthly": 4900, "annual_monthly_equivalent": 3900 },
  "seat_price": { "included": 5, "extra_monthly": 900 },
  "included_usage": { "ai_credits": 2000, "processing_minutes": 600, "pages": 1500, "storage_gb": 50 },
  "rate_card": {
    "ai_credits": { "llm.input.tokens": 0.0008, "llm.output.tokens": 0.003, "vision.images": 0.5, "ocr.pages": 0.2, "audio.transcription.seconds": 0.02 },
    "processing_minutes": { "audio.transcription.seconds": 0.0166667, "video.processing.seconds": 0.0166667 }
  },
  "overage": { "allowed": true, "ai_credits_per_1000": 1500 },
  "entitlements": "see plan_entitlements"
}
```

(All numbers illustrative; [OD-021](../governance/open-decisions.md#od-021).) Money in minor units; rates as decimal factors.

## Rules

- Subscriptions reference a PlanVersion; price changes create new versions; existing subscriptions migrate at renewal or by explicit migration.
- Multi-currency via separate PlanVersions per currency/market.
- Plan names never appear in domain code.
