# Tenant Isolation (Security View)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Tech Lead |
| Blueprint | BP §14.1 (Tenant data leak: "RLS, scoped repositories, BOLA tests, security tests"), App. D ("No cross-tenant access in authorization/RLS tests") |

Architecture: [tenancy-architecture](../architecture/tenancy-architecture.md). Database: [tenant-data-isolation](../data/tenant-data-isolation.md).

## Security Requirements

| # | Requirement | Test |
|---|---|---|
| TI-1 | Every tenant table has RLS (sensitive) or is platform-scoped by explicit classification | Schema lint compares catalog vs DB |
| TI-2 | App role cannot bypass RLS | CI assertion |
| TI-3 | Requests with foreign IDs return 404 | BOLA suite over all `{id}` routes |
| TI-4 | Cache keys and values tenant-bound | Unit tests on cache layer |
| TI-5 | Object keys tenant-prefixed; presign only after authz | Integration tests |
| TI-6 | Workers set tenant per transaction; no tenant-less queries on tenant tables | Repository tests; runtime guard logs (and fails in non-prod) any query without `app.org_id` |
| TI-7 | Retrieval/vector search filtered by tenant before ranking | Tests with cross-tenant near-duplicate embeddings |
| TI-8 | AI prompts only contain current tenant data | Prompt assembly tests; eval harness checks |
| TI-9 | Exports/renders only tenant assets | Renderer input tests |
| TI-10 | Event consumers reject cross-tenant resource references | Consumer tests |

## Incident Handling

Suspected cross-tenant exposure is a SEV1 security incident ([tenant-security-incident runbook](../operations/runbooks/tenant-security-incident.md)).
