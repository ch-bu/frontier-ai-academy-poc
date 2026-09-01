# Artifact templates

Three fixed HTML templates, one per learning-journey state. Claude fetches the
matching template and lesson content live from this repo (see
`../PROJECT_INSTRUCTIONS.md`) and fills in the placeholder tokens before rendering the
result as an artifact. The template structure and styling are fixed; only the
placeholder content changes per lesson/language.

No real visual design exists yet (ADR-0006 depends on S5-04 wireframes/CI that this POC
predates) — these are intentionally plain.

## Placeholder tokens

| Token | Meaning |
|---|---|
| `{{LESSON_ID}}` | e.g. `01-clear-specific-instructions` — used to namespace localStorage keys |
| `{{LESSON_TITLE}}` | The lesson title in the current language |
| `{{LANGUAGE}}` | `en` or `de` |
| `{{BODY_HTML}}` | The relevant markdown section, converted to simple HTML |

`practice-template.html` and `feedback-template.html` persist learner input to
`localStorage` only, under a key namespaced by `{{LESSON_ID}}` and `{{LANGUAGE}}` —
nothing is sent anywhere.
