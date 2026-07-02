---
name: lv1-writer-init
description: Set up, re-sync, or refactor a project into the canonical lv1-writer structure — inputs/ (interview + writing-instruction), manuscript/ (one file per chapter), sources/ library, feedback/, runs/, and the CLAUDE.md block. Reads the existing project first, pre-fills what it knows, and migrates any existing layout into the canonical one non-destructively after one confirmation (splitting a single-file book into chapter files). Safe to re-run. Trigger with "lv1-writer init", "set up lv1-writer", "initialize lv1-writer", or when starting in a folder that has no sources/library.md yet.
---

# lv1-writer — Setup (init)

Scaffold a project so `lv1-author` (the writer), `lv1-reviewer` (the judge), and
`lv1-address-feedback` (the responder) have a source library to cite, an `inputs/` folder
for user-authored material, a `manuscript/` folder for per-chapter prose, a `feedback/`
folder for reviews, and a recorded default tone and working mode. This is the **only**
init flow — the other skills do not run setup themselves.

**The canonical structure is the bible.** Init refactors whatever it finds into the
layout below — but **non-destructively**: it moves and copies, never deletes, archives any
original it supersedes, and makes any structural move only after the user confirms the
plan once.

```
inputs/        writing-instruction.md, intake.md        (user-authored)
manuscript/    one file per chapter (ch01.md …), _about.md, _archive/
sources/       library.md  + pdf/ word/ images/ web/
feedback/      user-review.md, editor-review.md  +  *-response.md (AI)
runs/<id>/     00-triage … 05-manifest  + decisions.md
```

Safe to re-run on an existing project: it syncs, updates, and fills gaps without
destroying user edits.

## Step 1 — Scan the existing project

Before asking anything, read the project to infer what's already known:

1. **`CLAUDE.md`** — extract the lv1-writer marker block
   (`<!-- lv1-writer:structure:start -->` … `<!-- lv1-writer:structure:end -->`). If
   present, it contains topic, angle, POV, tone profile, and working mode from the last
   init run. Treat these as the baseline inferred values.
2. **`manuscript/_about.md`** — read if the marker block is absent or incomplete; use
   it to infer topic and angle.
3. **`inputs/writing-instruction.md`** (also check the legacy location
   `manuscript/writing-instruction.md`) — read if present; use it to infer the tone
   profile and POV, and assess whether it is complete (has voice, structure, proof
   apparatus, and never-do sections) or thin/incomplete.
4. **`sources/library.md`** — note whether it exists and how many sources are indexed.

**Also map the existing layout for migration.** List every file and folder in the project
and classify each against the canonical structure, so Step 3 can refactor:
- Voice/instruction file in the wrong place (`manuscript/writing-instruction.md`,
  `writing-instruction.md` at root) → belongs in `inputs/`.
- Interview answers in the wrong place (`manuscript/intake.md`, `intake.md` at root) →
  belongs in `inputs/`.
- Prose: is it **one file per chapter** already, or a **single monolithic book file**
  (one large `.md`/`.docx`/`.txt` holding multiple chapters)? A monolith is a candidate
  for the chapter split in Step 3.
- Loose source files (`.pdf`, `.docx`, images, saved web pages) sitting outside
  `sources/` → belong under `sources/`.
- Stray review files → belong in `feedback/`.
- Anything you cannot confidently classify → leave it exactly where it is and list it for
  the user; never move a file you don't understand.

From these, form a confidence level for each of the four settings:
- **Confident** — found in the marker block or clearly inferable from files.
- **Guessed** — inferred but uncertain (e.g. no marker block, tone deduced from prose style).
- **Unknown** — no signal found.

## Step 2 — Confirm or ask

**If all four settings are Confident:** skip AskUserQuestion. State what was found and
proceed directly to Setup. The user can re-run init and answer differently to change any
setting.

**If any setting is Guessed or Unknown:** show one grouped round via AskUserQuestion
(or a single prompt if unavailable). Pre-fill each question with the inferred value as
the first/recommended option — label guessed values as "(AI guess)". Leave unknowns
blank. Each question accepts "Other" for free text or custom input:

1. **Topic & angle** — what is this book or project about, and the specific areas,
   perspectives, or questions it will explore. Pre-fill from the marker block or
   `_about.md` if found.
2. **Point of view** — how the author addresses the reader:
   - **First person (I / we)** — memoir, personal narrative, author as protagonist
   - **Third person** — "he / she / they"; journalistic or narrative distance
   - **Expert / authoritative** — "the evidence shows…"; analysis, guides, essays
   - **Instructional (you)** — direct address; how-to, self-help steps, coaching
3. **Tone style** — pick the *group* that fits; AI selects the best specific profile
   within it using topic, angle, and POV as tiebreakers:
   - **Inform & Argue** — academic · analytical-intelligence · popular-science · technical · persuasive
   - **Transform & Heal** — cognitive-behavior · mindset
   - **Story & Memoir** — narrative-nonfiction · personal-experience · love
   - **Creative Fiction** — literary · romance · childrens
4. **Working mode** — **auto** (AI researches and drafts autonomously wherever the genre
   allows) · **interactive** (involve me / interview me for personal material). AI resolves
   the concrete authorship approach from genre + this choice.

**Resolve the specific tone profile before running Setup.** From the confirmed group,
topic, angle, and POV, read `${CLAUDE_PLUGIN_ROOT}/references/tone-detect.md` and scan
`${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/` to pick the single best-fit profile. If the group
has only one plausible match, pick it directly. If tone is "decide per task", leave it
unresolved — per-task triage drives it instead.

