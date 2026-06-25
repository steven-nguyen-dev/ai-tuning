---
name: lv1-author
description: Run a book chapter, article, report, or any research-backed writing task through a disciplined pipeline so nothing ships unless every factual claim traces to a real, checked source — triage, source gathering and research, outline, draft, and an independent fact-inspection pass before delivery. Describe the writing or research task to run it (or say "use lv1-author to draft..."). Trigger this whenever factual accuracy, citations, or sourced prose matter for a writing task, even if the user doesn't name the skill directly. For project setup, use lv1-writer-init first.
---

# lv1-author

**Read first, every run:** `../../core/constitution.md` and `../../core/working-lessons.md`.
The bar, the A/B/C/D grades, the rigor tiers, the failure modes, and the steelman rule
are defined there. This skill applies them; it does not restate them.

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

This skill handles writing tasks. Two sibling skills handle the other entry points:

- **Init** — **`lv1-writer-init`** (triggers: "lv1-writer init", "set up lv1-writer", or
  a fresh project where `sources/library.md` doesn't exist). It scaffolds the project and
  stops — it does not start a writing task.
- **Review** — **`lv1-reviewer`** (triggers: "review this doc", "fact-check this
  document", "critique this draft"). It independently reviews an *existing* document
  without running the writing pipeline.
- **Task** — anything else: a writing or research request, a source handed over, a single
  station to re-run. This skill auto-triggers and routes via triage.

## Three knobs (set these first, in triage)

Every task is sorted along three independent knobs, recorded in `00-triage.md`:

1. **Genre / tone profile** — *what voice and shape*. Detected per
   `references/tone-detect.md`; selects a profile from `assets/tone-profiles/`. The
   profile also decides whether the A/B/C/D grading and proof package apply at all
   (fiction switches them off for craft contracts instead).
2. **Rigor tier** — *how much proof apparatus ships* (tiny / normal / full / high-stakes;
   full definitions and station-gate table in `../../core/constitution.md`).
3. **Working mode** — *auto | interactive* — which, with the genre, resolves the
   **authorship approach** (AI-autonomous | interview-driven | hybrid; see
   `references/interview.md`).

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
2. **Triage.** Read `references/triage.md` and follow it. Write `00-triage.md`. This
   sets all three knobs — detect the **genre / tone profile** and **structure shape**
   (`references/tone-detect.md`, `references/structure-shapes.md`), the **rigor tier**,
   and the **working mode → authorship approach**. If genre or scope is genuinely
   ambiguous, ask one batched round before continuing (rule 1). On `tiny`, say you're
   running the fast lane and skip straight to draft after triage. (`00-triage.md` is
   still written — lv1-inspect reads it for genre and rigor tier regardless of lane.)
3. **Gather material — routed by authorship approach.**
   - **AI-autonomous:** for a source handed over (PDF, Word, image, URL), read
     `references/source-intake.md` and add it to `sources/library.md`. For facts still
     missing, **delegate to the `lv1-research` subagent** — it researches in its own
     context and writes `01-research.md` as a graded claim ledger, returning a summary.
     Don't move on until every claim the task needs is a ledger row sourced (A/B) or
     marked a declared gap.
   - **Interview-driven:** the spine comes from the user — run the intake interview
     (`references/interview.md`), one batched round, and save answers to
     `manuscript/intake.md`. Research the web only for context *around* the user's
     material, if at all.
   - **Hybrid (e.g. cognitive-behavior):** do both — interview for the lived material
     (`intake.md`) *and* delegate to `lv1-research` for the science. Keep the two
     visibly distinct downstream.
4. **Outline.** Read `references/outline.md` and follow it. Write `02-outline.md`.
5. **Draft.** Read `references/draft.md` and follow it: compose the voice from the tone
   profile in `00-triage.md` (+ `manuscript/writing-instruction.md` overrides), apply the
   apparatus for the rigor tier, and let format follow content. For interview-driven /
   hybrid work, read `manuscript/intake.md` as a first-class source and invent no
   personal material. Write the prose to `manuscript/` and a copy to `03-draft.md`. Run
   its self-review pass before moving on.
6. **Inspect.** Before delegating, read `../../core/working-lessons.md` from the plugin
   path and inject its content into the subagent's task prompt — lv1-inspect runs from
   the project folder and cannot reach the plugin's `core/` on its own. Then **delegate
   to the `lv1-inspect` subagent** (pipeline mode) — running in its own context, it
   re-reads the real draft, `sources/library.md`, and `01-research.md` from disk (it
   never saw the drafting reasoning, which is what makes the check independent) and
   writes `04-inspection.md` with one verdict:
   - **FIX-IT** → a draft problem. Go back to draft with the full findings, fix every
     one, then re-inspect **from scratch**. Cap at 3 rounds; if still failing, REJECT.
   - **REDO-RESEARCH** → the draft is honest but the research base is too thin (the
     Coverage check). Go back to the **research** station with the named gaps. Cap at 2
     redos; then ship with gaps declared or REJECT.
   - **REJECT** → stop. Report honestly. Do not ship.
   - **PASS** → ship.
7. **Ship.** Write `05-manifest.md`: a confidence list (every factual claim, its grade,
   its source), an assumptions list, and a receipts index. For **full / high-stakes
   non-fiction**, also **append the proof package to the delivered file itself**
   (confidence table, assumptions, source index with in-window flags, data-limits) — the
   reader should be able to verify without opening `runs/`. Lighter tiers keep grades in
   a closing note; fiction ships none of this. Deliver the draft plus a short cover note;
   state overall confidence and any open assumptions plainly.

After each station, give the user a one-line status (e.g. "Research: 12 facts, all
sourced"). If you cut a corner, say so — never fake a step.

## A single source you're handed mid-task

If the user just hands over a file or link without asking for the full pipeline ("file
this in" / "add this source"), you don't need a full run folder — read
`references/source-intake.md`, add it to `sources/library.md`, and report the source
id and what's now citable.

## Reviewing an existing document (no pipeline run)

If the user asks you to review / critique / fact-check a document that already exists
(rather than write a new one) — whether through the `lv1-reviewer` skill or just in plain
words — hand it to the **lv1-inspect** subagent in **standalone review mode**, following
the same injection steps as `../lv1-reviewer/SKILL.md`: read `assets/ai-review-checklist-template.md`
and `../../core/working-lessons.md` from the plugin path and inject both into the
subagent's task prompt before delegating. lv1-inspect then generates a document-specific
`review-checklist.md` beside the target and writes `review-feedback.md` (verdict
PASS / FIX-IT / REJECT, every finding located precisely with a concrete fix). The
inspector is read-only — it never edits the document itself.