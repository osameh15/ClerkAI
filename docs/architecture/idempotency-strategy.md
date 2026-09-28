# Idempotency Strategy

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8.3 |

## Purpose

System-wide idempotency rules across HTTP commands, Temporal, events, AI calls, usage and billing. HTTP header contract: [api/idempotency](../api/idempotency.md). Decision: [ADR-012](../governance/adr/ADR-012-command-idempotency.md).

## Layers

| Layer | Mechanism | Key |
|---|---|---|
| HTTP commands | `Idempotency-Key` header + `idempotency_keys` table | `(organization_id, principal_id, method, route_template, key)` |
| Natural idempotency | State-machine transitions that are no-ops when already in target state (for example cancel on `cancelled`) | Resource state |
| Temporal workflow start | Deterministic workflow IDs + `WorkflowIDReusePolicy` = reject duplicate while running | `job/{job_id}/run/{run_no}` etc. |
| Temporal activities | Activities upsert by deterministic keys | `(processing_run_id, step_key, attempt_scope)` |
| Event consumers | Inbox dedup | `(consumer, event_id)` |
| AI executions | One row per attempt; the *logical* request has `request_key` for dedup of retried activities | `(processing_run_id, step_key, attempt_no)` |
| Usage events | Unique `idempotency_key` per meter emission | `(source_type, source_id, meter)` e.g. `ai_execution:{id}:llm.input` |
| Payment webhooks | Provider event ID uniqueness | `(provider, provider_event_id)` |
| Outbound webhooks | `event_id` in payload; receiver dedups | `event_id` |
| Numbering sequences | Number allocated once per artifact version finalisation, stored on the version | `(sequence_id, artifact_id)` |

## Commands Requiring `Idempotency-Key`

`POST /v1/uploads`, `POST /v1/uploads/{id}/complete`, `POST /v1/jobs`, `POST /v1/jobs/{id}/submit`, `/cancel`, `/retry`, `POST /v1/modules/{key}/install`, `/enable`, `/disable`, `/upgrade`, `/uninstall`, `POST /v1/artifacts/{id}/exports`, `POST /v1/artifacts/{id}/submit-for-approval`, `POST /v1/approvals/{id}/decision`, `POST /v1/review-items/{id}/resolve`, `POST /v1/api-keys`, `POST /v1/webhooks`, subscription changes. Other POSTs accept it optionally.

## Storage

```text
idempotency_keys(
  id BIGINT identity,
  organization_id UUID NULL,          -- null for pre-tenant calls (sign-in)
  principal_id UUID NOT NULL,
  method TEXT, route TEXT, key TEXT,
  request_hash BYTEA,                 -- SHA-256 of canonical body
  status TEXT,                        -- in_progress | completed
  response_status INT, response_body JSONB, resource_ref TEXT,
  created_at, expires_at              -- default 24h
  UNIQUE (organization_id, principal_id, method, route, key)
)
```

Algorithm: insert `in_progress` row (unique) → execute in the same DB transaction when possible → store response → commit. Concurrent duplicate while `in_progress` → `409 IDEMPOTENCY_REQUEST_IN_PROGRESS` with `Retry-After`. Same key, different hash → `422 IDEMPOTENCY_KEY_REUSED`.

## Invariants

- Retrying a submitted command never creates a second Job, ProcessingRun, Export, or charge.
- Every Temporal activity can execute ≥ 2 times without duplicate side effects.
- Every consumer can receive an event ≥ 2 times without duplicate side effects.

## Related Documents

[failure-model](failure-model.md), [retries](../jobs/retries.md), [event-architecture](event-architecture.md)
