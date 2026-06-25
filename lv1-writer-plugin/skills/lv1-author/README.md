# lv1-writer (skill)

The writing pipeline at the heart of the **lv1-writer plugin**: a disciplined,
research-backed flow that won't ship prose unless every factual claim traces to a real,
checked source. This skill is the **orchestrator** — it runs the pipeline and delegates
the two heaviest stations to isolated subagents.

## Install

This skill ships inside the `lv1-writer` plugin. Install the whole plugin — don't upload
this skill folder on its own (the init/review sibling skills and the research/inspect
subagents live elsewhere in the plugin, outside this folder).

- **Claude Cowork (desktop app):** load the plugin zip directly — "Upload local plugin"
  and point it at `lv1-writer-plugin.zip` (the zip whose root contains
  `.claude-plugin/plugin.json`, `skills/`, and `agents/`). No marketplace needed.
- **Claude Code (optional):** `claude --plugin-dir ./lv1-writer` (unzipped folder) or
  `claude --plugin-dir lv1-writer-plugin.zip`.

Plugins run in **Cowork** (and Claude Code), not in plain claude.ai chat.

## Entry points — all skills, no slash commands

This plugin uses the current Anthropic plugin format: every entry point is a
**skill** (`skills/<name>/SKILL.md`) that fires on its own natural-language trigger
phrases. There is **no `commands/` directory** (that older format still works but is
legacy). There are exactly two dedicated entry-point skills; everything else is handled
by the main `lv1-writer` skill auto-triggering on a described task.

- **`lv1-writer-init`** — set up the project (`sources/`, `manuscript/`, default tone,
  `CLAUDE.md`). Triggers on "lv1-writer init", "set up lv1-writer", or a fresh project
  with no `sources/library.md`. Safe to re-run.
- **`lv1-writer-review`** — independently review an existing document: generate a
  document-specific checklist, then review against it and write `review-feedback.md`.
  Triggers on "review this doc with lv1-writer", "fact-check this document", etc.
- **Everything else** — "draft a chapter on…", "suggest a title", "file this PDF into the
  library", "tighten the intro" — just say it. The main `lv1-writer` skill recognizes when
  it applies (even without the words "lv1-writer") and triage routes the job. There is
  deliberately no skill per task type: the task space is open-ended, and a model-invoked
  skill already covers it.

## Use, inside a Cowork project

```
lv1-writer init
```
Creates `sources/library.md` and `manuscript/_about.md` if they don't exist yet, scans
`sources/` and adds any files found there to the library (this part re-runs every time),
drops a default tone instruction at `manuscript/writing-instruction.md` the first time
(edit that file directly for a different tone — init won't overwrite it again), and updates
the project's `CLAUDE.md` with the current folder layout. Safe to run again later.

```
draft a 2,000-word chapter on the fall of the Western Roman economy
```
Runs the task through the full pipeline (or the fast lane for something small) and delivers
a sourced, inspected draft with its confidence list.

```
review manuscript/ch01.md with lv1-writer
```
Generates a checklist tailored to that document, then reviews it and writes
`review-checklist.md` + `review-feedback.md` beside it.

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
- **Entry-point skills**: `../lv1-writer-init/SKILL.md` and `../lv1-writer-review/SKILL.md`
  are thin skills that hand off to this orchestrator / the inspect subagent.
- **Review template**: `assets/ai-review-checklist-template.md` is the universal checklist
  that review mode specializes per document.

## The CLAUDE.md it writes

`lv1-writer init` writes a `CLAUDE.md` into the project folder. Cowork reads a project's
root `CLAUDE.md` as ordinary folder instructions — it isn't part of how the plugin is
installed or triggered; it's a courtesy so the project stays self-documenting even outside
a conversation where this skill is active.