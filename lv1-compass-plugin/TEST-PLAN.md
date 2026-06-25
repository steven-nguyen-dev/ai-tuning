# lv1-compass — post-install verification

A short manual test. Run each step in a **fresh, empty project folder** in Cowork (or
Claude Code) with the plugin installed. For each test: paste the prompt, then check the
"Pass if" line. The "Verifies" tag ties it back to the review checklist.

> The single most important thing to confirm is **T2/T3**: that the isolated subagents
> still apply the constitution even though they can't read `core/` from their own path (F1).
> The proof is simple — graded claims and a real verdict show up in their output files. If
> the subagents ran blind, those would be missing or generic.

---

## T0 — Install sanity
**Prompt:**
```
what lv1-compass skills and agents do I have?
```
**Pass if:** `lv1-assistant`, `lv1-advisor`, `lv1-compass-init` are listed as skills and
`lv1-research`, `lv1-inspect` as agents.
**Verifies:** layout / install (Pass contracts 3A).

---

## T1 — init scaffolds the project
**Prompt:**
```
lv1-compass init
```
**Pass if:** it creates `sources/library.md` and `runs/`, writes a marker-bounded
`lv1-compass` block into `CLAUDE.md`, and asks (or records) the default register + working
mode. Re-run it once — it should not duplicate the CLAUDE.md block or wipe anything.
**Verifies:** init flow, safe re-run.

---

## T2 — make pipeline + markdown-only + convert-after (the big one)
**Prompt:**
```
draft a full-rigor analytical board memo as a docx on the current state of
the EU AI Act's obligations for general-purpose AI models
```
**Pass if — check the `runs/<id>/` folder and `manuscript/`:**
- `00-triage.md` shows rigor = `full`, output described as **markdown** with a **convert-to: docx** note (no "output-format knob").
- A **markdown** deliverable is written to `manuscript/` *and* inspected **before** any `.docx` exists.
- The `.docx` appears only **after** the inspector's verdict — as the explicit convert step, not mid-pipeline.
- `01-research.md` exists and is a **graded claim ledger** (rows marked A/B/C, each with a source).
- `04-inspection.md` carries **one verdict** (PASS / FIX-IT / REDO-RESEARCH / REJECT).

**Verifies:** F2 (markdown-only + post-inspection convert), F4 (full tier runs research+outline+inspect), and — because the research ledger is graded and the inspector returned a real verdict — **F1** (subagents received the constitution).

---

## T3 — confirm the subagents weren't blind (F1, explicit)
After T2, **open the two subagent outputs** and eyeball them:
**Prompt:**
```
show me runs/<id>/01-research.md and runs/<id>/04-inspection.md
```
**Pass if:**
- `01-research.md` uses the **A/B/C/D grades** and refuses to grade unfetched claims `A` (proves `lv1-research` applied the constitution it was handed).
- `04-inspection.md` judges against the bar/grades and names located findings (proves `lv1-inspect` applied it).
**Fail signal:** either file is generic, ungraded, or says it "couldn't find core/constitution.md" → the orchestrator isn't passing the discipline in; re-check the delegation note in each SKILL.md.
**Verifies:** F1.

---

## T4 — tiers actually gate stations (F4)
**Prompt (tiny, no facts):**
```
tiny task: tighten this sentence, no facts needed —
"In order to facilitate the utilization of the system, we have created a guide."
```
**Pass if:** it runs the **fast lane** — returns the rewrite with **no** `01-research.md`,
`02-outline.md`, or `04-inspection.md` created. Contrast with T2's full run folder.
**Verifies:** F4 (tier changes which stations run), fast-lane station set.

---

## T5 — fast-lane escalates on a factual claim (F5)
**Prompt (looks tiny, but contains a fact):**
```
quick one-liner: what is Vietnam's current population?
```
**Pass if:** instead of answering from memory, it **escalates out of the fast lane** —
sources the figure and grades it (or states it's escalating because the task carries a
factual claim). It must **not** ship an unsourced number.
**Verifies:** F5 (no source, no claim — even on a "tiny" ask).

---

## T6 — advisor review writes both files, one verdict (F3)
Use the markdown memo from T2 (in `manuscript/`).
**Prompt:**
```
review manuscript/<the-memo>.md
```
**Pass if:** `lv1-inspect` (standalone mode) writes **both** `review-checklist.md` and
`review-feedback.md` beside the document; the feedback has one verdict
(PASS / FIX-IT / REJECT) with located findings (section · what's wrong · fix · severity);
and it does **not** edit the memo itself.
**Verifies:** F3 (single inspection contract), read-only inspector.

---

## T7 — advisor delegates to research with a run folder (F6)
**Prompt (decide mode, needs outside facts):**
```
should we standardize our backend on PostgreSQL or stay on MySQL for a new
high-write analytics service? help me decide.
```
**Pass if:** when it needs facts it delegates to `lv1-research`, and a `runs/<id>/` folder
with `01-research.md` is created **even though this is a quick judgment**; it ends with a
graded recommendation, a "steelman of my own front-runner" section, and **leaves the choice
with you**.
**Verifies:** F6 (research output location defined in quick mode), decide-mode contract.

---

## Scorecard

| Test | Verifies | Result |
|---|---|---|
| T0 | install / layout | ☐ |
| T1 | init + safe re-run | ☐ |
| T2 | F2 markdown→convert, F4 full tier, F1 | ☐ |
| T3 | F1 subagents got the discipline | ☐ |
| T4 | F4 tiers gate stations | ☐ |
| T5 | F5 fast-lane escalation | ☐ |
| T6 | F3 single inspection contract | ☐ |
| T7 | F6 advisor research folder | ☐ |

If T2/T3 fail, fix F1 first — everything downstream depends on the subagents actually
running under the constitution.
