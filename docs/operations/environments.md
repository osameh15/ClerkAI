# Environments

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (hosting Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | DevOps / SRE |
| Blueprint | BP §18 |

| Environment | Purpose (BP §18) | Data | Secrets | Providers | Feature flags |
|---|---|---|---|---|---|
| **Local** | Docker Compose; mock/sandbox providers | Synthetic fixtures | `.env.local` (gitignored), dev-only | Mock AI provider by default; optional personal sandbox keys | All toggleable |
| **CI** | Ephemeral DB/Redis; unit, integration, eval smoke | Fixtures | CI secret store (sandbox keys for eval smoke) | Mock + limited real (eval smoke, budget-capped) | Test defaults |
| **Development** (shared, optional) | Integration of branches before staging | Synthetic | Dev secret namespace | Sandbox keys with low budgets | Dev flags |
| **Staging** | Production-like; non-production keys; synthetic/redacted data | Synthetic/redacted only — **no production customer data** | Staging namespace | Non-production provider keys with budget caps | Canary rehearsal |
| **Production** | Isolated secrets/storage, backups, monitoring | Customer data | Production namespace (KMS) | Production keys | Controlled rollout |
| **Preview** (optional) | Frontend/PR previews with synthetic data | Synthetic | Preview namespace | Mock | — |

## Separation Rules

- Separate cloud accounts/projects (or at minimum separate networks + IAM) for production.
- Separate databases, buckets, Temporal namespaces, Redis instances per environment.
- AI usage controls per environment: budget caps on non-production keys; production keys only in production `ai` pool.
- Migrations promoted through CI → staging → production.
- No production data copied to lower environments; incident debugging uses production tooling with audit.

## Local Stack (Sprint 0)

PostgreSQL 16 + pgvector, Redis, MinIO, Temporal dev server + UI, Mailpit, OTel Collector + Jaeger (or Grafana stack), mock AI provider service, optional ClamAV container. One command (`task up`) brings it up ([sprint-0](../execution/sprint-0.md)).