## Step 3 — Refactor into the canonical structure (migration)

Using the layout map from Step 1, turn the existing project into the canonical structure.
**Always confirm before moving** and **never destroy**.

1. **Build the migration plan.** From the map, list the concrete moves/splits — e.g.
   "`manuscript/writing-instruction.md` → `inputs/writing-instruction.md`",
   "`my-book.md` (8 chapters detected) → split into `manuscript/ch01.md … ch08.md`,
   original archived to `manuscript/_archive/my-book.md`", "`/refs/study.pdf` →
   `sources/pdf/study.pdf` + library entry". If nothing needs moving (already canonical,
   or a fresh empty folder), say so and skip to Step 4.
2. **Detect chapter boundaries for any monolith.** For a single-file book, find chapter
   breaks from its headings (top-level `#`/`##`, or "Chapter N" / "Part N" lines).
   Produce a proposed split: chapter number, title, and first line of each.
3. **Confirm once.** Show the user the full plan (every move, and the proposed chapter
   split with detected titles) via AskUserQuestion or a single prompt, and get explicit
   approval before touching anything. If the user corrects the chapter boundaries, use
   their correction. Record the **plan and the user's verbatim confirmation** in
   `runs/<UTC-timestamp>-init/decisions.md`.
4. **Execute, non-destructively.**
   - **Move** misplaced files to their canonical home (move, don't copy-and-orphan).
   - **Split** a monolith by **copying** each chapter's text into `manuscript/chNN.md`
     (zero-padded, in order), then move the original intact into `manuscript/_archive/`.
     Never delete the original. If boundary detection is unreliable and the user can't
     confirm a clean split, leave the book as a single file and tell the user.
   - **Preserve content byte-for-byte** when moving/splitting prose — reformat nothing.
   - Anything unclassified stays put; report it.
5. After migration, the rest of Setup operates on the canonical paths.

## Step 4 — Run Setup

0. **Create the skeleton.** Ensure `inputs/`, `manuscript/`, `sources/`, `feedback/`, and
   `runs/` exist. Drop the review templates into `feedback/` if absent (read and write out
   their content; don't copy the asset files):
   `${CLAUDE_PLUGIN_ROOT}/assets/feedback-templates/user-review-template.md` →
   `feedback/user-review.md` and
   `${CLAUDE_PLUGIN_ROOT}/assets/feedback-templates/editor-review-template.md` →
   `feedback/editor-review.md` — but **only if no review file exists yet** (never
   overwrite a review the user has started). The `*-response.md` files are written by
   `lv1-address-feedback`, not by init.
1. **Sync the source library.** If `sources/library.md` doesn't exist, create it by
   writing the content of `${CLAUDE_PLUGIN_ROOT}/assets/library-template.md` into the file as
   fresh text. **Do not copy the asset file itself** — read its content and write it out
   so the new file is writable. After creating, confirm `sources/library.md` is writable
   (clear any read-only attribute if the host set one). Either way, run the **Library
   sync** from `${CLAUDE_PLUGIN_ROOT}/references/source-intake.md` — scan `sources/` for files
   not yet in the library and add them. Runs every time, not just on a fresh project.
2. **`manuscript/_about.md`** — write (or overwrite) with the confirmed topic and angle.
   This file always reflects the current project description after init runs.
3. **`inputs/writing-instruction.md`** — apply the following rules in order:
   - **Missing** → create it, seeded from the resolved tone profile
     (`${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/<id>.md`).
   - **Tone changed** (user picked a different profile this run) → regenerate it from
     the new profile. The old file was for a different voice; replace it.
   - **Exists, tone unchanged, but thin or incomplete** (missing voice, structure,
     proof-apparatus, or never-do sections) → enrich it: read the tone profile and
     fill the gaps, preserving any lines the user added that do not conflict.
   - **Exists, tone unchanged, and complete** → leave it untouched. The user's edits
     live there.
   - If tone is unresolved ("decide per task"), leave this file unwritten.
4. **`CLAUDE.md` (last).** If it doesn't exist, create from
   `${CLAUDE_PLUGIN_ROOT}/assets/claude-md-template.md`. If it exists, replace only the text
   between `<!-- lv1-writer:structure:start -->` and `<!-- lv1-writer:structure:end -->`
   with a fresh copy of that block; if those markers aren't present, append the block.
   Record the confirmed topic, angle, POV, resolved tone profile, and working mode inside
   that block. Never touch anything else in an existing `CLAUDE.md`.
5. **Assemble the full book once, if chapters exist.** If `manuscript/` now holds chapter
   files (e.g. after a split), build the full book per the "Assemble the full book" rule in
   `lv1-author` — an ordered, byte-for-byte concatenation written to the **project root** as
   `<book-title>.md` (slug of the recorded title), chapters only, no build marker. Skip on
   an empty project. Remind the user it is a generated build output — edit chapters and
   re-assemble, never edit the assembled book directly.

## Report back

State: what the scan found vs what the user confirmed (note any changes); **what the
migration moved, split, or archived** (or that nothing needed moving); the resolved tone
profile and which group it came from; POV; working mode; what was created vs updated vs
left alone; how many sources are now in the library (and how many newly added, if any);
the tone in 2–4 plain sentences distilled from `inputs/writing-instruction.md`; that
`CLAUDE.md` now reflects the current layout.

Tell the user: to change tone, re-run init and pick a different group — or edit
`inputs/writing-instruction.md` directly for fine-grained voice control. Available
profiles are in `${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/`.