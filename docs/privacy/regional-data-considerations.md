# Regional Data Considerations

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Founder + Privacy |
| Blueprint | BP §2 (Iran/regional markets), §5.1 (local pricing), §15 ("Architecture must be adaptable to jurisdiction-specific requirements"), R10, R11 |

## Confirmed

- `organizations.data_region` exists from v1 (single region initially).
- Tenant AI policy can restrict providers/regions.
- Vendor registry records provider regions and terms.

## Considerations (to be resolved with legal advice)

| Market | Consideration |
|---|---|
| Iran | Local data-hosting expectations for some sectors; availability/connectivity of foreign AI providers and payment systems; sanctions compliance for vendors; Persian-language legal texts |
| EU (if served) | GDPR rights and principles, DPA/SCCs for transfers, EU AI Act transparency obligations timeline (R11) |
| Other regional markets | Local data protection laws; cross-border transfer rules |

## TBD

Hosting region(s) [OD-005](../governance/open-decisions.md#od-005), data residency model [OD-028](../governance/open-decisions.md#od-028), billing providers [OD-010](../governance/open-decisions.md#od-010), legal texts for consent/recording notices [OD-032](../governance/open-decisions.md#od-032).
