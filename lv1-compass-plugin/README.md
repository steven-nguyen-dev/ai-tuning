# lv1-compass

An **assistant + advisor for work**, built on the lv1-writer discipline. Two skills share
one constitution and ship only work that is **impressive AND true**, with every fact
graded and traceable to a real source:

- **lv1-assistant** — the **maker**. Turns a work brief into a deliverable (analysis,
  report, memo, brief, slide deck, spreadsheet model) through `triage → research → outline
  → draft → inspect → ship`. Narrower than a general writing tool: work registers, not an
  open-ended genre/tone space.
- **lv1-advisor** — the **judge**. One skill, three modes: **review** an existing artifact,
  **challenge** a position with a sourced steelman, or **decide** a fork (it frames, weighs
  the options, recommends with a confidence grade, and leaves the choice with you).

The two roles share one engine — sourced, graded, steelman-first, independently checked —
and differ only in their terminal output: the assistant ends in a deliverable; the advisor
ends in a judgment you own.

> lv1-compass is a slim sibling of **lv1-writer**, not a replacement. lv1-writer stays for
> books and wide-genre long-form work; lv1-compass is tuned for work deliverables and adds
> the judge role lv1-writer doesn't have.

## Install

Install the whole plugin — don't upload a single skill folder on its own (the skills share
`core/` and the `agents/` subagents, which live at the plugin root outside any one skill).

- **Claude Cowork (desktop app):** "Upload local plugin" and point it at
  `lv1-compass-plugin.zip` (the zip whose root contains `.claude-plugin/plugin.json`,
  `core/`, `skills/`, and `agents/`). No marketplace needed.
- **Claude Code (optional):** `claude --plugin-dir ./lv1-compass-plugin` (unzipped) or
  `claude --plugin-dir lv1-compass-plugin.zip`.

Plugins run in **Cowork** (and Claude Code), not in plain claude.ai chat.

## Entry points — all skills, no slash commands

Every entry point is a **skill** that fires on its own natural-language trigger; there is no
`commands/` directory. You rarely name a skill — you describe what you want and the right
one triggers.

- **`lv1-compass-init`** — scaffold a project (`sources/library.md`, `runs/`, a `CLAUDE.md`
  block, a recorded default register). Triggers on "lv1-compass init", "set up
  lv1-compass", or a fresh folder with no `sources/library.md`. Light and safe to re-run —
  no genre/tone questions.
- **`lv1-assistant`** — auto-triggers when you want something **made**: "draft an analysis
  of…", "write a board memo on…", "build me a model for…", "turn this into a readout deck".
- **`lv1-advisor`** — auto-triggers when you want something **judged**: "review this memo",
  "fact-check this", "poke holes in my plan", "steelman the other side", "should I do X or
  Y?", "help me decide".

The dividing line: the advisor judges or advises on something; the assistant makes the
thing. "Review my report" → advisor. "Write me a report" → assistant. "Write me a report
that reviews vendor X" → assistant (the deliverable is the report).

## Use, inside a Cowork project

```
lv1-compass init
```
Creates `sources/library.md` and `runs/` if missing, scans `sources/` and adds any files
found to the library (this re-runs every time), records your default work register and
working mode, and updates the project's `CLAUDE.md`. Safe to run again later.

```
draft an analytical readout deck on our H2 IT-hiring outlook
```
Runs the make pipeline (or the fast lane for something small): triage sets the rigor tier
and register; research builds a graded claim ledger; the draft writes a **markdown**
deliverable; an independent inspection pass reads that real artifact and checks it; it
ships with a proof package. If you asked for a deck, the approved markdown is converted to
`pptx` as an explicit step *after* it passes — the pipeline itself stays markdown so the
inspector can actually read what it judges.

```
review manuscript/q3-strategy.md
```
The advisor hands the document to the independent inspector, which writes a tailored
`review-checklist.md` and a `review-feedback.md` beside it (verdict + located findings +
fixes). It judges; it never silently rewrites — fixes route back to the assistant.

