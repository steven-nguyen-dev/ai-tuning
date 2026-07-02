# Outline — Design

An outline you only say out loud is worthless — you can't check a draft against a plan
that only existed in conversation, and neither can the inspection pass later. Write it
down so it can be checked.

Read `00-triage.md` (for the tone profile, **structure shape**, and authorship
approach), `01-research.md` (if it exists), `sources/library.md`, the chosen
`${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/<id>.md` and `${CLAUDE_PLUGIN_ROOT}/references/structure-shapes.md`, and
`inputs/writing-instruction.md` (if it exists), then write `runs/<id>/02-outline.md`:

```
# Outline — <task title>

## Shape
<sections in order, the through-line/argument, target length and voice>
<follow the structure shape from triage:>
<- descriptive: independent slices, each advancing the through-line>
<- hypothesis-driven: state the central thesis/mechanism up front; show how EACH
   section applies it as a lens; end with a synthesis (winners/losers / "what this
   means"), not a flat recap>

## Steelman (required when the conclusion is contested)
<the strongest opposing case to plan for, and where it will be faced in the body>

## Evidence map
<which library claims (or intake.md material, for interview-driven/hybrid) support which
section — so no section is left unsourced>

## Inspection plan
- Checks: facts vs library · interpretation honestly labeled · coherence/voice · scope ·
  coverage (each headline claim to its own distinct source; no obvious source missed) ·
  tone matches the chosen profile · format follows content · steelman present if contested
- The inspection pass reads the real draft and sources/library.md directly, not a
  summary of either

## Risks
<thin spots, weak evidence, the trigger that forces a re-plan>
```

For a **hypothesis-driven** shape, the outline must name the central thesis/mechanism
and show each section's lens on it — an outline that lists topics without the through-line
defeats the shape. When the piece reaches a **contested conclusion**, the outline must
include a steelman beat; the draft and inspector both expect it.

Don't pad the outline to look thorough — every extra section or claim is a place a
fact can drift unchecked later. Report the path and a one-sentence summary of the
shape.
