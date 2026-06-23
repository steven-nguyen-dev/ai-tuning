---
name: lv1-writer-init
description: Set up the lv1-writer project structure in the current folder — the sources/ library, manuscript/, a default writing tone, and CLAUDE.md. Safe to re-run. Trigger with "lv1-writer init", "set up lv1-writer", "initialize lv1-writer", or when starting lv1-writer in a folder that has no sources/library.md yet.
---

# lv1-writer — Setup (init)

Run the **lv1-writer** skill's **Setup** flow in the current project folder. This is a
thin entry point — the authoritative Setup steps live in the main `lv1-writer` skill's
`## Setup` section; follow them there, in order:

1. Sync the source library (`sources/library.md`) against whatever files are in
   `sources/` — this runs every time, not just on a fresh project.
2. Create `manuscript/_about.md` if missing.
3. Create `manuscript/writing-instruction.md` if missing — **never overwrite it** once it
   exists (that's where the user's tone edits live).
4. Create or update `CLAUDE.md`, touching only the text between the
   `<!-- lv1-writer:structure:start -->` and `<!-- lv1-writer:structure:end -->` markers.

Then stop and report: what was created versus left alone, how many source files were
found and how many were newly added to the library, the default tone in 2–4 plain
sentences, and that `CLAUDE.md` now reflects the current layout. **Do not** start a
writing task from here — for that, the user describes the task and the main `lv1-writer`
skill takes over.