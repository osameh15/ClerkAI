# AI Execution Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI domain |
| Blueprint | BP §8.3, §11.2 ("Every AIExecution records model, prompt version, cost, latency, token/audio/image usage, and outcome"), §17 (`ai_execution_id`) |

## Definition (canonical)

An **AIExecution** is the immutable record of **one attempt** of one model call. Retries, repairs and fallbacks are separate AIExecutions linked by `fallback_of_execution_id` / `attempt_no`.

## Fields (`ai_executions`)

| Group | Fields |
|---|---|
| Identity | `id` (UUIDv7), `organization_id`, `request_key` (logical call id for dedup) |
| Attribution | `job_id`, `processing_run_id`, `step_id`, `source_id` (ingestion), `module_key`, `module_version`, `task_key` |
| Routing | `capability`, `profile_key`, `profile_version`, `provider_key`, `model_key`, `model_version`, `routing_decision` (JSONB) |
| Prompt/schema | `prompt_version_id`, `output_schema_ref` |
| Attempt | `attempt_no`, `attempt_kind ∈ {primary, retry, repair, fallback}`, `fallback_of_execution_id` |
| Outcome | `status ∈ {succeeded, failed, schema_invalid, timeout, cancelled, blocked_by_policy}`, `error_class`, `error_code`, `finish_reason` |
| Usage | `input_tokens`, `output_tokens`, `audio_seconds`, `images`, `pages`, `frames` |
| Cost | `cost_micros`, `currency`, `pricing_effective_date` |
| Performance | `latency_ms`, `queue_ms`, `provider_request_id` |
| Payload refs | `request_payload_ref`, `response_payload_ref`, `payload_expires_at` (only if policy allows storage) |
| Citations | `chunk_ids_sent` (IDs only) |
| Timestamps | `created_at`, `completed_at` |

## Invariants

- One row per attempt (INV-05). Usage events are emitted per AIExecution (idempotency key `ai_execution:{id}:{meter}`).
- Rows are append-only except for the completion update of the same attempt.
- No prompt/response content in the row; payloads, when stored, are encrypted objects with ≤ 30-day retention (Enterprise may disable) (BP §15.2).

## Queries Supported

Cost per job/run/module/org; failure rates per provider/model; latency distributions; prompt version usage; provenance for artifacts.
