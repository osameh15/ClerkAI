# Blueprint Traceability Matrix

| Field | Value |
|---|---|
| Status | Accepted (complete for Blueprint v1.0) |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Documentation Lead |
| Blueprint | All sections (0–30, App. A–F) |

## Legend

**Status:** `Specified` (implementation-ready spec exists) · `Specified–Provisional` (spec exists, values/vendors open) · `Deferred` (architecture-ready, implementation later by Blueprint intent) · `Open` (blocked on an open decision). Implementation status is tracked from Sprint 0 onward by adding `Implemented` in this column.

## Matrix

| ID | BP § | Requirement | Decision | Technical documents | Implementation domain | Validation / test | Status |
|---|---|---|---|---|---|---|---|
| TR-001 | 0 | Modular platform turning unstructured input into structured, traceable artifacts | PDR-001 | [product-overview](../product/product-overview.md), [architecture-overview](../architecture/architecture-overview.md) | All | [e2e-testing](../testing/e2e-testing.md) | Specified |
| TR-002 | 0 | No domain-specific behaviour hard-coded in Core | PDR-001, ADR-008, AP-01 | [architecture-principles](architecture-principles.md), [dependency-rules](../architecture/dependency-rules.md) | module, processing | [module-testing](../testing/module-testing.md) (Hello Module, grep check) | Specified |
| TR-003 | 0.1 | Multi-tenant core: orgs, users, teams, roles, permissions, entitlements | ADR-013, BDR-003 | [tenancy-architecture](../architecture/tenancy-architecture.md), [organizations](../platform/organizations/organizations.md), [authorization-model](../platform/access/authorization-model.md), [entitlement-model](../platform/entitlements/entitlement-model.md) | identity, organization, access, commerce | [database-testing](../testing/database-testing.md), [security-testing](../testing/security-testing.md) | Specified |
| TR-004 | 0.1 | Module Registry & Store: install, disable, versioning, config | ADR-008, PDR-002 | [module-registry](../modules/module-registry.md), [module-store](../modules/module-store.md), [module-lifecycle](../modules/module-lifecycle.md) | module | [module-testing](../testing/module-testing.md) | Specified |
| TR-005 | 0.1 | Universal ingestion for Audio, Video, Image, PDF, DOCX, PPTX, XLSX, Text, Scan | ADR-005 | [universal-ingestion](../ingestion/universal-ingestion.md) + per-kind docs | source | [integration-testing](../testing/integration-testing.md) | Specified–Provisional (providers OD-015) |
| TR-006 | 0.1 | Job engine + durable workflows (Temporal) | ADR-003 | [job-state-machine](../jobs/job-state-machine.md), [temporal-workflows](../jobs/temporal-workflows.md) | job, processing | [workflow-testing](../testing/workflow-testing.md) | Specified |
| TR-007 | 0.1 | Provider-neutral AI Gateway, Prompt & Model Registry, Evaluation pipeline | ADR-017, ADR-010 | [ai-gateway](../ai/ai-gateway.md), [model-registry](../ai/model-registry.md), [prompt-registry](../ai/prompt-registry.md), [evaluation-strategy](../ai/evaluation/evaluation-strategy.md) | ai, evaluation | [ai-testing](../testing/ai-testing.md) | Specified–Provisional (OD-016, OD-033) |
| TR-008 | 0.1 | Knowledge: entities, relations, evidence, corrections | ADR-011 | [knowledge-layer](../knowledge/knowledge-layer.md) | knowledge | [golden-tests](../testing/golden-tests.md) | Specified |
| TR-009 | 0.1 | Artifact engine: versioning, templates, PDF/DOCX, approvals | ADR-016 | [artifact-engine](../artifacts/artifact-engine.md), [approval-lifecycle](../artifacts/approval-lifecycle.md) | artifact, template, workflow | [golden-tests](../testing/golden-tests.md) (exports) | Specified–Provisional (OD-008/009/038) |
| TR-010 | 0.1 | Four launch modules via Card View + Launcher | PDR-002, PDR-003 | [modules/README](../modules/README.md), [module-launcher](../frontend/module-launcher.md) | modules, web | module acceptance criteria | Specified |
| TR-011 | 0.1 | Security, Privacy, Audit, Observability, CI/CD, Billing, Metering foundations | ADR-014 | security/, privacy/, [audit-model](../platform/audit/audit-model.md), operations/, billing/ | cross-cutting | [release-quality-gates](../testing/release-quality-gates.md) | Specified |
| TR-012 | 1 | "Intelligent documentation OS": choose output card, add sources, analyse/merge/validate | PDR-001, PDR-002 | [product-principles](product-principles.md), [information-architecture](../frontend/information-architecture.md) | web | [e2e-testing](../testing/e2e-testing.md) | Specified |
| TR-013 | 1.1 | 12 foundational principles (module-first … privacy by design) | AP-01..AP-12 | [architecture-principles](architecture-principles.md) | cross-cutting | invariants INV-01..10 tests | Specified |
| TR-014 | 1.2 | Not ERP/CRM/PM suite/only STT-OCR/chat; no third-party code in main process (v1) | PDR-001, ADR-008 | [non-goals](../product/non-goals.md), [product-boundaries](../product/product-boundaries.md), [module-sandboxing](../security/module-sandboxing.md) | — | architecture review | Specified |
| TR-015 | 2 | B2B-first; Personal workspace = one-person org | PDR-008 | [personal-workspaces](../platform/organizations/personal-workspaces.md), [personas](../product/personas.md) | organization | unit/API tests | Specified |
| TR-016 | 2 | Personas and needs | — | [personas](../product/personas.md), [use-cases](../product/use-cases.md) | product | UX reviews | Specified |
| TR-017 | 2.1 | KPIs: Time-to-Artifact, acceptance, correction, evidence coverage, activation, WAO, expansion MRR, gross margin | — | [product-kpis](../product/product-kpis.md), [metrics](../operations/metrics.md) | analytics | KPI queries tests | Specified |
| TR-018 | 3 | Four modules with inputs/outputs table | PDR-003 | [module-catalog](../product/module-catalog.md), manifests | modules | manifest validation | Specified |
| TR-019 | 3.1 | Meeting: diarization, fusion agenda/notes/audio, decision vs proposal vs unresolved, confidence+evidence, template+numbering, follow-up + task list; transcript secondary | PDR-005 | [meeting-minutes/](../modules/meeting-minutes/README.md) | module: meeting | [MM acceptance](../modules/meeting-minutes/acceptance-criteria.md), [evaluation-plan](../modules/meeting-minutes/evaluation-plan.md) | Specified–Provisional (thresholds) |
| TR-020 | 3.2 | Site Report: mobile, offline capture, voice↔photo linkage, Open/Resolved/Overdue, repeat inspection before/after, template + approval, independent actions | PDR-007 | [site-report/](../modules/site-report/README.md), [pwa/](../pwa/pwa-architecture.md) | module: site, web/pwa | [SR acceptance](../modules/site-report/acceptance-criteria.md) | Specified–Provisional (OD-023) |
| TR-021 | 3.3 | Handwriting: multi-page, low-confidence review, multiple artifacts from one Source | PDR-004 | [handwriting-document/](../modules/handwriting-document/README.md) | module: handwriting | [HW acceptance](../modules/handwriting-document/acceptance-criteria.md) | Specified |
| TR-022 | 3.4 | Lecture: concepts, definitions, examples, key points, questions, relations, flashcards; Persian + mixed slides/audio | — | [lecture-summary/](../modules/lecture-summary/README.md) | module: lecture | [LS acceptance](../modules/lecture-summary/acceptance-criteria.md) | Specified |
| TR-023 | 4 | Module Store is a top-level service: registry, install, deps, versioning, config, entitlement, permission registration | PDR-002 | [module-store](../modules/module-store.md), [module-dependencies](../modules/module-dependencies.md) | module | [module-testing](../testing/module-testing.md) | Specified |
| TR-024 | 4 | Module package layout (manifest, schemas, prompts, workflows, artifacts, templates, evaluations, translations, runtime) | ADR-008 | [module-platform](../modules/module-platform.md), [module-packaging](../modules/module-packaging.md), [manifest schema](../api/schemas/module-manifest.schema.json) | module | module validation | Specified |
| TR-025 | 4.1 | Module states (Available … Uninstalled); Disable ≠ Uninstall | — | [module-lifecycle](../modules/module-lifecycle.md), [module-disable-uninstall](../modules/module-disable-uninstall.md) | module | lifecycle matrix tests | Specified |
| TR-026 | 4.2 | Runtime types declarative/native/remote | ADR-008 | [module-runtime-contract](../modules/module-runtime-contract.md), [remote-modules](../marketplace/remote-modules.md) | processing, module | validator L5 | Specified (remote Deferred) |
| TR-027 | 5 | Revenue model; tokens never UX unit; AI Credits/Processing Minutes | BDR-001, BDR-002 | [billing-architecture](../billing/billing-architecture.md), [ai-credit-model](../billing/ai-credit-model.md) | commerce | billing tests | Specified–Provisional (OD-021) |
| TR-028 | 5.1 | Validation pricing (Personal/Team/Business/Enterprise), local pricing for Iran/regional markets | BDR-001 | [pricing-model](../billing/pricing-model.md), [plan-model](../billing/plan-model.md) | commerce | — | Specified–Provisional |
| TR-029 | 5.2 | Revenue streams incl. packs, premium modules, services, API, marketplace | BDR-001, BDR-004, BDR-005 | [business-principles](business-principles.md), [module-pricing](../billing/module-pricing.md) | commerce | — | Specified |
| TR-030 | 5.3 | Future Construction Pack | — | [module-catalog](../product/module-catalog.md), [pricing-model](../billing/pricing-model.md) | modules (future) | — | Deferred |
| TR-031 | 5.4 | Direct cost < 20–25% revenue; cost per Job/module/artifact/plan | — | [unit-economics](../billing/unit-economics.md), [cost-attribution](../billing/cost-attribution.md) | commerce, ai | cost dashboards | Specified–Provisional |
| TR-032 | 6 | Stack: Go modular monolith, Nuxt 4 PWA, PostgreSQL, pgvector, Temporal, Redis, S3/MinIO, REST+OpenAPI, SSE, OTel, Docker | ADR-001..006, 014, 015, 019..022 | [architecture-overview](../architecture/architecture-overview.md), [container-architecture](../architecture/container-architecture.md) | all | Sprint 0 | Specified |
| TR-033 | 6 | API and workers separate processes, shared codebase | ADR-002 | [runtime-architecture](../architecture/runtime-architecture.md), [worker-model](../jobs/worker-model.md) | apps | Sprint 0 | Specified |
| TR-034 | 6.1 | Proposed monorepo | ADR-002 | [container-architecture](../architecture/container-architecture.md#monorepo-mapping-bp-61) | repo | Sprint 0 S0-01 | Specified (tooling ADR-026) |
| TR-035 | 7 | 19 bounded contexts | ADR-002 | [bounded-contexts](../architecture/bounded-contexts.md), [component-architecture](../architecture/component-architecture.md) | internal/* | boundary lint | Specified |
| TR-036 | 7.1 | Core entity relationships | — | [domain-architecture](../architecture/domain-architecture.md), [erd](../data/erd.md) | data | schema review | Specified |
| TR-037 | 7.2 | UUIDv7; BIGINT+UUID for high volume | ADR-018 | [identifier-strategy](../data/identifier-strategy.md) | platform/ids | unit | Specified |
| TR-038 | 7.2 | Explicit `organization_id` on every tenant table | ADR-013 | [tenant-data-isolation](../data/tenant-data-isolation.md), [entity-catalog](../data/entity-catalog.md) | data | schema vs catalog test | Specified |
| TR-039 | 7.2 | Original Sources immutable; replace = new Source | PDR-004 | [immutable-data-policy](../data/immutable-data-policy.md), [source-model](../ingestion/source-model.md) | source | immutability tests | Specified |
| TR-040 | 7.2 | Artifacts never overwritten; every edit = version | ADR-016 | [artifact-versioning](../artifacts/artifact-versioning.md) | artifact | trigger tests | Specified |
| TR-041 | 7.2 | AuditEvent and UsageEvent append-only | — | [immutable-data-policy](../data/immutable-data-policy.md), [audit-security](../security/audit-security.md) | audit, commerce | grants tests | Specified |
| TR-042 | 7.2 | No large binaries in PostgreSQL | ADR-015 | [object-storage-layout](../data/object-storage-layout.md) | source | review | Specified |
| TR-043 | 7.2 | JSONB for extension; queryable fields normalised | ADR-006 | [schema-conventions](../data/schema-conventions.md) | data | review | Specified |
| TR-044 | 7.2 | RLS on sensitive tables | ADR-013 | [tenant-data-isolation](../data/tenant-data-isolation.md) | data | RLS tests | Specified |
| TR-045 | 7.3 | Core v1 tables | — | [entity-catalog](../data/entity-catalog.md) | data | migration tests | Specified |
| TR-046 | 8 | Frontend unaware of internal stages; schema-driven discovery/forms; API vs module versioning independent | ADR-004, ADR-009 | [api-principles](../api/api-principles.md), [module-ui-contract](../modules/module-ui-contract.md), [api-versioning](../api/api-versioning.md) | api, web | contract tests | Specified |
| TR-047 | 8 | Need→solution table (REST, OpenAPI, presigned multipart, SSE, WebSocket future, Temporal, outbox, webhooks + API keys) | ADR-004/005/007/021 | [api/](../api/api-principles.md), [openapi.yaml](../api/openapi/openapi.yaml) | api | OpenAPI validation | Specified |
| TR-048 | 8.1 | Job lifecycle states and transitions; upload ≠ submit; source preprocessing may start early | — | [job-state-machine](../jobs/job-state-machine.md), [universal-ingestion](../ingestion/universal-ingestion.md) | job | unit + workflow tests | Specified |
| TR-049 | 8.2 | Event envelope and baseline event types | ADR-007 | [event-architecture](../architecture/event-architecture.md), [event-envelope.schema.json](../api/schemas/event-envelope.schema.json) | platform/outbox | contract tests | Specified (broker OD-007) |
| TR-050 | 8.3 | Idempotency-Key for sensitive commands; outbox; activity-specific retries; AIExecution per fallback; signed webhooks with history | ADR-012 | [idempotency-strategy](../architecture/idempotency-strategy.md), [retries](../jobs/retries.md), [webhooks](../api/webhooks.md) | api, processing, ai, integration | API + workflow tests | Specified |
| TR-051 | 9 | Every attachment is a Source; preprocessing independent of Module; normalised content + chunks | ADR-005, PDR-004 | [universal-ingestion](../ingestion/universal-ingestion.md), [normalization](../ingestion/normalization.md), [chunking](../ingestion/chunking.md) | source | ingestion tests | Specified |
| TR-052 | 9 | Per-type baseline pipelines | — | audio/video/image/document/slide/spreadsheet processing docs | source | fixtures per kind | Specified–Provisional (OD-027) |
| TR-053 | 9.1 | Presigned direct upload; multipart; SHA-256 + tenant dedup; malware scan before processing; MIME sniffing; renditions separate | ADR-005 | [upload-flow](../ingestion/upload-flow.md), [deduplication](../ingestion/deduplication.md), [malware-scanning](../ingestion/malware-scanning.md), [mime-validation](../ingestion/mime-validation.md) | source | upload abuse tests | Specified (AV engine OD-011) |
| TR-054 | 10 | Artifact is a view of Knowledge | ADR-010 | [knowledge-layer](../knowledge/knowledge-layer.md), [artifact-engine](../artifacts/artifact-engine.md) | knowledge, artifact | golden tests | Specified |
| TR-055 | 10.1 | Generic entity types; module subtypes without Core tables | ADR-008 | [entity-model](../knowledge/entity-model.md) | knowledge | manifest validation | Specified |
| TR-056 | 10.2 | Human correction states | PDR-006 | [human-correction](../knowledge/human-correction.md) | knowledge | unit tests | Specified |
| TR-057 | 11 | No proprietary foundation model initially; moat via orchestration, fusion, schemas, evidence, eval, flywheel | — | [ai-architecture](../ai/ai-architecture.md), [training-strategy](../ai/training/training-strategy.md) | ai | — | Specified |
| TR-058 | 11.1 | Capability profiles; router by quality/language/modality/cost/latency/availability/tenant policy | ADR-017 | [model-profiles](../ai/model-profiles.md), [model-routing](../ai/model-routing.md) | ai | AI Gateway tests; G8 swap test | Specified |
| TR-059 | 11.2 | Registries: providers, models, profiles, prompts/versions; AIExecution recording | ADR-017 | [model-registry](../ai/model-registry.md), [prompt-registry](../ai/prompt-registry.md), [ai-execution-model](../ai/ai-execution-model.md) | ai | contract tests | Specified |
| TR-060 | 11.3 | Dataset flywheel; no training by default; review + consent + anonymisation | — | [model-improvement-flywheel](../ai/training/model-improvement-flywheel.md), [training-example-lifecycle](../ai/training/training-example-lifecycle.md), [training-data-policy](../privacy/training-data-policy.md) | evaluation, privacy | policy tests | Specified |
| TR-061 | 11.4 | Evaluation types/metrics; every change run against versioned dataset; flags/canary | — | [evaluation-metrics](../ai/evaluation/evaluation-metrics.md), [regression-gates](../ai/evaluation/regression-gates.md) | evaluation | eval runs | Specified–Provisional |
| TR-062 | 11.5 | When to fine-tune | — | [fine-tuning-decision-framework](../ai/training/fine-tuning-decision-framework.md) | ai | — | Specified |
| TR-063 | 12 | Artifact = structured JSON; renderers from same structure; HTML not source of truth | ADR-016 | [structured-content](../artifacts/structured-content.md), [rendering](../artifacts/rendering.md), [artifact.schema.json](../api/schemas/artifact.schema.json) | artifact | schema + golden tests | Specified |
| TR-064 | 12 | Template independent; Brand Kit; async export PDF/DOCX; transaction-safe numbering; configurable approval | — | [template-engine](../artifacts/template-engine.md), [brand-kit](../artifacts/brand-kit.md), [numbering-sequences](../artifacts/numbering-sequences.md), [approval](../platform/collaboration/approval.md) | template, workflow | numbering concurrency tests | Specified–Provisional (OD-008/009/038) |
| TR-065 | 12.1 | Lifecycle AI Draft → Review → Changes Requested → Approved → Final → Exported | — | [approval-lifecycle](../artifacts/approval-lifecycle.md) | artifact, workflow | unit tests | Specified |
| TR-066 | 13 | Entitled + Permitted + Resource Policy; RBAC first layer; project/team/data scope | BDR-003 | [authorization-model](../platform/access/authorization-model.md), [resource-policies](../platform/access/resource-policies.md) | access, commerce | authz matrix tests | Specified |
| TR-067 | 13 | Default roles Owner…Guest | — | [default-roles](../platform/access/default-roles.md) | access | unit | Specified |
| TR-068 | 13.1 | Permission namespace | — | [permission-namespaces](../platform/access/permission-namespaces.md) | access | validator L3 | Specified |
| TR-069 | 14 | Security as architecture; OWASP ASVS/Top10/API/GenAI alignment; ISO 27001 later | — | [security-architecture](../security/security-architecture.md) | cross-cutting | security testing | Specified |
| TR-070 | 14.1 | Threat model (tenant leak, injection, disclosure, malicious file, supply chain, cost attack, webhook/API abuse, privileged misuse) | — | [threat-model](../security/threat-model.md) | cross-cutting | security testing | Specified |
| TR-071 | 14.2 | Mandatory v1 controls (TLS/KMS, Argon2id, MFA, CSRF/CORS/CSP, object-level authz, secret manager, presign constraints, AV + parser isolation, layered rate limits, CI scans, SBOM/signing, immutable audit) | ADR-013 | [security-architecture](../security/security-architecture.md#mandatory-v1-controls-bp-142) + linked docs | cross-cutting | [security-testing](../testing/security-testing.md) | Specified (secret manager OD-013) |
| TR-072 | 14.3 | GenAI security controls | — | [ai-security](../security/ai-security.md), [prompt-injection](../security/prompt-injection.md), [output-validation](../security/output-validation.md) | ai | injection corpus | Specified |
| TR-073 | 14.4 | Security release gates Alpha/Private Beta/Paid Launch/Enterprise | — | [security-release-gates](../security/security-release-gates.md) | cross-cutting | gate evidence | Specified |
| TR-074 | 15 | Privacy principles; GDPR where applicable | — | [privacy-architecture](../privacy/privacy-architecture.md), [regional-data-considerations](../privacy/regional-data-considerations.md) | privacy | privacy review | Specified–Provisional |
| TR-075 | 15.1 | Data classification | — | [data-classification](../privacy/data-classification.md) | privacy | review | Specified |
| TR-076 | 15.2 | Configurable retention per category; async audited deletion; backups consistent | — | [data-retention](../privacy/data-retention.md), [retention-model](../data/retention-model.md), [backup-data-model](../data/backup-data-model.md) | privacy, data | retention sweep tests | Specified–Provisional (OD-037) |
| TR-077 | 15.3 | Export; async deletion with status; membership removal preserves records; recording consent; vendor registry | — | [export](../privacy/export.md), [deletion](../privacy/deletion.md), [consent](../privacy/consent.md), [memberships](../platform/organizations/memberships.md) | privacy | E2E | Specified (legal text OD-032) |
| TR-078 | 16 | AI governance (NIST AI RMF Govern/Map/Measure/Manage) | — | [ai-governance](../governance-ai/ai-governance.md) | ai | governance reviews | Specified |
| TR-079 | 16.1 | Model change management | — | [model-change-management](../governance-ai/model-change-management.md), [provider-change-management](../governance-ai/provider-change-management.md) | ai | change records | Specified |
| TR-080 | 16.2 | AI transparency | — | [transparency](../governance-ai/transparency.md) | web, artifact | UX review | Specified |
| TR-081 | 17 | OTel; correlatable IDs | ADR-014 | [observability](../operations/observability.md), [opentelemetry](../operations/opentelemetry.md), [tracing](../operations/tracing.md) | cross-cutting | Sprint 0 S0-18 | Specified (backend OD-012) |
| TR-082 | 17.1 | Initial SLI/SLO targets (provisional) | — | [sli-slo](../operations/sli-slo.md) | SRE | load tests | Specified–Provisional |
| TR-083 | 18 | Environments Local/CI/Staging/Production/Preview | — | [environments](../operations/environments.md) | DevOps | — | Specified |
| TR-084 | 18.1 | Pipeline stages; IaC; forward-compatible migrations; expand/contract; restore tested | — | [ci-cd](../operations/ci-cd.md), [migration-strategy](../data/migration-strategy.md), [restore](../operations/restore.md) | DevOps | CI | Specified |
| TR-085 | 19 | Test layers | — | [testing-strategy](../testing/testing-strategy.md) | QA | — | Specified |
| TR-086 | 19.1 | Golden tests for four modules | — | [golden-tests](../testing/golden-tests.md), [golden-datasets](../ai/evaluation/golden-datasets.md) | AI/QA | eval runs | Specified–Provisional |
| TR-087 | 20 | Card View launcher; Store top-level; Quick Clerk + module-first; navigation tree | PDR-002 | [module-launcher](../frontend/module-launcher.md), [navigation](../frontend/navigation.md), [information-architecture](../frontend/information-architecture.md) | web | E2E | Specified |
| TR-088 | 20.1 | Universal Artifact Editor (sources / editor / AI-review) | — | [artifact-editor](../frontend/artifact-editor.md), [editor-contract](../artifacts/editor-contract.md) | web | frontend tests | Specified (editor lib OD-014) |
| TR-089 | 20.2 | Design system components list | — | [component-catalog](../frontend/component-catalog.md), [design-system](../frontend/design-system.md) | web | component tests | Specified |
| TR-090 | 21 | PWA offline: IndexedDB, background sync + manual fallback, Local/Pending/Uploading/Synced, keep until server confirms, protection, minimal-interaction capture | PDR-007 | [pwa/](../pwa/pwa-architecture.md), [offline-site-report](../pwa/offline-site-report.md) | web/pwa | offline tests | Specified–Provisional (OD-023) |
| TR-091 | 22 | UsageEvent first, pricing engine later; meters table | BDR-002 | [usage-metering](../billing/usage-metering.md), [usage-event-catalog](../billing/usage-event-catalog.md) | commerce | unit + integration | Specified |
| TR-092 | 22.1 | Cost guardrails | BDR-002 | [quotas](../billing/quotas.md) | commerce, ai | tests + alerts | Specified |
| TR-093 | 23 | Marketplace in data model from day one; Clerk Official only in v1 | BDR-004 | [marketplace-architecture](../marketplace/marketplace-architecture.md), [publishers](../marketplace/publishers.md), [listings](../marketplace/listings.md) | marketplace | — | Specified (M1+ Deferred) |
| TR-094 | 23.1 | Maturity stages M0–M4 | BDR-004 | [marketplace-roadmap](../marketplace/marketplace-roadmap.md) | marketplace | — | Deferred (M1–M4) |
| TR-095 | 24 | Gate-based roadmap G0–G14 | — | [execution-roadmap](../execution/execution-roadmap.md) | leadership | gate reviews | Specified |
| TR-096 | 24.1 | Phase capacity estimates | — | [execution-roadmap](../execution/execution-roadmap.md) (Suggested Phases) | leadership | — | Specified–Provisional |
| TR-097 | 25 | Epic backlog EPIC-01..22 | — | [epic-backlog](../execution/epic-backlog.md), [dependency-map](../execution/dependency-map.md) | leadership | DoR | Specified |
| TR-098 | 26 | Team roles | — | [ownership-raci](../execution/ownership-raci.md) | leadership | — | Specified |
| TR-099 | 26.1 | Delivery model (trunk-based, ADR/PDR/BDR, AC before implementation, weekly demos, eval/migration/threat notes) | — | [branching-release-strategy](../operations/branching-release-strategy.md), [code-review-standards](../execution/code-review-standards.md) | all | PR checks | Specified |
| TR-100 | 27 | Risks and mitigations | — | [risk-register](../execution/risk-register.md) | leadership | risk reviews | Specified |
| TR-101 | 28.1 | ADR-001..016 | ADR-001..016 | [adr/README](adr/README.md) | — | — | Specified |
| TR-102 | 28.2 | PDR-001..007 | PDR-001..007 | [pdr/README](pdr/README.md) | — | — | Specified |
| TR-103 | 28.3 | BDR-001..005 | BDR-001..005 | [bdr/README](bdr/README.md) | — | — | Specified |
| TR-104 | 29.1 | Definition of Ready | — | [definition-of-ready](../execution/definition-of-ready.md) | all | — | Specified |
| TR-105 | 29.2 | Definition of Done | — | [definition-of-done](../execution/definition-of-done.md) | all | — | Specified |
| TR-106 | 29.3 | First sprint priorities; readiness criterion (Hello Module end to end) | — | [sprint-0](../execution/sprint-0.md), [sprint-1](../execution/sprint-1.md) | all | Sprint 1 DoD | Specified |
| TR-107 | 30 | Reference standards (no certification implied) | — | [security-architecture](../security/security-architecture.md), [ai-governance](../governance-ai/ai-governance.md) | — | — | Specified |
| TR-108 | 30.1 | Glossary | — | [glossary](glossary.md) | — | — | Specified |
| TR-109 | App. A | Screen inventory and flows (generic + four modules) | — | [information-architecture](../frontend/information-architecture.md), module product specs | web | E2E | Specified |
| TR-110 | App. B | API v1 inventory | ADR-004 | [openapi.yaml](../api/openapi/openapi.yaml) (all App. B endpoints marked `x-clerk-status: baseline`) | api | OpenAPI validation + contract tests | Specified |
| TR-111 | App. C | Baseline role matrix | — | [permission-matrix](../platform/access/permission-matrix.md) | access | authz matrix tests | Specified |
| TR-112 | App. C | Plan entitlement matrix v0 | BDR-001, BDR-003 | [plan-entitlements](../platform/entitlements/plan-entitlements.md) | commerce | entitlement tests | Specified–Provisional |
| TR-113 | App. D | Acceptance criteria and AI quality targets (initial baselines) | — | [evaluation-metrics](../ai/evaluation/evaluation-metrics.md), [module-evaluations](../ai/evaluation/module-evaluations.md), module acceptance criteria | ai, modules | golden tests | Specified–Provisional |
| TR-114 | App. E | Migration order and baseline schema | — | [migration-order](../data/migration-order.md) (one documented deviation: 000) | data | migration tests | Specified |
| TR-115 | App. E | Key index/constraint checklist | — | [indexing-strategy](../data/indexing-strategy.md) | data | database tests | Specified |
| TR-116 | App. F | Sprint 0 outputs | — | [sprint-0](../execution/sprint-0.md) | all | Sprint 0 DoD | Specified |
| TR-117 | App. F | Sprint 1 outputs | — | [sprint-1](../execution/sprint-1.md) | all | Sprint 1 DoD | Specified |
| TR-118 | App. F | Checklist before the four real modules | — | [implementation-entry-checklist](../execution/implementation-entry-checklist.md) | all | checklist evidence | Specified |

## Coverage Summary

- All Blueprint sections 0–30 and Appendices A–F are mapped (118 rows).
- No Blueprint requirement is unmapped. Items marked `Specified–Provisional` depend on open decisions or on data (pricing, thresholds) the Blueprint itself marks as hypotheses.
- `Deferred` rows are deferred by Blueprint intent (Construction Pack expansion modules, Marketplace M1–M4, remote runtime).

## Maintenance

Add a row when a new Blueprint revision adds requirements; update Status to `Implemented` with a link to the verifying test suite as implementation progresses.
