# Product Boundaries

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Baseline |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product |
| Blueprint | BP §1.2, §27 |

## Purpose

Where Clerk stops. Used to reject scope creep in PDR reviews.

## Boundary Rules

| Area | Inside Clerk | Outside Clerk (integrate or refuse) |
|---|---|---|
| Documents | Generating, reviewing, approving, versioning, rendering and exporting artifacts derived from Sources. | General document management/DMS, arbitrary file sharing, wiki. |
| Tasks | Action Items extracted from artifacts: owner, deadline, status (Open/Resolved/Overdue), link to source artifact and evidence. | Sprints, boards, dependencies, time tracking, portfolio management (Jira/ClickUp replacement). |
| People & companies | Participants, responsible parties as Knowledge entities; organization members. | CRM pipelines, contact management, sales. |
| Projects | Optional grouping context for Jobs, Artifacts and Actions (for example a construction project). | Project scheduling, budgeting, procurement, ERP. |
| AI interaction | Capability requests inside module workflows; structured extraction; grounded generation. | Open-ended chatbot as the primary interface; user-selected models. |
| Speech/OCR | As steps in the pipeline feeding Knowledge. | Selling raw transcription/OCR as the product. |
| Integrations | Webhooks, scoped API keys, public REST API; future connectors. | Becoming an integration platform (iPaaS). |
| Code execution | Declarative modules; official native steps. | Arbitrary third-party code in the main process (v1). |

## Test for New Features

A feature is inside the boundary if it strengthens at least one link of the chain **Sources → Understanding → Knowledge → Evidence → Artifact → Workflow** and can be delivered as a platform capability or a module without domain logic in Core.

## Related Documents

[non-goals](non-goals.md), [product-principles](../governance/product-principles.md)
