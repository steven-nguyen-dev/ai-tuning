# lv1-author (skill)

The writing pipeline at the heart of the **lv1-writer plugin**: a disciplined,
research-backed flow that won't ship prose unless every factual claim traces to a real,
checked source. This skill is the **orchestrator** — it runs the pipeline and delegates
the two heaviest stations to isolated subagents.

## Install

This skill ships inside the `lv1-writer` plugin. Install the whole plugin — don't upload
this skill folder on its own (the init/review sibling skills and the research/inspect
subagents live elsewhere in the plugin, outside this folder).

- **Claude Cowork (desktop app):** "Upload local plugin" and point it at
  `lv1-writer-plugin.zip` (the zip whose root contains `.claude-plugin/plugin.json`,
  `core/`, `skills/`, and `agents/`). No marketplace needed.
- **Claude Code (optional):** `claude --plugin-dir ./lv1-writer-plugin` (unzipped) or
  `claude --plugin-dir lv1-writer-plugin.zip`.

Plugins run in **Cowork** (and Claude Code), not in plain claude.ai chat.

## Entry points — all skills, no slash commands

Every entry point is a **skill** that fires on its own natural-language trigger. There
are three dedicated skills in this plugin:

- **`lv1-writer-init`** — set up or re-sync the project (`sources/`, `manuscript/`,
  tone profile, `CLAUDE.md`). Triggers on "lv1-writer init", "set up lv1-writer", or
  a fresh project with no `sources/library.md`. Safe to re-run.
- **`lv1-reviewer`** — independently review an existing document: generate a
  document-specific checklist, then write a located verdict (`review-feedback.md`).
  Triggers on "review this doc", "fact-check this document", "critique my chapter".
- **Everything else** — "draft a chapter on…", "file this PDF into the library",
  "continue the manuscript from…" — just say it. `lv1-author` auto-triggers and triage
  routes the job.

## Use, inside a Cowork project

```
lv1-writer init
```
Scans the project first and pre-fills what it already knows. Asks only about what's
uncertain. Creates `sources/library.md`, `manuscript/_about.md`, and
`manuscript/writing-instruction.md` (seeded from the resolved tone profile). If tone
changes on a re-run, `writing-instruction.md` is regenerated from the new profile.
Updates the project's `CLAUDE.md` with topic, angle, POV, tone, and working mode.

```
draft a 2,000-word chapter on the fall of the Western Roman economy
```
Runs the task through the full pipeline (or the fast lane for something small) and
delivers a sourced, inspected draft with its confidence list.

```
review manuscript/ch01.md
```
Triggers `lv1-reviewer`. Generates a checklist tailored to that document, then reviews
it and writes `review-checklist.md` + `review-feedback.md` beside it.

## How the contract is laid out

Each station's contract lives in exactly one place — no duplication:

- **Inline stations** (run in the orchestrator's own context): `references/triage.md`,
  `references/outline.md`, `references/draft.md`, `references/source-intake.md`.
- **Support contracts** (read by the inline stations): `references/tone-detect.md`
  (genre→profile detection), `references/structure-shapes.md` (descriptive vs
  hypothesis-driven), `references/interview.md` (interview-driven / hybrid material
  gathering, batched rounds), `references/working-lessons.md` (read before drafting),
  `references/anti-tells.md` (the AI register to write out of; checked at inspection).
- **Tone profiles**: `assets/tone-profiles/*.md` — one voice spec per genre; triage picks
  one per task. The profile decides voice, structure, and whether source-grading applies
  (fiction switches it off for craft contracts). Editing a project's default voice means
  editing `manuscript/writing-instruction.md`; editing a genre's voice means editing its
  profile file.
- **Subagent stations** (run in their own isolated context): the research and inspection
  contracts live in `../../agents/lv1-research.md` and `../../agents/lv1-inspect.md` at
  the plugin root. The orchestrator delegates to them by name; it keeps no inline copy of
  those two contracts. Editing a research or inspection rule means editing the agent file.
- **Entry-point skills**: `../lv1-writer-init/SKILL.md` handles project setup;
  `../lv1-reviewer/SKILL.md` delegates to the inspect subagent in standalone review mode.
- **Review template**: `assets/ai-review-checklist-template.md` is the universal checklist
  that review mode specializes per document.

## The CLAUDE.md it writes

`lv1-writer init` writes a marker-bounded block into the project's root `CLAUDE.md`.
Cowork reads that file as ordinary folder instructions — it isn't part of how the plugin
installs or triggers; it's a courtesy so the project stays self-documenting between
conversations. Re-running init only touches the text between its
`<!-- lv1-writer:structure:start -->` and `<!-- lv1-writer:structure:end -->` markers,
and never deletes a source or user edit.
