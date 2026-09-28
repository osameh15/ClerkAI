# Project Domain

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Projects domain |
| Blueprint | BP §7 ("Projects — Optional project context"), App. A (Projects screen) |

## Purpose

Optional organizational context grouping Sources, Jobs, Artifacts and Action Items (for example a construction project with repeated site visits). Projects enable project-scoped roles and approver rules, and cross-job features such as Repeat Inspection.

## Model

`projects(name, code, status, description, metadata)`; `project_members(project_id, membership_id, project_role)`. `project_role` is a string key; default keys `project_manager`, `member`, `viewer`. Modules may reference project roles in approval workflows (for example Site Report: `project_role: project_manager`).

## Rules

- Projects are optional; every feature must work without one (except features whose module declares a project requirement, such as Site Report's repeat inspection which requires a project to link visits).
- Project-scoped role assignments restrict visibility per [resource-policies](../access/resource-policies.md).
- Archiving a project makes it read-only (no new Jobs) without deleting anything.
- Not a project-management tool: no schedules, budgets or Gantt ([non-goals](../../product/non-goals.md)).

## API

`GET/POST /v1/projects`, `GET/PATCH /v1/projects/{id}`. Project overview aggregates (jobs, artifacts, open actions) are read models composed by the API layer from owning contexts' facades.

## Events

`project.created`, `project.updated`, `project.archived`, `project.member.changed`.
