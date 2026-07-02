---
name: lv1-author
description: Run a book chapter, article, report, or any research-backed writing task through a disciplined pipeline so nothing ships unless every factual claim traces to a real, checked source — triage, source gathering and research, outline, draft, and an independent fact-inspection pass before delivery. Describe the writing or research task to run it (or say "use lv1-author to draft..."). Trigger this whenever factual accuracy, citations, or sourced prose matter for a writing task, even if the user doesn't name the skill directly. For project setup, use lv1-writer-init first.
---

# lv1-author

**Read first, every run:** `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md` and `${CLAUDE_PLUGIN_ROOT}/discipline/working-lessons.md`.
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
   `${CLAUDE_PLUGIN_ROOT}/references/tone-detect.md`; selects a profile from `${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/`. The
   profile also decides whether the A/B/C/D grading and proof package apply at all
   (fiction switches them off for craft contracts instead).
2. **Rigor tier** — *how much proof apparatus ships* (tiny / normal / full / high-stakes;
   full definitions and station-gate table in `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md`).
3. **Working mode** — *auto | interactive* — which, with the genre, resolves the
   **authorship approach** (AI-autonomous | interview-driven | hybrid; see
   `${CLAUDE_PLUGIN_ROOT}/references/interview.md`).

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

> **Passing the discipline to a subagent (required).** A subagent runs from the project
> directory and **cannot read the plugin's `discipline/` from its own path**. So on **every**
> delegation, paste the governing context into the subagent's task prompt: the contents of
> `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md` and `${CLAUDE_PLUGIN_ROOT}/discipline/working-lessons.md` (and, for the
> inspector, `${CLAUDE_PLUGIN_ROOT}/assets/ai-review-checklist-template.md`) — which you, the orchestrator,
> can read. Without this the subagent runs blind to the bar, the grades, and the failure
> modes it must obey.
>
> **Fail loud — do not run a subagent blind.** Before you delegate, confirm the constitution
> and working-lessons text was actually assembled into the task prompt. If for any reason you
> could not read them from `${CLAUDE_PLUGIN_ROOT}/discipline/`, **stop and report it** — do not fall back
> to spawning the subagent without them. A research or inspection pass run without the
> discipline is not a cheaper version of the check; it is a fake step (R4), and it is worse
> than no check because it looks real. Missing discipline halts the run; it never degrades it
> silently.

1. **Set up the run.** Create `runs/<UTC-timestamp>-<short-slug>/` in the project
   folder and start `runs/<id>/decisions.md` (the decision log — see "Log decisions"
   below). Tell the user the run id.
2. **Triage.** Read `${CLAUDE_PLUGIN_ROOT}/references/triage.md` and follow it. Write `00-triage.md`. This
   sets all three knobs — detect the **genre / tone profile** and **structure shape**
   (`${CLAUDE_PLUGIN_ROOT}/references/tone-detect.md`, `${CLAUDE_PLUGIN_ROOT}/references/structure-shapes.md`), the **rigor tier**,
   and the **working mode → authorship approach**. If genre or scope is genuinely
   ambiguous, ask one batched round before continuing (rule 1). On `tiny`, say you're
   running the fast lane and skip straight to draft after triage. (`00-triage.md` is
   still written — lv1-inspect reads it for genre and rigor tier regardless of lane.)
3. **Gather material — routed by authorship approach.**
   - **AI-autonomous:** for a source handed over (PDF, Word, image, URL), read
     `${CLAUDE_PLUGIN_ROOT}/references/source-intake.md` and add it to `sources/library.md`. For facts still
     missing, **delegate to the `lv1-research` subagent** — it researches in its own
     context and writes `01-research.md` as a graded claim ledger, returning a summary.
     Don't move on until every claim the task needs is a ledger row sourced (A/B) or
     marked a declared gap.
   - **Interview-driven:** the spine comes from the user — run the intake interview
     (`${CLAUDE_PLUGIN_ROOT}/references/interview.md`), one batched round, and save answers to
     `inputs/intake.md`. Research the web only for context *around* the user's
     material, if at all.
   - **Hybrid (e.g. cognitive-behavior):** do both — interview for the lived material
     (`intake.md`) *and* delegate to `lv1-research` for the science. Keep the two
     visibly distinct downstream.
4. **Outline.** Read `${CLAUDE_PLUGIN_ROOT}/references/outline.md` and follow it. Write `02-outline.md`.
5. **Draft.** Read `${CLAUDE_PLUGIN_ROOT}/references/draft.md` and follow it: compose the voice from the tone
   profile in `00-triage.md` (+ `inputs/writing-instruction.md` overrides), apply the
   apparatus for the rigor tier, and let format follow content. For interview-driven /
   hybrid work, read `inputs/intake.md` as a first-class source and invent no
   personal material. **Work one chapter file at a time** (prefer rule): draft into a
   single `manuscript/<chapter>.md` (e.g. `ch01.md`) and load only the chapter(s) the
   task touches — don't read the whole book to write or fix one chapter (see the chapter
   rule in `draft.md`). Write a copy to `03-draft.md`. Run its self-review pass before
   moving on.
6. **Inspect.** Before delegating, inject the discipline into the subagent's task prompt per
   the **"Passing the discipline to a subagent (required)"** block above — the constitution,
   the working-lessons, and the review checklist — since lv1-inspect runs from the project
   folder and cannot reach the plugin's `discipline/` on its own. If you could not assemble
   that context, stop and report (the fail-loud rule); do not inspect blind. Then **delegate
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

   **The delivery gate (before you ship anything).** For any tier that ran the inspector
   (normal and up), verify with your own eyes — not from memory of the delegation — that
   `04-inspection.md` **exists on disk, is non-empty, and carries a PASS written by
   lv1-inspect**. If it's missing, empty, or the verdict is anything but an inspector-authored
   PASS, the gate stays shut: do not ship, and report why (constitution → The delivery gate).
   On `tiny` there is no inspector — say so; the self-review stands in its place.
