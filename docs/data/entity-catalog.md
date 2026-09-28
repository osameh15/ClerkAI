# Entity Catalog

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (logical model) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) / Data |
| Blueprint | BP §7.2, §7.3, App. E |

## Purpose

Canonical logical model: every table, its owning bounded context, tenancy, keys, constraints, immutability, deletion, retention and volume class. Physical DDL is written in migrations following this catalog ([migration-order](migration-order.md)). The table-ownership lint ([dependency-rules](../architecture/dependency-rules.md)) reads the **Owner** column.

## Legend

| Column | Values |
|---|---|
| Tenancy | `T` tenant-owned (`organization_id NOT NULL`), `P` platform-scoped (no tenant), `U` user-scoped (identity), `T?` nullable org (platform rows + tenant rows) |
| RLS | `Y` RLS enforced (`FORCE`), `N` no RLS (platform/user-scoped, accessed only via owning context) |
| Imm. | `I` immutable after insert, `A` append-only (insert only), `M` mutable |
| Del. | `S` soft delete (`deleted_at`), `H` hard delete (via retention/deletion workflow), `N` never deleted by application, `L` lifecycle by status only |
| Vol. | `L` low, `M` medium, `H` high-volume (partitioning candidate) |

Common columns (omitted below unless special): `id UUID PK` (UUIDv7, [identifier-strategy](identifier-strategy.md)), `created_at timestamptz NOT NULL DEFAULT now()`, `updated_at` for mutable tables, `row_version BIGINT` for optimistically-locked tables, `organization_id UUID NOT NULL` for `T` tables.

## Identity (owner: `identity`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `users` | U | N | M | S | L | `email citext UNIQUE` (active), `display_name`, `locale`, `timezone`, `status ∈ {active, disabled, deleted}`, `email_verified_at` |
| `auth_identities` | U | N | M | H | L | `user_id FK`, `provider`, `subject`; `UNIQUE(provider, subject)` |
| `sessions` | U | N | M | H | M | `user_id FK`, `token_hash UNIQUE`, `expires_at`, `last_seen_at`, `revoked_at`, `mfa_level`, `ip_prefix`, `ua_hash` |
| `mfa_factors` | U | N | M | H | L | `user_id FK`, `type ∈ {totp, webauthn}`, `secret_encrypted` (envelope-encrypted) |
| `magic_link_tokens` | U | N | M | H | M | `email`, `token_hash UNIQUE`, `expires_at`, `consumed_at` |

## Organizations (owner: `organization`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `organizations` | T (self: `id`) | Y (`id = app.org_id`) | M | L (`pending_deletion` → `deleted`) | L | `slug UNIQUE`, `name`, `kind ∈ {personal, team}`, `status`, `locale`, `timezone`, `data_region`, `is_internal`, `settings JSONB` |
| `memberships` | T | Y | M | L | L | `user_id FK users`, `status ∈ {invited, active, deactivated, removed}`; partial `UNIQUE(organization_id, user_id) WHERE status IN ('invited','active')` (BP App. E) |
| `invitations` | T | Y | M | L | L | `email`, `token_hash UNIQUE`, `role_ids UUID[]`, `expires_at`, `status ∈ {pending, accepted, revoked, expired}` |
| `teams` | T | Y | M | S | L | `name`; `UNIQUE(organization_id, lower(name)) WHERE deleted_at IS NULL` |
| `team_members` | T | Y | M | H | L | `PK(team_id, membership_id)`, `team_role ∈ {member, lead}` |

Note on RLS for `organizations` and `memberships`: listing "my organizations" before a tenant is selected uses a `SECURITY DEFINER` function `org.list_memberships_for_user(user_id)` owned by the migration role, returning only minimal columns. See [tenant-data-isolation](tenant-data-isolation.md).

## Access (owner: `access`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `permissions` | P | N | M | L (`deprecated_at`) | L | `key TEXT PK` (namespaced, [permission-namespaces](../platform/access/permission-namespaces.md)), `source ∈ {platform, module}`, `module_key`, `description` |
| `roles` | T? | Y (rows with null org visible to all) | M | S | L | `key`, `name`, `is_system`, `based_on_role_id`; `UNIQUE(organization_id, key)`; system roles have `organization_id NULL` |
| `role_permissions` | T? | Y | M | H | L | `PK(role_id, permission_key)` |
| `membership_roles` | T | Y | M | H | L | `membership_id`, `role_id`, `scope_type ∈ {organization, team, project}`, `scope_id`; `UNIQUE(membership_id, role_id, scope_type, scope_id)` |
| `resource_grants` | T | Y | M | H | M | `resource_type`, `resource_id`, `grantee_type ∈ {membership, team}`, `grantee_id`, `access ∈ {view, comment, edit}`, `expires_at` (Guest and per-resource access) |

