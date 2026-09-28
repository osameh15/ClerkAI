# Consent Requirements (AI Data Use)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (legal texts Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + Legal |
| Blueprint | BP §11.3 ("appropriate consent/legal basis"), §11.5, §15, App. A (AI Policy: training consent) |

## Consent Types

| Type | Default | Granted by | Scope |
|---|---|---|---|
| `evaluation_use` | Off | Organization Owner (policy) | Use of the org's data to build evaluation cases (stays in Clerk-controlled storage) |
| `training_use` | Off | Organization Owner (policy) + optional per-user notice | Use in training/fine-tuning datasets |
| `recording_notice` | Required per org policy | Organization (policy) + meeting organiser (per job attestation) | Meeting recordings ([OD-032](../../governance/open-decisions.md#od-032)) |

## Rules

- Consent is recorded in `consent_records` (append-only) with the exact policy text version.
- Enterprise organizations can contractually forbid any AI data use beyond processing; the platform enforces `training_use=false` and hides opt-in UI.
- Consent does not override personal-data minimisation: anonymisation still applies ([anonymization](anonymization.md)).
- Personal workspace users grant for themselves.
- Withdrawal takes effect for new candidates immediately and for datasets at the next dataset version.

## TBD

Legal wording per jurisdiction (GDPR where applicable, Iranian regulations) — [regional-data-considerations](../../privacy/regional-data-considerations.md).
