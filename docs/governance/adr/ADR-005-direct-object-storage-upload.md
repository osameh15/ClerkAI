# ADR-005: File Uploads = Direct-to-Object-Storage

| Field | Value |
|---|---|
| Status | Accepted |
| Date | 2026-09 (Blueprint v1.0) |
| Deciders | Technical Leadership |
| Blueprint | BP §8, §9.1, §14.2, §28.1 |

## Context

Sources include multi-hour audio/video and large scans. Streaming these through the Go API wastes memory/bandwidth, couples API availability to transfer duration, and complicates resumability on mobile networks.

## Decision

1. Client requests an **upload session** (`POST /v1/uploads`) declaring filename, size, declared MIME, SHA-256 (optional for single part, required at completion), and part count.
2. API authorises (tenant, permission, entitlement, quota, size limit) and returns **short-lived presigned URLs** constrained to a single object key, content length and content type (multipart for large files).
3. Client uploads **directly to Object Storage**.
4. Client calls `POST /v1/uploads/{id}/complete`; API verifies object existence, size and checksum, creates the `Source` + original `SourceFile`, and schedules scanning/processing asynchronously.

The Go API never proxies file bytes, except for small (≤ configurable, for example 1 MB) text/manual notes submitted as JSON.

## Consequences

- Requires S3-compatible storage with presigned PUT and multipart ([ADR-015](ADR-015-s3-compatible-storage.md)).
- Uploaded objects are untrusted until MIME sniffing and malware scanning pass ([upload-security](../../security/upload-security.md)).
- Orphan objects from abandoned sessions are cleaned by lifecycle rules.

## Related

[upload-flow](../../ingestion/upload-flow.md), [api/uploads](../../api/uploads.md), [object-storage-layout](../../data/object-storage-layout.md)
