# Logging

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Security |
| Blueprint | BP §14.1 (Sensitive data disclosure), §17 |

## Format

Structured JSON (Go `slog`), OTel log bridge. Standard fields: `ts`, `level`, `msg`, `service`, `version`, `env`, `request_id`, `correlation_id`, `trace_id`, `span_id`, `organization_id` (UUID), `principal_type`, `job_id`, `workflow_id`, `ai_execution_id`, `error_code`.

## Never Log

Customer content (transcripts, OCR text, artifact content, file names may contain PII → log file IDs instead), prompts/responses, secrets, tokens, cookies, API keys, Authorization headers, presigned URLs (contain signatures), email addresses (use user ID), IP addresses in full (prefix only in security logs), provider raw error bodies (may echo content).

## Enforcement

- Logging helpers accept typed fields only; a `Redacted` wrapper type prints `[redacted]`.
- HTTP middleware logs method, route template, status, duration — not query strings with values.
- CI test scans log output of integration tests for known sensitive fixture strings.

## Levels and Retention

`debug` disabled in production (per-request sampling via flag for investigations); retention 30 days hot, 90 days cold (provisional). Logs are not audit records.
