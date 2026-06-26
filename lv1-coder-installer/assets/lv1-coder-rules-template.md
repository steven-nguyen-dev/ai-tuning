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
10. **Token efficiency.** Run only the stations triage approved. Read only the files
    the task needs. Do not sweep the whole repo when a targeted read suffices.

## Confidence grades

| Grade | Meaning | Ships? |
|-------|---------|--------|
| **A — Proven** | Backed by a passing test, a run, or an authoritative doc you checked. | Yes |
| **B — Reasoned** | Derived from verified facts or a same-team check. A real conclusion. | Yes (labeled) |
| **C — From memory** | Memory or a single unchecked source. | Held back until confirmed |
| **D — Guess** | A guess. | **Never ships** — set aside or flag as an explicit assumption |

## Effort tier (triage sets this first)

- **tiny** — quick, clear, low-risk → fast lane: triage → build → inspect.
- **normal** — ordinary task → research and plan only when triage explicitly triggers them.
- **full** — important or multi-part → full pipeline.
- **high-stakes** — risky or irreversible → full pipeline + extra scrutiny at research
  and inspection.

Default to the lighter lane; escalate for a reason, not out of habit.

