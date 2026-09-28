# Security Boundaries

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + DevOps |
| Blueprint | BP §14 |

## Network Zones

| Zone | Contents | Ingress | Egress |
|---|---|---|---|
| Edge | LB/WAF | Internet 443 | App zone |
| App | api, workers, dispatcher, Temporal | Edge (api only) | Data zone; egress proxy (allowlist); KMS/secret manager; OTel |
| Sandbox | parsers, renderers, AV | App (job handoff) | None (except AV signature updates via dedicated proxy) |
| Data | PostgreSQL, Redis, Temporal DB | App | Backups destination |
| Admin | Bastion / admin tools | VPN/SSO with MFA | App/Data read paths (audited) |

## Identity Boundaries

| Principal | Can access |
|---|---|
| `clerk_app` DB role | Tenant rows under RLS, platform read tables |
| Worker pool credentials | Only buckets/providers their queue needs |
| Export renderer | Pre-fetched inputs + single output presigned URL |
| Platform admin | Separate role, break-glass, audited |
| CI/CD | Deploy credentials scoped per environment; no production data access |

## Data Boundaries

Customer content never leaves: the tenant's prefix in object storage, tenant rows, the AI provider call for that job (under policy), and customer-requested exports/webhooks (IDs only). Telemetry, error trackers and analytics receive no customer content.
