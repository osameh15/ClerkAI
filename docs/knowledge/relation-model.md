# Knowledge Relation Model

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Knowledge domain |
| Blueprint | BP §10 (example: `RESPONSIBLE_FOR`, `DUE_AT`) |

## Definition

A Relation is a typed, directed edge `from_entity —relation_type→ to_entity` with optional attributes, confidence, review state, origin and evidence.

## Platform Relation Types

| Type | From → To | Meaning |
|---|---|---|
| `RESPONSIBLE_FOR` | person/organization → action | Ownership |
| `DUE_AT` | action → deadline | Deadline |
| `DECIDED_BY` | decision → person | Decision maker(s) |
| `RAISED_BY` | risk/question/proposal → person | Origin |
| `RELATES_TO` | any → any | Generic association |
| `RESULTS_IN` | decision → action | Causal link |
| `LOCATED_AT` | observation/issue → location | Place |
| `PART_OF` | any → document_section/topic | Containment |
| `SUPERSEDES` | any → same type | Newer fact replaces older (cross-source) |
| `CONTRADICTS` | any → same type | Conflicting facts (drives review) |

Modules register additional types (uppercase snake case) in `knowledge.relationTypes` with allowed from/to subtypes and cardinality ([manifest-reference](../modules/sdk/manifest-reference.md)); the platform validates cardinality at write time.

## Rules

- Relations to rejected entities are ignored in artifacts.
- Cardinality violation (for example two `DUE_AT` on one action) → keep both, create `conflicting_sources` Review Item.
- Relations are revisioned the same way as entities ([knowledge-versioning](knowledge-versioning.md)).
