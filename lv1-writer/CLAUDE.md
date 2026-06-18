# lv1-writer — Operating rules

The shared contract for every writing task in this project. The `/lv1` pipeline and
every skill and subagent inherit these rules. They are not optional. Keep this file
small — it loads into every context.

## The bar

> **Impressive AND true.** Writing that dazzles but is false fails. Writing that is true
> but thin also fails. Ship only what clears both, backed by sources a reader can check.

## Core rules

1. **Understand the real ask first.** Pin down what is wanted — kind of text, length,
   audience, voice — and the limits, before planning.
2. **Check, don't trust memory.** Gather real material first — read the sources, search
   the web. Never write factual claims from memory alone.
3. **Plan in writing.** The outline is a visible artifact. If it changes, say so.
4. **Label confidence** on every factual claim (A/B/C/D below). A label is a reason to
   commit *harder*, not a hedge — never soften a finding the evidence supports. If you
   make a numeric or analytical claim, show the working, or don't make the precise claim.
5. **Source every factual claim** against the source library. No source, no claim.
6. **Mark interpretation as interpretation.** Analysis, reading, argument, and creative
   choices are *your* judgment — present them honestly as such. They don't need an
   external citation, but they must never be dressed up as established fact.
7. **Hold the scope, and steelman.** Don't quietly add or drop work. Before a conclusion
   that favors a view, state the strongest case against it, then decide if it still stands.
8. **Smallest team that works**, and an **independent check before delivery** — a
   separate inspector verifies the real draft against the source library, not a summary.

## Confidence grades

| Grade | Meaning | Ships? |
|-------|---------|--------|
| **A — Proven** | Backed by a primary source you fetched and read. | Yes |
| **B — Reasoned** | Derived with sound logic from verified material. A real conclusion. | Yes (labeled) |
| **C — From memory** | Memory or a single unchecked source. | Held back until confirmed |
| **D — Guess** | A guess. | **Never ships** — set aside or flag as an explicit assumption |

(Confidence grades apply to *factual* claims. Interpretation and creative prose are
governed by rule 6, not by citation.)

## The source library — the spine of this pack

Everything factual traces back to `sources/library.md`: each kept source, and each
claim drawn from it with a citation and a grade. Material arrives two ways — handed to
you (PDF, Word, image) via `source-intake`, or found via `research`. The inspector
checks the draft against this library.

## Effort tier (triage sets this first)

- **tiny** — short, clear → fast lane: triage → draft → inspect.
- **normal** — ordinary piece → core line.
- **full** — important or multi-part → full pipeline.
- **high-stakes** — published, contested, or high-visibility → full pipeline + extra
  scrutiny at research and inspection.

Default to the lighter lane; escalate for a reason.

## The line

```
triage → (source-intake / research) → outline → draft (+ self-review) → inspect → ship
                                                       ▲                    │
                                                       └────── fix-it ◀──────┘
```

- **Fix-it loop:** if inspection returns FIX-IT, fix every finding and **re-inspect
  from scratch** — the old report is never trusted. Cap at 3 rounds, then REJECT.
- **Ship only what passed**, with its proof: a confidence list, an assumptions list,
  and the receipts index.

## Receipts

Each real run writes to `runs/<UTC-timestamp>-<slug>/`:

```
00-triage.md       lane + effort + deliverable + audience/voice
01-research.md     findings, each with a source and a grade
02-outline.md      the written plan + team size + inspection plan
03-draft.md        the actual prose (also saved in manuscript/)
05-inspection.md   the inspector's verdict (written by the inspector)
manifest.md        confidence list + assumptions + receipts index
```

The inspector reads the real draft and the source library directly — never a summary.

## How to run

Type `/lv1 <your task>` to run the whole line. Hand a source file to `source-intake` to
file it into the library. Call any single station when that's all you need.
