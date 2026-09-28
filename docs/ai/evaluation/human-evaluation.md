# Human Evaluation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Product |
| Blueprint | BP §11.4 (Artifact, Business metrics), §16 |

## When Humans Evaluate

| Use | Method |
|---|---|
| Dataset annotation/adjudication | Two annotators + adjudicator |
| Artifact quality (fluency, formality, usefulness) | Blind pairwise comparison (A/B of versions) on a rubric (1–5): correctness, completeness, formality/tone, structure, language quality (Persian register) |
| LLM-judge validation | Humans label a sample to measure judge agreement; judges not trusted above measured agreement |
| Production sampling (consent-permitting) | Monthly sample per module reviewed by domain experts; corrections categorised by failure taxonomy |
| Pilot feedback | In-product thumbs/feedback on artifacts (G13) |

## Failure Taxonomy (shared)

`missed_item`, `spurious_item`, `wrong_attribute` (owner, deadline, severity…), `wrong_type` (decision vs proposal), `wrong_attribution` (speaker), `bad_evidence`, `unsupported_claim`, `language_quality`, `formatting`, `translation_error`, `date_calendar_error`, `ocr_misread`, `asr_misrecognition`.

## Rules

Human evaluators access only consented data; production samples are reviewed inside the platform with access logging; rubric versions are stored with results.
