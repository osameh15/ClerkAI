# Error Catalog

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (initial; extended per epic) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | BP §8, §13, §14 |

Rules: [errors](errors.md). Adding a code requires a PR to this file; codes are never reused for different meanings.

## Authentication — `AUTH_*`

| Code | HTTP | Retryable | Meaning |
|---|---|---|---|
| `AUTH_REQUIRED` | 401 | no | Missing or invalid credentials |
| `AUTH_SESSION_EXPIRED` | 401 | no | Session idle/absolute timeout |
| `AUTH_MFA_REQUIRED` | 401 | no | Operation requires MFA-verified session |
| `AUTH_MAGIC_LINK_INVALID` | 401 | no | Token invalid, expired or used |
| `AUTH_OIDC_FAILED` | 401 | no | OIDC state/nonce/code validation failed |
| `AUTH_API_KEY_REVOKED` | 401 | no | Key revoked or expired |
| `AUTH_API_KEY_SCOPE_INSUFFICIENT` | 403 | no | Key scope does not allow operation |
| `AUTH_CSRF_INVALID` | 403 | no | Missing/invalid CSRF token or origin |

## Organization — `ORG_*`

| Code | HTTP | Meaning |
|---|---|---|
| `ORG_ACCESS_DENIED` | 403 | No active membership in requested organization |
| `ORG_HEADER_REQUIRED` | 400 | `Clerk-Organization-Id` missing for tenant-scoped op |
| `ORG_SLUG_TAKEN` | 409 | Slug already used |
| `ORG_SUSPENDED` | 403 | Organization suspended (billing/policy) |
| `ORG_MEMBER_ALREADY_EXISTS` | 409 | Invitation/membership exists |
| `ORG_SEAT_LIMIT_REACHED` | 402 | No seats available on plan |
| `ORG_LAST_OWNER` | 409 | Cannot remove/demote the last Owner |
| `ORG_INVITATION_EXPIRED` | 410 | Invitation expired or revoked |

## Access — `ACCESS_*`

| Code | HTTP | Meaning |
|---|---|---|
| `ACCESS_PERMISSION_DENIED` | 403 | Role lacks permission (`meta.permission`) |
| `ACCESS_RESOURCE_DENIED` | 403 | Resource policy denies (visible resource) |
| `ACCESS_ROLE_IN_USE` | 409 | Role assigned; cannot delete |
| `ACCESS_SYSTEM_ROLE_IMMUTABLE` | 409 | System roles cannot be modified |
| `ACCESS_PERMISSION_UNKNOWN` | 422 | Unknown permission key in role definition |

## Modules — `MODULE_*`

| Code | HTTP | Meaning |
|---|---|---|
| `MODULE_NOT_FOUND` | 404 | Unknown module key |
| `MODULE_NOT_ENTITLED` | 402 | Organization not entitled (`meta.required_entitlement`) |
| `MODULE_NOT_INSTALLED` | 409 | Operation requires installation |
| `MODULE_ALREADY_INSTALLED` | 409 | Install on existing non-uninstalled installation |
| `MODULE_DISABLED` | 409 | New Jobs blocked because installation is disabled |
| `MODULE_SUSPENDED` | 409 | Installation suspended (`meta.reason`) |
| `MODULE_VERSION_INCOMPATIBLE` | 422 | Version incompatible with platform |
| `MODULE_DEPENDENCY_UNSATISFIED` | 422 | Required dependency missing/incompatible (`details`) |
| `MODULE_OPERATION_IN_PROGRESS` | 409 | Another install/upgrade/uninstall running |
| `MODULE_CONFIGURATION_INVALID` | 422 | Configuration fails schema (`details`) |
| `MODULE_MANIFEST_INVALID` | 422 | Manifest validation failed (registry/admin) |
| `MODULE_VERSION_YANKED` | 422 | Version withdrawn |

## Uploads and Sources — `UPLOAD_*`, `SOURCE_*`

| Code | HTTP | Meaning |
|---|---|---|
| `UPLOAD_TOO_LARGE` | 413 | Exceeds plan/module size limit (`meta.max_bytes`) |
| `UPLOAD_TYPE_NOT_ALLOWED` | 415 | Declared or sniffed type not accepted |
| `UPLOAD_SESSION_EXPIRED` | 410 | Session expired |
| `UPLOAD_INCOMPLETE` | 409 | Object/parts missing at completion |
| `UPLOAD_CHECKSUM_MISMATCH` | 422 | SHA-256 mismatch |
| `SOURCE_NOT_FOUND` | 404 | Not found/visible |
| `SOURCE_QUARANTINED` | 409 | Malware/policy quarantine |
| `SOURCE_NOT_READY` | 409 | Required extraction not available yet |
| `SOURCE_IN_USE` | 409 | Deletion blocked by retention/legal hold or active processing |
| `SOURCE_PROCESSING_FAILED` | 422 | Extraction failed (`meta.extraction_kind`) |

## Jobs — `JOB_*`

| Code | HTTP | Meaning |
|---|---|---|
| `JOB_NOT_FOUND` | 404 | Not found/visible |
| `JOB_INVALID_TRANSITION` | 409 | Command not allowed in current status (`meta.status`) |
| `JOB_NOT_READY` | 422 | Readiness rules unmet (`meta.missing`) |
| `JOB_SOURCE_NOT_ACCEPTED` | 422 | Source kind not accepted by any module slot |
| `JOB_SOURCE_LIMIT_EXCEEDED` | 422 | Module input limits exceeded |
| `JOB_CONCURRENCY_LIMIT` | 429 | Concurrent job limit reached (entitlement) |
| `JOB_RETRY_NOT_ALLOWED` | 409 | Not failed, or retry budget exhausted |
| `JOB_PROCESSING_FAILED` | — | Failure code carried in Job.failure (not an HTTP error) |

