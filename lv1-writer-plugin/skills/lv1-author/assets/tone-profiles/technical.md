# Tone profile — technical

- **Genre:** technical documentation / runbook / spec
- **Register:** precise, neutral, imperative — get the reader to a correct result
- **Proof apparatus:** low — spec-style references (version numbers, standards, source files); claims verified against the real system
- **Default authorship:** AI-autonomous
- **Structure shape:** task- or component-ordered; reference-style
- **Density & pacing:** **compress** (maximum) — tables, lists, and code blocks over prose; one fact per line; ruthless against vertical scroll

> Be correct, complete, and unambiguous. A reader follows this to *do* something; a
> wrong or vague step costs them real time. Read order: Requirements → Correctness →
> Structure → Review. **If two rules conflict, Correctness wins.**

---

## Spine & shape

- State **purpose, audience, and prerequisites** up front (what this covers, who it's
  for, what they need first).
- Order by the reader's task (steps in execution order) or by component; dependencies
  before the things that need them.
- Bound the scope explicitly; name non-goals.

## Correctness

- **Verify every instruction against the real system / API / standard** — exact command
  names, flags, version numbers, file paths, parameters. Don't write from memory of how
  an API "probably" works; check it.
- Cite the **authoritative reference** (official docs, the standard, the source file)
  for anything version-dependent; note version/date.
- **Flag assumptions and preconditions**; state what happens on failure and how to
  recover (rollback, error states).
- Where behaviour is uncertain or untested, say so — don't present a guess as a verified
  step.

## Tone & voice

- Neutral and precise; imperative mood for instructions ("Run…", "Set…").
- No narrative, no salesmanship, no filler. Define each technical term at first use and
  use it consistently.
- Brevity that doesn't sacrifice completeness — every necessary step present, nothing
  decorative.

## Structure & format

- **Format follows content** (see `references/draft.md`): **numbered steps** for
  sequences the reader executes in order; **tables** for parameters, options, error
  codes, compatibility; **code blocks** for commands/config/examples; **arrows** for
  data/control flow (`request → handler → store`); prose only for concepts that need
  explanation. This genre uses structured markup freely — a wall of prose is a failure.
- Cross-references resolve (every "see section X" target exists); examples are runnable
  and match the described behaviour.
- No author-only artifacts (TODOs, placeholders) in the published doc.

## Process

1. Identify the task, audience, and prerequisites; gather the authoritative references.
2. Outline by task/component with dependencies ordered.
3. Draft steps; verify each against the real system/standard, with version noted.
4. Dry-run or trace the procedure end to end; confirm every command, path, and
   cross-reference is correct.

## Never do this

- Write a command, flag, path, or parameter from memory without verifying it.
- Omit a precondition, a failure mode, or a recovery step.
- Present an untested step as verified.
- Bury an executable sequence in prose instead of numbered steps.
- Leave a stale cross-reference or a broken example.
