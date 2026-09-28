# Security Release Gates

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security |
| Blueprint | BP §14.4 |

| Gate | Minimum requirement (BP §14.4) | Evidence | Maps to roadmap |
|---|---|---|---|
| **Alpha** | SAST/secret scan, authorization tests, baseline threat model | CI results; authz matrix suite green; [threat-model](threat-model.md) accepted | G4 Security Foundation |
| **Private Beta** | Dependency/container scan, file scanning, incident runbook, backup restore test | CI; AV in pipeline; [incident-response](../operations/incident-response.md) + runbooks; restore test report | G13 Private Beta |
| **Paid Launch** | External penetration test, webhook/API security test, access review, DR test | Pen test report with Critical/High fixed; SSRF/webhook tests; access review record; DR exercise report | G14 Paid Launch |
| **Enterprise** | SSO/MFA, audit export, retention controls, vendor/security questionnaire, ISMS roadmap | Features live; questionnaire pack; ISMS plan | Enterprise readiness (post-launch) |

Every release additionally requires: no open Critical/High findings without approved exception; tenant-isolation tests green.
