# Artifact Domain

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Artifacts domain |
| Blueprint | BP §12, §30.1 ("Artifact: Usable output such as Minutes or a Report") |

## Aggregate

| Element | Notes |
|---|---|
| **Artifact** | Logical output: `job_id`, `module_key`, `artifact_type` (manifest type), `title`, `status`, `current_version_id`, `final_version_id`, `document_number`, `template_id`, `language`, `project_id` |
| **ArtifactVersion** | Immutable content snapshot: `version_no`, `parent_version_id`, `origin` (`ai`, `human`, `system`), `content` (envelope), `content_schema_ref`, `content_sha256`, `processing_run_id`, `provenance`, `created_by`, `change_summary` |
| **ArtifactExport** | Rendering of a version: format, template version, brand kit, status, object key |

## Current Version and AI/Human Versions

Example history (brief): `v1 AI Draft` → `v2 Human Edit` → `v3 Manager Approved`. In Clerk terms: v1 (`origin=ai`), v2 (`origin=human`), v3 = v2 approved — approval does **not** create a new content version; the approval decision references v2 and `final_version_id = v2`. A `system` version is created only when finalisation stamps content (for example document number inserted into metadata) — then v3 (`origin=system`, parent v2) becomes the final version.

## Use Cases

`Generate` (from step), `Edit` (If-Match), `GetVersion`, `ListVersions`, `SubmitForApproval`, `Finalize`, `Export`, `Delete/Restore`, `GenerateAdditional` (on-demand types).

## Invariants

- Every edit creates a new version (BP §7.2); versions never overwritten.
- Content always validates (envelope + module schema).
- `final` artifacts are immutable; exports of final artifacts reference `final_version_id`.
- Artifact deletion is soft and blocked for `final` artifacts under active retention/legal hold.

## Events

`artifact.generated`, `artifact.version.created`, `artifact.submitted`, `artifact.approved`, `artifact.finalized`, `artifact.export.completed`, `artifact.export.failed`.
