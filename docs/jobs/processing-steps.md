# Processing Steps

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Processing domain |
| Blueprint | BP §7.3 (`processing_steps`), §8 (progress) |

## Definition

A **ProcessingStep** records one unit of work in a run: typically one step node of the module workflow definition (for example `extract_actions`), which may execute one or several Temporal activities.

## Fields

`run_id`, `step_key` (from module workflow), `step_type` (platform step type, [workflow-reference](../modules/sdk/workflow-reference.md)), `attempt`, `status` (`pending`, `running`, `waiting`, `succeeded`, `failed`, `skipped`, `cancelled`), `progress` (0–1), `stage` (user-facing stage key), `error_code`, `error_detail` (redacted), `output_ref` (IDs of produced knowledge/artifacts/AIExecutions), timestamps.

## Rules

- Steps are written by activities at start/finish (idempotent upsert by `(run_id, step_key, attempt)`).
- User-facing progress aggregates steps into **stages** declared in the module workflow (`stage:` attribute); the frontend never sees `step_key`s ([api/sse](../api/sse.md)).
- `error_detail` never contains customer content or provider raw responses; full diagnostics go to logs/traces with request/workflow IDs.
