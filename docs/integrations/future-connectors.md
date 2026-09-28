# Future Connectors

| Field | Value |
|---|---|
| Status | Proposed |
| Spec class | Future |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §2 (future external sources and integrations), §5.2 (enterprise integration services) |

## Candidate Connectors

| Connector | Direction | Value |
|---|---|---|
| Calendar (meeting metadata, participants) | Inbound | Better Meeting Minutes attribution |
| Cloud drives (import sources, export artifacts) | Both | Source ingestion, document distribution |
| Task tools (sync Action Items) | Outbound | Commitment tracking in existing tools (without becoming a PM suite) |
| E-signature | Outbound | Signed final documents |
| Email ingestion (forward-to-Clerk) | Inbound | Quick Clerk from email attachments |
| Video conferencing recordings | Inbound | Meeting sources |

## Architectural Constraints (already satisfied)

Sources abstraction accepts external origins (`source.origin` metadata); OAuth tokens for connectors will be envelope-encrypted per organization ([encryption](../security/encryption.md)); connectors run as integration workers with SSRF-guarded egress; connector sync is idempotent and tenant-scoped.
