# Sharing

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Collaboration domain + Security |
| Blueprint | BP §7 (Share), App. A (Export / Share: "share link") |

## Share Types

| Type | Grantee | Access | v1 |
|---|---|---|---|
| Member share | Specific membership or team in the same org | view / comment / edit | Yes |
| Guest share | External person invited as `guest` membership with resource grant | view / comment | Yes |
| Link share | Anyone with the link (token), optional password, expiry | view only (rendered export or read-only view) | Proposed; requires entitlement and org policy switch |

## Rules

- Shares never cross organizations as data copies; link shares serve a read-only rendering under strict headers (no indexing, short cache).
- Link tokens: 256-bit random, hashed at rest, expiry required (default 7 days, max 90), revocable, access-logged (audit `artifact.share.accessed` aggregated).
- Organization policy can disable link sharing (default: disabled for Enterprise, enabled for Team/Business — TBD with Product, [OD-021](../../governance/open-decisions.md#od-021)).
- Restricted-classified artifacts cannot be link-shared.

## TBD

Link-share default policy per plan — tracked with [OD-021](../../governance/open-decisions.md#od-021) plan packaging.
