# Triage — Reception

Sort the job before any real work starts, so nothing is over- or under-built. Don't write
the deliverable here. Sorting wrong is expensive both ways: under-sort a high-stakes
analysis and a bad fact ships unchecked; over-sort a quick note and you burn a full
pipeline on something that needed three sentences.

The assistant sets **one knob (rigor) + a register + a structure shape** — there is no
genre/tone detection, and **no output-format knob: every deliverable is markdown.** Write
`runs/<id>/00-triage.md`:

```
# Triage — <task title>
- Lane: fast | full
- Rigor tier: tiny | normal | full | high-stakes   (gates which stations run — see core/constitution.md tier table)
- Work register: analytical | neutral-professional   (see references/register.md)
- Structure shape: descriptive | hypothesis-driven    (see below)
- Deliverable: <what it is, roughly how long/large, for whom>
- Sources on hand: <what the user already has, or "none yet">
- Open questions: <blocking ambiguities, or "none">
- Scope (one paragraph): <…>

## Research contract (full / high-stakes only)
- Recency window: <date boundary — newest acceptable, and what counts as out-of-window>
- Required source types: <primary surveys, official statistics, local-language press,
  filings — name what the topic actually demands>
- Local language(s) to search beyond English: <…, or "n/a">
- Exclusions / red lines: <sources or numbers not to anchor on, and why>
- Min distinct sources: <a target scaled to scope — research can't close below this
  without declaring the gap>
```

Understand the *real* ask (rule 1); name the scope precisely rather than gesturing at it.
Default to the lighter lane; escalate for a reason you could explain to the user. Keep it
short and decisive — this is a sort, not an essay.

If you set **high-stakes**, note in the triage file that an outline-approval beat is due
before drafting (SKILL.md step 4): the user signs off on the plan before the pipeline
commits effort to a draft. This is the single human gate in the assistant pipeline; lighter
tiers don't carry it.

## Choosing the knobs

- **Rigor tier** — see the tier→station table in `core/constitution.md`. The tier gates
  which stations actually run (tiny skips research/outline/inspect; high-stakes adds a
  second research pass and a re-inspection). Default light; escalate for a reason.
- **Work register** — `analytical` when the job is to reach and defend a conclusion
  (strategy, market read, recommendation-backing analysis); `neutral-professional` when the
  job is to inform clearly without taking a side (status brief, summary, documentation).
  When in doubt, default `neutral-professional` and note it.
- **Structure shape** — `descriptive` (independent slices, each advancing the through-line —
  good for overviews and briefs) or `hypothesis-driven` (state one central mechanism up
  front, use it as a lens through every section, close with a synthesis — good for
  strategy/analysis/contested topics). `analytical` register usually pairs with
  `hypothesis-driven`; `neutral-professional` usually with `descriptive`.

## The research contract

It exists so a *loose* brief still gets *tight* research. A demanding brief (a fixed
window, an excluded source, a named source-type) is what forces research past convenient
secondary numbers — when the user doesn't impose that, triage manufactures the equivalent
here. Skip it on `tiny`/`normal`; it earns its keep only when depth matters.
