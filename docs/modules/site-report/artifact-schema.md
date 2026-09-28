# Site Report — Artifact Schemas

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §3.2, §12 |

## `site_report` (primary, `construction.site-report/site-report@1.0.0`)

| Path | Content |
|---|---|
| `header` | project (name, code), visit date/time, inspection type, inspector(s), attendees, contractors present, weather, site areas |
| `summary` | AI/hybrid paragraph: overall status, counts by severity, key risks (citing knowledge) |
| `observations[]` | `{ ref_no, location, category, severity, description: richText, issue, corrective_action, responsible_party, deadline: dateValue, status (open/resolved), photos: imageRef[], voice_evidence_ref?, is_repeat, previous_ref? }` |
| `repeat_inspection` | `{ previous_visit: {job_id, document_number, date}, items[] { previous_ref, previous_photos[], current_photos[], outcome ∈ {resolved, persisting, worsened, not_inspected}, note } }` |
| `open_items_summary` | Table of open items including carried-over ones with overdue flags |
| `signatures[]` | Inspector, PM, consultant (from approval stages) |

## `open_items_list` (on demand, `…/open-items@1.0.0`)

Project-level table of open corrective actions with age, overdue status, responsible party, last evidence.

## Rendering Notes

Photo log template (`site-report-photo-log`) renders one observation per block with before/after pairs; formal template renders tables with thumbnail references and an evidence appendix option.
