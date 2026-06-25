# Draft — Assembly

Write the real deliverable, at full effort, following the outline. Save it as **markdown**
to `manuscript/` and a copy to `runs/<id>/03-draft.md`. Markdown is the only output of this
pipeline — see "Output is markdown" below.

## Compose the voice (in precedence order)

Read `00-triage.md` for the work register and rigor tier, then compose the voice:

1. **Explicit user instruction** for this task always wins.
2. **The work register** named in `00-triage.md` — `analytical` or `neutral-professional`.
   Read `references/register.md` and follow that voice's stance, shape, and apparatus.

Also read `core/working-lessons.md` before drafting. Write **out of the AI register** — no
generic filler, empty pivots, hype, or reflexive hedging.

## Apparatus by rigor level

Match how much proof machinery the deliverable carries to the rigor tier set in triage:

- **full / high-stakes** — open with a **Methodology & enforcement** section (recency
  window, source spine, enforcement rule, red-line exclusions, from the triage research
  contract); carry **inline A/B grades** on every factual claim in the body; **surface
  every DECLARED GAP** from `01-research.md` in-text where the number would go, phrased as
  an explicit refusal to invent; and **append the proof package to the delivered file
  itself** (confidence table, assumptions, source index, data-limits) — not only to
  `runs/<id>/05-manifest.md`.
- **normal** — grades in a closing notes/appendix, inline only on load-bearing claims;
  methodology compressed to a one-paragraph "About the sources" note.
- **tiny** — grades available in `sources/library.md`; keep the deliverable clean.

## Format follows content

Before writing each section, pick its presentation form from the *content type*, not a
fixed template:

- **Table** — multiple data points, stats, or numbers that invite comparison.
- **Bullets** — a list of genuinely discrete ideas or key points.
- **Dashes / arrows** (`A → B → C`) — a loop, sequence, pipeline, or cause-effect chain.
- **Prose** — content to be reasoned through (an argument, a synthesis, a recommendation).

A deliberate decision point ("what shape does this content want?"), not a license to
over-format. `analytical` uses tables/arrows freely and compresses; `neutral-professional`
structures for scanning but doesn't bullet prose that should read as prose. The inspector
flags both under-formatting (a wall of prose hiding a comparison that wants a table) and
over-formatting (bullets where an argument needed to be made).

## Output is markdown

This station emits **markdown only** — never a `docx`/`pptx`/`xlsx`. That is deliberate:
the independent inspector has only Read/Grep/Glob and must judge the *real* shipped
artifact, which it can do for markdown but not for a binary office file. Write the full
deliverable — sections, tables, the apparatus the rigor tier calls for, the appended proof
package — as markdown.

If the user needs an office file, the orchestrator runs the conversion as an **explicit
step after inspection passes** (SKILL.md step 8), on this approved markdown. For a deck,
shape the markdown so the conversion is clean: one idea per section/slide, with
grades/sources in a notes block rather than on the visible face. The conversion is format
only — it adds no claim.

## Honesty (non-negotiable)

1. **Every factual claim** carries a confidence grade and ties to a source in
   `sources/library.md`. No source, no claim. "D" guesses never ship.
2. **Mark interpretation as interpretation** (rule 6). Your reading, argument, or
   recommendation is honest judgment — never disguise it as established fact.
3. **Show the working** for any numeric or analytical claim, or don't make the precise
   claim (`working-lessons.md` §3).
4. **Don't change scope quietly.** If new material breaks the outline, stop, note it, flag
   for re-plan.

## Self-review (the best-self pass) before handing off to inspection

Check: clarity (anything muddy or over-hedged?); specificity where it matters; the whole
scope covered; a stronger framing left on the table?; grades and sources real and
consistent?; a genuine, true "wow" the reader didn't ask for — or merely adequate? Fix what
you find. This is your last chance before the inspection pass, which owes the draft nothing.

## On a FIX-IT send-back

Read `04-inspection.md`, address **every** finding, note exactly what changed, and don't
argue with the findings — the inspector's job is to be harder to satisfy than you are.

Report a one-line confidence count (e.g. "18 claims: 13×A, 4×B, 1×C held back").
