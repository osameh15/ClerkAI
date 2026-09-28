# Site Report — Repeat Inspection

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |
| Blueprint | BP §3.2 ("Repeat Inspection must be represented in the domain model from day one"; "before/after evidence comparison"), App. D ("Open item from previous visit can become Resolved/Overdue and receive new Evidence") |

## Domain Model

- An **issue** (`site.issue`) is created in the visit where first observed and has a corrective Action Item after finalisation.
- In later visits of the **same project**, new observations may `REINSPECTS` an existing issue (cross-job relation, project scope).
- The issue's lifecycle across visits is tracked through the Action Item (open / in_progress / resolved; overdue derived) and issue `status` + `reinspection_count`.

## Flow

```mermaid
sequenceDiagram
  participant U as Inspector
  participant P as PWA
  participant W as site-visit-processing
  participant K as Knowledge
  participant A as Actions
  U->>P: Start new visit in project (online: cache open items)
  P-->>U: Show previous open items checklist (with before photos)
  U->>P: Capture "after" photos per item (explicit link) + new observations
  P->>W: Sync + submit
  W->>K: knowledge.link_related (project scope):<br/>candidate previous issues (open items)
  W->>K: match_reinspection AI task:<br/>explicit links kept, others matched by location/category/visual similarity
  K-->>W: proposed outcomes (resolved / persisting / worsened / not_inspected)
  W->>K: review items for low-confidence matches and all "resolved" proposals (confirm)
  U->>K: confirm outcomes in review
  W->>A: on final: resolve Action Items with after-photo evidence,<br/>or keep open (overdue if past deadline), increment reinspection_count
```

## Rules

- **Resolved requires evidence:** a corrective action becomes `resolved` via repeat inspection only with at least one confirmed "after" evidence link.
- Explicit user links (checklist "after" capture) override AI matching.
- Items not inspected in this visit remain open; the report lists them as "not inspected".
- Before/after comparison: artifact section renders previous photos (from the earlier Source, if not deleted by retention) and current photos side by side.
- If the earlier Source was deleted by retention, the comparison shows metadata with "original photo no longer available".
