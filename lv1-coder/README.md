# lv1-coder

A self-contained discipline pack for coding work in Claude Code. It runs a task
through a small, honest pipeline — triage, research, plan, build, independent
inspection — and ships only work that is **impressive AND true**, with proof.

## Install

Copy the contents of this folder into your project root:

```
your-repo/
├── CLAUDE.md           ← the operating rules (loads automatically)
└── .claude/
    ├── skills/         ← the pipeline stations you invoke
    │   ├── lv1/        ← /lv1 orchestrator
    │   ├── triage/
    │   ├── plan/
    │   └── build/
    └── agents/         ← isolated workers (own context + model)
        ├── researcher.md
        └── inspector.md
```

You can also place these in `~/.claude/` to use them across every repo.

## Use

Run the whole line:

```
/lv1 Add rate limiting to the public API and prove it works
```

The orchestrator runs each station, writes receipts to `runs/<id>/`, and delivers
the finished work with its confidence list and assumptions. On a `tiny` task it runs
a fast lane (triage → build → inspect) and tells you so.

Call one station when that's all you need, e.g. ask for the `inspector` subagent to
review the work already in `runs/<id>/`.

## What makes the check real

The inspector runs in its own context and reads the actual code and tests — not a
summary. The independence is mechanical, not a promise.
