# Instruction-design principles

How to structure an instruction system for an agent so it is correct, cheap to run, and
easy to maintain. The themes are general; the mechanics they depend on are in
`claude-code-mechanisms.md`.

## 1. Layer the system by *kind*, not by topic

Every instruction belongs to exactly one of four layers, and each layer has a natural
home:

| Layer | What it is | Natural home | Loaded |
|---|---|---|---|
| **Contract** | Always-true rules and vocabulary that govern *every* task | The always-on instructions file | Every turn, every context |
| **Procedures** | Multi-step workflows that apply to *some* tasks | Skills | On demand, when triggered |
| **Workers** | Specialized roles that need isolation or their own tools/model | Subagents | When delegated to |
| **Substrate** | The platform primitives the above run on | (not authored — understood) | — |

Most bloated instruction sets fail because they mix these: a procedure lives in the
always-on file (paid on every turn even when irrelevant), or a contract rule is buried
in one worker (so it doesn't govern the others). Sort by kind first.

## 2. Single-source every rule

A rule should be stated **once**, in the layer that owns it, and referenced — never
restated — elsewhere. This matters most for the always-on file: it is paid on every
turn *and* re-loaded into every worker context. Anything duplicated there is multiplied
by the number of workers in a run. Keep the always-on file to the smallest complete set
of rules and vocabulary; let everything downstream point back to it instead of
re-explaining it.

Caveat — some repetition is load-bearing. A short, pointed reminder at the moment of use
("grade every claim here") can improve adherence. The thing to cut is *re-teaching the
concept*; the thing to keep is *applying it in context*. When in doubt: define once,
apply often, re-explain never.

## 3. Facts go in instructions; procedures go in skills

If a block is a *fact or standing rule* ("we never ship unverified numbers"), it belongs
in the always-on file. If it has grown into a *procedure* ("to release: do A, then B,
then check C"), it belongs in a skill, whose body loads only when invoked. The test:
does this apply to every task (→ instructions) or only when a certain kind of task comes
up (→ skill)? Moving procedures out of the always-on file is usually the single biggest
token win available, because it stops paying for workflow text on tasks that don't use it.

## 4. Isolation-critical or context-heavy work goes to a subagent

Use a worker (subagent) when a step either (a) must be **independent** — a reviewer's
judgment shouldn't be colored by the maker's reasoning — or (b) would **flood the main
context** with material referenced once (large documents, search dumps, logs). A
subagent runs in its own context and returns only a summary, so it delivers both
independence and context hygiene as a *mechanical* property, not a promise. Pair this
with the rule below.

## 5. Make independence mechanical, and use files as shared memory

Two agents in separate contexts cannot see each other's reasoning, so a reviewer that
reads the **real artifact** (not a summary handed to it) is genuinely independent by
construction. Because separate contexts also can't share state, write each step's output
to a **file**; the file system becomes the durable, inspectable memory of the run.
Receipts — a per-run trail plus a final manifest of claims, sources, and assumptions —
turn "I did it" into something anyone can verify.

## 6. Shared core + domain packs

When one agent serves two different domains (e.g. code and prose), don't write one flat
rulebook and don't fully duplicate two. Factor out a small **shared core** (the
philosophy and vocabulary common to both) and add a thin **domain pack** per domain (the
sourcing model, the proof model, the failure modes specific to that work). Each pack
stays specialized; the core stays single-sourced.

## 7. Pick the runtime per workload

Match the tool to the work, not to habit. Terminal/code-execution workloads (anything
that needs a shell, version control, tests) fit a developer agent. Document-and-research
workloads (ingesting PDFs/office files/images, web research, long-form drafting) fit a
document-oriented agent with project folders and persistent memory. Splitting a
two-domain system across two runtimes is worth it only when each domain clearly benefits;
otherwise the second runtime is maintenance cost for little gain.

## 8. Effort tiers must change the spend

Tag each task with an effort tier at intake — and make the tier actually **gate which
steps run**. A tier that exists only as a label is decoration. Cheap tasks should skip
stations and pay fewer tokens; expensive tasks should run the full line with extra
scrutiny. Default to the lighter lane and require a *reason* to escalate, not a reason to
compress — otherwise a cautious default quietly inflates the cost of everything.

## 9. Prefer sharp single-purpose skills over one generic mega-skill

A skill is selected by its description. One "does everything" skill gets a vague
description, so it triggers unreliably and ends up effectively always-loaded — losing the
on-demand benefit. Several narrow skills, each generic *within* its purpose but specific
in *what it does*, trigger cleanly and load only when needed. Reusable in applicability,
narrow in function.

## 10. Enforce what must not be skipped — don't just ask nicely

If a step is genuinely required (an independent check before shipping), a sentence in a
prompt is a soft request the model can rationalize past. For true enforcement, use a
platform mechanism (a hook that blocks completion unless the required artifact exists).
If you can't or won't enforce it mechanically, describe it honestly as a self-check
rather than dressing it up as a hard gate.