## Projects (owner: `project`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `projects` | T | Y | M | S (archive first) | L | `name`, `code`, `status ∈ {active, archived}`, `metadata JSONB`; `UNIQUE(organization_id, code)` |
| `project_members` | T | Y | M | H | L | `PK(project_id, membership_id)`, `project_role` (string key; default roles `project_manager`, `member`, `viewer`) |

## Commerce (owner: `commerce`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `plans` | P | N | M | L | L | `key UNIQUE` (`personal`, `team`, `business`, `enterprise`), `status` |
| `plan_versions` | P | N | I (after `active`) | N | L | `plan_id`, `version INT`, `status ∈ {draft, active, retired}`, `currency`, `prices JSONB`, `included_usage JSONB`, `rate_card JSONB` (raw meter → billable unit conversion), `effective_from`; `UNIQUE(plan_id, version)` |
| `entitlement_definitions` | P | N | M | L | L | `key TEXT PK`, `value_type ∈ {boolean, limit, quota, enum}`, `unit`, `module_key` |
| `plan_entitlements` | P | N | I | N | L | `PK(plan_version_id, entitlement_key)`, `value JSONB` |
| `subscriptions` | T | Y | M | L | L | `plan_version_id`, `status ∈ {trialing, active, past_due, cancelled, expired}`, `seats`, `current_period_start/end`, `provider`, `provider_ref`; partial `UNIQUE(organization_id) WHERE status IN ('trialing','active','past_due')` (BP App. E) |
| `subscription_addons` | T | Y | M | L | L | `subscription_id`, `addon_key` (for example `pack.construction`), `quantity`, `status` |
| `organization_entitlements` | T | Y | M | L (`ends_at`) | L | `entitlement_key`, `value JSONB`, `source ∈ {addon, contract, override, trial, promotion}`, `reason`, `starts_at`, `ends_at`, `created_by` |
| `usage_events` | T | Y | A | N (retention by partition) | H | internal `id BIGINT identity`, `event_id UUID UNIQUE`, `meter`, `quantity NUMERIC`, `unit`, `occurred_at`, attribution (`job_id`, `processing_run_id`, `ai_execution_id`, `artifact_id`, `source_id`, `module_key`, `module_version`), `provider_key`, `model_key`, `cost_micros BIGINT`, `cost_currency`, `billable BOOLEAN` ([billing-failure-cases](../billing/billing-failure-cases.md)), `estimated BOOLEAN`, `corrects_event_id`, `idempotency_key UNIQUE` |
| `usage_aggregates` | T | Y | M (recomputable) | N | M | `PK(organization_id, meter, granularity, period_start)`, `quantity`, `cost_micros`, `billable_units` |
| `quota_counters` | T | Y | M | N | M | `PK(organization_id, meter, period_start)`, `used`, `reserved` (PostgreSQL reconciliation of Redis counters) |
| `billing_customers` | T | Y | M | L | L | `provider`, `provider_customer_ref`, `billing_email`, `tax_region` |
| `invoices` | T | Y | I (snapshot) | N | L | `provider_invoice_ref`, `period`, `total_minor`, `currency`, `status`, `lines JSONB` |
| `payment_webhook_events` | P | N | A | H (90 days) | M | `UNIQUE(provider, provider_event_id)` |

The Blueprint's single `entitlements` table (BP §7.3, App. E 007) is realised as `entitlement_definitions` + `plan_entitlements` + `organization_entitlements` (refinement, [source-of-truth-policy](../governance/source-of-truth-policy.md)).

## Marketplace (owner: `marketplace`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `publishers` | P | N | M | L | L | `key UNIQUE` (`clerk-official`), `type ∈ {official, partner, community}`, `verification_status`, `status ∈ {active, suspended}` |
| `catalog_listings` | P | N | M | L | L | `module_id UNIQUE`, `publisher_id`, `title JSONB` (localised), `summary`, `description_md`, `categories`, `pricing JSONB`, `visibility ∈ {public, private, hidden}`, `review_status ∈ {draft, in_review, approved, rejected, suspended}` |

