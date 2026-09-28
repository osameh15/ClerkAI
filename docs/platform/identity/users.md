# Users

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Identity domain |
| Blueprint | BP §7 (Identity), §15.3 |

## Model

A `User` is a global person identity (not tenant-owned). Tenant-specific data about a person (roles, team membership, authored content) lives in `memberships` and domain tables.

| Field | Notes |
|---|---|
| `email` | `citext`, unique among non-deleted users; verified |
| `display_name`, `locale` (`fa`/`en`), `timezone` | Profile |
| `status` | `active`, `disabled` (platform action), `deleted` |

## Lifecycle

- Created on first successful sign-in (or when accepting an invitation).
- **Deletion (user request, BP §15.3):** asynchronous; memberships removed; PII (email, name) replaced with a tombstone; authored organizational records remain attributed to "Deleted user" per organization retention policy ([deletion](../../privacy/deletion.md)). Last Owner of an organization must transfer ownership or delete the organization first.

## Privacy

User profile data is classified Restricted (PII). Only minimal fields are exposed to other members (display name, avatar, email visible to admins).
