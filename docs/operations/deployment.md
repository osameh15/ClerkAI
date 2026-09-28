# Deployment

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (platform Open) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | DevOps / SRE |
| Blueprint | BP §6 (Docker; Kubernetes only when justified), §18 |

## Principles

- Infrastructure as Code for production (BP §18).
- Immutable, signed container images; deploy verifies signatures.
- Runtime platform: Docker-based; orchestrator per [OD-029](../governance/open-decisions.md#od-029).

## Release Sequence

1. Run migrations (expand-only or reviewed) with `clerk_migrator` job.
2. Deploy workers (new code must handle old workflow histories — replay-tested).
3. Deploy dispatcher (leader handover).
4. Deploy API canary → full.
5. Deploy web (static assets versioned; service worker update prompts users).
6. Post-deploy smoke tests + SLO watch window (30 min).

## Temporal Considerations

Worker deployments must remain replay-compatible with in-flight workflows (versioning markers); breaking interpreter changes use new workflow types ([temporal-workflows](../jobs/temporal-workflows.md)).

## Frontend/PWA

Service worker updates are staged: new SW installs in background and activates on next navigation after user confirmation to avoid interrupting offline capture.