## Modules (owner: `module`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `modules` | P | N | M | L | L | `key UNIQUE` (BP App. E), `publisher_id`, `category`, `status ∈ {active, deprecated, suspended, withdrawn}` |
| `module_versions` | P | N | I (after `published`) | N | L | `module_id`, `version TEXT` (SemVer), `manifest JSONB`, `manifest_sha256`, `package_uri`, `package_sha256`, `platform_compat`, `status ∈ {draft, published, deprecated, yanked, blocked}`, `published_at`; `UNIQUE(module_id, version)` (BP App. E) |
| `module_dependencies` | P | N | I | N | L | `module_version_id`, `depends_on_module_key`, `version_range`, `optional` |
| `module_installations` | T | Y | M | L | L | `module_id`, `module_version_id`, `status ∈ {enabled, disabled, suspended, uninstalled}`, `version_policy ∈ {pinned, auto_patch, auto_minor}`, `active_configuration_id`, `installed_by`, `suspension_reason`; partial `UNIQUE(organization_id, module_id) WHERE status <> 'uninstalled'`; partial index on `status='enabled'` (BP App. E) |
| `module_assignments` | T | Y | M | H | L | `installation_id`, `assignee_type ∈ {team, project, membership}`, `assignee_id` |
| `module_configurations` | T | Y | I | N | L | `installation_id`, `version INT`, `config JSONB`, `schema_ref`; `UNIQUE(installation_id, version)` |
| `module_operations` | T | Y | M | N | L | Install/upgrade/uninstall operation log: `installation_id`, `operation`, `from_version`, `to_version`, `status`, `error` |

## Sources (owner: `source`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `upload_sessions` | T | Y | M | H (expired > 7 d) | M | `created_by`, `status ∈ {created, uploading, completed, expired, aborted, failed}`, `filename`, `declared_mime`, `declared_size`, `declared_sha256`, `object_key`, `multipart_upload_id`, `part_size`, `expires_at`, `client_ref` (offline temp ID), `source_id` |
| `sources` | T | Y | I (content) / M (metadata) | S → H | M | `kind ∈ {audio, video, image, document, spreadsheet, presentation, text}`, `status` ([source-lifecycle](../architecture/source-lifecycle.md)), `title`, `original_filename`, `detected_mime`, `size_bytes`, `sha256`, `language_hint`, `captured_at`, `capture_location JSONB`, `project_id`, `created_by`, `replaces_source_id`, `retention_category`; index `(organization_id, sha256)` |
| `source_files` | T | Y | I | H | M | `source_id`, `role ∈ {original, rendition}`, `rendition_kind`, `object_key` (not unique: tenant-level dedup shares objects, reference-counted — [deduplication](../ingestion/deduplication.md)), `mime`, `size_bytes`, `sha256`, `width`, `height`, `duration_ms`, `page_count` |
| `source_extractions` | T | Y | A (status transitions only) | H | M | `source_id`, `kind`, `pipeline_key`, `pipeline_version`, `status ∈ {pending, running, succeeded, failed, superseded}`, `language`, `payload JSONB` (≤ 256 KB) or `payload_object_key`, `quality JSONB`; partial `UNIQUE(source_id, kind, pipeline_version) WHERE status <> 'failed'` |
| `source_chunks` | T | Y | I | H | H | `source_id`, `extraction_id`, `seq`, `chunk_kind ∈ {text_span, time_range, page_region, table, slide, frame}`, `text`, `locator JSONB`, `language`, `speaker_label`, `token_count`; `UNIQUE(extraction_id, seq)` |
| `chunk_embeddings` | T | Y | I | H | H | `chunk_id PK/FK`, `embedding_model`, `dims`, `embedding vector` ([vector-storage](vector-storage.md)) |

## Jobs and Processing (owners: `job`, `processing`)

