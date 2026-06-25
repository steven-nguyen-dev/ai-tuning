# lv1-compass build sequence — split into 5 runnable parts

*Companion to `lv1-compass-gap-and-architecture.md`. Decisions locked: D1 decide-mode
lives inside `lv1-advisor`; D2 compass is a **new slim sibling**, lv1-writer stays; D3
`lv1-assistant` ships office formats (docx/pptx/xlsx) + prose; D4 one shared
`core/constitution.md` imported by both skills.*

*Each part is a self-contained run: open this file + the architecture doc, do only that
part's files, run its **Verify** line, stop. A fresh session resumes from the next part.
All paths are relative to `lv1-compass-plugin/`. **[new]** = create, **[copy]** = port from
lv1-writer with edits.*

**Shared-core mechanism (settled):** both skills get the constitution the proven
lv1-writer way — the `SKILL.md` instructs *"read `core/constitution.md` and follow it"* at
the start of every run. It is a **read instruction, not an `@import`**, so it works
regardless of how the runtime resolves imports. Part 1's dry-run confirms the relative
path resolves from each skill dir.

---

## Part 1 — Plugin skeleton + shared core + subagents

The layer everything keys off. No skill logic yet — this is the constitution and the
reused machinery.

- **[new] `.claude-plugin/plugin.json`** — name `lv1-compass`, description, version.
- **[new] `core/constitution.md`** — the single-sourced discipline, ported from
  lv1-writer's *The bar* + *Core rules* + *Confidence grades* + *the `[A]` provenance
  test* + *effort tiers*, plus axiom's *six failure modes* and *smallest-team rule*
  (R1–R6 are the cleanest statement). Strip every writing/genre-specific line — this core
  serves both make and judge roles.
- **[new] `core/working-lessons.md`** — port lv1-writer's: neutral≠timid; a label is a
  reason to commit harder; a numeric claim isn't a claim until the arithmetic is shown;
  steelman ≠ false balance; a user's stated position is a signal to investigate, not a
  conclusion to confirm.
- **[copy] `agents/lv1-research.md`** — from lv1-writer, near-verbatim (graded claim
  ledger, conflict-hunt, steelman-material gathering). Point its "read the constitution"
  line at `core/constitution.md`.
- **[copy] `agents/lv1-inspect.md`** — from lv1-writer. Keep both pipeline + standalone
  review mode. Strip the fiction/craft-contract branches (compass has no fiction). Point
  its constitution reference at `core/`.

**Verify:** `core/constitution.md` contains the bar, A/B/C/D + `[A]` test, the rules, the
failure modes, effort tiers — and **nothing writing-genre-specific**. From a scratch skill
dir, confirm a relative read of `../../core/constitution.md` resolves (dry probe).

---

## Part 2 — `lv1-assistant` skill (the maker)

The slimmed lv1-writer pipeline for work deliverables. **No wide genre/tone system** —
just a 2-way work register and office-format output.

- **[new] `skills/lv1-assistant/SKILL.md`** — orchestrator. Opens by reading
  `core/constitution.md`. Pipeline: `triage → research → outline → draft → inspect →
  ship`. Description triggers on "write/draft an analysis, report, memo, brief, deck,
  model, deliverable" — **not** on review/decide language (that's advisor's).
- **[new] `skills/lv1-assistant/references/triage.md`** — sets two knobs only: **rigor
  tier** (tiny/normal/full/high-stakes, ported) and **output format** (prose · docx ·
  pptx · xlsx — chosen from the ask). Records `00-triage.md`. No genre detection.
- **[new] `skills/lv1-assistant/references/register.md`** — the 2 work voices:
  **analytical** (decisive, hypothesis-spine, winners/losers, inline grades — ported from
  `analytical-intelligence.md`) and **neutral-professional** (clear, measured, briefing
  voice). That's the whole tone surface.
- **[new] `skills/lv1-assistant/references/draft.md`** — compose voice from register;
  **format-follows-content incl. office handoff**: when the deliverable is a deck/model/
  Word doc, the draft station hands the structured content to the `pptx`/`xlsx`/`docx`
  skills rather than emitting markdown. Apparatus-by-rigor (methodology, inline grades,
  in-text declared gaps, appended proof package) ported from lv1-writer.
- **[new] `skills/lv1-assistant/references/outline.md`** — ported; descriptive vs
  hypothesis-driven shape, steelman section required when the conclusion is contested.
- **[copy] `skills/lv1-assistant/references/source-intake.md`** — from lv1-writer.

**Verify:** an `analytical`, `full`, `pptx` task routes triage → research → outline →
draft(hand to pptx) → inspect → ship; the proof package is appended; no genre questions
are asked.

---

## Part 3 — `lv1-advisor` skill: mode router + review & challenge

The judge. One skill, internal **mode router** (review · challenge · decide), built in two
parts (this part: the router + the two reactive modes; Part 4: decide).

- **[new] `skills/lv1-advisor/SKILL.md`** — opens by reading `core/constitution.md`.
  **Mode router** at the top: classify the ask into **review** (an artifact handed over),
  **challenge** (a position/plan/belief to red-team), or **decide** (a fork "X or Y?").
  Ambiguous → one batched clarifying round. Description triggers on "review / critique /
  fact-check / challenge / push back / steelman / poke holes / should I / which should I
  / help me decide / X or Y".
