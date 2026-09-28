# Meeting Minutes — Permissions

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Access |
| Blueprint | BP §13.1 |

| Permission | Default roles | Guards |
|---|---|---|
| `business.meeting-minutes.create` | owner, admin, manager, member | + `jobs.create` |
| `business.meeting-minutes.view` | owner, admin, manager, member, viewer | + `jobs.view`/`artifacts.view`, scope |
| `business.meeting-minutes.edit` | owner, admin, manager, member | + `artifacts.edit` (own) / `artifacts.edit-any` |
| `business.meeting-minutes.approve` | owner, admin, manager | + `artifacts.approve`, workflow stage eligibility |
| `business.meeting-minutes.view-transcript` | owner, admin, manager, member | Obligation: transcript artifact and transcript panel hidden otherwise |

Participants who are not members do not gain access by being mentioned. Confidential meetings: creators can restrict visibility to the job creator + named members via resource grants (project/team scope or explicit grants).