| Table | Owner | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|---|
| `jobs` | job | T | Y | M | S | M | `module_key`, `module_id`, `installation_id`, `module_version_id` (pinned at submit), `workflow_version_id`, `configuration_id`, `project_id`, `title`, `status` ([job-state-machine](../jobs/job-state-machine.md)), `input JSONB`, `input_schema_ref`, `created_by`, `submitted_by`, `submitted_at`, `completed_at`, `failure_code`, `current_run_id`; indexes `(organization_id, created_at)`, `(organization_id, status)` (BP App. E) |
| `job_sources` | job | T | Y | M | H (draft only) | M | `PK(job_id, source_id)`, `role` (module input slot), `position`, `attached_by` |
| `processing_runs` | processing | T | Y | M | N | M | `job_id`, `run_no`, `trigger ∈ {submit, retry, reprocess, additional_artifact}`, `status ∈ {pending, running, waiting_review, succeeded, failed, cancelled}`, `module_version_id`, `workflow_definition_sha256`, `temporal_workflow_id`, `temporal_run_id`, `failure_code`; `UNIQUE(job_id, run_no)` |
| `processing_steps` | processing | T | Y | M | N | H | `run_id`, `step_key`, `step_type`, `attempt`, `status ∈ {pending, running, waiting, succeeded, failed, skipped, cancelled}`, `progress`, `error_code`, `output_ref JSONB`; `UNIQUE(run_id, step_key, attempt)` |

## AI (owner: `ai`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `ai_providers` | P | N | M | L | L | `key UNIQUE`, `status ∈ {active, degraded, disabled}`, `regions`, `vendor_registry_id`, `credential_ref` (secret manager reference, never the secret) |
| `ai_models` | P | N | M | L | L | `provider_id`, `key`, `capabilities TEXT[]`, `modalities`, `languages`, `context_window`, `pricing JSONB`, `limits JSONB`, `status ∈ {active, deprecated, disabled}`; `UNIQUE(provider_id, key)` |
| `model_profiles` | P | N | M (versioned by `version`) | L | L | `key UNIQUE` (for example `persian_transcription`), `capability`, `quality_tier`, `languages`, `modalities`, `version` |
| `model_profile_routes` | P | N | M | L | L | `profile_id`, `priority`, `model_id`, `conditions JSONB`, `max_cost_micros`, `timeout_ms`, `status` |
| `prompts` | P | N | M | L | L | `key UNIQUE` (for example `meeting.action-extraction`), `owner_module_key`, `status` |
| `prompt_versions` | P | N | I (after `candidate`) | N | L | `prompt_id`, `version` (SemVer), `template`, `input_schema JSONB`, `output_schema_ref`, `compatible_profiles`, `evaluation_dataset_id`, `status ∈ {draft, candidate, active, deprecated, retired}`, `content_sha256`, `released_by`; `UNIQUE(prompt_id, version)` |
| `ai_executions` | T | Y | A (single completion update) | N (payload refs expire) | H | see [ai-execution-model](../ai/ai-execution-model.md) |

## Evaluation and Training (owner: `evaluation`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `evaluation_datasets` | P | N | I (per version) | N | L | `key`, `version`, `module_key`, `purpose ∈ {golden, regression, benchmark}`, `consent_basis`, `storage_prefix`; `UNIQUE(key, version)` |
| `evaluation_cases` | P | N | I | N | M | `dataset_id`, `case_key`, `input_refs JSONB`, `expected JSONB`, `tags TEXT[]` |
| `evaluation_runs` | P | N | M | N | M | `dataset_id`, `subject JSONB` (prompt version, profile/model, module version), `trigger`, `status`, `summary_metrics JSONB`, `gate_result ∈ {pass, warn, fail}` |
| `evaluation_results` | P | N | A | N | H | `run_id`, `case_id`, `metrics JSONB`, `passed`, `failure_category`, `output_ref` |
| `training_examples` | T | Y | M (curation states) | H (on withdrawal) | M | `source_ref JSONB`, `status` ([training-example-lifecycle](../ai/training/training-example-lifecycle.md)), `consent_record_id`, `anonymization_status`, `reviewed_by` |

