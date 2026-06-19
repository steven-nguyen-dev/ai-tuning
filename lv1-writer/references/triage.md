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
```

Understand the *real* ask (rule 1); name the scope precisely rather than gesturing at
it. Default to the lighter lane; escalate for a reason you could explain to the user.
Keep it short and decisive — this is a sort, not an essay.
