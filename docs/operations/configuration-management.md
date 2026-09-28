# Configuration Management

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | DevOps + Tech Lead |
| Blueprint | BP §18, App. F (Configuration strategy) |

## Layers

| Layer | Examples | Source | Change process |
|---|---|---|---|
| Static build config | Version, build SHA | Build | CI |
| Environment config | DB URLs, bucket names, Temporal address, OTel endpoint, rate limits | Environment variables (12-factor) from IaC/deploy manifests | PR to deployment repo/IaC |
| Secrets | DB credentials, provider keys, signing keys | Secret manager (references in env) | Secret rotation process |
| Runtime flags | Feature flags | Flag service | Audited changes |
| Business config (data) | Plans, entitlements, model registry, profiles, prompts, modules | Database (versioned records) | Admin tools + change management |

## Rules

- Typed config struct in Go; validation at startup; fail fast on missing required values; log effective (non-secret) config at start.
- Defaults are safe (for example mock AI provider **disabled** outside local/CI by explicit guard).
- No environment-specific code branches except via config values.
- Configuration changes to production go through PR review; emergency changes documented in incident log.