## Knowledge (owner: `knowledge`)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `knowledge_entities` | T | Y | M (revisioned) | H (with job) | H | `job_id`, `processing_run_id`, `entity_type`, `subtype`, `schema_ref`, `attributes JSONB`, `display_text`, `confidence NUMERIC(4,3)`, `review_state`, `origin ∈ {ai, human, system}`, `revision`, `supersedes_id`, `ai_execution_id`; index `(organization_id, job_id, entity_type)` |
| `knowledge_relations` | T | Y | M (revisioned) | H | H | `relation_type`, `from_entity_id`, `to_entity_id`, `attributes`, `confidence`, `review_state`, `origin` |
| `evidence_links` | T | Y | A (status updates only) | H | H | `target_type ∈ {entity, relation, artifact_field}`, `target_id`, `target_path`, `source_id`, `extraction_id`, `chunk_id`, `pointer_type`, `pointer JSONB`, `quote`, `support ∈ {supports, contradicts, context}`, `status ∈ {active, superseded, source_unavailable}` |
| `review_items` | T | Y | M | H | M | `job_id`, `target_type`, `target_id`, `target_path`, `reason`, `severity`, `status ∈ {open, resolved, dismissed, superseded}`, `resolution`, `assigned_to`, `resolved_by`; index `(organization_id, job_id, status)` |

## Artifacts and Templates (owners: `artifact`, `template`)

| Table | Owner | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|---|
| `artifacts` | artifact | T | Y | M | S | M | `job_id`, `project_id`, `module_key`, `artifact_type`, `title`, `status` ([approval-lifecycle](../artifacts/approval-lifecycle.md)), `current_version_id`, `final_version_id`, `document_number`, `template_id`, `language`; indexes `(organization_id, created_at)`, `(organization_id, status)` |
| `artifact_versions` | artifact | T | Y | I | H (with artifact) | H | `artifact_id`, `version_no`, `parent_version_id`, `origin`, `content JSONB`, `content_schema_ref`, `content_sha256`, `processing_run_id`, `provenance JSONB`, `created_by`; `UNIQUE(artifact_id, version_no)`; trigger blocks UPDATE/DELETE except by retention role |
| `artifact_exports` | artifact | T | Y | M (status) | H (expiry) | M | `artifact_version_id`, `format ∈ {pdf, docx}`, `template_version_id`, `brand_kit_id`, `status`, `object_key`, `sha256`, `expires_at` |
| `brand_kits` | template | T | Y | I (new row per version) | N | L | `version`, `logo_file_ref`, `fonts`, `colors`, `header`, `footer`, `signatures`, `is_current`; `UNIQUE(organization_id, version)` |
| `templates` | template | T? | Y | M | S | L | `key`, `artifact_type`, `module_key`, `status`, `current_version_id`; `UNIQUE(organization_id, key)`; null org = module/platform-provided |
| `template_versions` | template | T? | Y | I (after publish) | N | L | `template_id`, `version`, `engine`, `definition_ref`, `status ∈ {draft, published, retired}` |
| `numbering_sequences` | template | T | Y | M | N | L | `key`, `pattern`, `scope_key`, `next_value BIGINT`, `reset_policy ∈ {never, yearly}`; `UNIQUE(organization_id, key, scope_key)` |

## Workflow and Actions (owners: `workflow`, `action`)

| Table | Owner | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|---|
| `workflow_definitions` | workflow | T? | Y | M | L | L | `key`, `scope ∈ {platform, module, organization}`, `module_key`, `subject_type` |
| `workflow_versions` | workflow | T? | Y | I | N | L | `definition_id`, `version`, `definition JSONB`; `UNIQUE(definition_id, version)` |
| `workflow_instances` | workflow | T | Y | M | N | M | `workflow_version_id`, `subject_type`, `subject_id`, `subject_version_id`, `status`, `current_stage`, `temporal_workflow_id` |
| `approval_requests` | workflow | T | Y | M | N | M | `instance_id`, `stage_key`, `approver_type`, `approver_id`, `status`, `due_at`, `decided_at` |
| `approval_decisions` | workflow | T | Y | A | N | M | `request_id`, `instance_id`, `decision ∈ {approve, reject, request_changes}`, `comment`, `decided_by`, `signature JSONB` |
| `action_items` | action | T | Y | M | S | M | `job_id`, `artifact_id`, `knowledge_entity_id`, `project_id`, `title`, `owner_membership_id`, `owner_text`, `due_date`, `due_text_original`, `status ∈ {open, in_progress, resolved, cancelled}`, `is_overdue` (maintained), `is_orphaned` (source Knowledge removed), `severity`, `category`, `location JSONB`, `origin`; index `(organization_id, status, due_date)` |
| `action_item_history` | action | T | Y | A | N | M | `action_item_id`, `change JSONB`, `actor`, `evidence_ref` |

