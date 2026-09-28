# Model Registry

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §11.2 (ai_providers, ai_models) |

## AI Provider Registry (`ai_providers`)

`key`, `name`, `status` (`active`, `degraded`, `disabled`), `regions`, `credential_ref` (secret manager path), `vendor_registry_id` (data processing terms, retention, training policy), `supports` (structured output, media URL fetch, batch).

## AI Model Registry (`ai_models`)

| Field | Purpose |
|---|---|
| `provider_id`, `key` | Provider's model identifier (for example version-pinned name) |
| `capabilities[]` | transcription, diarization, structured_extraction, vision_analysis, ocr, handwriting_recognition, generation, summarization, classification, embedding, translation |
| `modalities`, `languages` | Supported inputs; languages with evaluated quality tiers |
| `context_window`, `limits` | Tokens, media duration, file size, rate limits |
| `pricing` | Unit prices (per 1k input/output tokens, per audio minute, per image/page) + currency + effective date |
| `status` | `active`, `deprecated` (no new routes), `disabled` |
| `version` | Provider version string; models are pinned (no floating "latest" aliases) |

## Rules

- Registry changes are platform-admin operations, audited, and follow [model-change-management](../governance-ai/model-change-management.md).
- Pricing history is kept (effective-dated) so historical AIExecution costs are reproducible.
- Adding a model does not route traffic until a profile route references it and evaluation passes.
