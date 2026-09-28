# Public API

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Integrations + Tech Lead |
| Blueprint | BP §8, App. B, App. C (API/Webhooks: Team limited, Business+ full) |

## Scope

The public API is the same `/v1` API used by the web app, restricted by API key scopes. Endpoints available to API keys in v1:

| Area | Operations | Scope |
|---|---|---|
| Uploads & Sources | create session, complete, list, get, text source | `sources:read/write` |
| Jobs | create, attach, submit, cancel, retry, get, list, SSE | `jobs:read/write` |
| Knowledge review | list/resolve review items | `jobs:write` |
| Artifacts | list, get, versions, export, get export | `artifacts:read/write` |
| Actions | list, create, update | `actions:read/write` |
| Usage | get usage | `usage:read` |
| Webhooks | manage endpoints | `webhooks:write` |
| Audit | query | `audit:read` (+ entitlement) |

Not available to API keys: identity/session endpoints, organization/member/role management, billing changes, module installation (v1).

## Documentation

Public reference generated from OpenAPI (operations tagged for public exposure), with examples ([examples](../api/examples.md)); changelog per release; deprecation policy ([api-versioning](../api/api-versioning.md)).
