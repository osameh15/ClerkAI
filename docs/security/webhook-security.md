# Webhook Security (Platform Controls)

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Integrations |
| Blueprint | BP §8.3, §14.1 |

Customer-facing contract: [api/webhook-security](../api/webhook-security.md).

## Outbound (Clerk → customer)

| Control | Detail |
|---|---|
| URL validation | HTTPS only; public DNS; reject IP literals in private/reserved ranges (RFC 1918, loopback, link-local 169.254/16 incl. cloud metadata, CGNAT, IPv6 ULA/link-local) |
| DNS rebinding | Resolve at delivery time, connect to the resolved IP, re-check range; pin for the request |
| Egress | Delivery workers only via egress proxy with the above rules; no access to internal networks |
| Redirects | Not followed |
| Timeouts | 10 s total, response body read capped (64 KB) |
| Signing | HMAC-SHA-256 over `timestamp.body`; secret rotation overlap |
| Payload | Public fields only |

## Inbound (payment provider → Clerk)

Signature verification with provider SDK/secret, timestamp tolerance, idempotency by provider event ID, strict schema validation, no trust of payload amounts without API confirmation for critical state changes.
