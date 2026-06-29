# lv1-writer

A **disciplined research-backed writing pipeline** for books, articles, and long-form work.
Three skills share one constitution and ship only work that is **impressive AND true** —
every factual claim graded and traceable to a real source. Fiction genres swap citations
for craft contracts instead.

- **lv1-author** — the **writer**. Turns a writing brief into a draft through
  `triage → research → outline → draft → inspect → ship`. Handles non-fiction (sourced,
  graded), interview-driven work (user's lived material is the spine), hybrid (both), and
  fiction (craft contracts, no citations).
- **lv1-reviewer** — the **judge**. Independently reviews an existing document — generates
  a document-specific checklist, then writes a located verdict (PASS / FIX-IT / REJECT)
  without touching the document itself.
- **lv1-writer-init** — **setup**. Scaffolds the project: source library, manuscript
  folder, tone profile, working mode, and a `CLAUDE.md` block. Safe to re-run.

## Install

Install the whole plugin — don't upload a single skill folder on its own (the skills share
`discipline/`, `references/`, `assets/`, and the `agents/` subagents, which live at the plugin root outside any one skill).

- **Claude Cowork (desktop app):** "Upload local plugin" and point it at
  `lv1-writer-plugin.zip` (the zip whose root contains `.claude-plugin/plugin.json`,
  `discipline/`, `references/`, `assets/`, `skills/`, and `agents/`). No marketplace needed.
- **Claude Code (optional):** `claude --plugin-dir ./lv1-writer-plugin` (unzipped) or
  `claude --plugin-dir lv1-writer-plugin.zip`.

Plugins run in **Cowork** (and Claude Code), not in plain claude.ai chat.

## Entry points — all skills, no slash commands

Every entry point is a **skill** that fires on its own natural-language trigger.

- **`lv1-writer-init`** — scaffold or re-sync a project. Triggers on "lv1-writer init",
  "set up lv1-writer", or a fresh folder with no `sources/library.md`. Scans the project
  first and pre-fills what it already knows; only asks about what's uncertain.
- **`lv1-author`** — auto-triggers when you want something **written**: "draft a chapter
  on…", "write an article about…", "research and write…", "continue the manuscript from…".
  Also handles "file this source" (adds to the library without a full pipeline run).
- **`lv1-reviewer`** — auto-triggers when you want something **judged**: "review this
  draft", "fact-check this document", "critique my chapter".

## Usage, inside a Cowork project

```
lv1-writer init
```
First run: ask four questions (topic & angle, POV, tone group, working mode), scaffold
`sources/library.md`, `manuscript/writing-instruction.md`, and `manuscript/_about.md`,
then write a `CLAUDE.md` block. Re-running is safe — it syncs, updates, and never
overwrites user edits to `writing-instruction.md` unless you changed the tone.

```
write a chapter on the science of habit formation for a popular-science book
```
Runs the full pipeline (or the fast lane for small tasks): triage sets the genre, rigor
tier, and authorship approach; research builds a graded claim ledger; the draft writes
prose in the project's tone; an independent inspection pass reads the real file and checks
every fact; it ships with a proof package.

```
I want to write a memoir about leaving my corporate job — interview me
```
Sets `working mode: interactive`. The pipeline switches to **interview-driven**: AI
interviews the user in one batched round per section, saves answers to
`manuscript/intake.md`, and drafts only from that material. An invented anecdote is
treated the same as a fabricated source — the inspector REJECTs it.

```
review manuscript/chapter-03.md
```
The reviewer delegates to the independent inspector (standalone mode): it generates a
document-specific `review-checklist.md` beside the file, then writes `review-feedback.md`
(verdict + every finding located precisely with a concrete fix). It judges; it never edits.

```
file this in: https://example.com/study.pdf
```
No pipeline needed — adds the source to `sources/library.md` with a grade and summary,
and reports what's now citable.

## Three knobs (set at triage)

Every writing task is sorted on three independent knobs, recorded in `00-triage.md`:

**1. Genre / tone profile** — detected automatically from the brief; selects one of 13
profiles across four groups:

| Group | Profiles |
|---|---|
| Inform & Argue | academic · analytical-intelligence · popular-science · technical · persuasive |
| Transform & Heal | cognitive-behavior · mindset |
| Story & Memoir | narrative-nonfiction · personal-experience · love |
| Creative Fiction | literary · romance · childrens |