- **[new] `skills/lv1-advisor/references/review-mode.md`** — delegate to the
  `lv1-inspect` subagent in standalone review mode (generates a doc-specific checklist,
  writes `review-feedback.md`, verdict PASS/FIX-IT/REJECT, located findings + concrete
  fixes). Read-only — never edits the artifact.
- **[new] `skills/lv1-advisor/references/challenge-mode.md`** — the red-team contract:
  (1) restate the user's position in its strongest form (no straw man), (2) assemble the
  **steelman against it** from sourced material (delegate to `lv1-research` if facts are
  needed — no challenge from memory), (3) the six-angle probe (clarity, completeness,
  alternatives, honesty, the un-asked question, the failure mode), (4) verdict: *holds /
  holds-with-revisions / does-not-hold*, each with the reasoning. Output is a judgment,
  not a rewrite.

**Verify:** "review this memo" routes to review-mode → lv1-inspect; "poke holes in my plan
to drop the Java team" routes to challenge-mode → sourced steelman + verdict, no artifact
produced.

---

## Part 4 — `lv1-advisor` decide mode + the decision memo (the new contract)

The genuinely new shape: output is **a choice you own**, captured as a one-page memo.

- **[new] `skills/lv1-advisor/references/decide-mode.md`** — the contract:
  1. **Frame** the decision: the real question, what's actually at stake, what's
     reversible vs not, the deadline.
  2. **Elicit your basis** via the batched-round loop (ported from lv1-writer's
     `interview.md`, repurposed): your constraints, values, risk tolerance, success
     criteria — one grouped round, never drip. A refusal to answer a load-bearing item =
     a declared gap, not a guess.
  3. **Options** — enumerate the real ones (incl. the do-nothing / default).
  4. **Trade-offs + steelman each** — the strongest case *for each* option and the
     strongest case *against your apparent front-runner*. Facts sourced via
     `lv1-research`; each claim graded A/B/C/D.
  5. **Recommendation with a confidence grade** — commit (neutral≠timid), state what
     would change the recommendation, and **leave the choice explicitly with you**.
- **[new] `skills/lv1-advisor/assets/decision-memo-template.md`** — the advisor's
  manifest-equivalent: *Decision · Framing · Your stated basis · Options table ·
  Trade-offs & steelmans · Recommendation [grade] · What would change it · Open
  gaps/assumptions · Sources*. Written to `runs/<id>/` and offered as a standalone file.
- **[edit] `skills/lv1-advisor/SKILL.md`** — wire decide into the mode router; on
  `high-stakes` decisions, require the steelman-of-the-front-runner step (the inspector
  fails a decide-output that recommends without one).

**Verify:** "should I rebuild the team around AI agents or keep the Java stack?" routes to
decide-mode → batched basis round → options → steelman of the recommendation → memo with a
graded recommendation that leaves the call to the user.

---

## Part 5 — `lv1-compass-init` + final consistency pass

Setup and the policing pass that makes the above real.

- **[new] `skills/lv1-compass-init/SKILL.md`** — interactive but light: ask **project
  domain/topic**, **default work register** (analytical | neutral-professional | you
  decide), and **working mode** (auto | interactive). Then: create `sources/library.md`
  (+ sync), `runs/`, and a marker-bounded block in `CLAUDE.md` describing the compass
  layout. No genre/fiction questions. Safe to re-run (create-if-missing).
- **[new] `skills/lv1-compass-init/assets/`** — `library-template.md`,
  `claude-md-template.md` (ported, compass-flavored).
- **Final consistency pass (no file):** grep the plugin for references to files that
  should exist (both subagents, every reference both skills name, the memo template);
  confirm both `SKILL.md`s read `core/constitution.md` and the relative path resolves;
  confirm the assistant↔advisor descriptions don't overlap-trigger (make vs judge).
  **Dry-run:** one make task (analytical report → pptx) and one decide task; check the
  router sends each to the right skill/mode.

**Verify:** init scaffolds a project with no genre prompts; the dry-run routes a make task
to assistant and a decide task to advisor/decide; the constitution is read once per run
from the shared core.

---

## At-a-glance

| Part | Theme | New | Copy/Port |
|---|---|---|---|
| 1 | Skeleton + shared core | plugin.json, core/constitution, core/working-lessons | lv1-research, lv1-inspect |
| 2 | lv1-assistant (maker) | SKILL, triage, register, draft, outline | source-intake |
| 3 | lv1-advisor: router + review + challenge | SKILL, review-mode, challenge-mode | — |
| 4 | lv1-advisor: decide + memo | decide-mode, decision-memo-template | (interview loop ported) |
| 5 | init + consistency | compass-init SKILL + assets | library/claude-md templates |

Each part is one safe run. Partial states are safe: until a skill exists, the other still
works; the system is only *fully* wired after Part 5. If a part feels too large mid-run,
stop at a file boundary and note where — the next run resumes from this file.

## What NOT to change

Keep the claim-ledger forcing function, the REDO-RESEARCH coverage gate, the isolated
research/inspect subagents, and the proof package. Compass **narrows** lv1-writer's
aperture (drops genres, adds the judge role) — it does not re-litigate the engine.
