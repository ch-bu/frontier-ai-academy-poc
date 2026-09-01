# frontier-ai-academy-poc

A small, throwaway proof of concept to test the assumptions behind
[ADR-0006](https://github.com/appliedAI-Initiative/academy-sprint-planning) of the
`academy-sprint-planning` project: *"Learning Engine MVP is delivered via a shared
Claude Project, not a hosted web app."*

This is a personal spike, not the real Academy repo. It exists to answer one question:
**does the shared-Claude-Project delivery mechanism actually work the way ADR-0006
describes, end to end?**

## What this tests

Mapped to ADR-0006's decision points:

1. **Source of truth** — lesson content lives here as plain, agent-agnostic markdown
   (`content/lessons/`), separate from any runtime.
2. **Runtime surface** — no web app. A shared Claude Project is the whole lesson
   experience: instructions, practice, and feedback in one conversation, no mid-lesson
   handoff.
3. **Front door** — a minimal static GitHub Pages page (`docs/index.html`) that explains
   the lesson and links into the Project. No interactivity, no API calls.
4. **Practice UI** — a small, fixed set of artifact templates (`templates/`), one per
   learning-journey state (instructions / practice / feedback), populated live from this
   repo's current content each time.
5. **State and privacy** — any learner progress persists only in the artifact's own
   browser localStorage, never sent anywhere, *and* is visible to the learner inside the
   artifact itself (a "Your progress" strip on every template), not only via browser
   developer tools.
6. **Bilingual delivery** — one Project, not two. Language is a conversational
   preference; Claude picks the matching DE/EN source file.

## What's in here

- `content/lessons/` — two short, independent lesson modules on effective prompting,
  each with parallel `en.md` / `de.md` files (instructions, practice exercise, feedback
  criteria).
- `templates/` — three fixed HTML artifact templates (instructions, practice, feedback)
  with placeholder tokens for Claude to fill in from the current lesson content.
- `docs/index.html` — the GitHub Pages front door. **You need to edit the Project link
  in this file** once you've created the Claude Project (see setup below).
- `PROJECT_INSTRUCTIONS.md` — the custom instructions to paste into the Claude Project.
  This is the one piece that can't be scripted — Claude Projects are set up by hand in
  the Claude Team workspace UI.

## Setup (manual, one-time)

1. In your appliedAI Claude Team workspace, create a new Project (e.g. "Academy Lesson
   POC").
2. Paste the contents of `PROJECT_INSTRUCTIONS.md` into the Project's custom
   instructions.
3. Share the Project with yourself/whoever else is testing.
4. Copy the Project's share link into `docs/index.html` (replace the placeholder href),
   commit, and push — GitHub Pages picks it up automatically.
5. Enable GitHub Pages on this repo if it isn't already (Settings → Pages → Source:
   `main` / `docs`).

## Testing checklist

Open the Pages front door, click into the Project, and check:

- [ ] The lesson runs start-to-finish (instructions → practice → feedback) in one
      conversation, no separate window.
- [ ] Asking for the lesson in German vs. English renders the matching DE/EN content.
- [ ] Editing a lesson file on `main` and re-opening the Project shows the new content,
      with no redeploy step.
- [ ] Practice answers/progress survive a page reload (localStorage) but are not visible
      to Claude or anywhere server-side on a fresh conversation.
- [ ] Each template's "Your progress" strip shows completed/in-progress state for
      Instructions, Practice, and Feedback, and updates live as you interact.
- [ ] The two lessons can be started independently — no forced order.
- [ ] Language preference is remembered and shown per template (`poc-academy-language`)
      — but confirm this is *display only*: a brand-new conversation still asks which
      language you want, since Claude can't read localStorage.

## Known gaps / not tested here

- No real S5-04 wireframes or CI file exist yet, so the templates use placeholder
  styling, not the eventual visual design.
- Only two short lessons — doesn't test content at scale or the full modern-AI-stack
  path structure.
- Doesn't test external-learner access (ADR-0006 explicitly scopes that out too).
- Doesn't test adding a colleague to the Team workspace/Project.

This repo is not open-source-ready and not linked from the real Academy repo. Delete or
keep private once the assumptions are validated.
