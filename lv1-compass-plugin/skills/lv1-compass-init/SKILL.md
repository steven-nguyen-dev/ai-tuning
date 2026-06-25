---
name: lv1-compass-init
description: Set up or re-sync the lv1-compass project structure — sources/ library, runs/, a default work register, and a CLAUDE.md block. Reads the existing project first and pre-fills what it already knows; only asks about what's uncertain. Safe to re-run. Trigger with "lv1-compass init", "set up lv1-compass", "initialize lv1-compass", or when starting in a folder that has no sources/library.md yet.
---

# lv1-compass — Setup (init)

Scaffold a project so `lv1-assistant` (the maker) and `lv1-advisor` (the judge) have a
library to cite, a place for receipts, and a recorded default. This is a **light** init —
no genre/tone questions, because the assistant has only two work registers, not a genre
system.

Safe to re-run on an existing project: it syncs, updates, and fills gaps without
destroying prior configuration.

## Step 1 — Scan the existing project

Before asking anything, read the project to infer what's already known:

1. **`CLAUDE.md`** — extract the lv1-compass marker block
   (`<!-- lv1-compass:structure:start -->` … `<!-- lv1-compass:structure:end -->`). If
   present, it contains the domain, default register, and working mode from the last
   init run. Treat these as the baseline inferred values.
2. **`runs/`** — if the marker block is absent or incomplete, scan up to 3 recent
   `runs/*/00-triage.md` files. The register recorded there signals what this project
   typically uses (analytical vs neutral-professional).
3. **`sources/library.md`** — note whether it exists and how many sources are indexed.

From these, form a confidence level for each of the three settings:
- **Confident** — found in the marker block or clearly consistent across runs.
- **Guessed** — inferred but uncertain (e.g. no marker block, register deduced from
  triage files).
- **Unknown** — no signal found.

## Step 2 — Confirm or ask

**If all three settings are Confident:** skip AskUserQuestion. State what was found and
proceed directly to Setup. The user can re-run init and answer differently to change any
setting.

**If any setting is Guessed or Unknown:** show one grouped round via AskUserQuestion
(or a single prompt if unavailable). Pre-fill each question with the inferred value as
the first/recommended option — label guessed values as "(AI guess)". Leave unknowns
blank. Each question accepts "Other" for free text or custom input:

1. **Project / domain** — what is this project's work about? Pre-fill from the marker
   block if found. (Free text via "Other"; offer "Decide per task" as an option.)
2. **Default work register** — **analytical** (reach and defend conclusions) ·
   **neutral-professional** (inform clearly) · **you decide** (pick per task in triage).
3. **Working mode** — **auto** (run autonomously, ask only on blocking ambiguity) ·
   **interactive** (involve me — and for decisions, expect the basis round).

## Step 3 — Run Setup

1. **Sync the source library.** If `sources/library.md` doesn't exist, create it by
   **writing the template's *content* into a new file** — read `assets/library-template.md`
   and write its text out as `sources/library.md`. **Do not copy the asset file itself**:
   installed plugin assets are read-only, and a file-copy inherits that read-only attribute,
   which would leave `library.md` unwritable so later runs can't append sources to the spine.
   After creating it, **confirm `sources/library.md` is writable** (if the host left a
   read-only attribute on it, clear it); the same applies to any project file built from an
   asset. Either way, run the **Library sync** in
   `../lv1-assistant/references/source-intake.md` — scan `sources/` for files not yet in
   the library and add them. Runs every time, not just on a fresh project.
2. **`runs/`** — create the directory if missing.
3. **`CLAUDE.md` (last).** If it doesn't exist, create it from
   `assets/claude-md-template.md`. If it exists, replace only the text between
   `<!-- lv1-compass:structure:start -->` and `<!-- lv1-compass:structure:end -->` with a
   fresh copy of that block; if the markers aren't present, append the block. Record the
   confirmed **domain**, **default register**, and **working mode** inside that block.
   Never touch anything else in an existing `CLAUDE.md`.

## Report back

State: what the scan found vs what the user confirmed (note any changes); domain /
default register / working mode; what was created vs updated vs left alone; how many
sources are now in the library (and how many newly added, if any); that `CLAUDE.md` now
reflects the current layout.

**Do not** start a task from here — to make a deliverable, describe it (`lv1-assistant`
takes over); to get something judged, ask (`lv1-advisor` takes over).
