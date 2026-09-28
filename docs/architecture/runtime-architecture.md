# Runtime Architecture

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §6, §8 |

## Purpose

How processes run: process types, Temporal task queues, concurrency limits, startup/shutdown, health, and configuration at runtime.

## Process Types

| Process | Binary | Mode flag | Notes |
|---|---|---|---|
| API | `apps/api` | — | Stateless HTTP; SSE connections held here |
| Worker | `apps/worker` | `--queues=general,media,document,ai,export,webhook` | One binary, queue subset per deployment |
| Dispatcher | `apps/worker` | `--dispatcher` | Outbox relay + consumers; leader election via PostgreSQL advisory lock |
| Migrator | `goose` v3 run as a migrator job ([ADR-025](../governance/adr/ADR-025-migrations-goose.md)) | — | Runs before deploy; separate DB role |

## Temporal Task Queues

| Queue | Activities | Resource profile | Default concurrency per worker (provisional) |
|---|---|---|---|
| `general` | Job orchestration workflows, knowledge fusion, artifact generation, approvals, notifications | CPU-light | workflows 200, activities 50 |
| `media` | ffmpeg normalise, audio extraction, frame sampling | CPU/IO heavy, temp disk | 4 |
| `document` | PDF/Office parsing, image preprocessing, scanning calls | CPU, sandboxed parsers | 8 |
| `ai` | AI Gateway calls (LLM, ASR, vision, OCR, embeddings) | Network-bound, rate-limited by provider | 32, plus per-provider semaphores |
| `export` | PDF/DOCX rendering | CPU, sandboxed, no egress | 4 |
| `webhook` | Outbound webhook delivery | Network-bound, SSRF-guarded egress | 16 |

Concurrency values are initial and must be tuned from load tests ([load-testing](../testing/load-testing.md)).

## Lifecycle

- **Startup:** load configuration → validate → connect PostgreSQL (fail fast), Redis (degraded allowed), object storage (health check), Temporal → register workflows/activities → readiness true.
- **Graceful shutdown:** stop accepting (readiness false) → drain HTTP (30 s) / close SSE with `retry:` hint → Temporal worker `Stop()` lets running activities finish or heartbeat-timeout → flush telemetry.
- **Health endpoints:** `/health/live` (process up), `/health/ready` (dependencies reachable per mode). Not exposed publicly.

## Configuration

Twelve-factor environment configuration, typed and validated at startup ([configuration-management](../operations/configuration-management.md)). No secrets in images.

## Resource Limits and Back-Pressure

- API request body limit 1 MB by default (uploads bypass API).
- Per-organization concurrent job limit (entitlement/quota), enforced at submit and at workflow start.
- AI queue uses per-provider token-bucket limiter in Redis (fallback: local limiter if Redis unavailable).
- Media/document activities run with `ulimit`/cgroup limits and temp-dir quotas.

## Related Documents

[container-architecture](container-architecture.md), [worker-model](../jobs/worker-model.md), [concurrency-strategy](concurrency-strategy.md), [deployment-architecture](deployment-architecture.md)
