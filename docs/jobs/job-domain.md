# Job Domain

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Jobs domain |
| Blueprint | BP §7 (Jobs), §8.1, §30.1 ("Job: One Clerk execution using a Module and a set of Sources") |

## Purpose

Job is a first-class domain: the user-visible unit of intent ("produce minutes for this meeting") and outcome (artifacts, knowledge, actions).

## Aggregate

| Element | Notes |
|---|---|
| Job | module key, installation, pinned module/workflow/config versions, title, project, input (schema-validated JSON), status, failure, timestamps |
| JobSource | Source attachment with `slot` (manifest input slot) and `position` |
| Relationships (by ID) | ProcessingRuns (processing context), Knowledge (knowledge context), Artifacts (artifact context), Action Items |

## Capabilities

| Capability | Support |
|---|---|
| Multiple Sources | Yes, across slots, within manifest limits |
| Multiple Artifacts | Yes: automatic artifact types at the first run; on-demand types via additional runs (Handwriting: one Source → several artifacts) |
| Multiple processing runs | Yes (submit, retry, additional_artifact, reprocess) |
| Retry | Failed jobs, new run, resume where safe |
| Resume | Temporal durability + step-level idempotency |
| Cancel | Any non-terminal state |
| Review | `needs_review` human wait |
| Reprocessing | Policy-controlled ([OD-019](../governance/open-decisions.md#od-019)) |
| Module version pinning | At submit (`module_version_id`) |
| Workflow version pinning | At submit (`workflow_version_id`, definition hash on the run) |

## Use Cases (app layer)

`CreateDraft`, `UpdateDraft`, `AttachSource`, `DetachSource`, `EvaluateReadiness`, `Submit`, `Cancel`, `Retry`, `RequestAdditionalArtifact`, `Reprocess` (policy), `Get`, `List`.

## Slot Assignment

When attaching a Source without a slot, the domain assigns the first slot whose `accepts` contains the Source kind; ambiguity (multiple candidate slots) → the client must specify (`422 JOB_SOURCE_NOT_ACCEPTED` with candidates in meta).

## Errors

`JOB_NOT_FOUND`, `JOB_INVALID_TRANSITION`, `JOB_NOT_READY`, `JOB_SOURCE_NOT_ACCEPTED`, `JOB_SOURCE_LIMIT_EXCEEDED`, `JOB_CONCURRENCY_LIMIT`, `JOB_RETRY_NOT_ALLOWED`, plus `MODULE_DISABLED`/`MODULE_NOT_ENTITLED`.

## Related Documents

[job-state-machine](job-state-machine.md), [processing-runs](processing-runs.md), [temporal-workflows](temporal-workflows.md)
