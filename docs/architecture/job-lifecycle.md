# Job Lifecycle (Architecture View)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Jobs domain |
| Blueprint | BP §8.1 |

## Purpose

Shows how the Job lifecycle spans API, PostgreSQL, Temporal, workers and SSE. The **canonical state machine** (states, transitions, guards) is in [jobs/job-state-machine.md](../jobs/job-state-machine.md); this document does not redefine it.

## Job Processing Lifecycle

```mermaid
sequenceDiagram
  autonumber
  participant UI as PWA
  participant API as clerk-api
  participant DB as PostgreSQL
  participant T as Temporal
  participant G as worker:general
  participant M as worker:media/document
  participant AI as worker:ai
  UI->>API: POST /v1/jobs {module_key} (draft)
  UI->>API: POST /v1/jobs/{id}/sources (attach)
  Note over API,DB: job becomes ready when input requirements satisfied
  UI->>API: POST /v1/jobs/{id}/submit (Idempotency-Key)
  API->>DB: tx: job ready→queued, pin module/workflow/config versions,<br/>insert processing_run(#n), outbox job.submitted, audit
  API->>T: StartWorkflow(JobProcessing, id=job/{job}/run/{n})
  API-->>UI: 202 Job(queued)
  T->>G: JobProcessingWorkflow starts
  G->>DB: job queued→processing, step records, outbox job.processing.started
  G->>M: await/trigger source extractions (child workflows per source, parallel)
  M-->>G: extractions ready (or reused from cache)
  G->>AI: capability requests per pipeline step
  AI-->>G: validated outputs + AIExecution ids
  G->>DB: knowledge + evidence + review items, artifact version v1 (ai)
  alt review required by policy
    G->>DB: job processing→needs_review, outbox job.review.required
    UI->>API: resolve review items
    API->>T: Signal ReviewCompleted
    G->>DB: regenerate affected artifact sections (new version)
  end
  G->>DB: job →completed, outbox job.processing.completed
  DB-->>UI: SSE via dispatcher → Redis pub/sub → API stream
```

## Key Properties

- **Draft is cheap:** attaching Sources does not start Job processing (BP §8.1). Source-level preprocessing may start at upload because it is reusable across Jobs.
- **Pinning:** module version, workflow version and configuration version are pinned at submit ([INV-06](../governance/architecture-principles.md#architectural-invariants-testable)).
- **Runs:** every submit/retry/reprocess creates a new ProcessingRun ([processing-runs](../jobs/processing-runs.md)).
- **Durability:** worker crash or timeout never loses a Job; Temporal resumes (BP App. D).
- **Parallelism:** Sources are processed in parallel child workflows; fusion waits for all required ones.
- **Human wait:** `needs_review` is a durable wait (Temporal signal/timer); reminders via timers.

## Related Documents

[job-state-machine](../jobs/job-state-machine.md), [temporal-workflows](../jobs/temporal-workflows.md), [processing-steps](../jobs/processing-steps.md), [api/sse](../api/sse.md)
