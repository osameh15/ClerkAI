# Pricing Model

| Field | Value |
|---|---|
| Status | Accepted (structure) / **Validation Pricing / Provisional** (numbers) |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Founder |
| Blueprint | BP §5, §5.1, §5.2, §5.3 |

## Validation Pricing / Provisional (BP §5.1)

> The numbers below are product hypotheses and must be revised after 4–6 weeks of real Cost-per-Job data and customer interviews. In high-inflation or volatile-FX markets, local prices should be reviewed periodically.

| Plan | Initial price hypothesis | Scope |
|---|---|---|
| Personal | $9–12 / month | 1 user, general modules, limited usage, no organization-level approval |
| Team | $39–59 / month | Up to 5 users, four launch modules, templates, higher usage |
| Business | $149–249 / month | Up to 20 users, advanced roles, approval, audit, Module Packs |
| Enterprise | Custom | SSO/SAML, retention policy, SLA, private AI policy, data residency, annual contract |

Iran and regional markets: local-currency pricing by purchasing power and infrastructure cost, not USD conversion. Enterprise contracts may include setup/onboarding fees.

## Customer-Facing Units

AI Credits, Processing Minutes, Pages, Storage (never tokens; [BDR-002](../governance/bdr/BDR-002-cost-aware-pricing.md)).

## Revenue Streams (BP §5.2)

Base subscription; seats; AI/processing overage; Module Packs (Construction Pack); standalone premium modules; enterprise services; API/automation volume; marketplace revenue share (later); limited professional services.

## Future Construction Pack (BP §5.3)

| Module | Stage |
|---|---|
| Site Report | v1 |
| Daily Progress Report | After v1 |
| Inspection Report | After v1 |
| HSE Report | Expansion |
| NCR | Expansion |
| RFI | Expansion |
| Change Order | Expansion |

## Technical Consequences

All prices, included usage, quotas and rate cards live in PlanVersions and add-on definitions; changing them requires a new PlanVersion, not a deploy. See [plan-entitlements](../platform/entitlements/plan-entitlements.md) for plan-dimension consequences.
