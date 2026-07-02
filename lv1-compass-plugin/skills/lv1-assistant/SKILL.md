---
name: lv1-assistant
description: Produce a trustworthy work deliverable — an analysis, report, memo, brief, slide deck, or spreadsheet model — through a disciplined pipeline: triage → sourced research → outline → draft → independent inspection → ship with a proof package. Use when the user wants something *made* or *written* for work. Every factual claim is graded (A/B/C/D) and traced to a real source; nothing ships unless it is impressive AND true. Not for reviewing an existing document or weighing a decision — that is lv1-advisor.
---

# lv1-assistant — the maker

The make-half of lv1-compass: a disciplined pipeline that turns a work brief into a
deliverable a reader can both trust and verify. Narrower than a general writing tool — it
covers **work deliverables** in one of two registers, not the open-ended genre/tone space.

**Read first, every run:** the shared constitution at `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md`
and `${CLAUDE_PLUGIN_ROOT}/discipline/working-lessons.md`. They define the bar,
the A/B/C/D grades, the `[A]` provenance test, the failure modes, and the lessons every
station obeys. This skill does not restate them — it applies them.

The two stations most at risk of being quietly under-built run as **isolated subagents**
with their own context: **research** (`lv1-research`) closes only when its claim ledger is
fully sourced, and **inspection** (`lv1-inspect`) is mechanically independent — it never
saw the drafting reasoning — and runs a coverage gate that can send thin research back for
more.

## The bar

> **Impressive AND true.** A deliverable that dazzles but is false fails. One that is true
> but thin also fails. Ship only what clears both, backed by sources a reader can check.

## One knob + a register (set in triage)

Unlike the wider writing tool, the assistant sets one main knob — there is no genre/tone
detection, and **no output-format knob**:

1. **Rigor tier** — `tiny | normal | full | high-stakes` (from `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md`).
   Per the tier→station table there, it gates *which stations actually run* and how much
   proof apparatus ships.

**Output is always markdown.** The pipeline produces a markdown deliverable so the
independent inspector — which has only Read/Grep/Glob — can judge the *real* shipped
artifact.

The **work register** (analytical | neutral-professional) and **structure shape** are the
two lighter choices, also set in triage — see `${CLAUDE_PLUGIN_ROOT}/skills/lv1-assistant/references/register.md`.

## The pipeline

```
triage → (source-intake / research) → outline → draft (+ self-review) → inspect → ship
              ▲                            ▲                               │
              │                            └────────── fix-it ◀────────────┤
              └─────────────── redo-research ◀──────────────────────────────┘
```

(`research` and `inspect` run as isolated subagents — `lv1-research` and `lv1-inspect`.
The orchestrator delegates and integrates; the two loops are driven by the inspector's
verdict.)

> **Passing the discipline to a subagent (required).** A subagent runs from the project
> directory and **cannot read the plugin's `discipline/` from its own path**. So when you
> delegate, paste the governing context into the subagent's task prompt: the contents of
> `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md`, `${CLAUDE_PLUGIN_ROOT}/discipline/working-lessons.md`, and (for the inspector)
> `${CLAUDE_PLUGIN_ROOT}/discipline/readability.md` and `${CLAUDE_PLUGIN_ROOT}/discipline/anti-tells.md` — which you, the
> orchestrator, can read. Without this the subagent runs blind to the bar, the grades, and
> the format and register rules it must enforce.
>
> **Fail loud — do not run a subagent blind.** Before delegating, confirm the constitution
> and working-lessons text was actually assembled into the task prompt. If you could not read
> them from `${CLAUDE_PLUGIN_ROOT}/discipline/`, **stop and report it** — never fall back to spawning the
> subagent without them. A research or inspection pass run without the discipline is a fake
> step (R4), worse than no check because it looks real. Missing discipline halts the run; it
> never degrades it silently.

1. **Set up the run.** Create `runs/<UTC-timestamp>-<short-slug>/`. Tell the user the run
   id.
2. **Triage.** Read `${CLAUDE_PLUGIN_ROOT}/skills/lv1-assistant/references/triage.md` and follow it. Write `00-triage.md`: rigor tier,
   work register, structure shape, scope, and (full/high-stakes) a research contract. If scope is genuinely ambiguous, ask one batched round
   (rule 1). On `tiny`, say you're running the fast lane: the station set is **triage →
   draft → self-review → ship** (research, outline, and the inspect subagent are skipped).
   **But if the tiny task carries any factual claim, escalate to `normal`** — the fast lane
   has no station to source a claim, and rule 5 (no source, no claim) still binds.
3. **Gather material.** For a source handed over (PDF, Word, image, URL), read
   `${CLAUDE_PLUGIN_ROOT}/references/source-intake.md` and add it to `sources/library.md`. For facts still
   missing, **delegate to the `lv1-research` subagent** — it writes `01-research.md` as a
   graded claim ledger and returns a summary. Don't move on until every claim the
   deliverable needs is a ledger row sourced (A/B) or marked a declared gap — **and until
   the spine actually landed**: confirm `sources/library.md` now holds the run's A/B source
   blocks. If `lv1-research` reported a library write failure (a `library.pending.md`
   fallback), surface it to the user and treat the spine as degraded; don't proceed as if
   `sources/library.md` is intact when it's empty.
