# Migration Order

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) |
| Blueprint | App. E |

## Purpose

Initial logical migration order. Baseline is BP App. E; deviations are marked **[D]** with justification; refinements that add tables the Blueprint lists elsewhere (§7.3) are marked **[R]**.

## Order

| # | Name | Tables | Notes |
|---|---|---|---|
| 000 | platform_infrastructure **[D]** | extensions (`citext`, `vector`), schemas, roles/grants scaffolding, `outbox_events`, `consumer_inbox`, `consumer_dead_letters`, `idempotency_keys` | **Deviation:** App. E places `outbox_events` in 015. Organization creation (002) and membership changes already emit events and require idempotency, so the infrastructure must exist first. No domain semantics change. |
| 001 | identity | `users`, `auth_identities`, `sessions`, `mfa_factors` **[R]**, `magic_link_tokens` **[R]** | |
| 002 | organizations | `organizations` | |
| 003 | memberships | `memberships`, `invitations` | |
| 004 | teams | `teams`, `team_members` | |
| 005 | access | `permissions`, `roles`, `role_permissions`, `membership_roles`, `resource_grants` **[R]** | Seeds system roles + platform permissions |
| 006 | projects | `projects`, `project_members` | |
| 007 | commerce | `plans`, `plan_versions`, `entitlement_definitions`, `plan_entitlements`, `organization_entitlements`, `subscriptions`, `subscription_addons` **[R]** | Realises App. E "entitlements" ([entity-catalog](entity-catalog.md)) |
| 008 | publishers | `publishers` | Seeds `clerk-official` |
| 009 | modules | `modules`, `catalog_listings` | |
| 010 | module_versions | `module_versions`, `module_dependencies` | |
| 011 | module_installations | `module_installations`, `module_assignments`, `module_configurations` **[R]**, `module_operations` **[R]** | |
| 012 | sources | `sources` | |
| 013 | source_files | `source_files`, `upload_sessions`, `source_extractions` **[R]**, `source_chunks` **[R]**, `chunk_embeddings` **[R]** | App. E omits extractions/chunks though §7.3 lists them |
| 014 | jobs | `jobs`, `job_sources` | |
| 015 | processing | `processing_runs`, `processing_steps` | `outbox_events` moved to 000 **[D]** |
| 016 | ai_registry | `ai_providers`, `ai_models`, `model_profiles`, `model_profile_routes` **[R]** | |
| 017 | prompts | `prompts`, `prompt_versions` | |
| 018 | ai_executions | `ai_executions` | |
| 019 | knowledge | `knowledge_entities`, `knowledge_relations` | |
| 020 | evidence | `evidence_links`, `review_items` | |
| 021 | artifacts | `artifacts`, `artifact_versions`, `artifact_exports` | |
| 022 | templates | `brand_kits`, `templates`, `template_versions`, `numbering_sequences` | |
| 023 | workflows | `workflow_definitions`, `workflow_versions`, `workflow_instances` | |
| 024 | approvals | `approval_requests`, `approval_decisions` | |
| 025 | actions | `action_items`, `action_item_history` | App. E "assignments/comments as needed" — comments moved to 030 |
| 026 | usage | `usage_events`, `usage_aggregates`, `quota_counters` **[R]** | |
| 027 | audit_privacy | `audit_events`, `retention_policies`, `data_usage_policies` | |
| 028 | evaluation | `evaluation_datasets`, `evaluation_cases`, `evaluation_runs`, `evaluation_results` | |
| 029 | training | `training_examples` | |
| 030 | collaboration **[R]** | `comments`, `mentions`, `shares` | §7.3 lists `comments` |
| 031 | notifications **[R]** | `notifications`, `notification_preferences` | §7.3 lists `notifications` |
| 032 | integrations **[R]** | `api_keys`, `webhook_endpoints`, `webhook_deliveries` | §7.3 lists webhook tables; App. B lists API keys |
| 033 | privacy_operations **[R]** | `consent_records`, `data_export_requests`, `data_deletion_requests`, `vendor_registry` | BP §15.3 |
| 034 | billing_provider **[R]** | `billing_customers`, `invoices`, `payment_webhook_events` | EPIC-22 |
| 035 | feature_flags **[R]** | `feature_flags` | Depends on [OD-017](../governance/open-decisions.md#od-017) |

## Cross-Migration Foreign Keys

Some FKs point "forward" (for example `jobs.project_id` → `projects` is backward-safe; `audit_events` has no FKs by design). Where an earlier table needs a FK to a later table (for example `artifacts.template_id` → `templates` in 022 after `artifacts` in 021), the FK is added in the later migration. The ordering above keeps such cases minimal: `artifacts.template_id` FK added in 022; `jobs.workflow_version_id` FK added in 023.

## Sprint Mapping

- Sprint 0: 000–005 (+ 008–011 skeletons for module registry) — [sprint-0](../execution/sprint-0.md).
- Sprint 1: 012–015, 019–021, 026–027 minimal — [sprint-1](../execution/sprint-1.md).
