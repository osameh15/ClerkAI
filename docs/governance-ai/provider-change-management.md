# Provider Change Management

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Privacy + Security |
| Blueprint | BP §15.3 ("Provider data-processing terms are registered in a Vendor Registry"), §16.1, §27 (Vendor lock-in) |

## Onboarding a Provider

1. Vendor registry entry: data processing terms, retention, training use, sub-processors, regions, security attestations, availability in target markets (sanctions/connectivity), pricing.
2. Privacy review: classification of data allowed to be sent; zero-retention/no-train options; contract (DPA) status.
3. Security review: credential handling, network egress, incident notification terms.
4. Adapter implementation + contract tests.
5. Evaluation against affected suites; routes added behind flag.
6. Tenant policy impact: organizations restricting providers are unaffected (route filtered).

## Offboarding / Deprecation

- Provider/model deprecation notices tracked; replacement evaluated before provider end-of-life.
- Routes removed via model change procedure; stored payloads at the provider (if any) deletion requested per terms.

## Tenant Communication

Organizations with provider restrictions or Enterprise contracts are notified of sub-processor changes per contract terms before activation.
