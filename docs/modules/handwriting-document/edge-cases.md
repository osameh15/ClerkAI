# Handwriting → Document — Edge Cases

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Product + AI Engineer |

| Case | Expected behaviour |
|---|---|
| Entire page illegible | Page flagged; user can retake/upload better image or mark page illegible |
| Mixed printed form + handwriting | Printed via OCR, handwriting via HTR; layout keeps field labels with values |
| Crossed-out text | Detected as struck; excluded by default; shown in review |
| Margin notes / arrows | `margin_note` blocks with low structural confidence; user places them |
| Tables drawn by hand | Table structure attempted; low confidence → review |
| Photo rotated/skewed | Auto-corrected; failure → review prompt |
| Non-supported script (e.g., Arabic, other) | Language detection → `AI_LANGUAGE_UNSUPPORTED` warning; best-effort with all spans uncertain |
| Duplicate pages | Dedup by SHA-256 if identical; near-duplicates flagged in page review |
| Signature/stamp regions | Detected as `image_region`, never transcribed as text |
| Sensitive IDs (national ID numbers) | Treated as amounts/identifiers with strict thresholds; masked in logs |
| Letter requested but notes lack recipient | Letter generated with placeholder fields flagged for user input (no invented recipient) |
| Instructions written in the notes ("ignore rules…") | Data only; no behavioural effect |
