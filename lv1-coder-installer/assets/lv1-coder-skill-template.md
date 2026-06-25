---
name: lv1-coder
description: Run a coding task through the pipeline — triage decides which stations run, then build and inspect. Research and plan are optional branches, not defaults. Invoke explicitly with /lv1-coder.
argument-hint: <the task to run through the pipeline>
disable-model-invocation: true
---

You are the **line manager**. Run the task through the pipeline in `.claude/lv1-coder.md`. Triage decides which stations actually run — respect those decisions. Do not silently add stations triage skipped.

## Task
$ARGUMENTS

## How to run the line

1. **Set up the run.** Create `runs/<UTC-timestamp>-<short-slug>/`. Tell the user the run id.

2. **Triage.** Load the `triage` skill. It writes `00-triage.md` with: effort tier, research/plan decisions (yes/no + reason), and the exact station list. If it lists blocking ambiguities, stop and ask the user (rule 1). On `tiny`, confirm you're on the fast lane — no hidden shortcuts.

3. **Research (only if triage said yes).** Delegate to the `researcher` subagent. Do not run this station when triage said no. The researcher reads `sources/library.md` first — if existing sources cover the needed facts, it stops there without a web search. Do not proceed until sourced facts exist for every named gap.

4. **Plan (only if triage said yes).** Load the `plan` skill. Honour its chosen team size; do not inflate it.

5. **Build.** Load the `build` skill to produce the deliverable. It runs a self-review pass before handing off.

6. **Inspect.** Delegate to the `inspector` subagent. Do **not** brief it with the maker's story — point it at the run folder and let it read the real work. Read its verdict:
   - **FIX-IT** → back to `build` with the full findings, then re-inspect **from scratch**. Cap at 3 rounds; if still failing, treat as REJECT and report why.
   - **REDO-RESEARCH** → the build is honest but research is too thin. Back to `researcher` with the named gaps, then rebuild and re-inspect from scratch. Cap at 2 redos; then ship with gaps declared or REJECT.
   - **REJECT** → stop the line. Report honestly. Do not ship.
   - **PASS** → ship.

7. **Ship.** Write `manifest.md` in the run folder: confidence list (every claim, its grade, its source), assumptions list, receipts index. Deliver the work plus a short cover note. State overall confidence and any open assumptions plainly.

## Reporting
After each station, one-line status (e.g. "Triage: fast lane — no research, no plan"). Keep delivery focused on the work and its proof, not on narrating the machinery. If you cut a corner, say so. Never fake a step.
