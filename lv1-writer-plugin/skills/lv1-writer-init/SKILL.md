---
name: lv1-writer-init
description: Set up or re-sync the lv1-writer project structure — sources/ library, manuscript/, writing tone, and CLAUDE.md block. Reads the existing project first and pre-fills what it already knows; only asks about what's uncertain. Safe to re-run. Trigger with "lv1-writer init", "set up lv1-writer", "initialize lv1-writer", or when starting in a folder that has no sources/library.md yet.
---

# lv1-writer — Setup (init)

Scaffold a project so `lv1-author` (the writer) and `lv1-reviewer` (the judge) have a
source library to cite, a manuscript folder for prose, and a recorded default tone and
working mode. This is the **only** init flow — `lv1-author` does not run setup itself.

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
3. **`manuscript/writing-instruction.md`** — read if present; use it to infer the tone
   profile and POV, and assess whether it is complete (has voice, structure, proof
   apparatus, and never-do sections) or thin/incomplete.
4. **`sources/library.md`** — note whether it exists and how many sources are indexed.

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

## Step 3 — Run Setup

1. **Sync the source library.** If `sources/library.md` doesn't exist, create it by
   writing the content of `${CLAUDE_PLUGIN_ROOT}/assets/library-template.md` into the file as
   fresh text. **Do not copy the asset file itself** — read its content and write it out
   so the new file is writable. After creating, confirm `sources/library.md` is writable
   (clear any read-only attribute if the host set one). Either way, run the **Library
   sync** from `${CLAUDE_PLUGIN_ROOT}/references/source-intake.md` — scan `sources/` for files
   not yet in the library and add them. Runs every time, not just on a fresh project.
2. **`manuscript/_about.md`** — write (or overwrite) with the confirmed topic and angle.
   This file always reflects the current project description after init runs.
3. **`manuscript/writing-instruction.md`** — apply the following rules in order:
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

## Report back

State: what the scan found vs what the user confirmed (note any changes); the resolved
tone profile and which group it came from; POV; working mode; what was created vs updated
vs left alone; how many sources are now in the library (and how many newly added, if any);
the tone in 2–4 plain sentences distilled from `manuscript/writing-instruction.md`; that
`CLAUDE.md` now reflects the current layout.

Tell the user: to change tone, re-run init and pick a different group — or edit
`manuscript/writing-instruction.md` directly for fine-grained voice control. Available
profiles are in `${CLAUDE_PLUGIN_ROOT}/