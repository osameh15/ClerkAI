# Module Pricing

| Field | Value |
|---|---|
| Status | Accepted (mechanism) / Provisional (prices) |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Founder + Commerce |
| Blueprint | BP §5.2 (Module Packs, standalone premium modules), §5.3, App. C (Construction Pack add-on) |

## Mechanisms

| Mechanism | Representation |
|---|---|
| Included in plan | `plan_entitlements: module.<key> = full/limited` |
| Pack add-on | Add-on (`pack.construction`) grants several module entitlements |
| Standalone premium module | Add-on per module |
| Usage-weighted modules | Rate card per meter (modules differ in consumption, not price per call) |
| Marketplace-priced modules (future) | Listing pricing metadata + revenue share ([marketplace-economics](marketplace-economics.md)) |

Listings show entitlement status and pricing hints; Store never hides a module because of pricing (upgrade CTA instead).