The profile determines voice, structure shape, proof apparatus, and — for fiction —
replaces citation checks with craft contracts (internal consistency, motivated action,
earned outcomes, POV, show-don't-tell).

**2. Rigor tier** — how much proof apparatus ships (tiny / normal / full / high-stakes).
Each tier gates which stations actually run and what the inspector enforces. Fiction
always runs craft contracts regardless of tier; non-fiction scales proof from a closing
note (normal) up to a full appended proof package (high-stakes).

**3. Working mode** — `auto` (AI researches and drafts autonomously) or `interactive`
(AI interviews the user for personal material). Combined with the genre, this resolves the
authorship approach:

| Genre + mode | Authorship approach |
|---|---|
| Research/analytical genres, auto | AI-autonomous |
| Personal-experience, love, fiction — any mode | Interview-driven |
| Cognitive-behavior, mindset | Hybrid (interview for cases + research for science) |
| Any genre + interactive mode | Interview-driven |

## How the system is laid out

The discipline is **single-sourced** in `discipline/`, so it can't drift between roles.
Every shared resource lives at the **plugin root** — `discipline/`, `references/`, `assets/`,
and `agents/` — so no single skill owns it; skills reach them via `${CLAUDE_PLUGIN_ROOT}/…`:

- **Governing discipline** (read first by the orchestrator; injected into subagents): `discipline/constitution.md` —
  the bar, A/B/C/D grades, `[A]` provenance test, core rules, failure modes, effort-tier→station
  table. `discipline/working-lessons.md` — hard-won lessons (neutral ≠ timid; labels commit harder;
  show the arithmetic; the user's position is a signal to investigate; steelman ≠ false balance).
- **Subagents** (run in their own isolated context, delegated to by name):
  `agents/lv1-research.md` (graded claim ledger, conflict-hunt, steelman material, declared
  gaps) and `agents/lv1-inspect.md` (independent check — pipeline mode after drafting,
  standalone review mode for `lv1-reviewer`). Editing a research or inspection rule means
  editing the agent file.
- **Station contracts** — `references/` holds the inline station contracts the orchestrator
  reads: `triage.md`, `tone-detect.md` (genre → profile resolution), `structure-shapes.md`,
  `source-intake.md`, `outline.md`, `draft.md`, `interview.md` (batching rule, gap handling,
  invention ban), `anti-tells.md` (AI-register checks the inspector enforces).
- **Templates & profiles** — `assets/` holds the 13 `tone-profiles/`, the
  `ai-review-checklist-template.md`, and the CLAUDE.md, library, writing-instruction, and
  manuscript-about templates.
- **Skills** — each is a single `SKILL.md` with no private resources: **lv1-author** orchestrates
  the pipeline; **lv1-reviewer** delegates directly to `lv1-inspect` in standalone mode (the
  inspection contract lives in the agent file); **lv1-writer-init** handles the full init flow
  (scan → confirm → setup).

## What ships with the work

Nothing ships bare.

- **Non-fiction (normal)** — a closing note with confidence grades and any open assumptions.
- **Non-fiction (full / high-stakes)** — a proof package appended to the delivered file:
  confidence table (every factual claim, its grade, its source), assumptions, source index
  with in-window flags, data-limits. Plus a `05-manifest.md` in the run folder.
- **Fiction** — no proof apparatus. The inspector checks craft contracts instead.
- **Interview-driven** — `manuscript/intake.md` is the sourcing spine. Every personal claim
  in the draft traces back to a user answer; gaps are declared, never invented.
- **A review** — `review-checklist.md` (document-specific) and `review-feedback.md`
  (PASS / FIX-IT / REJECT with located findings and concrete fixes), written beside the target.

## The CLAUDE.md it writes

`lv1-writer init` writes a marker-bounded block into the project's root `CLAUDE.md`. It
records topic, angle, POV, resolved tone profile, and working mode — a navigation aid
between conversations. Re-running init only touches the text between its
`<!-- lv1-writer:structure:start -->` and `<!-- lv1-writer:structure:end -->` markers,
and never deletes a source or user edit.

## Honest limits

- **How the governing discipline reaches subagents.** A skill reads `discipline/` via its
  `${CLAUDE_PLUGIN_ROOT}/discipline/` path. A subagent runs from the project directory and cannot reach the
  plugin's `discipline/` from its own path — so the orchestrator reads the constitution and
  working-lessons and **injects them into the subagent's task prompt** when it delegates.
  This pattern is sound but best confirmed with a real install test.
- **Interview-driven invention protection.** The inspector REJECTs invented anecdotes,
  and the interview contract bans invention and records gaps as `DECLARED GAP`. This is an
  enforced rule, not just a guideline — but it relies on the user providing real answers.
  Sparse answers produce a leaner draft, not a hallucinated one.
- **Fiction craft contracts.** Fiction profiles replace citation checks with craft
  contracts (internal consistency, earned outcomes, POV, voice). These are judgment calls,
  not mechanically verifiable — the inspector applies them as best it can from a cold read.
- **Tone detection is automatic but not infallible.** If the detected profile is wrong,
  correct it at triage or re-run init with a different tone group.
