---
name: lv1-compass-init
description: Set up the lv1-compass project structure in the current folder — the sources/ library, runs/, a default work register, and a CLAUDE.md block. Light and interactive; no genre/tone questions. Safe to re-run. Trigger with "lv1-compass init", "set up lv1-compass", "initialize lv1-compass", or when starting in a folder that has no sources/library.md yet.
---

# lv1-compass — Setup (init)

Scaffold a project so `lv1-assistant` (the maker) and `lv1-advisor` (the judge) have a
library to cite, a place for receipts, and a recorded default. This is a **light** init —
no genre/tone questions, because the assistant has only two work registers, not a genre
system.

## Ask first — one short grouped round

Before creating files, ask one grouped round (AskUserQuestion if available, else a single
prompt). Each accepts "you decide":

1. **Project / domain** — what is this project's work about? (free text, or "decide later
   per task")
2. **Default work register** — **analytical** (reach and defend conclusions) ·
   **neutral-professional** (inform clearly) · "you decide" (pick per task in triage).
3. **Working mode** — **auto** (run autonomously, ask only on blocking ambiguity) ·
   **interactive** (involve me — and for decisions, expect the basis round).

## Then run Setup

1. **Sync the source library.** If `sources/library.md` doesn't exist, write
   `assets/library-template.md` first. Either way, run the **Library sync** in
   `../lv1-assistant/references/source-intake.md` — scan `sources/` for files not yet in
   the library and add them. Runs every time, not just on a fresh project.
2. **`runs/`** — create the empty receipts directory if missing.
3. **`CLAUDE.md` (last).** If it doesn't exist, create it from
   `assets/claude-md-template.md`. If it exists, replace only the text between
   `<!-- lv1-compass:structure:start -->` and `<!-- lv1-compass:structure:end -->` with a
   fresh copy of that block; if the markers aren't present, append the block. Record the
   chosen **domain**, **default register**, and **working mode** inside that block. Never
   touch anything else in an existing `CLAUDE.md`.

Re-running init is safe: it syncs the library against `sources/`, never deletes a graded
source, and never touches `CLAUDE.md` outside its marker block.

## Report back

State: the chosen domain / default register / working mode; what was created vs left alone;
how many source files were found and how many newly added to the library (or "none yet");
and that `CLAUDE.md` now reflects the layout. **Do not** start a task from here — to make a
deliverable, describe it (the `lv1-assistant` skill takes over); to get something judged,
ask (`lv1-advisor` takes over).
