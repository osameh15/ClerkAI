# Cost Attribution

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Commerce + AI Engineer |
| Blueprint | BP §5.4 ("Every Job must expose attributable cost") |

## Attribution Keys

Every usage event carries the most specific available keys: `ai_execution_id` → `processing_run_id` → `job_id` → `module_key`/`module_version` → `organization_id`; `source_id` for ingestion; `artifact_id` for exports.

## Shared Costs

| Shared cost | Allocation rule |
|---|---|
| Source extraction reused by multiple jobs | Charged once to the first triggering context (source/job); later jobs record zero-cost "reuse" usage for analytics |
| Storage of a source used by several jobs | Allocated to the source; job-level reports pro-rate by jobs referencing it |
| Failed AI attempts | Attributed to the job (true cost) |
| Platform overhead (idle workers, Temporal, DB) | Not attributed to jobs; tracked as platform cost for margin analysis |

## Reports

Job cost available on the job (internal/admin view); organization cost dashboards for Clerk operators; customers see credits/minutes, not raw cost.
