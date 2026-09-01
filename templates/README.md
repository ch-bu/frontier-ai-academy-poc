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

All three templates read and write `localStorage` only, under keys namespaced by
`{{LESSON_ID}}` and `{{LANGUAGE}}` — nothing is sent anywhere:

- `instructions-template.html` sets `...-viewed` when opened.
- `practice-template.html` saves `...-answer` and `...-done`.
- `feedback-template.html` saves `...-feedback` (a JSON map of which self-check items
  are ticked).

All three also write a single global key, `poc-academy-language`, set to whichever
`{{LANGUAGE}}` they were rendered with — one shared preference across both lessons,
rather than per-lesson.

**Limitation:** Claude cannot read `localStorage` — it only exists in the learner's
browser, invisible to the model — so `poc-academy-language` cannot make Claude skip
asking a learner their language preference in a new conversation. It only lets a
template default/confirm its own language display if it were ever re-rendered without
Claude repopulating it. This is a constraint of the shared-Claude-Project architecture,
not something fixable at the template level.

Each template also renders a small "Your progress" strip built from these keys, so the
learner can see — inside the artifact itself — that their instructions, practice,
feedback, and language state is being kept, without needing to open browser developer
tools.
