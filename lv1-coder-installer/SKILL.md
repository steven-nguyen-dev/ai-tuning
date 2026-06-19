---
name: lv1-coder-installer
description: Installs the lv1-coder coding pipeline (triage → research → plan → build → inspect → ship) into the current project. Use /lv1-coder-installer init to scaffold the operating contract, the pipeline skills, the researcher and inspector subagents, and a sources/ knowledge base.
argument-hint: init
disable-model-invocation: true
---

You are the **lv1-coder installer**. You scaffold the lv1-coder pipeline into the
current project. You do not run coding tasks — that is the job of the `/lv1-coder`
skill this installer creates.

## Branching on `$ARGUMENTS`

Read `$ARGUMENTS`. There are exactly two cases:

1. If `$ARGUMENTS` (trimmed) **starts with the word `init`** → run the **Init**
   procedure below.
2. **Otherwise** (anything else, including empty) → print this usage message and
   stop, doing nothing else:

   > Run `/lv1-coder-installer init` to scaffold lv1-coder into this project, then
   > use `/lv1-coder <task>`.

No other modes exist. No `mode` argument. No re-init flag.

---

## Init procedure

Goal: in one shot, create the project files listed below. Use the templates in this
skill's `assets/` directory as the source content. Paths are relative to the
project's current working directory and use forward slashes.

### Step 1 — `CLAUDE.md` at the project root

The import path is **exactly** `@.claude/lv1-coder.md`.

- If `CLAUDE.md` does **not** exist: create it from `assets/claude-md-template.md`.
- If `CLAUDE.md` **already** exists: do **not** clobber it. Read it. If it does not
  already contain the line `@.claude/lv1-coder.md`, append a blank line and then
  that single line at the end of the file. If it already contains that line, leave
  the file untouched.

### Step 2 — `.claude/lv1-coder.md`

Copy `assets/lv1-coder-rules-template.md` to `.claude/lv1-coder.md`. This is the
operating contract that `CLAUDE.md` imports.

### Step 3 — the project runner skill

Copy `assets/lv1-coder-skill-template.md` to `.claude/skills/lv1-coder/SKILL.md`.
This is the `/lv1-coder` runner.

### Step 4 — the station skills

- Copy `assets/triage-skill-template.md` to `.claude/skills/triage/SKILL.md`.
- Copy `assets/plan-skill-template.md` to `.claude/skills/plan/SKILL.md`.
- Copy `assets/build-skill-template.md` to `.claude/skills/build/SKILL.md`.

### Step 5 — the subagents

- Copy `assets/researcher-agent-template.md` to `.claude/agents/researcher.md`.
- Copy `assets/inspector-agent-template.md` to `.claude/agents/inspector.md`.

### Step 6 — the knowledge base

- Copy `assets/library-template.md` to `sources/library.md` (create `sources/` if
  needed).

### Step 7 — the receipts directory

Create the directory `runs/` (empty) at the project root.

### Step 8 — report and ask for a restart

Print a short summary listing exactly what was created (and whether `CLAUDE.md` was
created fresh or appended to), then tell the user:

> ✅ lv1-coder is installed in this project. **Restart Claude Code** so the new
> skills, subagents, and the imported `.claude/lv1-coder.md` rules are picked up.
> Then run `/lv1-coder <your task>` to run a task through the pipeline.

Do not run any pipeline work yourself. Do not modify any files outside the list
above. If a target file other than `CLAUDE.md` already exists, overwrite it (the
templates are the canonical source); `CLAUDE.md` is the only file you must preserve.
