<!-- lv1-compass:structure:start -->
## lv1-compass project structure

This project uses **lv1-compass** — an assistant + advisor for work, sharing one
constitution. Two skills:

- **lv1-assistant** (the maker) — produces work deliverables (analysis, report, memo, deck,
  model) through `triage → research → outline → draft → inspect → ship`. Describe what you
  want made, or hand over a source to file it into the library.
- **lv1-advisor** (the judge) — **review** an artifact, **challenge** a position with a
  sourced steelman, or **decide** a fork (it frames, weighs, recommends, and leaves the
  choice with you). Ask it to review, push back, or help you decide.

Project defaults (set at init):
- **Domain:** <recorded at init>
- **Default work register:** <analytical | neutral-professional | per-task>
- **Working mode:** <auto | interactive>

Layout:
- `discipline/constitution.md`, `discipline/working-lessons.md`, `discipline/readability.md` — the shared
  discipline both skills read first and both subagents are handed: the bar, A/B/C/D grades,
  the `[A]` test, failure modes, lessons, and the format/readability rules.
- `sources/library.md` — every kept source and graded claim (A/B/C/D). The spine: no factual
  claim ships, and no recommendation rests on a fact, without a block here.
- `sources/pdf/ word/ images/ web/` — the source files, created as needed.
- `agents/lv1-research.md`, `agents/lv1-inspect.md` — research and inspection, run as
  isolated subagents (delegated to by name); the assistant's `references/` and the advisor's
  mode contracts hold the inline station logic.
- `runs/<id>/` — receipts for each run (triage, research, outline, draft, inspection,
  manifest; or a review-feedback / challenge / decision memo from the advisor).

Re-running "lv1-compass init" is safe: it syncs `sources/library.md` against whatever's in
`sources/`, never deletes a graded source, and never touches this file outside this block.
<!-- lv1-compass:structure:end -->
