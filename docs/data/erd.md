# Entity-Relationship Diagrams

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (logical) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Tech Lead (Go) / Data |
| Blueprint | BP §7.1, §7.3, App. E |

## Purpose

Logical ERD split by area. Diagrams show keys and relationships only; column-level details, tenancy, immutability and retention are in the [entity-catalog](entity-catalog.md). Every tenant table also carries `organization_id` (shown as `organization_id` FK where it is the primary relationship; always present per BP §7.2).

Cross-context relationships are **ID references** (no cross-context ORM navigation); foreign-key constraints are still declared in the database for integrity while the monolith shares one database ([future-service-extraction](../architecture/future-service-extraction.md)).

## 1. Identity, Organizations, Access, Projects

```mermaid
erDiagram
  users ||--o{ auth_identities : "authenticates via"
  users ||--o{ sessions : "has"
  users ||--o{ mfa_factors : "enrolls"
  users ||--o{ memberships : "holds"
  organizations ||--o{ memberships : "has"
  organizations ||--o{ invitations : "issues"
  organizations ||--o{ teams : "has"
  teams ||--o{ team_members : "has"
  memberships ||--o{ team_members : "joins"
  organizations ||--o{ roles : "defines custom"
  roles ||--o{ role_permissions : "grants"
  permissions ||--o{ role_permissions : "granted in"
  memberships ||--o{ membership_roles : "assigned"
  roles ||--o{ membership_roles : "assigned as"
  organizations ||--o{ resource_grants : "scopes"
  organizations ||--o{ projects : "owns"
  projects ||--o{ project_members : "has"
  memberships ||--o{ project_members : "participates"

  users {
    uuid id PK
    citext email UK
    text status
  }
  organizations {
    uuid id PK
    text slug UK
    text kind "personal or team"
    text data_region
  }
  memberships {
    uuid id PK
    uuid organization_id FK
    uuid user_id FK
    text status "unique active per org and user"
  }
  roles {
    uuid id PK
    uuid organization_id FK "null for system roles"
    text key
  }
  permissions {
    text key PK
    text source "platform or module"
  }
  membership_roles {
    uuid membership_id FK
    uuid role_id FK
    text scope_type
    uuid scope_id
  }
  projects {
    uuid id PK
    uuid organization_id FK
    text code
  }
```

## 2. Commerce

```mermaid
erDiagram
  plans ||--o{ plan_versions : "versioned as"
  plan_versions ||--o{ plan_entitlements : "includes"
  entitlement_definitions ||--o{ plan_entitlements : "valued in"
  entitlement_definitions ||--o{ organization_entitlements : "overridden in"
  organizations ||--o{ subscriptions : "subscribes"
  plan_versions ||--o{ subscriptions : "priced by"
  subscriptions ||--o{ subscription_addons : "adds"
  organizations ||--o{ organization_entitlements : "granted"
  organizations ||--o{ usage_events : "consumes"
  organizations ||--o{ usage_aggregates : "summarised"
  organizations ||--o{ quota_counters : "limited by"

  plan_versions {
    uuid id PK
    uuid plan_id FK
    int version "unique per plan"
    jsonb rate_card
  }
  subscriptions {
    uuid id PK
    uuid organization_id FK
    uuid plan_version_id FK
    text status "one active per org"
  }
  entitlement_definitions {
    text key PK
    text value_type
  }
  organization_entitlements {
    uuid id PK
    uuid organization_id FK
    text entitlement_key FK
    text source
  }
  usage_events {
    bigint id PK
    uuid event_id UK
    uuid organization_id FK
    text meter
    numeric quantity
    bigint cost_micros
    uuid job_id "attribution"
    uuid ai_execution_id "attribution"
  }
```

## 3. Module Platform and Marketplace

```mermaid
erDiagram
  publishers ||--o{ modules : "publishes"
  modules ||--|| catalog_listings : "listed as"
  modules ||--o{ module_versions : "released as"
  module_versions ||--o{ module_dependencies : "requires"
  organizations ||--o{ module_installations : "installs"
  modules ||--o{ module_installations : "installed as"
  module_versions ||--o{ module_installations : "pinned by"
  module_installations ||--o{ module_configurations : "configured by"
  module_installations ||--o{ module_assignments : "assigned to"
  module_installations ||--o{ module_operations : "changed by"

  modules {
    uuid id PK
    text key UK "e.g. business.meeting-minutes"
    uuid publisher_id FK
  }
  module_versions {
    uuid id PK
    uuid module_id FK
    text version "semver, unique per module"
    jsonb manifest
    text status
  }
  module_installations {
    uuid id PK
    uuid organization_id FK
    uuid module_id FK
    uuid module_version_id FK
    text status "one non-uninstalled per org and module"
  }
  module_configurations {
    uuid id PK
    uuid installation_id FK
    int version
    jsonb config
  }
```

## 4. Sources, Jobs, Processing, AI

