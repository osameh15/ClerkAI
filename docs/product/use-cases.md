# Use Cases

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §2, §3, App. A |

## Platform Use Cases

| ID | Use case | Actor | Outcome | Owning spec |
|---|---|---|---|---|
| UC-P01 | Create workspace (personal or organization) and set locale, timezone, initial policy | New user | Organization with Owner membership | [organizations](../platform/organizations/organizations.md) |
| UC-P02 | Invite members, assign roles and teams | Admin | Active memberships | [memberships](../platform/organizations/memberships.md) |
| UC-P03 | Discover and install a module from the Store | Admin | Installation enabled, permissions registered | [module-installation](../modules/module-installation.md) |
| UC-P04 | Configure module (defaults, approval chain, template) | Admin | Validated configuration version | [module-configuration](../modules/module-configuration.md) |
| UC-P05 | Capture Sources into Inbox without choosing a module (Quick Clerk) | Member | Reusable Sources | [source-model](../ingestion/source-model.md) |
| UC-P06 | Create Job from module card, attach Sources, submit | Member | Job queued with pinned versions | [job-domain](../jobs/job-domain.md) |
| UC-P07 | Watch processing, cancel, retry | Member | Visible progress; safe cancel/retry | [processing-status-ui](../frontend/processing-status-ui.md) |
| UC-P08 | Resolve review items with Evidence | Member/Manager | Knowledge confirmed/modified/rejected | [review](../platform/collaboration/review.md) |
| UC-P09 | Edit Artifact with optimistic concurrency | Member | New ArtifactVersion | [artifact-editor](../frontend/artifact-editor.md) |
| UC-P10 | Submit for approval; approve / request changes | Manager | Approved/Final Artifact | [approval](../platform/collaboration/approval.md) |
| UC-P11 | Export PDF/DOCX with organization template and numbering | Member | Export file | [rendering](../artifacts/rendering.md) |
| UC-P12 | Track Action Items (owner, deadline, status) | Member/Manager | Updated Actions | [action-items](../platform/actions/action-items.md) |
| UC-P13 | View usage, quotas and plan; upgrade | Owner | Subscription change | [billing-ui](../frontend/billing-ui.md) |
| UC-P14 | Configure AI policy, training consent and retention | Owner/Admin | Data usage policy | [enterprise-data-controls](../privacy/enterprise-data-controls.md) |
| UC-P15 | Query and export audit log | Admin/IT | Audit evidence | [audit-model](../platform/audit/audit-model.md) |
| UC-P16 | Create API key / webhook endpoint | Admin | Integration configured | [integration-architecture](../integrations/integration-architecture.md) |
| UC-P17 | Export or delete organization/user data | Owner/User | Asynchronous export/deletion with status | [export](../privacy/export.md), [deletion](../privacy/deletion.md) |

## Module Use Cases

| ID | Module | Use case |
|---|---|---|
| UC-M01 | Meeting Minutes | Record or upload meeting, add agenda and handwritten notes, obtain formal minutes with decisions, actions, owners, deadlines, risks and open questions. |
| UC-M02 | Meeting Minutes | Generate follow-up summary and task list from the same Knowledge. |
| UC-S01 | Site Report | Create visit in a project; capture photo+voice observations offline; sync; get grouped observations with location/category/severity/owner/deadline. |
| UC-S02 | Site Report | Next visit: repeat inspection of open items with before/after evidence; mark Resolved/Overdue. |
| UC-H01 | Handwriting | Upload multi-page handwritten notes; review uncertain spans; choose output: clean document, formal letter, minutes or task list; produce more than one from the same Source. |
| UC-L01 | Lecture Summary | Upload/record lecture plus slides/PDF; obtain summary, concepts, definitions, examples, questions; generate flashcards and study notes. |

Detailed flows: each module's `workflow.md`, and [information-architecture](../frontend/information-architecture.md) for screens.