4. **Outline.** Read `${CLAUDE_PLUGIN_ROOT}/skills/lv1-assistant/references/outline.md` and follow it. Write `02-outline.md`.
   **High-stakes approval beat (high-stakes only):** before drafting, present the outline
   (scope + structure + the research contract's coverage) to the user and **await an explicit
   go** — the one consequential decision in an otherwise autonomous pipeline stays with the
   human (P12). Record the outcome on its own line at the foot of `02-outline.md`
   (`Approval: approved <UTC> by user` — or the change requested). For tiny/normal/full the
   pipeline proceeds without this gate.
5. **Draft.** Read `${CLAUDE_PLUGIN_ROOT}/skills/lv1-assistant/references/draft.md` and follow it: compose the voice from the register
   in `00-triage.md`, apply the apparatus for the rigor tier, and let format follow content.
   Write the final deliverable to the project root as `<slug>.md`; keep the working copy as
   `runs/<id>/03-draft.md`. Run the self-review pass before moving on.
6. **Inspect.** **Delegate to the `lv1-inspect` subagent** (pipeline mode). It re-reads the
   real draft, `sources/library.md`, and `01-research.md` from disk and writes
   `04-inspection.md` with one verdict:
   - **FIX-IT** → a draft problem. Back to draft with the full findings, fix every one,
     re-inspect **from scratch**. Cap 3 rounds; else REJECT.
   - **REDO-RESEARCH** → honest draft, too-thin research base. Back to **research** with the
     named gaps. Cap 2 redos; then ship with gaps declared or REJECT.
   - **REJECT** → stop. Report honestly. Do not ship.
   - **PASS** → ship.

   **The delivery gate (before you ship anything).** For any tier that ran the inspector
   (normal and up), verify with your own eyes — not from memory of the delegation — that
   `04-inspection.md` **exists on disk, is non-empty, and carries a PASS written by
   lv1-inspect**. If it's missing, empty, or the verdict is anything but an inspector-authored
   PASS, the gate stays shut: do not ship, and report why (constitution → The delivery gate).
   On the `tiny` fast lane there is no inspector — say so; the self-review stands in its place.
7. **Ship.** Write `05-manifest.md`: a confidence list (every factual claim, its grade, its
   source), an assumptions list, a receipts index. **Verify every receipt before listing
   it** — list only files that are actually on disk; if a station's receipt is missing
   because a write failed (e.g. `04-inspection.md`), say so explicitly (`04-inspection.md —
   MISSING, write failed`) rather than listing it, and never report a verdict (PASS, etc.)
   drawn from a receipt that was never written (constitution → Write integrity, R4).
   Likewise flag an empty/degraded `sources/library.md` here. For **full / high-stakes**, also
   **append the proof package to the delivered file itself** (confidence table,
   assumptions, source index, data-limits) — the reader should verify without opening
   `runs/`. Lighter tiers keep grades in a closing note. Deliver the file plus a short
   cover note; state overall confidence and any open assumptions plainly.

After each station, give the user a one-line status (e.g. "Research: 12 facts, all
sourced"). If you cut a corner, say so — never fake a step (R4).

**Interactive by default.** When any station hits a blocking ambiguity or a real gap,
**ask the user in one batched round before proceeding** rather than guessing — collect
every open question for that step and put them in a single round (constitution rule 1's
batching rule). `auto` mode is the opt-out: proceed autonomously and ask only on a truly
blocking ambiguity.

## Iterating on feedback

The user's corrections are handled **in chat**, not through a feedback file or a separate
skill. When a note comes back, re-enter the pipeline at the **earliest affected station**
and re-run forward from there: a fact challenge → **research**; a scope or structure note →
**outline**; a wording or emphasis note → **draft**. Re-inspect per the rigor tier (a
`normal`/`full`/`high-stakes` change gets the inspect subagent again; a `tiny` change gets
the self-review pass), then re-ship. Append a line to the run's receipts noting **what
changed and why** so the run stays an honest record. Don't spin up a new run for a
correction to the same deliverable — continue the existing `runs/<id>/`.

## After ship (optional) — office-file conversion

The pipeline's shipped artifact is **always markdown**, on purpose: the independent
inspector (Read/Grep/Glob only) can judge markdown but not a binary office file, so
inspecting markdown is what makes the PASS mean something. Conversion is therefore **not a
pipeline station** and is never inspected — it runs only *after* a PASS, only if the user
needs a `.docx`/`.pptx`/`.xlsx`, and only on the already-approved markdown. It is a
format-only transform: **it adds no claim, changes no number, and is re-derivable from the
shipped markdown.** If the user asks for an office file, convert the approved markdown and
note plainly that the markdown remains the inspected source of truth.

## A single source you're handed mid-task

If the user just hands over a file or link without asking for the full pipeline, read
`${CLAUDE_PLUGIN_ROOT}/references/source-intake.md`, add it to `sources/library.md`, and report the source id and
what's now citable — no run folder needed.