## Collaboration, Notifications, Integrations (owners: `collaboration`, `notification`, `integration`)

| Table | Owner | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|---|
| `comments` | collaboration | T | Y | M | S | M | `subject_type`, `subject_id`, `subject_path`, `body` (sanitised Markdown subset), `author_membership_id`, `parent_id` |
| `mentions` | collaboration | T | Y | I | H | M | `comment_id`, `membership_id` |
| `shares` | collaboration | T | Y | M | L (`revoked_at`) | L | `subject_type`, `subject_id`, `share_type ∈ {member, team, link}`, `grantee_id`, `access`, `token_hash`, `expires_at` |
| `notifications` | notification | T | Y | M (`read_at`) | H (90 d) | H | `recipient_membership_id`, `type`, `subject_ref`, `payload JSONB` (IDs only) |
| `notification_preferences` | notification | T | Y | M | H | L | `membership_id`, `type`, `channel`, `enabled` |
| `api_keys` | integration | T | Y | M | L (`revoked_at`) | L | `name`, `prefix`, `key_hash UNIQUE` (Argon2id or HMAC-SHA-256 with pepper), `scopes TEXT[]`, `expires_at`, `last_used_at` |
| `webhook_endpoints` | integration | T | Y | M | S | L | `url`, `event_types TEXT[]`, `secret_ref`, `status ∈ {active, disabled, failing}` |
| `webhook_deliveries` | integration | T | Y | M | H (30 d) | H | `endpoint_id`, `event_id`, `attempt`, `status`, `response_code`, `next_attempt_at`; `UNIQUE(endpoint_id, event_id, attempt)` |

## Governance: Audit and Privacy (owners: `audit`, `privacy`)

| Table | Owner | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|---|
| `audit_events` | audit | T? | Y | A | N (retention by partition drop, policy-controlled) | H | see [audit-model](../platform/audit/audit-model.md) |
| `retention_policies` | privacy | T | Y | M | N | L | `category`, `duration_days`, `action ∈ {delete, anonymize}`, `source`, `legal_hold`; `UNIQUE(organization_id, category)` |
| `data_usage_policies` | privacy | T | Y | I (new version per change) | N | L | `version`, `training_use_allowed` (default false), `evaluation_use_allowed` (default false), `store_ai_payloads`, `ai_payload_retention_days`, `provider_restrictions JSONB`, `human_review_defaults JSONB` |
| `consent_records` | privacy | T | Y | A | N | L | `subject_type`, `subject_id`, `consent_type`, `granted`, `policy_version`, `recorded_by`, `withdrawn_at` |
| `data_export_requests` | privacy | T | Y | M | H (after expiry) | L | `scope_type`, `scope_id`, `status`, `result_object_key`, `expires_at` |
| `data_deletion_requests` | privacy | T | Y | M | N | L | `scope_type`, `scope_id`, `status`, `report JSONB` |
| `vendor_registry` | privacy | P | N | M | L | L | `vendor_key`, `category`, `dpa_ref`, `regions`, `data_categories`, `last_review_at` (BP §15.3) |

## Platform Infrastructure (owner: `platform` kernel)

| Table | Ten. | RLS | Imm. | Del. | Vol. | Key columns and constraints |
|---|---|---|---|---|---|---|
| `outbox_events` | T? | N (dispatcher role only) | A (+`published_at`) | H (7 d after publish) | H | `id BIGINT identity`, `event_id UUID UNIQUE`, `event_type`, `version`, `organization_id`, `payload JSONB`, `occurred_at`, `published_at`, `attempts` |
| `consumer_inbox` | T? | N | A | H (30 d) | H | `PK(consumer, event_id)`, `processed_at` |
| `consumer_dead_letters` | T? | N | M | H (manual) | L | `consumer`, `event_id`, `error`, `attempts` |
| `idempotency_keys` | T? | Y (org rows) | M | H (`expires_at`) | M | see [idempotency-strategy](../architecture/idempotency-strategy.md) |
| `feature_flags` | P | N | M | L | L | depends on [OD-017](../governance/open-decisions.md#od-017) |

## Related Documents

[erd](erd.md), [schema-conventions](schema-conventions.md), [tenant-data-isolation](tenant-data-isolation.md), [migration-order](migration-order.md), [bounded-contexts](../architecture/bounded-contexts.md)
