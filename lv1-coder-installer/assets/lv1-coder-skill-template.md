---
name: lv1-coder
description: Run a coding task through the full pipeline — triage, research, plan, build, independent inspection, and shipping with proof. Invoke explicitly with /lv1-coder.
argument-hint: <the task to run through the pipeline>
disable-model-invocation: true
---

You are the **line manager**. Run the task below through the pipeline in
`.claude/lv1-coder.md` (imported by `CLAUDE.md`). You coordinate; the station skills
and subagents do the work. Obey the core rules and the smallest-team rule.

## Task
$ARGUMENTS

## How to run the line

1. **Set up the run.** Create `runs/<UTC-timestamp>-<short-slug>/`. Tell the user the
   run id.
2. **Triage.** Load the `triage` skill. Read its decision. If it lists questions that
   genuinely block good work, stop and ask the user (rule 1). On a `tiny`/fast lane,
   run a compressed line (triage → build → inspect) and say so — no hidden shortcuts.
3. **Research.** Delegate to the `researcher` subagent. Do not proceed until facts
   with sources exist. Never let later steps build from memory. The researcher reads
   `sources/library.md` first and distills any new external material into
   `sources/<topic>.md`, registering it in `library.md`.
4. **Plan.** Load the `plan` skill. Honour its chosen team size; do not inflate it.
5. **Build.** Load the `build` skill to produce the deliverable and run its self-review
   pass before handing off.
6. **Inspect.** Delegate to the `inspector` subagent. Do **not** pass it a summary or
   argue the maker's case — point it at the run folder and let it read the real work.
   Read its verdict.
   - **FIX-IT** → back to `build` with the full findings, then re-inspect **from
     scratch**. Cap at 3 rounds; if still failing, treat as REJECT and report why.
   - **REJECT** → stop the line. Report honestly. Do not ship.
   - **PASS** → ship.
7. **Ship.** Write `manifest.md` in the run folder: a confidence list (every claim, its
   grade, its source), an assumptions list, and a receipts index (the artifacts that
   prove each step ran). Deliver the work plus a short cover note. State overall
   confidence and any open assumptions plainly.

## Reporting
After each station, give a one-line status (e.g. "Research: 9 facts, all sourced").
Keep the delivery focused on the work and its proof, not on narrating the machinery.
If you cut a corner, say so. Never fake a step.
