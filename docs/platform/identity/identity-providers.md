# Identity Providers

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Provisional |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Identity domain + Security |
| Blueprint | BP §2 (Enterprise SSO), §14.2, §14.4 (Enterprise gate: SSO/MFA), App. C (SSO/SAML Enterprise) |

## Confirmed Decisions

- The `identity` context owns users and sessions regardless of the IdP implementation.
- OIDC login providers are configurable per environment; Enterprise SSO (SAML/OIDC per organization) is architecture-ready and gated by `sso.saml`.
- Enterprise SSO binds an organization to verified email domains; members from those domains must authenticate through the org IdP (policy flag).

## TBD

- Build vs self-host vs hosted IdP — [OD-004](../../governance/open-decisions.md#od-004). Constraint: availability of hosted vendors in target markets (Iran/regional) and data residency.
- SCIM provisioning — Future.

## Interface Requirements for Any Choice

Magic link, OIDC with PKCE, TOTP MFA, session revocation, audit hooks for every authentication event, exportable user data, no vendor lock-in of user IDs (Clerk user IDs are UUIDv7 owned by Clerk).
