# API Versioning

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8 ("Platform API versioning and Module versioning are independent concerns") |

## Rules

1. **URL major version** (`/v1`). A new major (`/v2`) is introduced only for breaking changes that cannot be made additively; `/v1` then remains supported for a published deprecation window (minimum 12 months for public API consumers; first-party frontend migrates immediately).
2. **Additive changes are non-breaking:** new endpoints, new optional request fields, new response fields, new enum values in response fields *documented as extensible*. Clients must ignore unknown response fields.
3. **Breaking changes** (within a major, forbidden): removing/renaming fields, changing types, making optional request fields required, changing error codes for existing conditions, tightening validation in a way that rejects previously valid requests.
4. **Module content versions are independent:** module schemas (`schema_ref`) evolve with module versions; the API transports them as JSON objects. A module major version does not change the API version.
5. **Deprecation:** deprecated operations carry `deprecated: true` in OpenAPI and a `Deprecation` + `Sunset` response header.
6. **OpenAPI document version** (`info.version`) follows SemVer and is published with each release; CI diff (`oasdiff`-style) blocks breaking changes on `/v1`.
7. **Events** are versioned per event type ([event-architecture](../architecture/event-architecture.md)); webhooks include `version`.

## Related Documents

[api-principles](api-principles.md), [compatibility-policy](../modules/compatibility-policy.md)
