# Server-Sent Events (Job Progress)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Jobs domain + Frontend |
| Blueprint | BP §6, §8 ("Job Progress: SSE"), App. B `/v1/jobs/{id}/events` |

## Endpoint

`GET /v1/jobs/{id}/events` → `Content-Type: text/event-stream`. Decision: [ADR-021](../governance/adr/ADR-021-sse-progress.md).

## Frame Format

```text
id: 42
event: run.stage
data: {"type":"run.stage","job_id":"…","seq":42,"stage":"extracting_knowledge","status":"running","at":"2026-09-28T10:00:00Z"}

id: 43
event: heartbeat
data: {"type":"heartbeat","job_id":"…","seq":43,"at":"…"}
```

- `data` conforms to `JobStreamEvent` in [openapi.yaml](openapi/openapi.yaml).
- `id` is the monotonically increasing per-job `seq`.
- Event types: `job.status`, `run.stage`, `run.progress`, `review.required`, `artifact.generated`, `export.completed`, `heartbeat` (every 20 s), `error`.
- Stages are **user-facing stage keys** declared by the module workflow (for example `transcribing`, `extracting_knowledge`, `generating_artifact`), not internal step names (BP §8 "frontend must not know internal processing stages").

## Delivery Path

Processing step changes are persisted (PostgreSQL) → outbox → dispatcher publishes to Redis channel `t:{org}:job:{job_id}` → API instances holding SSE connections forward frames.

## Reconnect and Replay

- Client reconnects with `Last-Event-ID`. The API reconstructs missed frames from persisted `processing_steps`/job status newer than `seq` (bounded to last 500), then continues live.
- If Redis pub/sub is unavailable, the API emits `event: error` with `{"code":"SYSTEM_UNAVAILABLE","fallback":"poll"}` and clients poll `GET /v1/jobs/{id}` every 5 s.
- Server closes streams when the Job reaches a terminal state (`completed`, `failed`, `cancelled`) after sending the final `job.status`.

## Limits and Security

- Authorization identical to `GET /v1/jobs/{id}`; re-validated on reconnect.
- Max concurrent SSE connections per user: 10 (provisional).
- Frames contain no customer content (stage keys, counts, IDs only).
- Proxies must disable buffering (`X-Accel-Buffering: no`).
