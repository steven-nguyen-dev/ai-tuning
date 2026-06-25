# Tone profile — analytical-intelligence

- **Genre:** intelligence / market / strategy report; IT-industry analysis (IT-tuned variant below)
- **Register:** decisive, epistemic, hypothesis-driven — the author argues, never merely narrates
- **Proof apparatus:** **full** — inline A/B grades, a methodology/enforcement section up front, mandatory steelman, and a proof appendix that ships *inside* the document
- **Default authorship:** AI-autonomous
- **Structure shape:** hypothesis-driven (see `references/structure-shapes.md`)
- **Density & pacing:** **compress** — lead with tables/lists; every paragraph earns its space; no narrative throat-clearing before the finding

> An *audited intelligence memorandum*, not a market overview. A reader — or an
> independent auditor — should be able to pick up the document and re-verify every
> number without asking the author a single question. Read order: Requirements → Truth
> → Methodology → Structure → Honesty → Review. **If two rules conflict, Truth wins.**

---

## Spine & shape (hypothesis-driven)

- **Plant one central mechanism up front** — a single sentence the whole piece turns
  on (e.g. "AI drives the cost of plain code toward zero while raising the marginal
  value of senior judgment"). State it at the end of the executive summary and tell
  the reader to keep it in their pocket.
- **Every section is a lens, not a slice.** Open each with the same question — "what
  does the mechanism do to *this*?" (demand, salary, layoffs, hiring…) — so sections
  compound into one argument instead of sitting side by side.
- **Close each section with a synthesis beat** — "winners vs losers", "what shifted",
  or an equivalent — never a flat recap.
- **End the body with a steelman** (see Honesty) before recommendations.
- Lead with the thesis; a reader in a hurry should get the whole argument from §1.

## Truth & confidence (absolute)

- **Grade every factual claim inline** with `[A]`/`[B]`/`[C]` at the point it appears
  in the prose, not only in an appendix. `[A]` = primary source **present in this
  session** (fetched/read this run, handed over, or returned by a tool call you made) —
  recall from training is **not** `[A]`; `[B]` = sound reasoning from verified material or
  a reputable secondary citing a primary; `[C]` = single unchecked/from memory, ships only
  with an explicit caveat; `[D]` = guess, **never ships**.
- **A label is a reason to commit harder, not a hedge.** When the data points one way,
  say so plainly. "This raises questions about X" when the data already answered X is
  lying by understatement. Neutral ≠ timid.
- **A numeric claim is not a claim until the arithmetic is shown.** If you give a
  percentage, ratio, or crossover point, show the working or don't give the precise
  number.
- **Never fabricate** a citation, figure, date, or page number. A made-up source
  survives unnoticed until a reader checks it — worse than a declared gap.

## Methodology & enforcement (open the document with this)

For `full`/`high-stakes` work, a short methodology section is the second thing the
reader sees, stating:

- **Analysis window** — the date boundary; what counts as in-window vs out-of-window.
- **Enforcement rule** — every headline number must be sourced inside the window.
  A pre-window baseline ships **only** paired with an in-window comparison point in
  the same paragraph and a verdict word ("stable"/"shifted"/"polarized"). A metric
  with no in-window comparison is **dropped, not guessed**.
- **Red-line exclusions** — sources or numbers you will not anchor on, and why
  (e.g. a survey whose methodology you can't verify). Name them so the reader sees the
  discipline.
- **Source spine** — the handful of primary reports the analysis rests on.

## Honesty — steelman, gaps, conflicts

- **Steelman is mandatory** wherever the piece reaches a contested conclusion. Build
  the *strongest* version of the opposing case from real, sourced data (not a
  strawman), then show why the conclusion still stands — or revise it. Steelman ≠
  false balance: the point is to stress-test the thesis, not to avoid committing.
- **Declare gaps in-text**, at the point the reader expects the missing number, phrased
  as an explicit refusal to invent ("in-window data does not separate X by tier; we
  present three tiers and note lead-level X is not separable — we do not fabricate
  it"). Repeat them in a data-limits appendix entry. Never paper a gap over with a
  merged or approximate table presented as complete.
- **Resolve conflicts, don't report them flat.** When a widely-cited or viral number
  contradicts a primary record, surface both, reconcile them (definitional mismatch,
  wrong reference frame), and cite the dated correction. Keep genuinely conflicting
  sources side by side rather than silently picking the convenient one.

## Structure & format

- Executive summary of numbered findings → methodology → per-section lens analysis →
  steelman + synthesis → recommendations → proof appendix.
- **Format follows content** (see `references/draft.md`): tables for comparable
  data/stats; bullets for discrete lists; dashes/arrows (`A → B → C`) for sequences and
  cause-effect; prose for the argument connecting them. This profile leans on tables
  and structured markup freely.
- The **proof appendix ships inside the document**: a confidence table (every
  quantitative claim, source, date, grade), an assumptions list, a source index with
  in-window flags, and the declared data-limits.

## Tone & voice

- Decisive and analytical; the author owns the information. Strip timid hedges
  ("may suggest", "could indicate") when the evidence is firm.
- Concept-cutting assertions are welcome ("this is polarization, not decline") when
  the data backs them.
- Professional and neutral-with-evidence — not marketing, not hype. Weight comes from
  sourcing and logic, not adjectives.

## IT-tuned variant

For IT-industry analysis, keep everything above and bias toward: skill-demand splits
(AI/Data vs legacy stacks), salary polarization by AI premium and seniority tier,
restructuring vs layoff distinctions, hiring-bar shifts, and per-segment
winners/losers (fresher / junior / mid / senior / lead). Prefer local-language primary
sources (job platforms, company filings, local press) over English aggregators.

## Self-review before inspection

Check: is the central mechanism stated early and applied in every section? Is every
number graded inline and its arithmetic shown? Is the methodology section present with
window + enforcement + red lines? Is there a real steelman, not false balance? Are all
gaps declared in-text, none papered over? Are conflicts reconciled, not flat? Does the
proof appendix let a reader re-verify unaided? Fix what you find; the inspector owes
the draft nothing.
