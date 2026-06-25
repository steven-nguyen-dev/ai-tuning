# lv1-compass — gap analysis & proposed architecture

*Goal: build **lv1-compass**, a plugin that is **assistant + advisor + critic + decision
partner**, based on lv1-writer. axiom is a reference for moves to borrow, **not** the
framework. lv1-coder stays the coding pipeline (it can do system design but not general
work). This doc is Part 1 (gap analysis) + Part 2 (proposed architecture) + the open
decisions I need from you before writing the build sequence.*

*Basis: a full read of `lv1-writer-plugin/` (SKILL, references, tone-profiles, the two
subagents), `sources/axiom/`, `lv1-coder-installer/`, and your own
`instruction-design-principles.md`. Claims about how those systems work are from their
files read this session; everything in Part 2–3 is my design judgment, marked as such.*

---

## Part 1 — Gap analysis: lv1-writer vs the four-role goal

### 1.1 What lv1-writer is — and what that means for compass

lv1-writer is a **production pipeline**: `triage → research → outline → draft → inspect →
ship`. Every part of it is shaped to end in a **made thing** — prose, a report. Its
discipline (A/B/C/D grading, steelman, declared gaps, conflict-hunting, the independent
inspector that re-reads the real file) is excellent and **role-agnostic** — it is exactly
the "honest thinking" engine a compass needs. The thing that is *writing-specific* is only
the surface: the wide genre/tone system (fiction, romance, children's, memoir…), which
compass doesn't need.

So the reusable core is large; the writing-specific surface is small and droppable.

### 1.2 The four roles, mapped to what exists today

| Role | Status | What already exists | What's missing |
|---|---|---|---|
| **Assistant** (do the task, produce a deliverable) | ✅ Built | the whole lv1-writer pipeline | scope-narrowing to work deliverables; office-format output (docx/pptx/xlsx) |
| **Critic** (review an artifact, verdict + fixes) | ✅ Built, mature | `lv1-inspect` (pipeline + standalone review mode), `lv1-writer-review`, axiom's six-angle coach | nothing structural — this is the strongest asset |
| **Advisor** (recommend, with reasoning & alternatives) | 🟡 Ingredients only | steelman, winners/losers synthesis, inline grades in `analytical-intelligence.md` | a mode whose **output is advice**, not a drafted artifact |
| **Decision partner** (help *you* own a choice between options) | 🔴 Biggest gap | adjacent only: the ADR skill `engineering:architecture` (per its description), `interview.md`'s batched-elicitation loop, memory + `runs/` receipts | a mode that frames a fork → lays out options → steelmans each → recommends with confidence → leaves the choice with you, output = a decision memo |

### 1.3 The two *structural* gaps (not ingredient gaps)

Most of the pantry is stocked. What's actually missing is two shapes:

1. **An output-shape gap.** Every current pipeline terminates in a *deliverable*.
   Assistant and critic fit that natively (a critique *is* an artifact). Advisor and
   decision-partner terminate in a **shared judgment you own** — the "deliverable" is your
   better thinking, optionally captured as a one-page memo. The `draft → ship` tail
   doesn't fit "help me decide." This is why compass feels close but not done: you'd be
   bending a make-machine into a think-with-me machine.

2. **An intent-router gap.** Triage today detects *genre / rigor / authorship* — all
   writing parameters. Nothing detects **intent**: is this a *make* task, a *review*
   task, a *challenge* task, or a *decide* task? Compass needs that router up front, the
   way `tone-detect.md` routes voice today.

### 1.4 The pantry — what ports directly (verified present on disk)

- The **constitution**: A/B/C/D grading + the `[A]` provenance test, steelman rule,
  declared-gaps, conflict-hunting, independent inspection, effort tiers, smallest-team.
  This is the shared core for *all* four roles.
- **`lv1-inspect`** — already runs both a pipeline check and a standalone review mode.
  This *is* the critic, almost unchanged.
- **`lv1-research`** — graded claim-ledger subagent. Powers assistant *and* advisor
  (you can't advise on unsourced facts).
- **`interview.md`** — the batched-elicitation loop (one grouped round, never drip).
  Repurposable to elicit *your constraints / values / risk tolerance* for a decision,
  instead of eliciting an author's lived material.
- **`analytical-intelligence.md`** — hypothesis spine, steelman, winners/losers,
  inline grades: the spine of good advice, already written.
- **Memory + `runs/` receipts** — for remembering past decisions and revisiting them.
- **Plugin/installer packaging** — init, library sync, marker-bounded `CLAUDE.md` edits.

**Verdict: ~70% of the ingredients are in the pantry.** The honest-thinking engine, the
critic, the elicitation loop, and the proof machinery all exist. The genuine build is the
**two judgment-modes' output contracts** + the **intent router** + a **scope/format
narrowing** of the assistant. Real work, but contained — the hard part (the discipline) is
done and battle-tested.

---

## Part 2 — Proposed architecture

### 2.1 The shape: one shared core + two skills + shared subagents

Your own `instruction-design-principles.md` argues against a "does-everything" mega-skill
(#9: vague description → unreliable triggering) and for a shared core + thin packs (#6) and
single-sourced rules (#2). Applied here:

```
lv1-compass (plugin)
├── core/                      the constitution — single-sourced, both skills import it
│   ├── constitution.md        bar, core rules, A/B/C/D grades, [A] test, steelman,
│   │                          effort tiers, smallest-team, failure modes
│   └── working-lessons.md     neutral≠timid; label=commit harder; show the arithmetic;
│                              steelman≠false balance
├── skills/
│   ├── lv1-assistant/         MAKE — work deliverables (your skill 1)
│   └── lv1-advisor/           JUDGE — review · challenge · decide (your skill 2)
└── agents/
    ├── lv1-research.md        reused from lv1-writer (graded claim ledger)
    └── lv1-inspect.md         reused from lv1-writer (independent check + review mode)
```

The two skills **share the same engine** and differ only in **terminal output**:
assistant ends in a deliverable; advisor ends in a judgment you own.

### 2.2 Skill 1 — `lv1-assistant` (the maker)

**Purpose:** produce work deliverables — analyses, reports, memos, briefs, decks,
spreadsheets. Narrower than lv1-writer: **drop** the fiction/romance/children's/memoir
genres and the wide tone system; **keep** the work-register profiles
(`analytical-intelligence`, `technical`, `academic`, `persuasive`) collapsed into a small
fixed set (likely just *analytical* and *neutral-professional*).

**Pipeline:** the lv1-writer line, slimmed —
`triage → research → outline → draft → inspect → ship` — with two changes:

- **No tone-detection** beyond a 2-way work-register pick; no per-genre profile files.
- **Format follows content, including office formats.** A work deliverable is often a
  `.docx`, `.pptx`, or `.xlsx`, not just prose. The draft station chooses the artifact
  type and hands to the `docx`/`pptx`/`xlsx` skills. *(This is a capability question —
  see decision D3.)*

Inspection and the proof package (inline grades, declared gaps, appendix) are **kept** —
they're the whole point of a trustworthy work deliverable.

### 2.3 Skill 2 — `lv1-advisor` (the judge: review · challenge · decide)

You folded critic + advisor + decision-partner into one "advisor." I think that's right —
they share one engine (skeptical, sourced, steelman-first) — **provided** the skill has an
internal **mode router**, exactly like lv1-writer routes init/review/task. Three modes:

- **review** — input is an existing artifact (doc, plan, your draft). Output = a verdict
  (PASS / FIX-IT / REJECT) + located findings + concrete fixes. *This is `lv1-inspect`'s
  standalone review mode, already built.*
- **challenge** — input is a position, plan, or belief you hold. Output = the strongest
  steelman against it, the failure modes, the un-asked question — then whether it still
  stands. Proactive red-team. *Reuses steelman + the coach's six-angle probe.*
- **decide** — input is a fork ("X or Y?"). Output = framed options → trade-offs →
  steelman of each → a **recommendation with a confidence grade** → the choice left with
  you, captured as a one-page **decision memo** (the advisor's equivalent of the
  manifest). *This is the genuinely new contract — see decision D1.*

All three end in a **judgment you own**, never a silently-shipped artifact. The advisor
elicits your constraints/values/risk-tolerance via the `interview.md` batched-round loop
when a decision needs them.

### 2.4 Shared subagents — reuse, don't rebuild

`lv1-research` and `lv1-inspect` are role-agnostic and already isolated. lv1-compass
should **share** them, not fork them. (If you keep lv1-writer installed too, decide
whether compass copies them or both plugins point at one shared definition — see D4.)

### 2.5 How compass relates to lv1-writer

My recommendation: **lv1-compass is a new, slimmed sibling of lv1-writer, and lv1-writer
stays** for books / wide-genre / long-form work. Compass copies the engine, drops the
genre system, adds the advisor's decide-mode. Same discipline, narrower aperture, plus a
judgment role lv1-writer doesn't have. *(See decision D2.)*

---

## Part 3 — Open decisions (my concerns, steelmanned)

These genuinely change the build, so I want your call before writing the build sequence.

**D1 — Is `decide` (decision-partner) in v1, and where does it live?**
The decision-partner is the biggest gap *and* the role with a brand-new output shape (a
decision memo, not a deliverable). Options: (a) a **mode inside lv1-advisor** —
*recommended*, keeps your 2-skill shape, shares the engine; (b) a **deliverable type
inside lv1-assistant** — wrong, it conflates "make me a thing" with "help me choose"; (c)
**defer to v2** — ship review+challenge first. My recommendation: **(a)**. Concern: it's
the most new contract to write, so if you want a fast v1, (c) is legitimate.

**D2 — Compass vs lv1-writer: new sibling, replace, or evolve-in-place?**
(a) **New slim sibling, keep lv1-writer** for books — *recommended*; (b) **replace**
lv1-writer with compass (you lose the wide-genre book pipeline); (c) **evolve lv1-writer
in place** (rename + add advisor — risks bloating the book pipeline with work concerns).
Recommendation: **(a)**.

**D3 — Does `lv1-assistant` produce office formats (docx/pptx/xlsx), or prose only?**
"Work deliverables" usually means Word/PowerPoint/Excel, not just markdown prose. (a)
**Wire in docx/pptx/xlsx** so the assistant can ship a real deck or model — *recommended*;
(b) **prose/markdown only**, you convert later. Recommendation: **(a)**.

**D4 — Shared core: one constitution both skills import, or each skill self-contained?**
Your principle #2 (single-source) and #6 (shared core) both say factor it out. (a)
**Shared `core/constitution.md`** imported by both skills + both subagents — *recommended*;
(b) each skill self-contained (simpler to read, but the discipline drifts across copies).
Recommendation: **(a)**. Minor concern: a plugin-level shared file both skills read is the
cleanest, but I'll confirm the import mechanism works the same way in the Cowork/plugin
runtime as in Claude Code before relying on it.

---

## Part 4 — Build sequence (deferred)

I'll write the file-by-file build sequence (in the style of
`lv1-writer-build-sequence.md`, split into runnable parts) **once D1–D4 are settled**, so
the plan isn't built on an unresolved fork — the same "don't paper over a gap" discipline
the system itself enforces.
