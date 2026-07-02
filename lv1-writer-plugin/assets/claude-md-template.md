<!-- lv1-writer:structure:start -->
## lv1-writer project structure

This project uses **lv1-writer** — a sourced-writing pipeline (triage, research,
outline, draft, independent inspection). Describe a writing task to run `lv1-author`,
hand over a source file to file it into the library, or drop a review into `feedback/`
and run `lv1-address-feedback` to get a verified response and improvement plan.

Project defaults (set at init):
- **Topic & angle:** <recorded at init>
- **Point of view:** <first-person | third-person | expert-authoritative | instructional>
- **Tone profile:** <profile name, e.g. narrative-nonfiction>
- **Working mode:** <auto | interactive>

This folder layout is the canonical structure. Re-running "lv1-writer init" refactors
the project into it (non-destructively, after a single confirmation):

- `inputs/` — **user-authored inputs.**
  - `inputs/writing-instruction.md` — this project's default prose tone, seeded from a
    tone profile at init. Edit it directly for fine-grained voice control; re-run
    "lv1-writer init" and pick a different tone group to regenerate it from a new profile.
  - `inputs/intake.md` — for interview-driven / hybrid genres: the user's lived material,
    gathered in batched rounds. A first-class source the draft cites, like the library.
- `manuscript/` — the prose. **One file per chapter** (e.g. `ch01.md`, `ch02.md`) is the
  source of truth; the pipeline drafts and inspects **one chapter at a time** rather than
  loading the whole book. `manuscript/_about.md` holds the project description (topic and
  angle). If init split a single-file book into chapters, the original is archived under
  `manuscript/_archive/` — nothing is deleted.
- `<book-title>.md` (project root) — the **assembled full book**, a generated build output
  named after the book's title (say "assemble the book" to rebuild it from the chapters).
  Never edit it by hand — edit the chapters in `manuscript/` and re-assemble.
- `sources/library.md` — every kept source and graded claim (A/B/C/D). The spine: no
  factual claim in the manuscript ships without a block here.
- `sources/pdf/ word/ images/ web/` — the source files themselves, created as needed.
- `feedback/` — human reviews and AI responses.
  - `user-review.md`, `editor-review.md` — **you write these** (a review of the draft).
  - `user-review-response.md`, `editor-review-response.md` — **AI writes these**:
    `lv1-address-feedback` verifies each point against the real draft + sources, grades
    it, and writes a prioritized improvement plan. It does not rewrite the manuscript —
    accepted items are handed to `lv1-author`.
- `runs/<id>/` — receipts for each pipeline run (triage, research, outline, draft,
  inspection, manifest) **plus `decisions.md`** — the log of how the run was steered (AI
  reasoning behind each call and your confirmations, captured verbatim).
- `agents/lv1-research.md`, `agents/lv1-inspect.md` — the research and inspection
  stations, run as isolated subagents. Their contracts live there (the orchestrator
  delegates by name); `references/` holds the inline-station contracts (triage,
  source-intake, outline, draft) plus the support contracts (tone-detect,
  structure-shapes, interview, working-lessons).

Re-running "lv1-writer init" is safe: it refactors the project into the layout above,
syncs `sources/library.md`, updates `_about.md` and this block with the current settings,
and only touches `inputs/writing-instruction.md` if the tone changed or it is incomplete.
It never deletes a source, a draft, or a user edit.
<!-- lv1-writer:structure:end -->
