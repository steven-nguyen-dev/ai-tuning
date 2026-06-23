---
name: lv1-writer
description: Run a book chapter, article, report, or any research-backed writing task through a disciplined pipeline so nothing ships unless every factual claim traces to a real, checked source — triage, source gathering and research, outline, draft, and an independent fact-inspection pass before delivery. Say "lv1-writer init" once in a new project folder to set up the sources and manuscript folders, sync the source library against whatever's already in sources/, set a default writing tone, and update the project's CLAUDE.md — then describe the writing or research task to run it (or say "use lv1-writer to draft..."). Trigger this whenever factual accuracy, citations, or sourced prose matter for a writing task, even if the user doesn't name the skill directly.
---

# lv1-writer

A disciplined pipeline for book and research writing. It turns finding, keeping, and
using source material into a repeatable line — triage, source curation and research,
outline, draft, independent inspection — and ships only work that is **impressive AND
true**, with every fact traceable to a source.

This skill is the **orchestrator** of a plugin pipeline. It preserves the discipline
that matters: a written plan before drafting, a graded source behind every fact, and a
genuinely skeptical re-read of the real files before anything ships. The two stations
most at risk of being quietly under-built run as **isolated subagents** with their own
context: **research** (`lv1-research`) closes only when a claim ledger is fully sourced,
and **inspection** (`lv1-inspect`) is mechanically independent — it never saw the
drafting reasoning — and runs a coverage gate that can send thin research back for more.
Each subagent carries its own contract (in `agents/`); this orchestrator delegates to
them by name and integrates what they return.

## First: init, review, or a task?

This skill is reached three ways. Two are handled by dedicated sibling skills that fire
on their own trigger phrases; everything else is just described in plain language and the
main skill auto-triggers on its description. There is deliberately **no skill per task
type** beyond init and review (the task space is open-ended — a model-invoked skill covers
it).

