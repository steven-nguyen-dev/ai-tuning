# lv1-coder-installer

A globally-installable Claude Code skill that scaffolds the **lv1-coder** coding
pipeline into any project. lv1-coder runs each task through an honest line —
**triage → research → plan → build (+ self-review) → inspect → ship** — and ships
only work that is **impressive AND true**, with proof.

## Install (once, globally)

1. Extract `lv1-coder-installer.zip`. You'll get a top-level `lv1-coder-installer/`
   folder.
2. Move that folder into your user-level Claude skills directory:
   - **macOS / Linux:** `~/.claude/skills/lv1-coder-installer/`
   - **Windows:** `%USERPROFILE%\.claude\skills\lv1-coder-installer\`
3. **Restart Claude Code** so the new skill is registered.

After restart, the `/lv1-coder-installer` slash command is available in every
project.

## Use (per project)

1. From the project root, run:

   ```
   /lv1-coder-installer
   ```

   This scaffolds:

   ```
   your-project/
   ├── CLAUDE.md                                  ← created or appended-to (adds @.claude/lv1-coder.md + @.claude/working-lessons.md)
   ├── .claude/
   │   ├── lv1-coder.md                           ← operating rules (the contract)
   │   ├── working-lessons.md                     ← hard-won lessons (imported into CLAUDE.md)
   │   ├── skills/
   │   │   ├── lv1-coder/SKILL.md                 ← /lv1-coder pipeline runner
   │   │   ├── triage/SKILL.md
   │   │   ├── plan/SKILL.md
   │   │   └── build/SKILL.md
   │   └── agents/
   │       ├── researcher.md                      ← model: sonnet, owns sources/
   │       └── inspector.md                       ← model: opus
   ├── sources/
   │   └── library.md                             ← knowledge base navigator
   └── runs/                                      ← receipts land here
   ```

2. **Restart Claude Code** so the project's skills, agents, and the imported
   `@.claude/lv1-coder.md` rules and `@.claude/working-lessons.md` load.

3. Each session in this project, run a task:

   ```
   /lv1-coder Add rate limiting to the public API and prove it works
   ```

   The orchestrator runs each station, writes receipts to `runs/<UTC-timestamp>-<slug>/`,
   and delivers the work with its confidence list and assumptions. On a `tiny` task it
   runs a fast lane (triage → build → inspect) and tells you so.

## Effort tiers (set by triage)

- **tiny** — fast lane: triage → build → inspect.
- **normal** — ordinary task → core line.
- **full** — important or multi-part → full pipeline.
- **high-stakes** — risky or irreversible → full pipeline + extra scrutiny.

Default is the lighter lane; escalate for a reason.

## The knowledge base (`sources/`)

The `researcher` subagent owns `sources/` and maintains `sources/library.md` as the
navigator/index. When research needs external material (web pages, PDFs, books, local
documents), the researcher distills the *needed* knowledge into a focused
`sources/<topic>.md` and registers it in `library.md`. Raw material is never dumped
verbatim — only the distilled, sourced, graded notes are kept. You can ask the
assistant to add a new source from a URL or a local file at any time.

## What makes the check real

The `inspector` subagent runs in its own context and reads the actual code and
tests — never a summary from the maker. The independence is mechanical, not a
promise.

## Idempotence and existing files

- `CLAUDE.md` is preserved (append-only): if it exists, the import lines
  `@.claude/lv1-coder.md` and `@.claude/working-lessons.md` are appended only if missing.
  It is never overwritten.
- Other scaffolded files (the rules file, working-lessons, skills, agents,
  `sources/library.md`): if a
  file already exists and its content differs from the template, the installer flags it
  as `⚠ CONFLICT: <path> — differs from template. Overwrite? (y/n)` and waits for your
  confirmation before touching it. If you decline, the file is left as-is. Files that
  don't exist yet are created from the template without prompting.

## Uninstall

Delete the `lv1-coder-installer` folder from `~/.claude/skills/` (or
`%USERPROFILE%\.claude\skills\`) and restart Claude Code. To remove lv1-coder from a
project, delete `.claude/lv1-coder.md`, `.claude/working-lessons.md`,
`.claude/skills/{lv1-coder,triage,plan,build}/`,
`.claude/agents/{researcher,inspector}.md`, `sources/library.md`, and `runs/`, and
remove the `@.claude/lv1-coder.md` and `@.claude/working-lessons.md` lines from `CLAUDE.md`.
