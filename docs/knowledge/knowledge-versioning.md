# Knowledge Versioning

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Knowledge domain |
| Blueprint | BP §1.1 (Version everything), §10.2 |

## Model

- Knowledge is revisioned per item: each entity/relation row has `revision` and `supersedes_id`. The **current** item is the latest non-superseded revision.
- A new revision is created on: human modification, fusion merge, regeneration in a new run (reprocess), or evidence change that alters confidence materially.
- Old revisions remain for provenance and evaluation until job deletion/retention.

## Runs and Knowledge

- A new ProcessingRun (reprocess) creates new AI revisions; items already `human_confirmed`/`human_modified` are **not** overwritten by AI — conflicts between new AI output and human-confirmed values create Review Items instead.
- ArtifactVersions record a hash of the knowledge revision set they were generated from (`knowledge_revision_ids_hash`), enabling "knowledge changed since this version" indicators.

## Schema Evolution

Entity attributes carry `schema_ref`. Module upgrades may introduce new attribute schema versions; existing items keep their schema ref and are migrated only via declared module data transforms ([module-migrations](../modules/module-migrations.md)).
