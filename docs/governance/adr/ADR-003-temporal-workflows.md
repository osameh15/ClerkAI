# ADR-003: Durable Workflows = Temporal

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §0, §6, §8, §28.1, App. D ("Job reliability") |

## Context

Job processing spans minutes to hours: transcription, OCR, multiple AI calls with fallback, human review waits, approvals, export rendering. A worker crash or provider outage must never lose a Job (BP App. D).

## Decision

Use **Temporal** for durable, resumable, long-running orchestration of processing, review waits, approval chains and exports.

Boundaries:

1. **Temporal is not a system of record.** Business state (Job status, ProcessingRun/Steps, Knowledge, Artifacts, approvals) is written to PostgreSQL by activities. Temporal history is execution state.
2. **Workflows are deterministic orchestrators**; all I/O happens in activities.
3. **Activities are idempotent** and keyed by stable activity IDs ([idempotency-strategy](../../architecture/idempotency-strategy.md)).
4. **Tenant context is part of every workflow input** and re-established in each activity.
5. Workflow IDs are deterministic from business IDs (for example `job/{job_id}/run/{run_no}`), preventing duplicate starts.

## Consequences

- **Positive:** automatic retries with backoff, timers for deadlines/approval waits, cancellation, heartbeats for long activities, visibility.
- **Negative:** additional infrastructure (Temporal server + its persistence); determinism discipline and workflow versioning required. Mitigation: workflow replay tests in CI ([workflow-testing](../../testing/workflow-testing.md)).

## Alternatives Considered

- **Queue + cron + DB state machine (Redis/Asynq, River):** simpler infra, but re-implements durable timers, sagas and human waits.
- **Cloud-specific workflow services:** lock-in.

## Related

[temporal-workflows](../../jobs/temporal-workflows.md), [workflow-architecture](../../architecture/workflow-architecture.md), [ADR-007](ADR-007-event-architecture.md)
