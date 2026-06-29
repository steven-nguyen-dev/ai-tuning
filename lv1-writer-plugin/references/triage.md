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

## Genre & tone
- Detected genre: <from ${CLAUDE_PLUGIN_ROOT}/references/tone-detect.md, or "UNRESOLVED — asking" if ambiguous>
- Tone profile: <profile id at ${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/, or "project default (writing-instruction.md)">
- Proof-apparatus level: <none (fiction) | low | medium | full> — scaled by genre + effort
- Structure shape: <descriptive | hypothesis-driven> (see ${CLAUDE_PLUGIN_ROOT}/references/structure-shapes.md)
- Working mode: <auto | interactive> (from init / the user)
- Authorship approach: <AI-autonomous | interview-driven | hybrid> — resolved from genre + working mode
- Voice/apparatus composition: <none | e.g. "Voice: academic · Apparatus: experience-truth
  (lived spine) — composed, not stock academic"; set when an explicit genre's apparatus
  contradicts the material, per tone-detect.md "When voice and material disagree">
- Why this profile/approach: <one line; or the one question to ask the user if ambiguous>

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

For the **Genre & tone** block, read `${CLAUDE_PLUGIN_ROOT}/references/tone-detect.md` and follow its
detection signal order and taxonomy; read `${CLAUDE_PLUGIN_ROOT}/references/structure-shapes.md` to choose
the shape. The genre is decisive downstream: it picks the voice *and* whether the
A/B/C/D grading and proof package apply at all (fiction switches them off for craft
contracts instead). When the genre is genuinely ambiguous, ask one batched round
rather than guessing. Resolve the authorship approach from the genre's default
authorship and the user's auto/interactive working mode (see `${CLAUDE_PLUGIN_ROOT}/references/interview.md`).
If an explicitly requested genre's apparatus contradicts the supplied material (e.g. an
academic paper about the user's own divorce), follow tone-detect.md's "When voice and
material disagree" — compose voice + material-appropriate apparatus, record it in the
**Voice/apparatus composition** line, and a lived spine pushes authorship toward
interview-driven/hybrid.

The research contract exists so a *loose* brief still gets *tight* research. A demanding
brief (a fixed window, an excluded source, a named source-type) is itself what forces
the research station past convenient secondary numbers — when the user doesn't impose
that, triage manufactures the equivalent here. Skip the contract on `tiny`/`normal`; it
earns its keep only when depth matters.
