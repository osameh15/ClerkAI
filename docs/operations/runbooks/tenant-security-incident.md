# Runbook: Tenant Security Incident

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Security (IC) + Tech Lead |

**Scope:** suspected cross-tenant data exposure, account takeover, leaked API key, malware campaign, audit hash-chain break, suspicious privileged access.

**Symptoms:** customer reports seeing foreign data; RLS guard trip alerts; anomalous access patterns; secret scanning notice; hash chain verification failure.

**Detection:** security alerts ([alerting](../alerting.md)), customer support, external researcher report.

**Immediate actions (SEV1):**
1. Declare SEV1; Security is Incident Commander.
2. **Contain:** disable affected feature/endpoint via flag; revoke leaked keys/sessions; suspend compromised accounts; block IPs; if cross-tenant leak via a code path, roll back or enable read-only mode.
3. **Preserve evidence:** snapshot logs, audit exports, DB snapshots, traces before changes that destroy evidence.

**Diagnosis:** reproduce with test tenants; identify code path (missing tenant filter? cache key? presign? AI prompt assembly?); determine window and affected tenants/records using audit and access logs.

**Mitigation/Recovery:** fix + regression test (BOLA/RLS case); rotate credentials; invalidate caches; re-verify audit chain.

**Validation:** tenant-isolation test suite green; targeted tests for the exploited path; monitoring for recurrence.

**Escalation/Communication:** Founder + legal counsel; notify affected organizations and regulators per contractual and jurisdictional obligations and timelines; coordinated disclosure with reporters.

**Post-incident:** full postmortem; threat model update ([threat-model](../../security/threat-model.md)); risk register update.
