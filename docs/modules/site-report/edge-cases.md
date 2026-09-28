# Site Report — Edge Cases

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + Frontend + AI Engineer |

| Case | Expected behaviour |
|---|---|
| Device offline for days | Captures retained; warning after `offlineRetentionDays`; never auto-deleted |
| Storage nearly full | Warn at 70%, block video at 90%, allow photos/voice |
| App killed mid-upload | Resume multipart from last completed part |
| Same photo uploaded twice | Tenant dedup: reuse object/extractions; observation references both captures only once |
| Voice note describes several photos | Fusion splits into multiple observations; ambiguous linkage → review with drag-to-link UI |
| Photo with no description | Observation from vision analysis marked low confidence; review |
| Description contradicts photo | `CONTRADICTS`, review item |
| No project selected (requireProject=false) | Repeat inspection unavailable; report still works |
| Previous visit not finalised | Repeat inspection uses latest **final** visit only; drafts ignored |
| Previous photos deleted by retention | Comparison shows metadata only |
| GPS unavailable/inaccurate | Location from voice/text/vocabulary; GPS optional |
| Faces/people in photos | Optional blurring in exports; originals unchanged |
| Contractor not a member | Responsible party stored as organization text; Action Item owner_text |
| Critical safety issue found | Immediate notification option to project manager (config), independent of report approval |
| Very large visit (> limits) | Split into multiple visits suggested by UI |
| Session expired while offline | Captures kept; re-auth required before sync; no data loss |
