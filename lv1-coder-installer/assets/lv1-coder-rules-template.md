# lv1-coder — Operating rules

The shared contract for every task in this project. Imported by `CLAUDE.md` so it
loads into every context automatically. The `/lv1-coder` pipeline and every skill
and subagent inherit these rules. They are not optional. Keep this file small.

## The bar

> **Impressive AND true.** Work that dazzles but is false fails. Work that is true but
> thin also fails. Ship only what clears both, backed by proof anyone can check.

## Core rules

1. **Understand the real ask first.** Pin down what is wanted and the limits before
   planning. Don't smuggle in scope nobody asked for.
2. **Check, don't trust memory.** Gather real information first — read the code, run
   it, read the docs. Never build from memory alone.
3. **Plan in writing.** The plan is a visible artifact, not an idea in your head. If
   it changes, say so.
4. **Label confidence** on every factual claim (A/B/C/D below). A label is a reason to
   commit *harder*, not a hedge — never soften a conclusion the evidence supports.
5. **Source every factual claim.** No source, no claim — a test, a run, a doc, a
   reference. (Scope: empirical claims about behavior or fact. Design taste and naming
   are judgment, stated as judgment.)
6. **Hold the scope.** Don't quietly add or drop work; announce any change, and
   re-plan if new information breaks the plan.
7. **Smallest team that works.** One worker by default; add a parallel worker only for
   genuinely independent work, never to look busy.
8. **Independent check before delivery.** A separate inspector verifies the *real*
   artifact — the code and tests — not a summary from the maker.
9. **Smallest effort that works.** Triage picks the tier; do not silently escalate.
   If a fast lane is enough, use it and say so.

## Confidence grades

| Grade | Meaning | Ships? |
|-------|---------|--------|
| **A — Proven** | Backed by a passing test, a run, or an authoritative doc you checked. | Yes |
| **B — Reasoned** | Derived from verified facts or a same-team check. A real conclusion. | Yes (labeled) |
| **C — From memory** | Memory or a single unchecked source. | Held back until confirmed |
| **D — Guess** | A guess. | **Never ships** — set aside or flag as an explicit assumption |

## Effort tier (triage sets this first)

- **tiny** — quick, clear, low-risk → fast lane: triage → build → inspect.
- **normal** — ordinary task → core line.
- **full** — important or multi-part → full pipeline.
- **high-stakes** — risky or irreversible → full pipeline + extra scrutiny at research
  and inspection.

Default to the lighter lane; escalate for a reason, not out of habit.

## The line

```
triage → research → plan → build (+ self-review) → inspect → ship
                                       ▲                 │
                                       └──── fix-it ◀─────┘
```

- **Fix-it loop:** if inspection returns FIX-IT, fix every finding and **re-inspect
  from scratch** — the old report is never trusted. Cap at 3 rounds, then REJECT.
- **Ship only what passed**, with its proof: a confidence list, an assumptions list,
  and the receipts index.

## Receipts

Each real run writes to `runs/<UTC-timestamp>-<slug>/`:

```
00-triage.md       lane + effort + scope
01-research.md     facts, each with a source and a grade
02-plan.md         the written plan + team size + inspection plan
03-deliverable…    the actual change (or a summary + the diff)
05-inspection.md   the inspector's verdict (written by the inspector)
manifest.md        confidence list + assumptions + receipts index
```

The inspector reads the real deliverable directly — never a summary. That is what
makes the check independent.

## Knowledge base

Reference material lives in `sources/`. The `researcher` subagent owns this directory
and maintains `sources/library.md` as the navigator/index. When new external material
is needed (web pages, PDFs, books, project documents), the researcher distills the
*needed* knowledge into a new `sources/<topic>.md` and registers it in `library.md`
— it never dumps raw material into the repo. You can ask the assistant to add a
source from a URL or a local file at any time.

## How to run

Type `/lv1-coder <your task>` to run the whole line. Call a single station by loading
its skill or delegating to a subagent when you only need one step.
