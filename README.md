# lv1 — discipline packs for Claude

Two self-contained instruction packs that make an AI agent do careful work — research,
plan, build, and **independently check** before delivering — instead of answering from
memory and hoping it's right. One pack is tuned for coding in Claude Code, the other for
book and research writing in Claude Cowork. Both run from a single command: **`/lv1`**.

The shared bar for everything these packs produce:

> **Impressive AND true.** Work that dazzles but is false fails. Work that is true but
> thin also fails. Ship only what clears both, backed by proof anyone can check.

## What's in here

| Folder | For | Runtime |
|---|---|---|
| **`lv1-coder/`** | Coding tasks — implement, fix, refactor, with tests as proof | Claude Code |
| **`lv1-writer/`** | Book & research writing — gather sources, draft, fact-check | Claude Cowork |
| **`sources/`** | The general design knowledge behind the packs (reusable notes) | — |

The two packs are independent and may duplicate files on purpose, so each folder works
on its own with nothing else from this repo.

## How they work

Each pack is built from three layers:

- **A constitution** (`CLAUDE.md`) — a small set of always-on rules: understand the real
  ask, check facts instead of memory, label confidence, source every factual claim, hold
  scope, use the smallest team, and get an independent check.
- **Stations as skills** (`.claude/skills/`) — the pipeline steps, each loaded only when
  used, orchestrated by `/lv1`.
- **Workers as subagents** (`.claude/agents/`) — research and inspection run in their own
  isolated contexts. The inspector reads the **real** work, never a summary, so its
  independence is mechanical, not a promise.

Every run leaves receipts in a `runs/<id>/` folder and ships with a manifest listing
each claim, its confidence grade, and its source.

---

## lv1-coder (Claude Code)

**Use it for:** any coding task you want done carefully and verifiably — a feature, a
bug fix, a refactor — where "it works" should mean a test proves it.

**Install** — copy the contents of `lv1-coder/` into your project root (or into
`~/.claude/` to use it across every repo):

```
your-repo/
├── CLAUDE.md
└── .claude/
    ├── skills/{lv1, triage, plan, build}/SKILL.md
    └── agents/{researcher, inspector}.md
```

**Use:**

```
/lv1 Add rate limiting to the public API and prove it works
```

`/lv1` triages the task, researches with sources, writes a plan, builds the change with
a self-review, then hands it to an independent inspector that runs the tests. On a small,
clear task it takes a fast lane (triage → build → inspect) and tells you so. You can also
call one station alone — e.g. ask the `inspector` subagent to review what's already in a
run folder.

---

## lv1-writer (Claude Cowork)

**Use it for:** book chapters, essays, and research writing where the hard part is
**finding, keeping, and using source material** — PDFs, Word docs, images, web research —
and every factual claim should trace back to a real source.

**Install** — copy the contents of `lv1-writer/` into your Cowork project folder:

```
my-book/
├── CLAUDE.md
├── .claude/
│   ├── skills/{lv1, triage, source-intake, outline, draft}/SKILL.md
│   └── agents/{librarian, researcher, inspector}.md
├── sources/library.md      ← every source + every claim, cited and graded
└── manuscript/             ← your plain-text drafts
```

**Use** — run the whole line:

```
/lv1 Draft a 2,000-word chapter on the fall of the Western Roman economy
```

Or file a source you already have into the library:

```
Use source-intake on sources/pdf/heather-2005.pdf
```

`/lv1` triages, curates sources into `sources/library.md` (delegating heavy PDFs/images
to an isolated `librarian`), researches anything missing, outlines, drafts in plain text,
then hands the draft to an independent inspector that checks every fact against the
library. Factual claims must be sourced; interpretation is allowed but must be labeled as
interpretation, never dressed up as fact.

---

## sources/ — the design knowledge base

General, reusable notes distilled from building these packs: how to layer an instruction
system, what wastes tokens, what backfires, and how the platform actually loads and runs
instructions. Start with `sources/README.md`. Useful if you want to adapt these packs or
build your own.

## A note before your first run

Skills and subagents load at the start of a session. After copying a pack into place,
start a fresh session (or restart) so `/lv1` and its stations register.
