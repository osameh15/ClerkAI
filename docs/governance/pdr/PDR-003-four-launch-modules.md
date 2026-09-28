# PDR-003: Launch Modules — Meeting, Site Report, Handwriting, Lecture

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Product / Founder |
| Blueprint | BP §3, §27 ("Scope creep"), §28.2 |

## Decision

v1 launches exactly four official modules:

| Key | Name |
|---|---|
| `business.meeting-minutes` | Meeting Minutes |
| `construction.site-report` | Construction Site Report |
| `general.handwriting-document` | Handwriting → Document |
| `education.lecture-summary` | Lecture Summary |

They are built **after** the platform vertical slice is proven (BP §29.3: Sprint 0 must not start with the Meeting module) and are the first consumers of the generic module contract.

## Consequences

- Additional modules require a new PDR and gate approval.
- Each launch module has a full specification set under [modules/](../../modules/README.md).
- Evidence that the platform is generic: all four install as packages (G5 exit) with the same manifest schema.
