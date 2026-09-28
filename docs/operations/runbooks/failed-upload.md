# Runbook: Failed Upload / Source Processing

| Field | Value |
|---|---|
| Status | Accepted |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | SRE + Sources domain |

**Symptoms:** users cannot upload; sessions expire; checksum mismatches; sources stuck `scanning`; extraction failures.

**Detection:** upload completion failure alert; scan engine alerts; extraction failure rate.

**Immediate actions:** determine scope (one user/device/network vs global); check storage and scanner health.

**Diagnosis:**
| Signal | Likely cause |
|---|---|
| Presigned PUT 403 | Clock skew, header mismatch (content-type/checksum), expired URL |
| Checksum mismatch | Client bug, proxy modification, partial upload |
| Stuck `scanning` | Scanner unavailable (never bypassed) |
| Extraction failures for one format | Parser bug/corrupt files; provider OCR/ASR issue |
| Mobile-only failures | Network interruptions → verify resume path |

**Mitigation:** fix scanner; roll back client release; increase URL TTL temporarily (security review); re-run extractions for affected sources (admin backfill).

**Recovery/Validation:** success rates back to target; affected users' queued captures synced.

**Escalation:** Security if malware detections involved.

**Post-incident:** add failing file types to fixtures.
