<!-- lv1-writer:structure:start -->
## lv1-writer project structure

This project uses **lv1-writer** — a sourced-writing pipeline (triage, research,
outline, draft, independent inspection). Describe a writing task to run `lv1-author`,
or hand over a source file to file it into the library.

Project defaults (set at init):
- **Topic & angle:** <recorded at init>
- **Point of view:** <first-person | third-person | expert-authoritative | instructional>
- **Tone profile:** <profile name, e.g. narrative-nonfiction>
- **Working mode:** <auto | interactive>

Layout:
- `sources/library.md` — every kept source and graded claim (A/B/C/D). The spine: no
  factual claim in the manuscript ships without a block here.
- `sources/pdf/ word/ images/ web/` — the source files themselves, created as needed.
- `manuscript/writing-instruction.md` — this project's default prose tone, seeded from
  a tone profile at init. Edit it directly for fine-grained voice control; re-run
  "lv1-writer init" and pick a different tone group to regenerate it from a new profile.
- `manuscript/_about.md` — the project description (topic and angle), updated on each
  init run.
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

Re-running "lv1-writer init" is safe: it syncs `sources/library.md`, updates
`_about.md` and this block with the current settings, and only touches
`writing-instruction.md` if the tone changed or it is incomplete.
<!-- lv1-writer:structure:end -->
