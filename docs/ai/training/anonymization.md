# Anonymization

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy + AI Engineer |
| Blueprint | BP §11.3 ("anonymization where possible"), §15 |

## Techniques by Data Type

| Data | Technique |
|---|---|
| Text (transcripts, OCR, artifacts) | NER-based PII detection (Persian + English) for names, phone numbers, national IDs, emails, addresses, IBAN/cards; replace with consistent pseudonyms per example; organization/project names replaced |
| Audio | Not retained for training by default; if needed, voice conversion or use of transcripts only |
| Images | Face and licence-plate blurring; document header/logo masking; EXIF stripped |
| Metadata | Remove tenant IDs, user IDs, GPS, timestamps shifted |

## Verification

Automated detection + human verification on a sample (100% for Restricted-adjacent content). Residual-risk assessment recorded per dataset version. Where anonymisation would destroy task value (for example speaker attribution), the example is rejected rather than kept identifiable.

## Note

Pseudonymised data may still be personal data under some jurisdictions; consent and purpose limitation still apply.
