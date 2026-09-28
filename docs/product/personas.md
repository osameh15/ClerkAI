# Personas

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §2 |

## Market Position

B2B-first. Lecture Summary opens a B2C/B2Edu path. Account and billing are organization-centric; a Personal Workspace is a one-person Organization ([PDR-008](../governance/pdr/PDR-008-personal-workspace-as-organization.md)).

## Personas

| Persona | Primary need (BP §2) | Primary modules | Typical role | Critical platform capabilities |
|---|---|---|---|---|
| Business owner / executive | Meeting minutes, directives, commitment tracking, formal company documents | Meeting Minutes, Handwriting | Owner / Manager | Approval, numbering, brand templates, Action Items |
| Project manager | Meetings, progress documentation, action items, approvals | Meeting Minutes, Site Report | Manager | Projects, approvals, Action Items, review queue |
| Construction engineer / site inspector | Site visits, photos, voice notes, observations, corrective actions | Site Report | Member | Mobile capture, offline queue, repeat inspection, evidence |
| Administrative staff | Handwriting, letters, reports, turning scans into formal documents | Handwriting → Document | Member | Multi-page scans, low-confidence review, templates |
| Lecturer / student | Lecture summaries, concepts, flashcards, questions, study notes | Lecture Summary | Member (often Personal workspace) | Audio + slides fusion, Persian support |
| Organization admin | Module Store, roles, permissions, billing, policy, audit | — | Owner / Admin | Store, RBAC, entitlements, usage, audit log |
| Enterprise IT / Security | SSO, retention, audit, data residency, AI policy controls | — | Admin (Enterprise) | SSO/SAML, retention policies, audit export, AI data policy |

## Design Implications

- Inspectors work one-handed, outdoors, on unstable networks → minimal-tap capture, offline, large touch targets.
- Executives and administrative staff are output-focused → template fidelity, numbering, formal tone, Persian RTL.
- Admins/IT need governance surfaces separate from creation flows.

## Related Documents

[use-cases](use-cases.md), [information-architecture](../frontend/information-architecture.md)
