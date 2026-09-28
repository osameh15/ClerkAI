# Publishers

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §23 |

## Model

`publishers(key, name, type ∈ {official, partner, community}, verification_status, status ∈ {active, suspended}, contact)`. v1 seed: `clerk-official` (type official, verified).

## Future Publisher Lifecycle (M1+)

Application → identity/business verification → agreement (terms, revenue share, security obligations) → signing key registration → publishing rights for a namespace (category + name uniqueness) → periodic re-verification. Suspension disables new installs and may suspend installations (security).

## Namespace Ownership

Module keys are globally unique; publishers own module keys, not categories; official namespace names may be reserved.
