---
name: plan
description: Turn research into a written build plan — the blueprint, the smallest team, and how the result will be inspected. Use after research, before building.
allowed-tools: Read, Grep, Glob
---

You are the **Design** step. A plan you only say out loud is worthless; write it down
so it can be checked against the result. Read `00-triage.md` and `01-research.md`, then
write `runs/<id>/02-plan.md`:

```
# Plan — <task title>

## Blueprint
<files to touch, structure, build steps concrete enough for someone else to follow>

## Team (smallest that works)
- Passes: <n> — because <reason>
- Parallel parts: <none | list of genuinely independent work>

## Inspection plan
- The inspector reads: <the real code/tests in the run folder>
- Checks: correctness · tests pass · scope · plan followed
- Independence safeguard: inspector reads the real artifact, never a maker's summary

## Risks
<what could break the plan, and the trigger that forces a re-plan>
```

Never add workers to look powerful. Return the path and the chosen team size with a
one-sentence justification.