```
should we rebuild the team around AI agents or keep the Java stack?
```
The advisor's decide mode frames the real decision, asks one batched round for your
constraints/values/risk tolerance, lays out the options (including status-quo), steelmans
each — and its own front-runner — then writes a `decision-memo.md` with a graded
recommendation and what would change it. The call stays yours.

## How the system is laid out

The discipline is **single-sourced** in `core/`, so it can't drift between the two roles:

- **Shared core** (read first by both skills and both subagents): `core/constitution.md` —
  the bar, the A/B/C/D grades and the `[A]` provenance test, the core rules, the R1–R6
  failure modes, the smallest-team rule, effort tiers. `core/working-lessons.md` — the
  hard-won lessons (neutral ≠ timid; a label is a reason to commit harder; show the
  arithmetic; the user's position is a signal to investigate, not a mirror; steelman ≠
  false balance).
- **Subagents** (run in their own isolated context, delegated to by name):
  `agents/lv1-research.md` (graded claim ledger, conflict-hunt, steelman material) and
  `agents/lv1-inspect.md` (independent check — pipeline mode for the assistant, standalone
  review mode for the advisor). Editing a research or inspection rule means editing the
  agent file.
- **lv1-assistant** — `SKILL.md` is the orchestrator; `references/` holds the inline station
  contracts: `triage.md` (one knob — rigor — plus the register and structure shape; no
  genre detection; **markdown-only output**, with an optional convert-to target), `register.md`
  (the two work voices: analytical · neutral-professional), `outline.md`, `draft.md`
  (apparatus-by-rigor, format-follows-content, markdown out), `source-intake.md`. Conversion
  to `docx`/`pptx`/`xlsx` is an explicit post-inspection step, not a pipeline format.
- **lv1-advisor** — `SKILL.md` holds the mode router; `references/` holds one contract per
  mode: `review-mode.md` (delegates to the inspector), `challenge-mode.md` (restate →
  sourced steelman → six-angle probe → verdict), `decide-mode.md` (frame → basis round →
  options → steelman each → recommend → hand back). `assets/` holds the
  `review-checklist-template.md` and the `decision-memo-template.md`.
- **lv1-compass-init** — `SKILL.md` plus `assets/` (`library-template.md`,
  `claude-md-template.md`).

## What ships with the work

Nothing ships bare. A deliverable carries a proof package (confidence list, assumptions,
receipts; appended to the file itself for full/high-stakes work). A review carries a located
verdict. A decision memo carries the options, the steelmans, the graded recommendation, and
what would change it. The product is its own proof: you can trust it *and* check it.

## The CLAUDE.md it writes

`lv1-compass init` writes a marker-bounded block into the project's root `CLAUDE.md`. Cowork
reads that file as ordinary folder instructions — it isn't part of how the plugin installs
or triggers; it's a courtesy so the project stays self-documenting between conversations.
Re-running init only touches the text between its `<!-- lv1-compass:structure:start -->` and
`<!-- lv1-compass:structure:end -->` markers, and never deletes a graded source.

## Honest limits

- **How the shared core reaches each runner.** A *skill* reads it directly: from a
  `SKILL.md`, `../../core/constitution.md` resolves to `<plugin-root>/core/` — inside the
  plugin, so it survives install (a plugin may not reference files *outside* its root, but
  this lands on it). A *subagent*, though, runs from the project directory and can't read
  the plugin's `core/` from its own path — so the orchestrator reads the constitution and
  working-lessons and **passes them into the subagent's task prompt** when it delegates.
  This pattern is sound in principle but best confirmed with a real install test.
- Decide mode's "steelman your own front-runner" rule is a **self-check**, not a
  mechanically enforced gate (there is no inspector station on advisor outputs). If you want
  it hard-enforced, that's a deliberate add.
