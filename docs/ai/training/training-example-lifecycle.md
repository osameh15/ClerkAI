# Training Example Lifecycle

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Privacy |
| Blueprint | BP §11.3 ("Reviewed + Consent + Anonymization"), App. E (029 training) |

## States

```mermaid
stateDiagram-v2
  [*] --> candidate: generated from correction (policy allows)
  candidate --> eligible: eligibility checks pass<br/>(consent, data class, retention)
  candidate --> rejected: ineligible
  eligible --> in_review: assigned to curator
  in_review --> anonymized: anonymisation applied + verified
  in_review --> rejected: low quality / sensitive
  anonymized --> approved: curator + privacy approval
  approved --> included: added to dataset version (eval or training)
  candidate --> withdrawn: consent revoked / deletion request
  eligible --> withdrawn
  in_review --> withdrawn
  anonymized --> withdrawn
  approved --> withdrawn
  included --> withdrawn: removed in next dataset version
  rejected --> [*]
  withdrawn --> [*]
```

## Eligibility Checks

Consent type covers intended use; source data classification ≠ Restricted unless contract allows; source not under deletion request; retention permits; module and language tags present.

## Versioning

Datasets referencing examples are versioned (`dataset@version`); inclusion/withdrawal changes produce new versions; each version records its example IDs and consent basis snapshot.

## Access

Curators are platform staff with dedicated role; access through audited tooling; examples never exported outside controlled storage except to approved fine-tuning providers under no-reuse terms.
