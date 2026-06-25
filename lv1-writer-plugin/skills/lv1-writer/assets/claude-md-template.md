<!-- lv1-writer:structure:start -->
## lv1-writer project structure

This project uses the **lv1-writer** Cowork skill — a sourced-writing pipeline
(triage, research, outline, draft, independent inspection). Say "lv1-writer
<task>" to run it, or hand over a source file to file it into the library.

- `sources/library.md` — every kept source and graded claim (A/B/C/D). The spine: no
  factual claim in the manuscript ships without a block here.
- `sources/pdf/ word/ images/ web/` — the source files themselves, created as needed.
- `manuscript/writing-instruction.md` — this project's default prose tone, seeded from
  a tone profile at init. Edit it directly to change the voice; the skill won't
  overwrite it once it exists.
- `assets/tone-profiles/*.md` — the genre voice library (analytical-intelligence,
  popular-science, cognitive-behavior, memoir, romance…). Triage detects the genre and
  picks one per task; the profile decides voice, structure, and whether source-grading
  applies (fiction switches it off for craft contracts).
- `manuscript/intake.md` — for interview-driven / hybrid genres: the user's lived
  material, gathered in batched rounds. A first-class source the draft cites, like the
  library.
- `manuscript/*.md` — the drafts.
- `agents/lv1-research.md`, `agents/lv1-inspect.md` — the research and inspection
  stations, run as isolated subagents. Their contracts live here (the orchestrator
  delegates by name); `references/` holds the inline-station contracts (triage,
  source-intake, outline, draft) plus the support contracts (tone-detect,
  structure-shapes, interview, working-lessons).
- `runs/<id>/` — receipts for each pipeline run (triage, research, outline, draft,
  inspection, manifest).

Re-running "lv1-writer init" is safe: it syncs `sources/library.md` against whatever's
actually in `sources/`, but never overwrites `writing-instruction.md`, never deletes a
graded source, and never touches anything in this file outside this block.
<!-- lv1-writer:structure:end -->
