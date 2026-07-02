# Writing Instruction template — moved

This file is no longer the source of the default tone. Since the genre/tone system was
added, `lv1-writer init` seeds `inputs/writing-instruction.md` from the **tone
profile** the user chooses, drawn from `${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/`.

- The old default voice (science-grounded narrative nonfiction) now lives, verbatim, at
  **`${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/narrative-nonfiction.md`** — edit it there.
- Genre detection and the full profile taxonomy: **`${CLAUDE_PLUGIN_ROOT}/references/tone-detect.md`**.
- If the user picks "you decide" for genre at init, no `writing-instruction.md` is
  seeded and tone is detected per task instead.

Editing this file has no effect — it is kept only to avoid breaking any external link.
