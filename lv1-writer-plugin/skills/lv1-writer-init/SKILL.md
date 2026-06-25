---
name: lv1-writer-init
description: Set up the lv1-writer project structure in the current folder — the sources/ library, manuscript/, a default writing tone, and CLAUDE.md. Safe to re-run. Trigger with "lv1-writer init", "set up lv1-writer", "initialize lv1-writer", or when starting lv1-writer in a folder that has no sources/library.md yet.
---

# lv1-writer — Setup (init)

Run the **lv1-writer** skill's **Setup** flow in the current project folder. This is a
thin entry point — the authoritative Setup steps live in the main `lv1-writer` skill's
`## Setup` section; follow them there, in order.

## Ask first — one round of project questions

Before creating files, ask the user a single grouped round of questions (use Cowork's
AskUserQuestion if available, otherwise plain prompts). Each accepts "you decide":

1. **Topic** — what is this book/project about? (free text, or "decide later per task")
2. **Genre / format** — pick from the taxonomy in `references/tone-detect.md` (e.g.
   popular-science, cognitive-behavior, IT-industry analysis, memoir, mindset,
   romance…), or "you decide" — then genre is inferred per task from each request.
3. **Tone** — accept the genre's default tone profile, describe a custom tone, or
   "you decide".
4. **Working mode** — **auto** (let AI research and draft autonomously wherever the
   genre allows) or **interactive** (you want to be involved / interviewed). This is
   the only authorship question; AI resolves the concrete approach from genre + this
   choice (see `references/interview.md`).

## Then run Setup

Follow the main skill's `## Setup` steps, in order:

1. Sync the source library (`sources/library.md`) against whatever files are in
   `sources/` — this runs every time, not just on a fresh project.
2. Create `manuscript/_about.md` if missing.
3. Create `manuscript/writing-instruction.md` if missing — seed it from the chosen
   tone profile (`assets/tone-profiles/<id>.md`); if the user said "you decide" for
   genre, leave it unwritten and let per-task detection drive tone. **Never overwrite
   it** once it exists (that's where the user's tone edits live).
4. Create or update `CLAUDE.md`, touching only the text between the
   `<!-- lv1-writer:structure:start -->` and `<!-- lv1-writer:structure:end -->`
   markers — and record the chosen topic, genre, and working mode there.

If the resolved authorship approach is **interview-driven** or **hybrid**, run the
intake interview now (`references/interview.md`) so the project starts with the user's
material captured.

Then stop and report: the chosen topic / genre / tone profile / working mode; what was
created versus left alone; how many source files were found and how many were newly
added to the library; the tone in 2–4 plain sentences; and that `CLAUDE.md` now
reflects the current layout. **Do not** start a writing task from here — for that, the
user describes the task and the main `lv1-writer` skill takes over.