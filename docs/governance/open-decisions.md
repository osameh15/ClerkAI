# Open Decisions

| Field | Value |
|---|---|
| Status | Accepted (living register) |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Technical Leadership |
| Blueprint | BP §24, §28, §29 |

## Purpose

Register of decisions the Blueprint deliberately leaves open or does not address. No document may silently invent an answer to any of these; documents reference them as `OD-xxx`. When an OD is closed it becomes an ADR/PDR/BDR and its row is updated with `Status: Closed → <record>`.

**Blocks Sprint 0** marks decisions that must close during Sprint 0 (usually in its first days) because Sprint 0 deliverables depend on them. See [documentation-readiness-report.md](../execution/documentation-readiness-report.md).

## Summary

| ID | Question | Required by | Blocks Sprint 0 | Owner | Status |
|---|---|---|---|---|---|
| [OD-001](#od-001) | Go HTTP router / framework | Sprint 0 | Closed | Tech Lead | **Closed → [ADR-023](adr/ADR-023-http-router-chi.md)** (`go-chi/chi v5`) |
| [OD-002](#od-002) | SQL access library | Sprint 0 | Closed | Tech Lead | **Closed → [ADR-024](adr/ADR-024-sql-access-sqlc-pgx.md)** (`sqlc + pgx/v5`) |
| [OD-003](#od-003) | Migration tooling | Sprint 0 | Closed | Tech Lead | **Closed → [ADR-025](adr/ADR-025-migrations-goose.md)** (`pressly/goose v3`) |
| [OD-004](#od-004) | Identity provider implementation | Sprint 0 (interface), G4 (production) | No — Sprint 0 uses dev provider behind interface | Tech Lead + Security | Open |
| [OD-005](#od-005) | Initial hosting provider and region | G13 | No | DevOps + Founder | Open |
| [OD-006](#od-006) | Production object-storage provider | G6 / G13 | No — MinIO locally | DevOps | Open |
| [OD-007](#od-007) | Event bus deployment | G7 | No — outbox table + in-process dispatcher suffices | Tech Lead | Open |
| [OD-008](#od-008) | PDF rendering implementation | G10 | No | Tech Lead + Frontend | Open |
| [OD-009](#od-009) | DOCX rendering implementation | G10 | No | Tech Lead | Open |
| [OD-010](#od-010) | Billing / payment provider by region | G12 | No | Founder | Open |
| [OD-011](#od-011) | Malware scanning engine | G6 | No | Security | Open |
| [OD-012](#od-012) | Observability backend | Sprint 0 (local), G13 (prod) | No — local OTel collector + Jaeger/Grafana acceptable | DevOps | Open |
| [OD-013](#od-013) | Secret manager | G4 | No — `.env` excluded from Git locally | DevOps + Security | Open |
| [OD-014](#od-014) | Frontend UI kit and rich-text editor foundation | Sprint 1 / G10 | No | Frontend Lead | Open |
| [OD-015](#od-015) | Transcription / diarization providers (Persian) | G6 / G8 | No | AI Engineer | Open |
| [OD-016](#od-016) | Initial LLM / vision providers (≥ 2 adapters) | G8 | No — mock adapter in Sprint 0/1 | AI Engineer | Open |
| [OD-017](#od-017) | Feature flag implementation | Sprint 1 | No | Tech Lead | Open |
| [OD-018](#od-018) | Declarative workflow definition format | G5 / G7 | No — Hello Module uses fixed pipeline | Tech Lead | Open |
| [OD-019](#od-019) | Reprocessing semantics for completed Jobs | G7 | No | Product + Tech Lead | Open |
| [OD-020](#od-020) | Amendment of Final Artifacts | G10 | No | Product | Open |
| [OD-021](#od-021) | AI Credit conversion rates and plan quotas | G12 | No | Founder | Open |
| [OD-022](#od-022) | Default confidence thresholds per module | G11 | No | AI Engineer + Product | Open |
| [OD-023](#od-023) | Offline data protection on device | G11 (Site Report) | No | Frontend + Security | Open |
| [OD-024](#od-024) | Monorepo tooling (task runner, JS package manager) | Sprint 0 | Closed | Tech Lead | **Closed → [ADR-026](adr/ADR-026-monorepo-pnpm-taskfile.md)** (`pnpm workspace + Taskfile`) |
| [OD-025](#od-025) | Web SDK generation tool | Sprint 0 | No — thin wrapper allowed | Frontend Lead | Open |
| [OD-026](#od-026) | Calendar and date canonicalisation (Jalali/Gregorian) | G9 | No | Product + AI | Open |
| [OD-027](#od-027) | Video frame analysis strategy | G6 | No | AI Engineer | Open |
| [OD-028](#od-028) | Data residency / regional deployment model | Enterprise demand | No | Founder + DevOps | Open |
| [OD-029](#od-029) | Production runtime platform (Compose/VM vs Kubernetes) | G13 | No | DevOps | Open |
| [OD-030](#od-030) | Extent of custom roles in v1 | G4 | No | Product | Open |
| [OD-031](#od-031) | Search implementation | G9 | No | Tech Lead | Open |
| [OD-032](#od-032) | Recording-consent notice content and enforcement | G11 (Meeting) | No | Product + Legal | Open |
| [OD-033](#od-033) | Evaluation runner tooling | G8 | No | AI Engineer | Open |
| [OD-034](#od-034) | Session storage (PostgreSQL vs Redis) | Sprint 0 | No — PostgreSQL recommended | Tech Lead | Open |
| [OD-035](#od-035) | Webhook delivery execution | G12 | No | Tech Lead | Open |
| [OD-036](#od-036) | Live meeting capture scope in v1 | G11 (Meeting) | No | Product | Open |
| [OD-037](#od-037) | Retention defaults per plan | G12 | No | Product + Privacy | Open |
| [OD-038](#od-038) | Template authoring format | G10 | No | Tech Lead + Design | Open |
| [OD-039](#od-039) | Embedding model and dimension | G9 | No | AI Engineer | Open |
| [OD-040](#od-040) | Customer support / break-glass access process | G13 | No | Security + Founder | Open |
| [OD-041](#od-041) | Marketplace revenue share and payout terms | Marketplace M3 | No | Founder | Open |

---

## Entries

### OD-001
**Go HTTP router / framework**
- **Why it matters:** Shapes middleware, OpenAPI integration, context propagation.
- **Options:** `net/http` (Go ≥ 1.22 pattern routing) + small middleware set; `chi`; `echo`; `gin`; generated server from OpenAPI (`oapi-codegen` strict server).
- **Recommendation:** `net/http` or `chi` with `oapi-codegen`-generated request/response types so the OpenAPI document stays the contract (ADR-004). Avoid frameworks that own the context or hide errors.
- **Dependencies:** ADR-004, OD-025.
- **Deadline:** Sprint 0 day 1–2. **Owner:** Tech Lead. **Status:** **Accepted / Closed (2026-09-28) — `go-chi/chi v5`, see [ADR-023](adr/ADR-023-http-router-chi.md).**

### OD-002
**SQL access library**
- **Why it matters:** Repository layer, RLS session variables, transactions, testing.
- **Options:** `pgx` + `sqlc`; `pgx` + hand-written SQL; `GORM`/`ent` ORM.
- **Recommendation:** `pgx/v5` + `sqlc`. Explicit SQL keeps tenant filters reviewable and makes `SET LOCAL app.org_id` trivial inside transactions. ORMs obscure tenant filtering.
- **Dependencies:** ADR-006, ADR-013, OD-003. **Deadline:** Sprint 0 day 1–2. **Owner:** Tech Lead. **Status:** **Accepted / Closed (2026-09-28) — `sqlc + pgx/v5`, see [ADR-024](adr/ADR-024-sql-access-sqlc-pgx.md).**

### OD-003
**Migration tooling**
- **Options:** `goose`, `golang-migrate`, `atlas` (declarative + lint), `tern`.
- **Recommendation:** A tool supporting plain SQL, transactional migrations, and CI linting of destructive changes; `atlas` (lint) or `goose` (simplicity). Must support expand/contract discipline ([migration-strategy](../data/migration-strategy.md)).
- **Deadline:** Sprint 0 day 1–2. **Owner:** Tech Lead. **Status:** **Accepted / Closed (2026-09-28) — `pressly/goose v3`, see [ADR-025](adr/ADR-025-migrations-goose.md).**

### OD-004
**Identity provider implementation**
- **Why it matters:** Magic link, OAuth/OIDC, MFA, sessions, future SAML (Enterprise).
- **Options:** In-house (magic link + OIDC login + TOTP/WebAuthn) behind an `Authenticator` interface; self-hosted IdP (Ory Kratos, Keycloak, Zitadel); hosted (WorkOS, Auth0, Clerk.com — note naming collision).
- **Recommendation:** Keep the `identity` bounded context as the owner of users/sessions regardless; Sprint 0 ships a local/dev provider behind the interface (BP App. F). Choose production IdP before G4 considering Iranian-market availability of hosted vendors.
- **Deadline:** G4. **Owner:** Tech Lead + Security. **Status:** Open.

### OD-005
**Initial hosting provider and region**
- **Why it matters:** Latency, sanctions/availability constraints for Iran/regional markets, KMS/secret manager availability, cost.
- **Options:** Iranian cloud providers; EU/regional hyperscaler; hybrid (regional compute + external AI providers).
- **Dependencies:** OD-006, OD-013, OD-028, provider data-processing terms. **Deadline:** before G13. **Owner:** DevOps + Founder. **Status:** Open.

### OD-006
**Production object-storage provider**
- **Options:** MinIO self-hosted; provider S3-compatible service; AWS S3.
- **Constraint:** Must support presigned PUT, multipart upload, server-side encryption, lifecycle rules, object lock or versioning for originals (ADR-015).
- **Deadline:** G6 (staging), G13 (production). **Status:** Open.

### OD-007
**Event bus deployment**
- **Why it matters:** Fan-out, ordering, replay, operational cost.
- **Options:** (a) PostgreSQL outbox + in-process dispatcher with per-consumer inbox tables; (b) outbox relay to NATS JetStream; (c) outbox relay to Redis Streams; (d) Kafka/Redpanda later.
- **Recommendation:** Start with (a) for v1; keep the `EventBus` port so (b) can be adopted without domain change when external consumers or throughput require it (ADR-007).
- **Deadline:** G7. **Owner:** Tech Lead. **Status:** Open.

### OD-008
**PDF rendering implementation**
- **Options:** Headless Chromium (HTML/CSS print); Typst; WeasyPrint; commercial SDK.
- **Constraints:** RTL + Persian shaping, embedded fonts, page numbering, headers/footers, golden-file determinism, sandboxed renderer with no network egress.
- **Deadline:** G10. **Status:** Open.

### OD-009
**DOCX rendering implementation**
- **Options:** Go library (for example `unioffice`, commercial licence), template-based docx generation, Pandoc, LibreOffice headless conversion.
- **Constraints:** RTL, styles mapped from TemplateVersion, editable output, deterministic tests.
- **Deadline:** G10. **Status:** Open.

### OD-010
**Billing / payment provider by region**
- **Options:** Stripe/Paddle (international); Iranian payment gateways (Shaparak PSPs); manual invoicing for Enterprise.
- **Constraint:** Billing context exposes a provider-agnostic `PaymentProvider` port (EPIC-22).
- **Deadline:** G12. **Owner:** Founder. **Status:** Open.

### OD-011
**Malware scanning engine**
- **Options:** ClamAV sidecar; commercial scanning API; cloud-native scanning.
- **Constraint:** Scan must complete before any processing (BP §9.1); quarantined objects never reach parsers.
- **Deadline:** G6. **Owner:** Security. **Status:** Open.

### OD-012
**Observability backend**
- **Options:** Grafana stack (Tempo, Loki, Prometheus/Mimir); SigNoz; Honeycomb/Datadog.
- **Constraint:** Instrumentation is OpenTelemetry only (ADR-014), so the backend is replaceable.
- **Deadline:** production choice before G13. **Status:** Open.

### OD-013
**Secret manager**
- **Options:** HashiCorp Vault / OpenBao; cloud provider secret manager; SOPS + age for GitOps.
- **Constraint:** No secrets in Git or plaintext DB columns (BP §14.2). Rotation supported.
- **Deadline:** G4. **Status:** Open.

### OD-014
**Frontend UI kit and rich-text editor foundation**
- **Options:** Nuxt UI, PrimeVue, custom Tailwind design system; editor: TipTap/ProseMirror, Lexical-equivalent.
- **Constraints:** RTL, accessibility, structured (schema-bound) editing rather than free HTML.
- **Deadline:** Sprint 1 (UI kit), G10 (editor). **Status:** Open.

### OD-015
**Transcription / diarization providers**
- **Why it matters:** Persian WER/CER is the largest AI quality risk (BP §27).
- **Options:** Commercial ASR APIs, open-weight Whisper-family models self-hosted, Persian-specialised providers.
- **Recommendation:** Decide from a Persian benchmark run on the golden dataset ([golden-datasets](../ai/evaluation/golden-datasets.md)), not from vendor claims.
- **Deadline:** G8. **Status:** Open.

### OD-016
**Initial LLM / vision providers**
- **Constraint:** ≥ 2 provider/model adapters, or one provider + mock/fallback adapter, before real module work (BP App. F).
- **Deadline:** G8. **Status:** Open.

### OD-017
**Feature flag implementation**
- **Options:** In-house table + Redis cache; OpenFeature SDK with a provider (Unleash, Flagsmith, flagd).
- **Recommendation:** OpenFeature interface to avoid lock-in.
- **Deadline:** Sprint 1. **Status:** Open.

### OD-018
**Declarative workflow definition format**
- **Why it matters:** Declarative modules need a way to describe processing pipelines and approval flows interpreted by generic Temporal workflows (ADR-008).
- **Options:** Constrained YAML step graph interpreted by a generic Temporal "module pipeline" workflow; Serverless Workflow spec subset; per-module native Temporal workflows (violates declarative-first).
- **Recommendation:** Constrained YAML DAG of platform-provided step types (`ingest.require`, `ai.extract`, `knowledge.fuse`, `review.gate`, `artifact.generate`, `approval.start`). See [workflow-reference](../modules/sdk/workflow-reference.md).
- **Deadline:** G5 draft, G7 final. **Status:** Open.

### OD-019
**Reprocessing semantics for completed Jobs**
- **Question:** May a `completed` Job be reprocessed (new ProcessingRun) and what happens to existing Knowledge and Artifacts?
- **Options:** (a) forbid, require new Job; (b) allow, creating new Knowledge revision and new Artifact versions; (c) module-configurable.
- **Recommendation:** (c) with default (a); when allowed, existing Artifacts gain new versions, never overwrite.
- **Deadline:** G7. **Status:** Open.

### OD-020
**Amendment of Final Artifacts**
- **Options:** (a) Final is terminal; amendments create a new Artifact linked as `amends`; (b) reopen creates a new draft version and re-runs approval.
- **Recommendation:** (a) for regulated documents (Site Report, Minutes) — preserves the signed record.
- **Deadline:** G10. **Status:** Open.

### OD-021
**AI Credit conversion rates and plan quotas**
- **Constraint:** Validation pricing (BP §5.1) is provisional; conversion from raw usage to credits is data in PlanVersions.
- **Deadline:** G12 (after 4–6 weeks of Cost-per-Job data). **Status:** Open.

### OD-022
**Default confidence thresholds per module**
- **Constraint:** Thresholds must be calibrated against evaluation data; defaults in docs are provisional.
- **Deadline:** G11. **Status:** Open.

### OD-023
**Offline data protection on device**
- **Options:** Rely on OS/device encryption; WebCrypto encryption of IndexedDB blobs with key held in memory/session; shorter local retention.
- **Constraint:** Browser capabilities vary; must not lose captured data.
- **Deadline:** G11. **Status:** Open.

### OD-024
**Monorepo tooling**
- **Options:** Go workspace + `pnpm` workspaces + `Taskfile`/`make`; Nx/Turborepo for JS side.
- **Recommendation:** Go workspace + `pnpm` + `Taskfile` (simplest cross-platform).
- **Deadline:** Sprint 0 day 1. **Status:** **Accepted / Closed (2026-09-28) — pnpm workspace + Taskfile; Nx/Turborepo/Bazel excluded, see [ADR-026](adr/ADR-026-monorepo-pnpm-taskfile.md).**

### OD-025
**Web SDK generation tool**
- **Options:** `openapi-typescript` + `openapi-fetch`; `orval`; hand-written thin wrapper.
- **Deadline:** Sprint 0 (thin wrapper acceptable, BP §29.3). **Status:** Open.

### OD-026
**Calendar and date canonicalisation**
- **Question:** Knowledge stores deadlines such as "12 Mehr". Canonical storage format and display rules?
- **Recommendation:** Store ISO-8601 Gregorian date plus `original_text` and `calendar: jalali|gregorian` of the utterance; render per organization locale. Relative dates resolved against the Source's recorded date with Evidence.
- **Deadline:** G9. **Status:** Open.

### OD-027
**Video frame analysis strategy**
- **Options:** Fixed-rate sampling; scene-change detection; on-demand frames only near referenced timestamps.
- **Constraint:** Cost guardrails (BP §22.1).
- **Deadline:** G6. **Status:** Open.

### OD-028
**Data residency / regional deployment model**
- **Constraint:** "Architecture-ready" for Enterprise (BP §27). Tenant → region binding is modelled in `organizations.data_region` from v1.
- **Deadline:** On Enterprise demand. **Status:** Open.

### OD-029
**Production runtime platform**
- **Constraint:** "Docker; Kubernetes only when scale requirements justify it" (BP §6).
- **Options:** Docker Compose on VMs with systemd; Nomad; managed Kubernetes.
- **Deadline:** G13. **Status:** Open.

### OD-030
**Extent of custom roles in v1**
- **Constraint:** Plan matrix: Team "Limited", Business/Enterprise full (BP App. C).
- **Question:** What "Limited" means (for example clone-and-adjust of system roles with a cap).
- **Deadline:** G4. **Status:** Open.

### OD-031
**Search implementation**
- **Options:** PostgreSQL full-text + trigram; pgvector hybrid; external engine later.
- **Constraint:** Persian tokenisation quality.
- **Deadline:** G9. **Status:** Open.

### OD-032
**Recording-consent notice content and enforcement**
- **Constraint:** BP §15.3 requires recording consent/notice to be represented in organization UX/policy.
- **Deadline:** G11. **Owner:** Product + Legal. **Status:** Open.

### OD-033
**Evaluation runner tooling**
- **Options:** In-house Go runner writing to `evaluation_runs/results`; Python harness; third-party eval platforms.
- **Constraint:** Results must land in Clerk tables and gate CI.
- **Deadline:** G8. **Status:** Open.

### OD-034
**Session storage**
- **Options:** PostgreSQL `sessions` table (BP §7.3 lists it) with Redis cache; Redis only.
- **Recommendation:** PostgreSQL as record (revocation, audit), Redis as cache.
- **Deadline:** Sprint 0. **Status:** Open.

### OD-035
**Webhook delivery execution**
- **Options:** Temporal workflow per delivery; dedicated delivery worker reading `webhook_deliveries`.
- **Deadline:** G12. **Status:** Open.

### OD-036
**Live meeting capture scope in v1**
- **Question:** BP §3.1 mentions live meetings; App. A flow says "Live Record". Is real-time transcription in v1 scope, or record-then-upload with chunked uploads?
- **Recommendation:** v1 = in-browser recording with chunked resumable upload; processing after stop. Real-time transcription is Future.
- **Deadline:** G11. **Status:** Open.

### OD-037
**Retention defaults per plan**
- **Constraint:** BP §15.2 suggested defaults are initial; configurable per organization.
- **Deadline:** G12. **Status:** Open.

### OD-038
**Template authoring format**
- **Options:** HTML/CSS templates with constrained slots; Typst templates; DOCX reference templates.
- **Dependencies:** OD-008, OD-009.
- **Deadline:** G10. **Status:** Open.

### OD-039
**Embedding model and dimension**
- **Constraint:** pgvector initially (ADR-019); indexing only after measurement (BP App. E). Multilingual (Persian) quality required.
- **Deadline:** G9. **Status:** Open.

### OD-040
**Customer support / break-glass access process**
- **Constraint:** BP §14.1 "break-glass process, separate admin path"; must be audited and time-boxed.
- **Deadline:** G13. **Status:** Open.

### OD-041
**Marketplace revenue share and payout terms**
- **Why it matters:** Publisher economics, payouts, tax handling (BP §5.2, §23.1 M3).
- **Options:** Fixed percentage; tiered by volume; category-based; payouts via payment-provider marketplace features vs manual.
- **Dependencies:** OD-010 (payment provider), Marketplace M3 readiness.
- **Deadline:** Marketplace M3. **Owner:** Founder. **Status:** Open.

## Related Documents

- [source-of-truth-policy.md](source-of-truth-policy.md)
- [risk-register.md](../execution/risk-register.md)
- [documentation-readiness-report.md](../execution/documentation-readiness-report.md)
