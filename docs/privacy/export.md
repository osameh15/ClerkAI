# Data Export

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Privacy |
| Blueprint | BP §15.3 ("Export user/organization data in a usable format") |

## Export Types

| Type | Initiated by | Contents | Format |
|---|---|---|---|
| User data export | User | Profile, memberships, own sessions list, authored comments, own jobs/artifacts metadata | ZIP: JSON + readme |
| Organization export | Owner (`privacy.requests`) | All tenant data: sources (originals), artifacts (JSON + latest exports PDF/DOCX), knowledge (JSON), actions, audit (per entitlement), settings | ZIP (multi-part), JSON Lines + original files + manifest with checksums |
| Artifact export | Users | PDF/DOCX | [rendering](../artifacts/rendering.md) |

## Rules

Asynchronous (`data_export_requests`), notification with short-lived download URL (24 h), export logged and audited, exports stored in exports bucket and deleted after expiry, rate-limited (1 active org export).
