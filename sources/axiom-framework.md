# The AXIOM Framework

*A disciplined agent pipeline for research and writing — from academic to literary work — built to ship only what is **impressive AND true**, with proof anyone can verify.*

This document is a self-contained reference for AXIOM v9 as implemented for [Claude Code](https://docs.claude.com/en/docs/claude-code/overview). It covers four things: an **introduction** to what the framework is and where it came from, the **concept** (the discipline that governs every part of it), the **pipeline** (its architecture — stations, agents, gates, and artifacts), and the **workflow** (how a single task moves through it end to end). A closing section distils the design principles and file layout so the framework can be rebuilt or adapted.

Every factual description below is drawn directly from the framework's own source files (a constitution, an orchestrator command, and six agent definitions). Where this document offers design commentary rather than describing the source, it is marked as interpretation.

---

## 1. Introduction

AXIOM began as **AXIOM v9 — "Living Factory,"** originally an HTML file that *illustrated* a production process. The Claude Code implementation turns that illustration into a working system: a real assembly line of agents used for **research and writing tasks, from academic to literary**. The rebuild keeps the original's guiding idea intact — *Wow = impressive AND true*, backed by checkable evidence.

The organizing metaphor is a **smart factory**. A task is a job that enters the factory, moves through a sequence of stations, is inspected before it leaves, and ships with its own paperwork. Each station is staffed by a specialized worker — in Claude Code terms, a **subagent** — and a line manager (the **orchestrator**) coordinates the flow. The factory framing is not decoration; it drives the framework's two central commitments:

- **Nothing is built from memory.** A smart factory checks its materials before assembling. AXIOM gathers real, sourced information before any writing begins.
- **Nothing ships without an independent inspection.** A separate inspector examines the *actual product* — not a summary of it — before delivery.

The framework fits Claude Code specifically because three of its features make the discipline enforceable rather than aspirational: subagents run with their **own separate context** (so an inspector can be genuinely uninfluenced by the maker's reasoning), work is passed as **files on disk** (so "read the real thing, not a summary" becomes a mechanical fact rather than a request), and Claude Code can **delegate sequentially or in parallel** (so the framework can use the smallest team that fits each job).

**A note on what "pipeline" means here — read this before taking the factory metaphor literally.** AXIOM is a *workflow-shaped agent*, not a deterministic workflow in the software sense. On Claude Code the orchestrator is a slash command whose subagent calls are **model-driven**: Claude decides to delegate to each station by following the command's written instructions, not by executing a hard-coded call graph the way a build system runs a DAG. There is no runtime guarantee that exactly these stations fire in exactly this order every run — the guarantee is only as strong as the model's instruction-following, backed by the Safety Gate's after-the-fact check that the inspection artifact exists. Consequently, **"consistent output" in AXIOM means *structural* consistency — the same stations, the same named artifacts, the same output schema, and the same pass criteria every run — not byte-identical text.** Two runs of the same task will differ in wording; both should carry the same shape and clear the same bar. The mechanisms that buy this consistency are the fixed output schemas (Section 3.5), the concrete effort tiers (Section 2.5), and the gate — not determinism, which a language-model pipeline does not have.

The simplest way to use it is a single command that runs the whole line:

```
/axiom Write a 1500-word essay on the influence of existentialism on The Tale of Kiều
```

The orchestrator then walks the job through triage → research → design → assembly → a coaching pass → an independent inspection → a safety gate → shipping with an evidence package.

---

## 2. Concept — the discipline

Everything in AXIOM stands on a shared **constitution**: a set of rules that every subagent and the orchestrator inherit and must obey. The mechanics in Section 3 exist to enforce this discipline; the discipline is the point.

### 2.1 The Golden Rule

> **Wow = impressive AND true.**

A result that dazzles but is false fails. A result that is true but thin also fails. The bar is *both*, every time, and it must be backed by proof a reader can check. This single line is the standard every station is measured against.

### 2.2 The 9 Axioms

The framework's namesake rules. They are stated as non-optional:

1. **Understand the real ask first.** Don't rush in. Pin down what is truly being asked, and the limits, *then* plan how to do it.
2. **Check facts, don't trust memory.** Never build from memory alone — memory can be wrong. Gather real information first, and keep a source for it.
3. **Break it into steps first.** Before starting, write out what you'll deliver, the steps, and the plan.
4. **Put the plan in writing.** The plan is a written, visible artifact — not an idea in your head. If it changes, the change is noted.
5. **Say how sure you are.** Label every claim by confidence. Never present a guess as if it were a fact.
6. **Back every claim with a source.** No source, no claim. Give the most helpful real answer — don't dodge with vague words.
7. **Adjust when you learn something new.** If new information breaks the plan, stop, update it, and say so — don't quietly carry on.
8. **Stick to the agreed scope.** Don't quietly add or drop work. Any change to the agreed scope is announced.
9. **Get an independent check.** Before delivery, a separate, unbiased inspector verifies the work by looking at the real thing — not a summary from the maker.

### 2.3 Confidence grading

Every factual claim in any deliverable carries exactly one grade. The grade tells the reader precisely how much to trust the claim — and, per the working lessons below, is a reason to speak *more* firmly within its bounds, not a hedge.

| Grade | Meaning | Ships? |
|-------|---------|--------|
| **A — Proven** | The source text is **present in this run** — fetched and read this session, handed over by the user, or returned by a tool call actually made — such that the claim could be quoted right now. The strongest grade. | Yes |
| **B — Reasoned** | Soundly derived by logic from verified material, or a reputable secondary source citing a primary. A real conclusion, not "might be wrong." | Yes (labeled) |
| **C — From memory** | From memory, or a single unchecked source. | Held back until confirmed |
| **D — Guess** | A guess. | **Never ships.** Set aside, or flagged as an explicit assumption only. |

**The provenance test for `[A]`.** An agent cannot feel the difference between *reading* a
source and *recalling* one, so the grade is made physical: a claim earns `[A]` only if the
agent can point to where the source text sits *in this run*. A claim "known" from training
but not pulled this run is **not** `[A]` — it is `[C]` (memory) until confirmed, or `[B]`
only if it is a sound derivation from material that *is* present. Synthesized search results
are not source text: they support `[B]`, not `[A]`. If you can't point to the source, it
isn't an `[A]`.

### 2.4 The 6 failure modes

Named anti-patterns the framework forbids. Each has a short code (R1–R6) so an inspector can point to the exact fault:

- **R1 · One method as the only way.** Freezing a tool or habit as if it were a sacred rule. Stay flexible; pick the right method for each job.
- **R2 · Faking "done."** Polishing something to *look* complete when it isn't — all shine, no substance.
- **R3 · Doing too little.** Thin, corner-cutting work — the opposite mistake. The bar is impressive AND true.
- **R4 · Faking the steps.** Claiming a step happened when it didn't — "I checked it" with no real check behind it.
- **R5 · Rigging the inspection.** Feeding the inspector a biased story so the check only *looks* independent. A fed inspector isn't independent.
- **R6 · Too many cooks.** Adding agents for show when fewer would do better. More hands isn't more power — it's more chances for mistakes.

### 2.5 Smallest-team rule and effort triage

Two rules keep the framework from over- or under-building any job.

The **smallest-team rule**: use the smallest team that can do the job well. One worker for simple jobs; add agents only when parts can *truly* run in parallel or need genuinely separate expertise. Adding agents to look busy or powerful is R6.

**Effort triage**: every task is tagged before work begins, so nothing is over- or under-built. When unsure, the framework chooses the more careful path. A tier is not a label — it must change *which stations run and how many times*, so that the orchestrator and the inspector can verify the declared tier was actually honored. The tier maps to a concrete, checkable station set:

| Tier | When | Research | Design | Inspect | Proof |
|------|------|----------|--------|---------|-------|
| **tiny** | Quick, already-clear request with **no factual claim** | skip | skip | skip — self-review only | grades noted inline |
| **normal** | Ordinary task | 1 pass | yes | 1 pass | closing confidence note + 1-line method |
| **full** | Important or multi-part | 1 pass + a research contract (recency window, source types, min distinct sources) | yes | 1 pass | full proof package (`manifest.md`) |
| **high-stakes** | Risky, irreversible, or high-visibility | 2 passes (or a raised min-distinct-sources target) | yes | re-inspect **even on a first PASS**; redo cap raised by 1 | proof package **+** a methodology/enforcement note |

Two hard rules keep the tiers honest: **if a `tiny` task carries any factual claim it must escalate out of the fast lane** (there is otherwise no station to source the claim), and any skipped station must be *declared*, never silently dropped (R4). Default to the lighter lane; escalate for a concrete reason, not by habit.

### 2.6 Working lessons — how the discipline self-corrects

The constitution instructs every session to **read a working-lessons file before doing any work.** These are corrections drawn from real sessions, not theory, and they exist because the cost of skipping them is repeating the same mistake. Five lessons anchor it:

1. **Neutral ≠ timid.** Declining to make an *unrequested policy recommendation* is correct restraint. Dodging a *clear conclusion the data already supports* is not neutrality — it is lying by understatement. If the evidence answers the question, say so plainly.
2. **A confidence label is a reason to speak stronger, not weaker.** A `[B]` grade means "a real conclusion, reasoned from verified data" — not "might be right, might be wrong." It licenses commitment within its bounds; it is not a shield against committing at all.
3. **A numeric conclusion isn't a conclusion until the arithmetic is shown.** A figure can carry a source label and still be wrong. If you can't show the calculation, don't present the precise number.
4. **A user's lived experience is data, not a conclusion.** Take the user's framing as a *serious starting point* to investigate — then search for the strongest evidence against it, and report both. Treating their view as a default conclusion to be confirmed is flattery dressed as method.
5. **The steelman duty.** Before writing any conclusion that favors a view, state the strongest possible version of the opposing case, and answer it honestly. This is distinct from false balance:

| | False balance | Steelman |
|---|---|---|
| **What it is** | "Both sides have a point" | "Here is the strongest version of the opposing side" |
| **Purpose** | Avoid committing | Stress-test the conclusion |
| **Result** | Concludes nothing | A firmer conclusion, or a corrected one |

The lessons file closes with its own rule: it is only worth anything if it is read, and a lesson that gets violated again should be re-added rather than deleted.

---

## 3. The pipeline

The discipline in Section 2 is enforced by a concrete architecture: a **7-step line**, six specialized **subagents**, an **orchestrator**, a **safety gate**, and a convention of **file receipts**.

### 3.1 The seven stations

```
1 Reception → 2 Research → 3 Design → 4 Assembly → 5 Coach → 6 Inspection → [GATE] → 7 Shipping
                                          ▲                        │
                                          └──── Fix-it loop ◀──────┘
```

| # | Station | Agent | Job |
|---|---------|-------|-----|
| 1 | Reception | `axiom-intake` | Triage: full process or quick lane? effort level? what "done fully" means. |
| 2 | Research | `axiom-research` | Gather real, current facts **with sources**. Never build from memory. |
| 3 | Design | `axiom-design` | Write the plan down. Decide the smallest team. Plan the inspection. |
| 4 | Assembly | `axiom-assembly` | Do the work at full effort. Label confidence, cite sources, leave receipts. |
| 5 | Coach | `axiom-coach` | Ask once: "Is this really the best?" Never gives the answer. |
| 6 | Inspection | `axiom-inspector` | An **independent** check of the real artifact. Verdict: PASS / FIX-IT / REJECT. |
| — | Gate | orchestrator | Won't open unless a real inspection happened and the verdict came from the inspector. |
| 7 | Shipping | orchestrator | Deliver only what passed, with its full proof package attached. |

Six stations are staffed by subagents; the seventh (Shipping) and the gate are handled by the orchestrator itself.

### 3.2 The six subagents

Each subagent is a separate Claude Code agent with its own system prompt, its own tool set, and an assigned model chosen to balance cost against the difficulty of its job. Each writes a numbered artifact into the run folder (Section 3.5).

The topology is deliberately flat: the orchestrator spawns each of the six subagents; the subagents do not spawn one another. This keeps the design compatible with Claude Code both before and after the nested-subagent capability was added (as of Claude Code v2.1.172 a subagent may spawn its own subagents, up to a fixed depth of five). AXIOM does not need nesting today; if a future version wants the inspector to dispatch a per-finding verifier, that capability now exists and would count toward the depth limit.

**Station 1 — `axiom-intake` (Reception).** *Model: haiku. Tools: Read, Write, Glob, Grep.*
Sorts the job before any work happens, so it is neither over- nor under-built. It does not do the task. It answers three questions in writing — full process or quick lane? which effort level (tiny/normal/full/high-stakes)? what does "done fully" mean here (the concrete deliverable, length, audience, voice)? — and captures any genuinely blocking open questions rather than inventing answers. Output: `00-intake.md`.

**Station 2 — `axiom-research` (Research Lab).** *Model: sonnet. Tools: Read, Write, Glob, Grep, WebSearch, WebFetch.*
Gathers real, current information before anyone builds, because memory can be wrong. It looks up the best way to build the thing (rather than defaulting to habit — guarding R1), checks which sources and materials actually exist, reviews known pitfalls in the topic, and pulls the latest authoritative guidance. Its honesty rules are hard requirements: every fact carries a source; every fact is graded A/B/C/D; primary sources are preferred over aggregators; conflicting sources are both shown; citations are never fabricated. Output: `01-research.md`.

**Station 3 — `axiom-design` (Design Room).** *Model: sonnet. Tools: Read, Write, Glob, Grep.*
Designs *how* the job will be done before it is done — a written blueprint that can later be checked against the result. It produces the deliverable's structure and build steps; decides the **smallest team** that works (default: one assembly pass; add parallel parts only when sections are genuinely independent — guarding R6); and **plans the inspection in advance**, deciding what real artifact the inspector will read so it cannot later be fed a biased summary (guarding R5). It avoids stamping a generic template onto the job. Output: `02-plan.md`.

**Station 4 — `axiom-assembly` (Assembly Floor).** *Model: sonnet. Tools: Read, Write, Edit, Glob, Grep, WebSearch, WebFetch.*
Where the real work happens, at full effort. It builds the actual deliverable following the plan, under two non-negotiable rules: **label how sure you are** (every claim graded; D-grade guesses never reach the product) and **no claim without a source**. It leaves **receipts** — a record of which sources were read, which steps ran, and what was checked — because "I did it" is not proof (guarding R2). If new information forces a change to the plan or scope, it stops and flags it rather than quietly absorbing it. On a FIX-IT send-back, it reads the inspection report, addresses **every** finding, and records exactly what changed — without arguing with the inspector. Output: `03-deliverable.*`.

**Station 5 — `axiom-coach` (The Coach).** *Model: sonnet. Tools: Read, Glob, Grep (read-only by design).*
Asks one honest question before the work goes to inspection: *"Is this really your best, or just good enough?"* It **only asks** — it never writes the answer, rewrites the work, or edits files; its tools are read-only on purpose. It reads the deliverable directly and probes six angles — **clarity, detail, completeness, alternatives, honesty, and "the wow"** — and sanity-checks two process points (could the team have been smaller? is the inspection set up to be genuinely fair?). It ends with exactly one verdict: `SHIP-AS-IS`, or `ONE-IMPROVEMENT-PASS` with a specific list of changes for a single further pass — never an open-ended loop. Output: `04-coach.md`.

*Why the coach is not a duplicate of the inspector (guarding against R6).* The two stations ask different questions. The coach asks **"could this be better?"** — a cheap, read-only, quality-maximizing nudge *before* the expensive opus inspection, so weak work is caught early. The inspector asks **"is this true and on-scope?"** — an independent, authoritative correctness check that the gate depends on. The coach's read-only tool set is deliberate: it must not edit or re-bless the work, only prompt one improvement. Keep the two distinct; do not collapse them or let the coach start writing.

**Station 6 — `axiom-inspector` (Independent Inspector).** *Model: opus. Tools: Read, Glob, Grep, WebSearch, WebFetch.*
The most important station. The inspector is not the maker and owes them nothing; its only loyalty is to the truth and the reader. Its defining rule: **read the real work itself.** It opens the actual deliverable and inspects it directly — it does not accept, request, or rely on a summary from the maker, because a check based on the maker's story is theatre (R5). It verifies four things — the **result** (correct, complete, on-scope, verified the strongest way available), the **steps** (did every claimed step actually run; are the receipts real — guarding R4), the **plan** (was it followed; were changes announced), and the **honesty of claims** (every claim sourced and graded correctly; no D-grade guess leaked in; nothing dressed up beyond its evidence). It returns exactly one verdict: **PASS** (only cosmetic issues remain), **FIX-IT** (a real problem; every issue listed precisely; work returns to Assembly and is re-inspected from scratch), or **REJECT** (a serious failure — fabricated sources, faked steps, abandoned scope; the line stops). Output: `05-inspection.md`.

### 3.3 The orchestrator (the line manager)

The `/axiom` command is the orchestrator, or "factory line manager." It does not do station work itself; it coordinates the subagents, enforces the gate, and handles Shipping. It obeys the same 9 Axioms, 6 failure modes, and smallest-team rule as everyone else. Its full run procedure is Section 4.

**The delegation contract (every station, every time).** A subagent starts with a *fresh, isolated context*: it does not see the conversation history, the files the orchestrator has already read, or another station's work-in-progress — it receives only its own system prompt plus the working directory. So the orchestrator cannot rely on anything "carrying over." Every delegation must explicitly hand the subagent: (1) the **run-folder path** and the **exact input file(s)** to read, (2) the concrete task and its **required output filename and schema** (Section 3.5), and (3) the **discipline** it must obey. The constitution reaches every custom subagent automatically because it lives in `CLAUDE.md`, which Claude Code loads into each subagent's initial context; the separate `working-lessons` file does **not** auto-load, so it is either inlined into `CLAUDE.md` or the agent is told its path to read. Assuming a station "just knows" what came before is the most common way a file-passing pipeline breaks silently.

### 3.4 The Safety Gate

An **orchestrator-enforced** checkpoint between Inspection and Shipping — enforced because the orchestrator follows its own written rule and reads the artifact itself, not because a compiler blocks the path (see the note on "pipeline" in Section 1). It will **not open** unless all three are true:

1. A real inspection actually ran — an inspection artifact exists on disk **and is non-empty / schema-valid** (not merely present), and
2. The verdict came from the **inspector**, not from the maker's own words, and
3. The verdict is PASS.

The framework is honest about the gate's limit: it can confirm that an inspection *took place* — it cannot vouch that the inspection was thorough or fair. That thoroughness is the job of the independent inspector at Station 6. The gate is a check that the check happened, not a substitute for it.

**Write integrity (why the gate checks content, not just presence).** A subagent's `Write`/`Edit` can, under some sandbox configurations, report success while the file does not actually persist to disk — a silent, false-success failure. So a station's output is *verified*, not assumed: after each station returns, the orchestrator confirms the expected artifact exists and is non-empty before proceeding. If a write fails, the orchestrator does not invent a work-around and does not leave a silent gap — it retries once, and on continued failure writes the same content to a clearly named `*.pending.md` beside the target and reports the failure in the run summary. This is what stops a missing or stale artifact from either falsely opening the gate or falsely stopping the line.

### 3.5 Receipts — the run folder

Every run writes its artifacts to a timestamped folder, `.axiom/runs/<timestamp>-<slug>/`. These files *are* the receipts — the evidence that each step genuinely ran:

```
00-intake.md       triage decision + effort level + scope
01-research.md     facts, each with a source and a confidence grade
02-plan.md         the written plan + chosen team size + inspection plan
03-deliverable.*   the actual work (essay, paper, report, draft…)
04-coach.md        the single "can this be better?" pass
05-inspection.md   the inspector's verdict and findings (written by the inspector)
manifest.md        the proof package: confidence list, assumptions, receipts index
```

The inspector reads `03-deliverable.*` **directly**, never a summary — which is the mechanism that makes the independent check real rather than nominal.

**Each artifact has a required shape (this is what makes output consistent and checkable).** A fixed filename is not enough: for two runs to be *structurally* consistent and for the gate and inspector to verify "requirements met," every station fills a defined schema. Each station's output is rejected by the next station (or the gate) if a required section is missing. The minimum contract per artifact:

| Artifact | Required sections |
|----------|-------------------|
| `00-intake.md` | tier (tiny/normal/full/high-stakes); lane (full or quick); **deliverable definition** (concrete format, length, audience, voice); genuinely-blocking open questions (or "none") |
| `01-research.md` | a claim ledger — one row per fact with its **source** and **confidence grade (A/B/C/D)**; a "declared gaps" section for anything that could not be verified |
| `02-plan.md` | the deliverable's structure and build steps; the chosen **team size** (and why); the **inspection plan** (which real artifact the inspector will read) |
| `03-deliverable.*` | the actual work, with **inline confidence labels** on every factual claim; no D-grade guess present |
| `04-coach.md` | the single-pass verdict — `SHIP-AS-IS` or `ONE-IMPROVEMENT-PASS` with a specific change list |
| `05-inspection.md` | one verdict (PASS / FIX-IT / REJECT); **per-finding items with locations**; explicit confirmation each checklist area was covered |
| `manifest.md` | confidence list (every claim, grade, source); assumptions list; receipts index (the `00`–`05` artifacts) |

These schemas are the primary lever for consistency: because the shape is fixed, run-to-run variation is confined to content, and any station can mechanically check that the previous one delivered what was required.

### 3.6 The proof package

Nothing ships bare. Every delivery carries its own paperwork so the reader can both trust it *and* check it:

- **Confidence list** — every claim, its grade (A/B/C/D), and where it came from.
- **Assumptions list** — anything assumed but not proven, stated openly.
- **Receipts / trace** — the artifacts left by each step, so anyone can confirm each step truly happened.

This package is written at Shipping as `manifest.md`. The product is its own proof.

---

## 4. The workflow — a run, end to end

This is the orchestrator's procedure when a task enters via `/axiom <task>`. Each station's output feeds the next; the orchestrator reports a one-line status after each and never fakes a step.

**1. Set up the run.** Create `.axiom/runs/<UTC-timestamp>-<short-slug>/` and tell the user the run id. Every artifact below lands in this folder.

**2. Reception (Station 1).** Delegate to `axiom-intake`; read its `00-intake.md`. **If it lists open questions that genuinely block good work, stop and ask the user before continuing** (Axiom 1). If the lane is *quick* and effort is *tiny*, the orchestrator may run a compressed line (research-lite → assembly → inspection) — but must say so explicitly. No hidden shortcuts (R4).

**3. Research (Station 2).** Delegate to `axiom-research`. Do not proceed to design until facts with sources exist. No later station is allowed to build from memory.

**4. Design (Station 3).** Delegate to `axiom-design`. Honor its chosen team size; do not silently inflate it (R6).

**5. Assembly (Station 4).** Delegate to `axiom-assembly` to produce `03-deliverable.*`.

**6. Coach (Station 5).** Delegate to `axiom-coach`. If its verdict is `ONE-IMPROVEMENT-PASS`, send the deliverable back to `axiom-assembly` **once**, with the coach's specific notes, then continue. One pass, not a loop.

**7. Inspection (Station 6).** Delegate to `axiom-inspector`. **Critical for independence:** do not pass the inspector a summary of the work or argue the maker's case — just point it at the run folder and let it read `03-deliverable.*` itself (R5). Then read its verdict in `05-inspection.md` and act on it:

- **FIX-IT** → send back to `axiom-assembly` with the inspector's full findings, then run inspection **again from scratch** — the old report is never trusted on the next round. Repeat until PASS or REJECT, capped at 3 rounds; if still failing, treat as REJECT and report why.
- **REJECT** → stop the line. Report the failure honestly to the user. Do not ship.
- **PASS** → proceed to the gate.

**8. The Safety Gate.** Before shipping, the orchestrator verifies with its own eyes that `05-inspection.md` exists, was written by the inspector, and carries a PASS verdict. If any of that is missing, the gate stays shut and nothing ships.

**9. Shipping (Station 7).** Only now, assemble the proof package. Write `manifest.md` with the confidence list (every claim, grade, source), the assumptions list, and the receipts index (the `00`–`05` artifacts proving each step ran). Then deliver to the user: the finished work plus a short cover note linking the manifest, stating overall confidence and any open assumptions plainly.

**The send-back loop, in one line:** FIX-IT returns work to Assembly for a from-scratch re-inspection; REJECT stops the line; PASS (only tiny issues left) proceeds to the gate.

**Reporting style.** After each station the orchestrator gives a one-line status (for example, "✓ Research: 11 facts, all sourced"). The final delivery stays focused on the work and its proof, not on narrating the machinery. If a corner is ever cut, it is stated; a step is never faked.

Individual stations can also be run on their own when the full line isn't needed — for example, pointing `axiom-inspector` at an existing draft in a run folder for a standalone check.

---

## 5. Building your own

*This section is design commentary — interpretation of the source, not a description of it — offered to make the framework reproducible.*

AXIOM is worth copying not because of any single clever agent but because of how the pieces enforce each other. Four design principles carry most of the weight:

**Separate the discipline from the workers.** The rules (Golden Rule, 9 Axioms, grades, failure modes) live in one constitution file that every agent inherits. Single-sourcing the discipline is what stops it drifting between agents. Build this file first; treat the agents as instances that apply it.

**Make honesty mechanical, not aspirational.** "Read the real thing, not a summary" only holds because work is passed as **files on disk** and the inspector is a **separate agent with its own context** that literally cannot see the maker's reasoning. If your platform supports isolated sub-agents and file artifacts, independence is enforced by construction rather than by good intentions. This is the framework's most important structural bet.

**Right-size the team per job.** The effort tiers and the smallest-team rule exist to prevent both over-engineering a quick note and under-building a high-stakes analysis. A tier that never changes what actually runs is decoration; wire each tier to a concrete station set.

**Ship the proof with the product.** The manifest — grades, assumptions, and a receipts index pointing at the per-step artifacts — is what lets a reader both trust and audit the result. The receipts are a natural byproduct of giving each station its own output file.

A minimal recreation on Claude Code needs only this layout:

```
your-project/
├── CLAUDE.md                  ← the constitution: rules, grades, failure modes, gates
└── .claude/
    ├── agents/                ← one file per station worker
    │   ├── axiom-intake.md        (1 · Reception)
    │   ├── axiom-research.md      (2 · Research)
    │   ├── axiom-design.md        (3 · Design)
    │   ├── axiom-assembly.md      (4 · Assembly)
    │   ├── axiom-coach.md         (5 · Coach)
    │   └── axiom-inspector.md     (6 · Inspection)
    └── commands/
        └── axiom.md           ← the orchestrator: runs the line, enforces the gate, ships
```

Practical build order: (1) write the constitution; (2) write each agent as a short system prompt with a tight tool set and a model chosen for its job's difficulty — cheap models for triage and coaching, the strongest model for inspection; (3) give each agent a fixed output filename so the run folder becomes self-documenting; (4) write the orchestrator command to delegate station by station, enforce the send-back loop and the gate, and assemble the manifest at the end. Tune each agent's `model:` to trade cost against quality. System prompts can be written in English for portability even when the delivered output is in another language — output follows the language of the task, not the prompt.

The relationship to the original illustration is a clean mapping:

| In the original HTML illustration | In this runnable implementation |
|---|---|
| 7 factory stations | 6 subagents (stations 1–6) + Shipping (station 7) done by the orchestrator |
| The "robots" (worker, carrier, research, verifier, coach) | Mapped onto subagents + Claude Code's delegation mechanism |
| The Safety Gate | A check inside `/axiom` that won't open without a real inspection result |
| The 9 AXIOMS | The "9 Axioms" section of the constitution |
| The 6 failure modes (R1–R6) | The "6 failure modes" section of the constitution |
| The 4 confidence levels A–D | The grading table in the constitution, applied at every station |
| The send-back-for-fixes loop | The FIX-IT loop in `/axiom` (re-check from scratch, never trust the old report) |
| The delivery dossier | `manifest.md` — the confidence list, assumptions, and receipts index |

---

## Source note

This document synthesizes a single, closed source for its *description of the framework*: the local AXIOM v9 source folder — a README, the `CLAUDE.md` constitution, a `working-lessons.md` file, the `/axiom` orchestrator command, and six agent definition files. Every factual description of the framework is drawn directly from those files, which were read in full. Section 5 ("Building your own") is explicitly design commentary — the author's interpretation of *why* the framework is structured as it is — and is not a claim about anything stated in the source.

**Revision note.** This edition incorporates fixes from an independent review measuring the framework against two goals: that it fit how Claude actually works, and that its pipeline outputs be consistent and fulfil the framework's own requirements. Where the document now describes Claude Code's *platform behavior* — subagents run in isolated context windows and inherit no prior reads; `CLAUDE.md` auto-loads into subagents while a separate file does not; subagent delegation is model-driven rather than a hard-coded call graph; `AskUserQuestion` is unavailable inside subagents; subagents may nest to depth five as of v2.1.172; and subagent writes can silently fail to persist under some sandbox configurations — those statements are grounded in Anthropic's primary documentation, principally *Building effective agents* (anthropic.com/engineering/building-effective-agents) and the Claude Code *Create custom subagents* docs (docs.claude.com/en/docs/claude-code/sub-agents), read directly during the review. Those are the only external sources used; the framework's own files remain the authoritative description of the framework itself.