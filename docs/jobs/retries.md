# Retries

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (values Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain |
| Blueprint | BP §8.3 ("Retries are activity-specific and use appropriate exponential backoff"; "Model fallback creates a separate AIExecution record for every attempt") |

## Automatic (Temporal activity) Retry Policies

| Activity class | Initial interval | Backoff | Max interval | Max attempts | Start-to-close | Heartbeat | Non-retryable errors |
|---|---|---|---|---|---|---|---|
| DB/state activities | 1 s | 2.0 | 30 s | 10 | 30 s | — | validation, authorization, not found |
| Object storage IO | 2 s | 2.0 | 1 min | 8 | 5 min | 30 s | object missing (after verification), checksum mismatch |
| Media (ffmpeg) | 5 s | 2.0 | 2 min | 3 | 60 min | 30 s | unsupported codec, corrupt file |
| Document parse | 5 s | 2.0 | 2 min | 3 | 15 min | 30 s | encrypted, malformed beyond repair |
| Malware scan | 30 s | 2.0 | 5 min | 12 | 10 min | — | — (never skip) |
| AI Gateway call | 2 s | 2.0 | 1 min | 3 per route | per profile timeout | 30 s (long ASR) | policy blocked, content rejected, schema invalid after repair |
| Rendering (export) | 5 s | 2.0 | 1 min | 3 | 5 min | — | template invalid |
| Webhook delivery | per [webhooks](../api/webhooks.md) | | | | 10 s | — | endpoint disabled |

## AI Retry vs Fallback

Within one gateway call: retry the same route for transient errors (429/5xx/timeouts) → then fallback route → each attempt recorded as a separate AIExecution with `attempt_no` and `fallback_of_execution_id` ([fallback-strategy](../ai/fallback-strategy.md)).

## Manual Retry (Job level)

`POST /v1/jobs/{id}/retry` on `failed` jobs whose failure class is retryable (`AI_PROVIDER_UNAVAILABLE`, `SYSTEM_*`, transient `SOURCE_PROCESSING_FAILED`). Non-retryable classes (for example `SOURCE_QUARANTINED`, `AI_POLICY_BLOCKED`) require changing inputs/policy first. Max 3 manual retries per job (provisional).
