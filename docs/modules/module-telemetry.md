# Module Telemetry

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform + SRE |
| Blueprint | BP §2.1 (KPIs), §17 |

## Platform-Provided (automatic, every module)

- Per module/version: jobs created/submitted/completed/failed, time-to-artifact, review items per job, review resolution time, artifacts generated/approved/finalised, exports, AI cost per job, correction rate, acceptance rate, evidence coverage, module activation.
- Attributes: `module_key`, `module_version`, `organization_id` (internal dashboards), `plan`.

## Module-Declared

`telemetry.metrics[]` (counter/histogram/gauge) emitted by platform steps from structured results (for example `meeting.actions_extracted` = count of entities of type `meeting.action` created) — modules do not emit arbitrary telemetry code. `telemetry.kpis[]` selects KPI dashboards to generate.

## Privacy

No content in telemetry; counts and durations only.
