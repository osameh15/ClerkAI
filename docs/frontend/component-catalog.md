# Component Catalog

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead |
| Blueprint | BP §20.2, App. F ("Base Design System components exist for ModuleCard, SourceUploader, Processing, and ArtifactEditor") |

| Component (BP §20.2) | Purpose | Key props / data | States | Needed by |
|---|---|---|---|---|
| ModuleCard | Launcher card | `ModuleCard` DTO | available, locked, disabled, offline | Sprint 1 |
| ModuleBadge / PublisherBadge | Module/publisher identity, verified mark | key, publisher | — | Store |
| SourceUploader | Multi-file resumable upload | slot, accept, limits | idle, uploading (per file), paused, failed, done, offline-queued | Sprint 1 |
| AudioRecorder | Record audio (and video variant) | slot, max duration | idle, recording, paused, saving, error (permission) | G11 |
| CameraCapture | Photo/document capture | mode (photo/scan), grouping | preview, captured, retake, permission denied | G11 |
| SourcePreview | Preview any source kind | source DTO, file URL | loading, unavailable, quarantined | Sprint 1 |
| ProcessingTimeline | Stage timeline via SSE | job id | streaming, polling fallback, failed, completed | Sprint 1 |
| ArtifactEditor | Structured editor | artifact + schema + ETag | editing, saving, conflict, read-only | Sprint 1 (read), G10 (edit) |
| EvidenceViewer | Source at pointer | evidence[] | loading, unavailable | G9 |
| ConfidenceBadge | Band indicator | band, calibrated flag | high (hidden), medium, low | G9 |
| ReviewItem | Resolve one item | ReviewItem DTO | open, resolving, resolved, conflict | G9 |
| ApprovalTimeline | Workflow stages/decisions | WorkflowInstance | active, completed, rejected | G10 |
| ActionEditor | Edit action items | ActionItem | open, overdue, resolved | G10 |
| ExportDialog | Choose format/template/version | artifact, templates, entitlements | idle, rendering, ready, failed, not entitled | G10 |
| UsageMeter | Quota visualisation | meter DTO | normal, warning (80%), exceeded | G12 |
| EntitlementGate | Commercial gate | entitlement key | allowed, upgrade | Sprint 1 |
| PermissionGate | Permission gate | permission key | allowed, hidden, disabled | Sprint 1 |
| SchemaForm (platform) | JSON Schema + x-ui forms | schema, uiSchema, value | valid, invalid, readonly | Sprint 1 |
| SyncStatusChip (platform) | Offline sync status | queue stats | synced, pending, uploading, failed | G11 |

Each component: Storybook-style stories for all states in fa (RTL) and en (LTR), visual regression tests ([frontend-testing](../testing/frontend-testing.md)).