## Knowledge — `KNOWLEDGE_*`

| Code | HTTP | Meaning |
|---|---|---|
| `KNOWLEDGE_REVIEW_ITEM_ALREADY_RESOLVED` | 409 | Concurrent resolution |
| `KNOWLEDGE_VALUE_INVALID` | 422 | Edited value fails schema |

## AI — `AI_*` (mostly surfaced as Job failure codes)

| Code | HTTP | Meaning |
|---|---|---|
| `AI_PROVIDER_UNAVAILABLE` | 503 / job failure | All routes unavailable |
| `AI_OUTPUT_INVALID` | job failure | Schema-invalid after repair/fallback budget |
| `AI_POLICY_BLOCKED` | 422 / job failure | Tenant AI policy forbids required route (`meta.reason`) |
| `AI_BUDGET_EXCEEDED` | 402 | Organization AI budget hard limit |
| `AI_LANGUAGE_UNSUPPORTED` | 422 / job failure | No profile route supports language |
| `AI_CONTENT_REJECTED` | job failure | Provider safety refusal |

## Artifacts, Templates, Workflow — `ARTIFACT_*`, `TEMPLATE_*`, `WORKFLOW_*`

| Code | HTTP | Meaning |
|---|---|---|
| `ARTIFACT_NOT_FOUND` | 404 | Not found/visible |
| `ARTIFACT_VERSION_CONFLICT` | 412 | `If-Match` stale (`meta.current_etag`) |
| `ARTIFACT_PRECONDITION_REQUIRED` | 428 | `If-Match` missing |
| `ARTIFACT_CONTENT_INVALID` | 422 | Content fails module artifact schema |
| `ARTIFACT_LOCKED` | 409 | Final or in review; edit not allowed |
| `ARTIFACT_UNRESOLVED_REVIEW_ITEMS` | 422 | Blocking review items prevent submission/finalisation |
| `ARTIFACT_EXPORT_FORMAT_NOT_ENTITLED` | 402 | Export format not entitled |
| `ARTIFACT_EXPORT_FAILED` | job failure | Rendering failed |
| `TEMPLATE_INVALID` | 422 | Template definition invalid |
| `WORKFLOW_NOT_CONFIGURED` | 422 | No approval workflow configured |
| `WORKFLOW_NOT_APPROVER` | 403 | Caller not an approver for this request |
| `WORKFLOW_REQUEST_CLOSED` | 409 | Already decided/superseded |
| `WORKFLOW_VERSION_MISMATCH` | 409 | Decision targets a superseded version |

## Actions — `ACTION_*`

| Code | HTTP | Meaning |
|---|---|---|
| `ACTION_NOT_FOUND` | 404 | |
| `ACTION_INVALID_TRANSITION` | 409 | |

## Billing — `BILLING_*`

| Code | HTTP | Meaning |
|---|---|---|
| `BILLING_ENTITLEMENT_REQUIRED` | 402 | Feature entitlement missing (`meta.required_entitlement`) |
| `BILLING_QUOTA_EXCEEDED` | 402 | Hard quota reached (`meta.meter`) |
| `BILLING_SUBSCRIPTION_INACTIVE` | 402 | Subscription cancelled/expired |
| `BILLING_PAYMENT_REQUIRED` | 402 | Past due beyond grace |

## Integrations — `INTEGRATION_*`

| Code | HTTP | Meaning |
|---|---|---|
| `INTEGRATION_WEBHOOK_URL_INVALID` | 422 | Non-HTTPS, private address, or disallowed host |
| `INTEGRATION_EVENT_TYPE_UNKNOWN` | 422 | Unknown event type |

## Security / Privacy — `SECURITY_*`, `PRIVACY_*`

| Code | HTTP | Meaning |
|---|---|---|
| `SECURITY_REQUEST_BLOCKED` | 403 | Blocked by security policy (WAF rule, suspicious pattern) |
| `SECURITY_REAUTH_REQUIRED` | 401 | Sensitive operation requires recent authentication |
| `PRIVACY_LEGAL_HOLD` | 409 | Deletion blocked by legal hold |
| `PRIVACY_REQUEST_IN_PROGRESS` | 409 | Export/deletion already running |

## Generic — `VALIDATION_*`, `IDEMPOTENCY_*`, `RATE_*`, `SYSTEM_*`

| Code | HTTP | Retryable | Meaning |
|---|---|---|---|
| `VALIDATION_FAILED` | 422 | no | Schema/semantic validation (`details`) |
| `VALIDATION_MALFORMED_JSON` | 400 | no | Unparseable body |
| `VALIDATION_UNKNOWN_PARAMETER` | 400 | no | Unknown query parameter |
| `VALIDATION_CURSOR_INVALID` | 400 | no | Cursor tampered or filter mismatch |
| `IDEMPOTENCY_KEY_REQUIRED` | 400 | no | Missing key |
| `IDEMPOTENCY_KEY_REUSED` | 422 | no | Different payload with same key |
| `IDEMPOTENCY_REQUEST_IN_PROGRESS` | 409 | yes | Concurrent duplicate |
| `RATE_LIMITED` | 429 | yes | Rate limit (`meta.scope`) |
| `SYSTEM_UNAVAILABLE` | 503 | yes | Dependency unavailable |
| `SYSTEM_READ_ONLY` | 503 | yes | Maintenance/failover |
| `SYSTEM_INTERNAL_ERROR` | 500 | yes | Unexpected error |
