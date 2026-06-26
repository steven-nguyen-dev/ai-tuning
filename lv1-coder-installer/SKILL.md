---
name: lv1-coder-installer
description: Installs the lv1-coder coding pipeline (triage → [research?] → [plan?] → build → inspect → ship) into the current project. Run /lv1-coder-installer to scaffold the operating contract, working lessons, the pipeline skills, the researcher and inspector subagents, and a sources/ knowledge base.
disable-model-invocation: true
---

You are the **lv1-coder installer**. You scaffold the lv1-coder pipeline into the
current project. You do not run coding tasks — that is the job of the `/lv1-coder`
skill this installer creates.

Run the procedure below immediately — no argument needed.

---

## Install procedure

Goal: in one shot, create the project files listed below. Use the templates in this
skill's `assets/` directory as the source content. Paths are relative to the
project's current working directory and use forward slashes.

### Step 1 — `CLAUDE.md` at the project root

The import paths are **exactly** `@.claude/lv1-coder.md` and
`@.claude/working-lessons.md`.

- If `CLAUDE.md` does **not** exist: create it from `assets/claude-md-template.md`.
- If `CLAUDE.md` **already** exists: do **not** clobber it. Read it. For each of the
  two import lines, if it is not already present, append a blank line and then that
  line at the end of the file. If both lines already exist, leave the file untouched.

### Step 2 — `.claude/lv1-coder.md`

Copy `assets/lv1-coder-rules-template.md` to `.claude/lv1-coder.md`. This is the
operating contract that `CLAUDE.md` imports.

### Step 3 — `.claude/working-lessons.md`

Copy `assets/working-lessons-template.md` to `.claude/working-lessons.md`. This is
the living lessons file all stations read at the start of a run.

### Step 4 — the project runner skill

Copy `assets/lv1-coder-skill-template.md` to `.claude/skills/lv1-coder/SKILL.md`.
This is the `/lv1-coder` runner.

### Step 5 — the station skills

- Copy `assets/triage-skill-template.md` to `.claude/skills/triage/SKILL.md`.
- Copy `assets/plan-skill-template.md` to `.claude/skills/plan/SKILL.md`.
- Copy `assets/build-skill-template.md` to `.claude/skills/build/SKILL.md`.

### Step 6 — the subagents

- Copy `assets/researcher-agent-template.md` to `.claude/agents/researcher.md`.
- Copy `assets/inspector-agent-template.md` to `.claude/agents/inspector.md`.

### Step 7 — the knowledge base

- Copy `assets/library-template.md` to `sources/library.md` (create `sources/` if
  needed).

### Step 8 — the receipts directory

Create the directory `runs/` (empty) at the project root.

### Step 9 — report and ask for a restart

Print a short summary listing exactly what was created (and whether `CLAUDE.md` was
created fresh or appended to), then tell the user:

> ✅ lv1-coder is installed in this project. **Restart Claude Code** so the new
> skills, subagents, and the imported `.claude/lv1-coder.md` rules and
> `.claude/working-lessons.md` are picked up. Then run `/lv1-coder <your task>` to start.

Do not run any pipeline work yourself. Do not modify any files outside the list
above. `CLAUDE.md` is the only file you must preserve (append-only, never overwrite).
For all other target files: if the file already exists and its content differs from
the template, do **not** overwrite silently — list it in the Step 9 report as
`⚠ CONFLICT: <path> — differs from template. Overwrite? (y/n)` and wait for user
confirmation before proceeding. If the user declines, skip that file and note it as
skipped. If the file does not exist, create it from the template without prompting.
