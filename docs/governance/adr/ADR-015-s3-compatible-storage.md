# ADR-015: Binary Storage = S3-Compatible Object Storage

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership, DevOps |
| Blueprint | BP §6, §9.1, §28.1 |

## Decision

Raw Sources, derived renditions, extraction payloads too large for PostgreSQL, rendered exports and evaluation assets are stored in **S3-compatible object storage** (MinIO locally). Access goes through a `BlobStore` port offering presign-put, presign-get, multipart, head, copy, delete, and tagging. The production provider is [OD-006](../open-decisions.md#od-006).

Required capabilities: presigned URLs with content constraints, multipart upload, server-side encryption, bucket lifecycle rules, and (for originals) versioning or write-once policy.

## Related

[object-storage-layout](../../data/object-storage-layout.md), [ADR-005](ADR-005-direct-object-storage-upload.md)
