# Triage — Reception

Sort the job before any real work starts, so nothing is over- or under-built. Don't
write the piece here. Sorting wrong is expensive in both directions: under-sort a
high-stakes piece and a bad fact ships unchecked; over-sort a quick note and you burn a
full pipeline on something that needed three sentences.

Write `runs/<id>/00-triage.md`:

```
# Triage — <task title>
- Lane: fast | full
- Effort: tiny | normal | full | high-stakes
- Deliverable: <what text, roughly how long, for whom, in what voice/register>
- Sources on hand: <what the user already has, or "none yet">
- Open questions: <blocking ambiguities, or "none">
- Scope (one paragraph): <…>

## Research contract (full / high-stakes only)
- Recency window: <date boundary — newest acceptable, and what counts as out-of-window>
- Required source types: <e.g. primary surveys, official statistics, local-language
  press, company filings — name what the topic actually demands>
- Local language(s) to search beyond English: <…, or "n/a">
- Exclusions / red lines: <sources or numbers not to anchor on, and why>
- Min distinct sources: <a target scaled to scope — the research station can't close
  below this without declaring the gap>
```

Understand the *real* ask (rule 1); name the scope precisely rather than gesturing at
it. Default to the lighter lane; escalate for a reason you could explain to the user.
Keep it short and decisive — this is a sort, not an essay.

The research contract exists so a *loose* brief still gets *tight* research. A demanding
brief (a fixed window, an excluded source, a named source-type) is itself what forces
the research station past convenient secondary numbers — when the user doesn't impose
that, triage manufactures the equivalent here. Skip the contract on `tiny`/`normal`; it
earns its keep only when depth matters.
