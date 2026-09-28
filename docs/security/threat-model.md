# Threat Model

| Field | Value |
|---|---|
| Status | Accepted (baseline threat model — Alpha gate artefact) |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security + Tech Lead |
| Blueprint | BP §14.1, §14.3, §14.4 (Alpha: baseline threat model) |

## Method

STRIDE per trust boundary ([security-architecture](security-architecture.md)) plus OWASP API and LLM Top 10 mapping. Reviewed at each gate and for every security-sensitive change ("threat note", BP §26.1).

## Threats and Controls

| ID | Threat | Scenario | Likelihood / Impact | Controls | Owner | Verification |
|---|---|---|---|---|---|---|
| T01 | Cross-tenant data leakage | Missing tenant filter, RLS gap, ID guessing, cache key collision, presign for wrong key | M / Critical | App guard + RLS + composite FKs + tenant-prefixed keys + BOLA tests ([tenant-isolation](tenant-isolation.md)) | Tech Lead | RLS tests, BOLA suite in CI |
| T02 | Broken authorization (BOLA/BFLA) | Viewer calls approve endpoint; member edits others' artifacts | M / High | Use-case-level authz, permission + policy checks, matrix tests ([authorization-security](authorization-security.md)) | Tech Lead | Authz matrix tests |
| T03 | Malicious file upload | Parser exploit, zip bomb, polyglot | M / High | Sniffing, AV, sandbox, limits, updated parsers ([upload-security](upload-security.md)) | Security | Upload abuse tests |
| T04 | Prompt injection (direct) | User text instructs model to ignore schema/leak | H / Medium | Content isolation, schema-only outputs, no tools ([prompt-injection](prompt-injection.md)) | AI Eng | Injection test corpus |
| T05 | Indirect prompt injection | Uploaded PDF contains hidden instructions to exfiltrate other content or alter decisions | M / High | Untrusted content delimiting, tenant-scoped context only, citation checks, human review | AI Eng | Injection corpus in eval |
| T06 | Malware distribution | Clerk serving infected files back to other users | L / High | Quarantine; downloads only of clean originals/renditions | Security | AV tests |
| T07 | Stored XSS | Model or user content with script rendered in editor/exports | M / High | No HTML model; sanitisation; CSP; renderer sandbox ([output-validation](output-validation.md)) | Frontend | XSS tests |
| T08 | SSRF | Webhook URL to internal metadata; provider media fetch abuse | M / High | Egress proxy, IP range blocks, DNS re-check, no redirects ([webhook-security](webhook-security.md)) | Security | SSRF tests |
| T09 | Webhook abuse / replay | Forged or replayed deliveries to customers | L / Medium | HMAC + timestamp + event ID | Integrations | Tests |
| T10 | API key leakage | Key in public repo | M / High | Hashed storage, prefixes for scanning, scopes, rotation, revoke ([api-keys](../api/api-keys.md)) | Security | Secret scanning partners |
| T11 | Credential theft / session hijack | Stolen cookie, phishing | M / High | HttpOnly/Secure cookies, rotation, MFA for admins, session revoke, anomaly alerts ([authentication-security](authentication-security.md)) | Security | Pen test |
| T12 | Privilege escalation | Custom role grants self Owner rights | L / High | Cannot grant permissions not held; owner-only permissions; MFA + audit | Tech Lead | Unit + matrix tests |
| T13 | Model/provider data leakage | Provider retains/trains on data; logs leak content | M / High | Vendor registry, no-train/zero-retention settings, redaction, tenant AI policy ([ai-security](ai-security.md)) | Privacy | Vendor review |
| T14 | Third-party module abuse (future) | Remote module exfiltrates tenant data | — (v1 N/A) / High | No third-party code in v1; remote gateway scopes later ([module-sandboxing](module-sandboxing.md)) | Security | Design review |
| T15 | Object-storage exposure | Public bucket, long-lived URLs | L / Critical | Private buckets, short TTL presign, IAM least privilege, config scanning | DevOps | IaC policy checks |
| T16 | Audit tampering | Insider deletes audit rows | L / High | INSERT-only grants, triggers, hash chain, export to WORM storage (Enterprise) ([audit-security](audit-security.md)) | Security | Tests |
| T17 | Billing abuse | Free-tier farming, quota bypass, refund fraud | M / Medium | Quotas enforced server-side, one personal workspace per user, idempotent usage, anomaly detection | Commerce | Tests |
| T18 | Denial of service | Request floods, huge uploads, SSE exhaustion | M / Medium | Edge rate limits, size limits, connection caps, autoscaling | SRE | Load tests |
| T19 | AI cost abuse | Scripted jobs to burn AI budget; prompt amplification | M / High | Budgets, per-org concurrency, cost ceilings, anomaly alerts ([ai-cost-spike runbook](../operations/runbooks/ai-cost-spike.md)) | AI Eng | Alerts |
| T20 | Supply chain | Compromised dependency/container/module package | M / High | SBOM, pinning, signed images, scanning, package hashes ([dependency-security](dependency-security.md)) | DevOps | CI |
| T21 | Privileged misuse | Support engineer browses tenant data | L / High | Least privilege, break-glass, separate admin path, audit ([OD-040](../governance/open-decisions.md#od-040)) | Security | Access review |
| T22 | Sensitive data in logs/telemetry | Transcripts in logs | M / Medium | Logging policy, redaction, tests ([logging](../operations/logging.md)) | SRE | Log scanning |
| T23 | Offline device loss | Lost phone with captured site photos | M / Medium | Local purge after sync, sign-out handling, device protection ([OD-023](../governance/open-decisions.md#od-023)) | Frontend | Review |
| T24 | System prompt / secret disclosure via model | Model reveals system prompt or provider errors expose keys | M / Low-Med | No secrets in prompts; provider errors mapped to generic codes | AI Eng | Tests |

## Residual Risks

Tracked in [risk-register](../execution/risk-register.md): Persian-specific injection techniques not covered by public corpora; provider-side data handling reliance; device-level offline protection limits.