7. **Ship.** Write `05-manifest.md`: a confidence list (every factual claim, its grade,
   its source), an assumptions list, and a receipts index. For **full / high-stakes
   non-fiction**, also **append the proof package to the delivered file itself**
   (confidence table, assumptions, source index with in-window flags, data-limits) — the
   reader should be able to verify without opening `runs/`. Lighter tiers keep grades in
   a closing note; fiction ships none of this. Deliver the draft plus a short cover note;
   state overall confidence and any open assumptions plainly.

After each station, give the user a one-line status (e.g. "Research: 12 facts, all
sourced"). If you cut a corner, say so — never fake a step.

## Log decisions (every run)

Keep `runs/<id>/decisions.md` as a running record of *how the run was steered* — the
reasoning behind the calls, not the deliverable. Append to it (don't overwrite) at each
station and whenever a fork is resolved. Capture two things:

- **AI reasoning** — each material decision and *why*: the genre/tier/approach chosen at
  triage and what tipped it, scope cuts, a structure or framing choice, a FIX-IT you
  accepted or pushed back on, a declared gap and why it stayed open.
- **User confirmations — verbatim.** Whenever you ask the user a question (an
  AskUserQuestion round, a scope or direction check) record the question and the user's
  answer **word for word**, with a timestamp. This is the recoverable basis for why the
  piece is shaped the way it is.

Format each entry as `## <UTC time> — <station>` followed by `Decision:`, `Why:`, and
(if any) `User confirmed:` lines. Keep it terse; it is a log, not prose.

## Assemble the full book (on demand)

The chapter files in `manuscript/` are the source of truth; the full book is a **build
output**, regenerated from them — never hand-edited. When the user asks to "assemble the
book", "compile the manuscript", "stitch the chapters", or "export the full book", do this
(no run folder needed):

**Assembly is a file operation, not a reading task.** Concatenate the chapter files with a
shell command (`cat`); never `Read` full chapter contents into context in order to
reassemble them — the prose is copied byte-for-byte anyway, so reading it first only burns
tokens. Reading full chapters is for editing, review, or a consistency audit (see below) —
not for assembly.

1. Collect the chapter files in `manuscript/` in order — the zero-padded `chNN.md` sort
   (`ch01.md`, `ch02.md`, …). **Exclude** `_about.md` and anything under `_archive/`. If
   the chapter naming is irregular, order them by `02-outline.md` (the latest run's
   outline) instead, and say which ordering you used.
2. **Decide the filename from the book's title** — slugified to kebab-case (e.g. *The
   Science of Habit* → `the-science-of-habit.md`). Take the title from the marker block in
   `CLAUDE.md` / `manuscript/_about.md`; if none is recorded, ask the user for the title
   once, then proceed. Use the **same filename every rebuild** so it overwrites cleanly.
3. **Build it in the shell**, at the **project root** (not inside `manuscript/`): `cat` the
   chapters straight into the file, in order, with a blank line forced between each so
   spacing is right regardless of what each chapter file ends with — e.g.
   `(cat manuscript/ch01.md; echo; cat manuscript/ch02.md; echo; cat manuscript/ch03.md) > <book-title>.md`
   (list every chapter file explicitly, in order). Each chapter keeps its own heading.
   **Copy the prose byte-for-byte — reformat nothing, and don't retype it from context. Write
   only the chapters' own content — no build marker, comment, or metadata gets added to the
   file.** This is the finished book, the reader-facing artifact — not a scratch or
   prototype file, so nothing goes in it that isn't part of the book itself.
4. **Regenerate fresh every time** (overwrite the existing `<book-title>.md`); never merge
   into a stale copy. Get the chapter count and word count from the shell (`wc -l
   manuscript/ch*.md`, `wc -w <book-title>.md`) rather than by reading the content — report
   both.

**If the result looks wrong** (a chapter too short, one missing, a stale mount/tool read) —
verify with metadata first: `wc -c`/`wc -l` on the chapter files, or `md5sum` before/after
an edit. Run one targeted check, report what it shows, and only fall back to reading full
chapter contents if that check actually confirms a discrepancy. Don't re-verify a clean
result "just in case" — one check, one conclusion, move on.

This is assembly only — it does not draft, edit, or inspect. To change the book, edit the
chapter and re-assemble.

## A single source you're handed mid-task

If the user just hands over a file or link without asking for the full pipeline ("file
this in" / "add this source"), you don't need a full run folder — read
`${CLAUDE_PLUGIN_ROOT}/references/source-intake.md`, add it to `sources/library.md`, and report the source
id and what's now citable.

## Reviewing an existing document (no pipeline run)

If the user asks you to review / critique / fact-check a document that already exists
(rather than write a new one) — whether through the `lv1-reviewer` skill or just in plain
words — hand it to the **lv1-inspect** subagent in **standalone review mode**, following
the same injection steps the `lv1-reviewer` skill uses: read
`${CLAUDE_PLUGIN_ROOT}/assets/ai-review-checklist-template.md`
and `${CLAUDE_PLUGIN_ROOT}/discipline/working-lessons.md` from the plugin path and inject both into the
subagent's task prompt before delegating. lv1-inspect then generates a document-specific
`review-checklist.md` beside the target and writes `review-feedback.md` (verdic                          