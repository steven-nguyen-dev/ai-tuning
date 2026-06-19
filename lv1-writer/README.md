# lv1-writer (skill)

A single Skill, not a project pack — upload it once and it's available in any Cowork
project (or chat).

## Install

1. In Claude Desktop: **Customize → Skills → + → Upload a skill**.
2. Upload `lv1-writer.zip`.
3. It appears in your skills list as **lv1-writer**, toggled on.

## Use, inside a Cowork project

```
lv1-writer init
```
Creates `sources/library.md` and `manuscript/_about.md` if they don't exist yet,
scans `sources/` and adds any files found there to the library (this part re-runs
every time — it's how new sources you drop into the folder get picked up), drops a
default tone instruction at `manuscript/writing-instruction.md` the first time (edit
that file directly for a different tone — init won't overwrite it again), and updates
the project's `CLAUDE.md` with the current folder layout. Safe to run again later;
nothing customized gets clobbered.

```
lv1-writer draft a 2,000-word chapter on the fall of the Western Roman economy
```
Runs the task through the full pipeline (or the fast lane for something small) and
delivers a sourced, inspected draft with its confidence list.

You can also just describe the task naturally, or hand over a source file ("file this
PDF into the source library") — Claude will recognize when this skill applies without
needing the exact word "lv1-writer".

## Why this looks different from a Claude Code project

The original design used a `CLAUDE.md` + `.claude/skills/` + `.claude/agents/`
project layout with isolated subagents for research and inspection. That format is
specific to Claude Code / Cowork *projects* you point at a folder — it isn't what
"Upload a skill" accepts. A skill upload is exactly one `SKILL.md` (plus optional
`references/` and `assets/`), with no separate subagent files and no Claude
Code-only frontmatter fields (no `model`, `tools`, `argument-hint`).

This version folds all five stations and the three former subagent roles into that
one `SKILL.md` plus reference files, so it installs as one upload. The trade-off:
the inspection pass is no longer running in a truly separate context — it's the same
Claude deliberately re-reading the real draft and the real source library fresh,
rather than a process-isolated reviewer. The instructions are written to make that
re-read as honest as a single context can make it, but it isn't mechanical
independence the way a real subagent would be.

`lv1-writer init` does write a `CLAUDE.md` into the project folder — but that's a
different mechanism than the old project layout. Cowork reads a `CLAUDE.md` at a
project's root as ordinary folder instructions, the same way it would for any
project, skill or no skill. It isn't part of how this skill gets installed or
triggered; it's just a courtesy so the project stays self-documenting even outside a
conversation where this skill is active.
