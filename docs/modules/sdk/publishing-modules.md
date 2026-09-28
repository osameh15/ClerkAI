# Publishing Modules

| Field | Value |
|---|---|
| Status | Accepted (v1 official) / Future (partners) |
| Spec class | Implementation-ready (M0) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Module Platform |
| Blueprint | BP §23, §23.1 |

## v1 (M0 — Clerk Official)

1. PR to `modules/<cat>/<name>` with version bump and changelog.
2. CI: validate, tests, evaluation smoke → merge.
3. Release job: build package, upload, register `draft` version, run full evaluation suites.
4. Review: Product (behaviour), Tech Lead (contract), Security (new permissions/native steps), AI Engineer (evaluation).
5. Publish → listing updated → auto-update policies apply (patch/minor) → admins notified of majors.

## Future (M1+ partners)

Publisher onboarding and verification, signed packages, manual review, listing moderation, revenue share setup ([review-process](../../marketplace/review-process.md), [publishers](../../marketplace/publishers.md)).
