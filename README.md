# Clerk AI

**From real-world input to verified business artifact.**

Clerk AI is a modular AI documentation platform. It transforms unstructured human input — audio, video, images, handwriting, scans, PDF, DOCX, PPTX, XLSX and plain text — into structured, traceable, editable, professional artifacts.

```text
Unstructured Sources → Understanding → Structured Knowledge → Evidence → Professional Artifact → Workflow
```

## Status

**Documentation Gate closed — READY FOR SPRINT 0.** This repository currently contains the complete technical documentation and contracts; production implementation starts with [Sprint 0](docs/execution/sprint-0.md).

## Launch Modules

| Module | Purpose |
|---|---|
| `business.meeting-minutes` | Meetings → formal minutes, decisions, actions, owners, deadlines, risks |
| `construction.site-report` | Mobile/offline site visits → observations, corrective actions, repeat inspection |
| `general.handwriting-document` | Handwritten pages → clean document, formal letter, minutes, task list |
| `education.lecture-summary` | Lectures + slides → summary, concepts, definitions, flashcards |

All modules run on a generic platform; none is hard-coded into the core.

## Technology Baseline

| Layer | Choice |
|---|---|
| Backend | Go 1.27.x — Modular Monolith (API + worker processes), chi v5, sqlc + pgx/v5, goose v3 |
| Frontend | Nuxt 4 + TypeScript + PWA (IndexedDB offline capture) |
| Data | PostgreSQL (+ Row Level Security, pgvector), Redis, S3-compatible object storage |
| Workflows | Temporal |
| APIs | REST + OpenAPI 3.1, SSE, signed webhooks |
| AI | Provider-neutral AI Gateway, model profiles, prompt registry, evaluation framework |
| Observability | OpenTelemetry |
| Repo tooling | pnpm workspace + Taskfile |

## Documentation

Start at the documentation portal: **[docs/README.md](docs/README.md)**.

| Topic | Entry point |
|---|---|
| Product | [docs/product/product-overview.md](docs/product/product-overview.md) |
| Architecture | [docs/architecture/architecture-overview.md](docs/architecture/architecture-overview.md) |
| Decisions | [ADRs](docs/governance/adr/README.md) · [PDRs](docs/governance/pdr/README.md) · [BDRs](docs/governance/bdr/README.md) · [Open decisions](docs/governance/open-decisions.md) |
| API contract | [docs/api/openapi/openapi.yaml](docs/api/openapi/openapi.yaml) |
| Module platform | [docs/modules/README.md](docs/modules/README.md) |
| Execution | [Roadmap](docs/execution/execution-roadmap.md) · [Sprint 0](docs/execution/sprint-0.md) · [Sprint 1](docs/execution/sprint-1.md) · [Readiness report](docs/execution/documentation-readiness-report.md) |

The authoritative source of truth is the Blueprint in [docs/source/](docs/source/README.md).
