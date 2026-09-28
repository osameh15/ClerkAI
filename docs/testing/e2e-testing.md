# End-to-End Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | QA |
| Blueprint | BP §19 (E2E: Login → Module → Upload → Job → Review → Export), App. F (Sprint 1 E2E) |

## Critical Journeys

| ID | Journey | Stage |
|---|---|---|
| E2E-01 | Login → install Hello Module → create job → upload text/image → process (mock AI) → artifact v1 read | Sprint 1 |
| E2E-02 | Permission/entitlement denial paths (viewer cannot create; non-entitled org sees upgrade) | Sprint 1 |
| E2E-03 | Meeting: upload audio + agenda → review ambiguous owner → edit → approve → export PDF/DOCX → action items created | G11 |
| E2E-04 | Site Report: offline capture (network emulation) → sync → review → approval chain → final → repeat inspection visit | G11 |
| E2E-05 | Handwriting: multi-page upload → resolve uncertain spans → clean document → generate letter | G11 |
| E2E-06 | Lecture: audio + slides → summary → flashcards | G11 |
| E2E-07 | Admin: invite member, custom role, module disable/enable, audit log shows events | G10/G12 |
| E2E-08 | Billing: quota warning and hard limit behaviour (sandbox payment provider) | G12 |

## Environment

Staging-like stack with mock AI (deterministic) for functional E2E; a nightly E2E variant uses real providers with small fixtures. Playwright on Chromium + WebKit (iOS Safari proxy) + mobile viewport.
