# Temporal Workflows

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain / Tech Lead |
| Blueprint | BP §0, §6, §8.3, [ADR-003](../governance/adr/ADR-003-temporal-workflows.md), G7 |

## Separation of Concerns

| Concern | Where |
|---|---|
| Business history (job/run/step status, knowledge, artifacts, approvals, usage, audit) | PostgreSQL (written by activities) |
| Execution state (what's next, timers, retries, waiting for signals) | Temporal |
| Notification of facts | Event Bus (outbox) |

**Temporal must never be the sole system of record.** Every decision visible to users is persisted in PostgreSQL by an activity before/after the corresponding workflow step.

## Namespaces and Task Queues

One namespace per environment (`clerk-{env}`). Task queues per [runtime-architecture](../architecture/runtime-architecture.md). Search attributes: `OrganizationId`, `JobId`, `ModuleKey`, `RunNo` (for operations, not for tenant queries).

## Workflow IDs

| Workflow | ID | Reuse policy |
|---|---|---|
| JobProcessingWorkflow | `job/{job_id}/run/{run_no}` | Reject duplicate |
| SourceIngestionWorkflow | `source/{source_id}/ingest/{pipeline_set_version}` | Allow duplicate failed only |
| SourceExtractionWorkflow (child) | `source/{source_id}/extract/{kind}@{version}/{options_hash}` | Reject duplicate while running; reuse result |
| ArtifactExportWorkflow | `export/{export_id}` | Reject duplicate |
| ApprovalWorkflow | `approval/{workflow_instance_id}` | Reject duplicate |
| ModuleInstallWorkflow | `module-op/{operation_id}` | Reject duplicate |
| RetentionSweepWorkflow | `retention/{org_id}/{yyyy-mm-dd}` | Reject duplicate |

## Activity IDs and Idempotency

Activity IDs default to Temporal's; idempotency is enforced by **business keys** inside activities: `(run_id, step_key, attempt_scope)` for step records; `(run_id, step_key, segment)` for AI calls; artifact version creation keyed by `(run_id, artifact_type, generation_seq)`.

## Timeouts and Heartbeats

Per activity class in [retries](retries.md). Workflow execution timeout: none for JobProcessing (human waits); run timeout per module default (for example 24 h processing excluding review waits enforced via timers).

## Signals and Queries

| Workflow | Signals | Queries |
|---|---|---|
| JobProcessing | `ReviewProgress`, `Cancel` (via cancellation API), `SourceReady` | `Progress` (for diagnostics; UI uses PostgreSQL) |
| Approval | `DecisionRecorded`, `Withdrawn`, `SubjectSuperseded` | `State` |
| SourceIngestion | — | `Progress` |

## Human Approval Waits

Approval and review waits use `workflow.Await` on signals with timers for due dates/escalations/reminders. Decisions are written to PostgreSQL by the API first, then signalled ([workflow-architecture](../architecture/workflow-architecture.md)).

## Compensation

Saga-style compensation on cancellation/failure: release quota reservations, delete partial export objects, mark partial artifacts, abort multipart uploads created by the workflow.

## Provider Outage Handling

AI activities classify errors; when all routes for a profile are unavailable, the workflow enters a bounded wait (default 30 min, exponential checks) before failing the step with `AI_PROVIDER_UNAVAILABLE` ([ai-provider-outage runbook](../operations/runbooks/ai-provider-outage.md)).

## Parallel Source Processing

`JobProcessingWorkflow` launches child `SourceExtractionWorkflow`s (bounded fan-out) and awaits all required kinds; optional kinds have a soft deadline after which processing continues with warnings.

## Workflow Versioning

- **Interpreter code changes:** `workflow.GetVersion` markers; replay tests with recorded histories in CI ([workflow-testing](../testing/workflow-testing.md)).
- **Module workflow definitions:** loaded by an activity at run start and stored in workflow state; running workflows are immune to module upgrades.
- **Breaking interpreter changes:** new workflow type name (`JobProcessingWorkflowV2`) for new runs; old runs drain.

## Tenant Propagation

Workflow input includes `tenant {organization_id, acting_principal}`; activities rebuild `TenantContext` and set `app.org_id` per transaction; context propagators carry `correlation_id` and trace context.
