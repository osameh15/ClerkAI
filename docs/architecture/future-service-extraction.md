# Future Service Extraction

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Future |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §0, §1.1 "Scale by extraction", §6 |

## Purpose

Defines how a bounded context or worker pool becomes a separately deployed service **without rewriting its domain model**, and what triggers extraction.

## Extraction Candidates (ordered by expected likelihood)

| Candidate | Trigger | Already separated by |
|---|---|---|
| Media/document processing workers | CPU/cost isolation, GPU needs | Separate Temporal task queues and processes |
| AI Gateway | Multi-region routing, independent scaling, provider credential isolation | `Gateway` facade, own tables |
| Export renderers | Different runtime (for example Chromium/LibreOffice), security isolation | Sandboxed process, `export` queue |
| Integrations (webhooks, API) | External traffic isolation | Event consumers, own tables |
| Remote Module Gateway | Marketplace M2 | New component; never in main process |
| Commerce/billing | Compliance boundary, payment provider integration | Facade + events |

## Extraction Procedure

1. **Confirm boundary health:** no cross-context SQL, facade-only access (lint history clean).
2. **Replace facade implementation** with a client (HTTP/gRPC) implementing the same Go interface; callers unchanged.
3. **Move persistence:** context tables move to their own schema → own database; foreign keys to other contexts are replaced by ID references (already required by DR-03).
4. **Events:** the service publishes via its own outbox to the shared EventBus (requires external broker; see [OD-007](../governance/open-decisions.md#od-007)).
5. **Tenant context:** propagated in request metadata; the service enforces its own RLS.
6. **Transactions:** any use case that previously spanned the context in one transaction is converted to a saga (Temporal) — the list of such use cases is maintained in [bounded-contexts](bounded-contexts.md) via DR-08.

## Non-Goals

No extraction before a measured trigger. Microservices are not a v1 goal ([ADR-002](../governance/adr/ADR-002-modular-monolith.md)).