- **Init** — the **`lv1-writer-init`** skill (triggers: "lv1-writer init", "set up
  lv1-writer", or a clearly fresh project where `sources/library.md` doesn't exist yet).
  It runs the **Setup** flow below, then stops and reports what was created or updated.
- **Review** — the **`lv1-writer-review`** skill (triggers: "review this doc with
  lv1-writer", "fact-check this document", etc.). It independently reviews an *existing*
  document by delegating to the **lv1-inspect** subagent in **standalone review mode** —
  generating a document-specific checklist from `assets/ai-review-checklist-template.md`,
  then reviewing the document against it and writing `review-checklist.md` and
  `review-feedback.md` beside it. This does **not** run the writing pipeline below — use it
  when the document already exists; use the pipeline when producing new prose.
- **Task** — anything else: a writing or research request, a source handed over, a single
  station to re-run. The main `lv1-writer` skill auto-triggers and routes via triage.

Re-running Setup on an already-initialized project is safe. Every step except the
library sync is create-if-missing, and the library sync only adds new sources or
flags missing ones — it never deletes a graded claim or overwrites a tone edit. So
when it's ambiguous, it's fine to run Setup; the cost is a few extra read calls, not
lost work.

## Setup (`lv1-writer init`, or whenever `sources/library.md` is missing)

Run every step below, in order. Step 1 always runs, even on a project that already
has a library — everything else is create-if-missing, so a re-run never destroys a
customization.

1. **Sync the source library.** If `sources/library.md` doesn't exist yet, write the
   content of `assets/library-template.md` first. Either way, read
   `references/source-intake.md`'s "Library sync" section and follow it — this scans
   `sources/` for files not yet represented in the library and adds them, every time
   init runs, not just on a fresh project.
2. **`manuscript/_about.md`** — if missing, write the content of
   `assets/manuscript-about-template.md` verbatim.
3. **`manuscript/writing-instruction.md`** — if missing, write the content of
   `assets/writing-instruction-template.md` verbatim. This is the project's default
   prose tone; the `draft` station reads it. **Never overwrite this file if it
   already exists** — that's where the user's own edits to the tone live.
4. **`CLAUDE.md` (the final step).** If it doesn't exist, create it containing the
   block from `assets/claude-md-template.md`. If it exists, replace only the text
   between `<!-- lv1-writer:structure:start -->` and `<!-- lv1-writer:structure:end
   -->` with a fresh copy of that block; if those markers aren't present, append the
   block to the end. Never touch anything else in an existing `CLAUDE.md` — the user
   or Cowork itself may have put other instructions there.

### Report back after setup

- What was created versus what was already present and left alone — don't claim to
  have written something you didn't touch.
- How many source files were found in `sources/` and how many of those were newly
  added to the library (or "none yet" on a fresh project with no `sources/` files).
- **The default tone**, in 2-4 plain sentences distilled from
  `manuscript/writing-instruction.md` — e.g. "a storyteller telling one true thing to
  one reader; vivid, case-driven, every claim graded and sourced; Scene → Mechanism →
  Reflection → Bridge structure; real documented cases only, nothing invented." Tell
  the user that to use a different tone, they should edit
  `manuscript/writing-instruction.md` directly — init won't touch it again once it
  exists.
- That `CLAUDE.md` now reflects the current project layout.

## The bar

> **Impressive AND true.** Writing that dazzles but is false fails. Writing that is
> true but thin also fails. Ship only what clears both, backed by sources a reader can
> check.

## Core rules

1. **Understand the real ask first.** Pin down what's wanted — kind of text, length,
   audience, voice — and the limits, before planning.
2. **Check, don't trust memory.** Gather real material first — read the sources,
   search the web. Never write factual claims from memory alone.
3. **Plan in writing.** The outline is a visible artifact. If it changes, say so.
4. **Label confidence** on every factual claim (A/B/C/D below). A label is a reason to
   commit *harder*, not a hedge — never soften a finding the evidence supports. If you
   make a numeric or analytical claim, show the working, or don't make the precise
   claim.
5. **Source every factual claim** against the source library. No source, no claim.
6. **Mark interpretation as interpretation.** Analysis, reading, argument, and
   creative choices are *your* judgment — present them honestly as such. They don't
   need an external citation, but they must never be dressed up as established fact.
7. **Hold the scope, and steelman.** Don't quietly add or drop work. Before a
   conclusion that favors a view, state the strongest case against it, then decide if
   it still stands.
8. **Inspect for real before delivery.** Before shipping, re-read the actual draft and
   the actual source library fresh — the way an outside reviewer would, not the way
   the person who just wrote it would. Don't let the habit of having just drafted it
   stand in for genuine verification.

## Confidence grades

| Grade | Meaning | Ships? |
|-------|---------|--------|
| **A — Proven** | Backed by a primary source you fetched and read. | Yes |
| **B — Reasoned** | Derived with sound logic from verified material. A real conclusion. | Yes (labeled) |
| **C — From memory** | Memory or a single unchecked source. | Held back until confirmed |
| **D — Guess** | A guess. | **Never ships** — set aside or flag as an explicit assumption |

(Confidence grades apply to *factual* claims. Interpretation and creative prose are
governed by rule 6, not by citation.)

## Effort tier (set this first, in triage)

- **tiny** — short, clear → fast lane: triage → draft → inspect.
- **normal** — ordinary piece → full line below.
- **full** — important or multi-part → full line, more care at each station.
- **high-stakes** — published, contested, or high-visibility → full line + extra
  scrutiny at research and inspection.

Default to the lighter lane; escalate for a reason.

## The pipeline

```
triage → (source-intake / research) → outline → draft (+ self-review) → inspect → ship
              ▲                            ▲                               │
              │                            └────────── fix-it ◀────────────┤
              └─────────────── redo-research ◀──────────────────────────────┘
```

(`research` and `inspect` run as isolated subagents — `lv1-research` and `lv1-inspect` —
each with its own context and contract. The orchestrator delegates and integrates their
results; the two loops are driven by the inspector's verdict.)

1. **Set up the run.** Create `runs/<UTC-timestamp>-<short-slug>/` in the project
   folder. Tell the user the run id.
2. **Triage.** Read `references/triage.md` and follow it. Write `00-triage.md`. If it
   surfaces a genuinely blocking question, stop and ask the user (rule 1). On `tiny`,
   say you're running the fast lane and skip straight to draft after triage.
3. **Gather material.** For a source the user hands over (PDF, Word, image, URL),
   read `references/source-intake.md` and follow it to add it to
   `sources/library.md`. For facts still missing, **delegate to the `lv1-research`
   subagent** — it researches in its own context and writes `01-research.md` as a graded
   claim ledger, returning a summary. Don't move on to outline until every claim the task
   needs is a ledger row that's sourced (A/B) or marked a declared gap.
4. **Outline.** Read `references/outline.md` and follow it. Write `02-outline.md`.
5. **Draft.** Read `references/draft.md` and follow it, including the tone in
   `manuscript/writing-instruction.md` if it exists. Write the prose to
   `manuscript/` and a copy to `03-draft.md`. Run its self-review pass before moving
   on.
6. **Inspect.** **Delegate to the `lv1-inspect` subagent** (pipeline mode) — running in
   its own context, it re-reads the real draft, `sources/library.md`, and `01-research.md`
   from disk (it never saw the drafting reasoning, which is what makes the check
   independent) and writes `04-inspection.md` with one verdict:
   - **FIX-IT** → a draft problem. Go back to draft with the full findings, fix every
     one, then re-inspect **from scratch**. Cap at 3 rounds; if still failing, REJECT.
   - **REDO-RESEARCH** → the draft is honest but the research base is too thin (the
     Coverage check). Go back to the **research** station with the named gaps. Cap at 2
     redos; then ship with gaps declared or REJECT.
   - **REJECT** → stop. Report honestly. Do not ship.
   - **PASS** → ship.
7. **Ship.** Write `05-manifest.md`: a confidence list (every factual claim, its grade,
   its source), an assumptions list, and a receipts index. Deliver the draft plus a
   short cover note. State overall confidence and any open assumptions plainly.

After each station, give the user a one-line status (e.g. "Research: 12 facts, all
sourced"). If you cut a corner, say so — never fake a step.

## A single source you're handed mid-task

If the user just hands over a file or link without asking for the full pipeline ("file
this in" / "add this source"), you don't need a full run folder — read
`references/source-intake.md`, add it to `sources/library.md`, and report the source
id and what's now citable.

## Reviewing an existing document (no pipeline run)

If the user asks you to review / critique / fact-check a document that already exists
(rather than write a new one) — whether through the `lv1-writer-review` skill or just in
plain words — hand it to the **lv1-inspect** subagent in **standalone review mode**. It reads
`assets/ai-review-checklist-template.md`, generates a document-specific `review-checklist.md`
beside the target, then reviews against it and writes `review-feedback.md` (verdict
PASS / FIX-IT / REJECT, every finding located precisely with a concrete fix). The
inspector is read-only — it never edits the document itself.