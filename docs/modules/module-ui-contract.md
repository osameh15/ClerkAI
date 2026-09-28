# Module UI Contract

| Field | Value |
|---|---|
| Status | Accepted |
| Spec class | Implementation-ready |
| Version | 1.0.0 |
| Last Updated | 2026-09-28 |
| Owner | Frontend Lead + Module Platform |
| Blueprint | BP §8 ("Module discovery and forms should be schema-driven"), §20, [ADR-009](../governance/adr/ADR-009-schema-driven-module-ui.md) |

## UI Modes

| Mode | Rendering | Allowed for |
|---|---|---|
| `generic` | 100% schema-driven: job creation, capture (per slot `capture`), review, artifact editor, configuration | Any module |
| `hybrid` | Generic + named extension points filled by platform-registered components | Official modules in v1 (launch modules use hybrid) |
| `custom` | Full custom flow using the Web SDK | Official modules only in v1 |

## How a Module Advertises UI Needs (without coupling Core to module internals)

1. **Schemas + UI hints**: `jobInput.uiSchema`, `configuration.uiSchema`, artifact schema `x-ui` annotations (widget, order, group, help, readOnly, visibility conditions).
2. **Labels**: `ui.labels` (localised nouns like "Meeting", "Site visit").
3. **Card**: icon, accent token, order.
4. **Extensions**: `ui.extensions[] { point, component, fallback }` — `component` is a key of a component compiled into the Nuxt app's component registry (for example `capture.mobile-observation`). If the component is unknown to the running frontend version, the `fallback` (`generic` or `hidden`) applies — so a module never breaks the shell.

## Extension Points (v1)

| Point | Purpose | Example component |
|---|---|---|
| `job.create` | Custom job setup | `capture.site-visit-setup`, `capture.multi-page-scan` |
| `job.capture` | Capture UX | `capture.meeting-recorder`, `capture.mobile-observation` |
| `job.review` | Specialised review | `review.speaker-resolver`, `review.observation-board`, `review.handwriting-spans` |
| `artifact.view` | Alternate artifact view | `artifact.flashcards-viewer`, `artifact.transcript-panel` |
| `artifact.edit` | Alternate editor | — |
| `artifact.section` | Section renderer inside generic editor | `artifact.before-after-compare`, `artifact.concept-map` |
| `settings.configuration` | Custom settings UI | — |
| `project.dashboard` | Project widget | `project.open-items-board` |

## Component Rules

Extension components: use only the Web SDK and platform stores; receive typed props (job/artifact IDs, schema, content); respect permission obligations; support RTL; handle offline states where the slot is `offline: true`. No remote JavaScript loading; third-party UI is Future and would require sandboxed iframes ([module-sandbox](../marketplace/module-sandbox.md)).

## UI Hint Vocabulary (`x-ui`)

`widget` (`text`, `textarea`, `richtext`, `date`, `jalali-date`, `select`, `multiselect`, `member-picker`, `location-picker`, `severity`, `table`, `image-gallery`, `audio-player`), `order`, `group`, `label`, `help`, `placeholder`, `readOnly`, `hidden`, `visibleIf` (JSON Logic subset), `evidence` (`show`/`hide`), `confidence` (`show`/`hide`).
