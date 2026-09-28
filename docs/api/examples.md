# API Examples

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (illustrative) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |

IDs are shortened for readability. All requests are over HTTPS with a session cookie and CSRF token unless noted.

## 1. Create a Meeting Minutes Job End-to-End

```http
POST /v1/jobs
Clerk-Organization-Id: 0192f4a8-…-0001
Idempotency-Key: 5f0c2b7e-2d1f-4c1b-9e0e-9a7b1f3c2d10
Content-Type: application/json

{ "module_key": "business.meeting-minutes", "title": "Weekly coordination – Mehr 5" }
```

```http
HTTP/1.1 201 Created
ETag: "v1"

{ "id": "…job", "module_key": "business.meeting-minutes", "status": "draft",
  "readiness": { "ready": false, "missing": ["recording|notes|agenda"] }, "sources": [], … }
```

```http
POST /v1/uploads
Idempotency-Key: 8a1…
{ "filename": "meeting.m4a", "size_bytes": 48211200, "content_type": "audio/mp4",
  "multipart": false, "job_id": "…job", "job_slot": "recording" }
```

```http
HTTP/1.1 201 Created
{ "id": "…upl", "status": "created", "expires_at": "2026-09-29T10:00:00Z",
  "upload_url": "https://storage…/org/…/uploads/…?X-Amz-Signature=…",
  "required_headers": { "Content-Type": "audio/mp4", "x-amz-checksum-sha256": "<base64>" } }
```

Client PUTs the file directly to `upload_url`, then:

```http
POST /v1/uploads/…upl/complete
Idempotency-Key: 8a2…
{ "sha256": "3a7bd3e2360a3d29eea436fcfb7e44c735d117c42d1c1835420b6b9942dd4f1b" }
```

```http
HTTP/1.1 201 Created
{ "id": "…src", "kind": "audio", "status": "scanning", … }
```

The Job becomes `ready` (Source attached to slot `recording`). Submit:

```http
POST /v1/jobs/…job/submit
Idempotency-Key: 1c9…
```

```http
HTTP/1.1 202 Accepted
{ "id": "…job", "status": "queued", "module_version": "1.0.0",
  "current_run": { "run_no": 1, "trigger": "submit", "status": "pending" } }
```

## 2. Watch Progress

```http
GET /v1/jobs/…job/events
Accept: text/event-stream
```

```text
id: 1
event: job.status
data: {"type":"job.status","job_id":"…job","seq":1,"status":"processing","at":"…"}

id: 2
event: run.stage
data: {"type":"run.stage","job_id":"…job","seq":2,"stage":"transcribing","at":"…"}

id: 9
event: review.required
data: {"type":"review.required","job_id":"…job","seq":9,"at":"…"}
```

## 3. Resolve an Ambiguous Owner

```http
GET /v1/jobs/…job/review-items?status=open
```

```json
{ "data": [ { "id": "…ri1", "reason": "ambiguous_speaker", "severity": "blocking",
  "target_type": "entity", "target_id": "…act3", "target_path": "/attributes/owner",
  "current_value": null, "candidates": ["Reza Ahmadi", "Reza Karimi"],
  "evidence": [ { "pointer": { "type": "time_range", "start_ms": 2538000, "end_ms": 2551000 }, "support": "supports", … } ] } ] }
```

```http
POST /v1/review-items/…ri1/resolve
Idempotency-Key: 77d…
{ "action": "edit", "value": { "display_name": "Reza Ahmadi", "membership_id": "…m7" } }
```

## 4. Edit Artifact with Optimistic Concurrency

```http
PATCH /v1/artifacts/…art
If-Match: "v3"
{ "patch": [ { "op": "replace", "path": "/body/decisions/0/text", "value": [ { "text": "Approved revised budget." } ] } ],
  "change_summary": "Clarified decision wording" }
```

Stale version:

```http
HTTP/1.1 412 Precondition Failed
{ "error": { "code": "ARTIFACT_VERSION_CONFLICT", "message": "This document was changed by someone else.",
  "request_id": "…", "retryable": false, "meta": { "current_etag": "\"v4\"" } } }
```

## 5. Entitlement Denial

```http
POST /v1/jobs
{ "module_key": "construction.site-report" }
```

```http
HTTP/1.1 402 Payment Required
{ "error": { "code": "MODULE_NOT_ENTITLED", "message": "Your current plan does not include this module.",
  "request_id": "…", "retryable": false,
  "meta": { "required_entitlement": "module.construction.site-report", "upgrade_available": true } } }
```

## 6. Webhook Delivery

```http
POST https://customer.example/hooks/clerk
Clerk-Event-Id: 0192f4a8-…
Clerk-Event-Type: artifact.approved
Clerk-Timestamp: 1790000000
Clerk-Signature: v1=5d41402abc4b2a76b9719d911017c592…

{ "event_id": "0192f4a8-…", "event_type": "artifact.approved", "version": 1,
  "occurred_at": "2026-09-28T12:00:00Z", "organization_id": "…", "actor_id": "…",
  "correlation_id": "…", "causation_id": "…",
  "data": { "artifact_id": "…", "version_id": "…", "module_key": "business.meeting-minutes" } }
```
