# Object Storage Layout

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) + DevOps |
| Blueprint | BP §6, §9.1, §14.2 |

## Buckets

| Bucket | Contents | Versioning / lock | Encryption | Lifecycle |
|---|---|---|---|---|
| `clerk-{env}-sources` | Originals, renditions, extraction payloads | Versioning **on** for `originals/` (or object lock where provider supports) | SSE (KMS in production) | Abort incomplete multipart after 7 d; tenant retention via workflow |
| `clerk-{env}-exports` | Rendered PDF/DOCX | Off | SSE | Expire exports after 30 d (re-renderable) unless pinned by approval policy |
| `clerk-{env}-ai-payloads` | Optional raw AI request/response | Off | SSE | Hard max 30 d |
| `clerk-{env}-platform` | Module packages, evaluation datasets, templates assets | Versioning on | SSE | None |
| `clerk-{env}-quarantine` | Quarantined uploads | Versioning on | SSE | 30 d then delete; access limited to security role |

Separate buckets allow distinct IAM policies, lifecycle rules and encryption keys.

## Key Layout (tenant-prefixed)

```text
sources bucket:
  org/{organization_id}/uploads/{upload_session_id}/{part...}          # in-progress multipart
  org/{organization_id}/sources/{source_id}/original/{file_id}         # immutable original
  org/{organization_id}/sources/{source_id}/renditions/{kind}/{file_id}
  org/{organization_id}/sources/{source_id}/extractions/{extraction_id}/payload.json
exports bucket:
  org/{organization_id}/artifacts/{artifact_id}/versions/{version_id}/exports/{export_id}.{pdf|docx}
ai-payloads bucket:
  org/{organization_id}/ai/{yyyy}/{mm}/{ai_execution_id}/{request|response}.json.zst
platform bucket:
  modules/{module_key}/{version}/package.tar.zst
  evaluation/{dataset_key}/{version}/...
quarantine bucket:
  org/{organization_id}/{source_id}/{file_id}
```

Rules:
- Keys never contain user-provided filenames (filenames stored in PostgreSQL; `Content-Disposition` set on download).
- Keys are generated server-side only.
- Application credentials are scoped to buckets; presigned URLs scoped to one key, method, content-length range and content-type, TTL ≤ 15 min for PUT and ≤ 5 min for GET (provisional).
- Downloads are always via presigned GET after authorization; buckets are private, no public ACLs.

## Checksums

SHA-256 computed by client (declared) and verified server-side after upload (object `x-amz-checksum-sha256` where supported, otherwise a verification activity streams and hashes). Mismatch → upload `failed`.
