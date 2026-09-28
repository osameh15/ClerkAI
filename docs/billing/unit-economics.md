# Unit Economics

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready (targets Provisional) |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Founder + AI Engineer |
| Blueprint | BP §5.4, §2.1 (Gross Margin after AI), G1 (Unit Economics model v0) |

## Principle (BP §5.4)

For self-service customers, direct AI + storage + compute + delivery cost should preferably remain **below 20–25% of account revenue** (initial target). Every Job exposes attributable cost so Cost per Module, Cost per Artifact and Gross Margin by Plan can be measured.

## Cost Components per Job

| Component | Source |
|---|---|
| AI input/output tokens | `usage_events` (`llm.*`) cost |
| Audio seconds | `audio.transcription` cost |
| Vision/image usage | `vision` cost |
| Document pages (OCR) | `ocr` cost |
| Storage | `storage` byte-hours × storage unit cost (allocated to job via its sources, pro-rated) |
| Compute | Worker compute allocated by activity duration × pool unit cost (estimated from infra cost / total activity-seconds) |
| Rendering | `artifact.export` × unit cost |
| Provider charges | As recorded per AIExecution |
| Delivery | Egress/CDN allocated (estimate) |

## Analyses Supported

| Analysis | Query basis |
|---|---|
| Cost per Job | Σ cost by `job_id` |
| Cost per Artifact | Job cost allocated to artifacts (by generation step costs; shared extraction cost split evenly) |
| Cost per Module | Σ by `module_key` |
| Cost per Organization | Σ by org |
| Gross margin by plan | (Revenue − Σ org costs) grouped by plan |
| Gross margin by module | Revenue attribution (plan share + add-on revenue) vs module cost |
| Cost per accepted artifact | Job costs ÷ artifacts reaching approved/final (BP §11.4 business metric) |

## G1 Deliverable

Unit Economics model v0 spreadsheet/notebook using provider price lists and assumed usage per module; replaced by actual data after 4–6 weeks of beta ([OD-021](../governance/open-decisions.md#od-021)).
