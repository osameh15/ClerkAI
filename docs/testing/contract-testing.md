# Contract Testing

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead |
| Blueprint | BP §19 (Contract: OpenAPI, event schemas, Module manifests), §18.1 (schema/API compatibility) |

| Contract | Test |
|---|---|
| OpenAPI document | Structural validation (3.1); lint rules (operationIds, tags, error responses, idempotency header on commands, permission annotations); breaking-change diff vs last release |
| Server ↔ OpenAPI | Every route implemented is declared; responses validated against schemas in API tests (response validation middleware in test mode) |
| Web SDK ↔ OpenAPI | Generated client compiles; frontend type-check |
| JSON Schemas (`api/schemas`) | Meta-schema validation; examples validate; OpenAPI component equivalence checks for Error, Evidence, Event envelope |
| Event schemas | Every produced `(event_type, version)` has a schema; producer tests validate payloads; consumers tested with recorded payloads |
| Module manifests | All manifests (official modules + doc examples) validate; semantic validator rules |
| Artifact content | Fixture artifacts validate against envelope + module schema |
| Provider adapters | Recorded fixtures replayed per adapter |

Documentation validation (this repository) runs the same schema/OpenAPI checks plus Mermaid parsing and link checking ([documentation-conventions](../governance/documentation-conventions.md#9-review-and-quality-gates-for-documentation)).
