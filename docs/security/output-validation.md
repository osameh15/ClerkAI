# Output Validation

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | AI Engineer + Frontend + Security |
| Blueprint | BP §14.3 ("Structured model output is validated and sanitized before use"; "Artifact renderers never execute raw HTML or URLs produced by the model") |

## Pipeline

1. **AI Gateway:** parse → schema → semantic checks → sanitisation ([schema-validation](../ai/schema-validation.md)).
2. **Knowledge write:** attribute schemas; citation integrity.
3. **Artifact content:** envelope + module schema; richText model only (no HTML); links https-only and optional; mention targets must exist.
4. **Frontend rendering:** Vue text interpolation only (no `v-html` on content); CSP blocks inline scripts; URL rendering through a safe-link component.
5. **Renderers:** template engine escapes all values; no script execution; no remote resource loading.
6. **Exports:** DOCX/PDF generated without active content (no macros, no JavaScript in PDF).

## Unicode Safety

Strip bidi override/isolate control characters (U+202A–U+202E, U+2066–U+2069) from model outputs except where legitimately needed (handled by renderer bidi algorithm instead), to prevent text-spoofing ("Trojan Source"-style) in documents.
