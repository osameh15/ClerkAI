# Context Fusion

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Knowledge domain |
| Blueprint | BP §0 ("verifiable knowledge layer from multiple sources"), §3.1 ("Fusion of agenda and handwritten notes with the audio stream"), §3.2 (linking voice to photo), §3.4 (slides + audio) |

## Purpose

Combine content and extracted Knowledge from multiple Sources of one Job into one consistent Knowledge graph.

## Fusion Stages

| Stage | Description | Implemented by |
|---|---|---|
| 1. Alignment | Relate sources temporally/structurally: slide ↔ transcript segments; photo ↔ voice note by capture time/location/grouping window; agenda items ↔ discussion segments | Platform step `knowledge.fuse` with module-configured alignment strategies (`temporal`, `semantic`, `capture_group`) |
| 2. Joint extraction | Prompts receive aligned context (for example agenda item + related transcript chunks + note chunks) and extract entities with citations to any of them | `ai.extract` steps |
| 3. Entity resolution | Merge duplicates across sources; canonicalise ([entity-model](entity-model.md#canonicalization)) | Platform |
| 4. Conflict detection | Same attribute with different values from different sources → `CONTRADICTS` + Review Item `conflicting_sources`; resolution preference rules configurable (for example spoken decision overrides agenda proposal) but never silent | Platform + module rules |
| 5. Confidence composition | Agreement boosts, conflicts penalise ([confidence-model](confidence-model.md)) | Platform |

## Alignment Strategies

| Strategy | Signals | Example |
|---|---|---|
| `temporal` | Media timestamps, frame slide-change detection | Lecture slides ↔ audio |
| `capture_group` | `captured_at` proximity (configurable window), GPS proximity, explicit grouping in capture UI | Site photo + voice note → one Observation |
| `semantic` | Embedding similarity + LLM verification | Agenda item ↔ discussion; handwritten notes ↔ meeting segments |
| `explicit` | User links in UI | "This voice note describes this photo" |

Explicit user linking always overrides automatic alignment.

## Output

Fused entities with union evidence across sources; alignment records stored as relations (`RELATES_TO` with `attributes.alignment`), so the Evidence Viewer can show "discussed at 00:12:30 and written on notes page 2".
