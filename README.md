# ai-tuning

A collection of AI pipeline plugins and skills built around the same discipline:
understand the real ask, source the facts, plan before building, inspect independently,
and ship only work that is **impressive AND true**.

Three projects, three domains — writing, work deliverables + advisory, coding:

---

## lv1-writer

**Platform:** Claude Cowork / Claude Code (plugin)

A writing pipeline for research-backed long-form content. Runs tasks through
**triage → research → outline → draft (+ self-review) → inspect → ship**. Research and
inspection run as isolated subagents in their own context; every factual claim is graded
A/B/C/D and traced to a real source before the draft ships.

Three skills: `lv1-author` (main pipeline), `lv1-writer-init` (project setup),
`lv1-reviewer` (independent review of an existing document).

Install by uploading `lv1-writer-plugin.zip` as a plugin in Cowork ("Upload local
plugin"). Works in Claude Code via `--plugin-dir`.

→ See [`lv1-writer-plugin/README.md`](./lv1-writer-plugin/README.md)

---

## lv1-compass

**Platform:** Claude Cowork / Claude Code (plugin)

A work deliverable + advisory pipeline. Two roles share one constitution and one pair
of subagents:

- **lv1-assistant** (maker) — turns a work brief into an analysis, report, memo, or brief
  through `triage → research → outline → draft → inspect → ship`. Markdown-only pipeline.
- **lv1-advisor** (judge) — three modes: **review** an existing artifact, **challenge**
  a position with a sourced steelman, or **decide** a fork with a graded recommendation.
  Judges and advises; never makes the thing.

Shared `core/` holds the constitution, working-lessons, and readability rules — single-
sourced so discipline can't drift between the two roles. Skills trigger on natural-
language descriptions; no slash commands needed.

Install by uploading `lv1-compass-plugin.zip` as a plugin in Cowork ("Upload local
plugin"). Works in Claude Code via `--plugin-dir`.

→ See [`lv1-compass-plugin/README.md`](./lv1-compass-plugin/README.md)

---

## lv1-coder

**Platform:** Claude Code (globally-installed installer skill)

A coding pipeline that installs into any project via `/lv1-coder-installer`. Unlike
the writing pipelines, **research and plan are conditional opt-in** — the default lane
is `triage → build → inspect` because AI is already strong at standard coding tasks.
Research fires only when triage identifies a specific external fact the codebase cannot
answer (a dep version, a third-party API change). Plan runs only for multi-file or
high-stakes tasks.

Pipeline (full): `triage → [research?] → [plan?] → build (+ self-review) → inspect → ship`

Inspector and researcher run as isolated subagents. The inspector has three verdicts:
PASS, FIX-IT (build problem), or REDO-RESEARCH (honest build, thin research base).

**Install once globally** — copy `lv1-coder-installer/` into `~/.claude/skills/` and
restart Claude Code. Then per project: run `/lv1-coder-installer` to scaffold all
skills, subagents, and the knowledge base into that project. Restart and use
`/lv1-coder <task>`.

→ See [`lv1-coder-installer/README.md`](./lv1-coder-installer/README.md)

---

## The shared discipline

All three enforce the same floor:

- Triage decides scope and effort before any work starts.
- Claims are graded A/B/C/D. D never ships.
- Inspection runs in an independent context — the maker never self-certifies.
- Nothing ships without a proof package: confidence list, assumptions, receipts.

The key difference between pipelines: **lv1-writer and lv1-compass treat research as a
default station** (writing and work analysis depend on sourced facts). **lv1-coder
treats research as a conditional branch** — AI can reason from the codebase directly
for most coding tasks, so triggering research by default wastes tokens and adds no value.