```mermaid
erDiagram
  organizations ||--o{ upload_sessions : "opens"
  upload_sessions |o--o| sources : "completes into"
  organizations ||--o{ sources : "owns"
  sources ||--o{ source_files : "stored as"
  sources ||--o{ source_extractions : "extracted into"
  source_extractions ||--o{ source_chunks : "chunked into"
  source_chunks ||--o| chunk_embeddings : "embedded as"
  sources |o--o{ sources : "replaced by"
  organizations ||--o{ jobs : "runs"
  module_installations ||--o{ jobs : "executes"
  module_versions ||--o{ jobs : "pinned for"
  jobs ||--o{ job_sources : "uses"
  sources ||--o{ job_sources : "attached to"
  jobs ||--o{ processing_runs : "attempted by"
  processing_runs ||--o{ processing_steps : "consists of"
  processing_steps ||--o{ ai_executions : "calls"
  ai_providers ||--o{ ai_models : "offers"
  model_profiles ||--o{ model_profile_routes : "routes"
  ai_models ||--o{ model_profile_routes : "target of"
  prompts ||--o{ prompt_versions : "released as"
  prompt_versions ||--o{ ai_executions : "used by"
  ai_models ||--o{ ai_executions : "executed on"

  sources {
    uuid id PK
    uuid organization_id FK
    text kind
    text sha256 "tenant-level dedup"
    uuid replaces_source_id FK
  }
  source_extractions {
    uuid id PK
    uuid source_id FK
    text kind
    text pipeline_version
  }
  source_chunks {
    uuid id PK
    uuid extraction_id FK
    int seq
    jsonb locator
  }
  jobs {
    uuid id PK
    uuid organization_id FK
    text module_key
    uuid module_version_id FK
    text status
  }
  processing_runs {
    uuid id PK
    uuid job_id FK
    int run_no "unique per job"
    text temporal_workflow_id
  }
  ai_executions {
    uuid id PK
    uuid organization_id FK
    uuid processing_run_id FK
    uuid prompt_version_id FK
    uuid model_id FK
    int attempt_no
    uuid fallback_of_execution_id
  }
```

## 5. Knowledge, Artifacts, Templates, Workflow, Actions

```mermaid
erDiagram
  jobs ||--o{ knowledge_entities : "yields"
  knowledge_entities ||--o{ knowledge_relations : "from"
  knowledge_entities ||--o{ knowledge_relations : "to"
  knowledge_entities ||--o{ evidence_links : "supported by"
  source_chunks ||--o{ evidence_links : "cited by"
  jobs ||--o{ review_items : "requires"
  jobs ||--o{ artifacts : "produces"
  artifacts ||--o{ artifact_versions : "versioned as"
  artifact_versions ||--o{ artifact_exports : "rendered as"
  templates ||--o{ template_versions : "versioned as"
  template_versions ||--o{ artifact_exports : "lays out"
  organizations ||--o{ brand_kits : "brands"
  organizations ||--o{ numbering_sequences : "numbers"
  workflow_definitions ||--o{ workflow_versions : "versioned as"
  workflow_versions ||--o{ workflow_instances : "instantiated as"
  artifact_versions ||--o{ workflow_instances : "subject of"
  workflow_instances ||--o{ approval_requests : "requests"
  approval_requests ||--o{ approval_decisions : "decided by"
  knowledge_entities ||--o{ action_items : "tracked as"
  artifacts ||--o{ action_items : "references"
  action_items ||--o{ action_item_history : "changes"

  knowledge_entities {
    uuid id PK
    uuid job_id FK
    text entity_type
    text review_state
    numeric confidence
  }
  evidence_links {
    uuid id PK
    text target_type
    uuid target_id
    uuid source_id FK
    uuid chunk_id FK
    jsonb pointer
  }
  artifacts {
    uuid id PK
    uuid job_id FK
    text status
    uuid current_version_id FK
  }
  artifact_versions {
    uuid id PK
    uuid artifact_id FK
    int version_no "unique per artifact"
    text origin "ai, human, system"
    jsonb content
  }
  workflow_instances {
    uuid id PK
    text subject_type
    uuid subject_id
    uuid subject_version_id
    text status
  }
  action_items {
    uuid id PK
    uuid knowledge_entity_id FK
    uuid artifact_id FK
    text status
    date due_date
  }
```

## 6. Governance, Evaluation, Integration

```mermaid
erDiagram
  organizations ||--o{ audit_events : "audited"
  organizations ||--o{ retention_policies : "retains by"
  organizations ||--o{ data_usage_policies : "governs AI use by"
  organizations ||--o{ consent_records : "consents"
  consent_records ||--o{ training_examples : "permits"
  evaluation_datasets ||--o{ evaluation_cases : "contains"
  evaluation_datasets ||--o{ evaluation_runs : "evaluated by"
  evaluation_runs ||--o{ evaluation_results : "produces"
  evaluation_cases ||--o{ evaluation_results : "scored in"
  organizations ||--o{ api_keys : "issues"
  organizations ||--o{ webhook_endpoints : "registers"
  webhook_endpoints ||--o{ webhook_deliveries : "receives"

  audit_events {
    bigint id PK
    uuid event_uuid UK
    uuid organization_id FK
    text actor_type
    text action
    text resource_type
  }
  training_examples {
    uuid id PK
    uuid organization_id FK
    uuid consent_record_id FK
    text status
  }
  evaluation_runs {
    uuid id PK
    uuid dataset_id FK
    jsonb subject
    text gate_result
  }
```
