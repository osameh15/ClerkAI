# Organization Entitlement Overrides

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce domain |
| Blueprint | BP §5.1 (Enterprise custom), [BDR-005](../../governance/bdr/BDR-005-enterprise-services-revenue.md) |

## Sources of Override

| `source` | Created by | Typical use |
|---|---|---|
| `addon` | Billing (subscription add-on) | Construction Pack |
| `contract` | Platform admin (sales ops) with contract reference | Enterprise terms, custom quotas, SLA |
| `trial` | Automated / sales | Time-boxed module or feature trial |
| `promotion` | Marketing campaign | Extra credits |
| `override` | Platform admin (support) | Temporary fix, goodwill credit, **suspension** (`value=false`) |

## Rules

- Each override has `starts_at`, optional `ends_at`, `reason`, `created_by`; changes are audited and emit `entitlement.changed`.
- Overrides are created only via the platform-admin path (never by tenant users).
- Expiry is evaluated at read time and by a daily sweep that emits events.
- Enterprise custom plans may alternatively be modelled as private PlanVersions (`plans.visibility=private`) when the whole bundle differs.
