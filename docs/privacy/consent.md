# Consent

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (legal texts Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + Product |
| Blueprint | BP §11.3, §15.3 ("Recording consent/notice for meetings must be represented in organization UX/policy") |

## Consent and Notice Types

| Type | Who | Mechanism | Record |
|---|---|---|---|
| Terms/privacy acceptance | User | Sign-up | `consent_records` (policy version) |
| Organization data processing terms | Owner | Org creation / plan change | `consent_records` |
| AI evaluation use | Owner | AI Policy screen (default off) | `consent_records` + `data_usage_policies` |
| AI training use | Owner (+ user notices where required) | AI Policy screen (default off) | same |
| Meeting recording notice | Org policy + recorder attestation per job | Policy setting + job input `recording_notice_confirmed` + in-app reminder text | job input + audit |

## Rules

- Consent records are append-only with policy text version.
- Withdrawal is as easy as granting; effect documented per type ([consent-requirements](../ai/training/consent-requirements.md)).
- Recording notice text per org (template provided); legal wording per jurisdiction TBD ([OD-032](../governance/open-decisions.md#od-032)).
