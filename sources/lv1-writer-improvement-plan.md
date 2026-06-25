# lv1-writer → axiom level: improvement plan

*Scope: raise the lv1-writer plugin so its delivered work reaches the rigor and intellectual weight of the axiom output, and add automatic book/document-type detection that sets the writing tone. Based on a full read of both systems and the two real reports.*

Sources read: `sources/axiom/` (CLAUDE.md, command, 6 agents, working-lessons), `lv1-writer-plugin/` (SKILL.md, README, 2 agents, 4 references, 5 assets, 2 sibling skills), and the three artifacts in `sources/` (lv1-writer report 150 lines, axiom report 437 lines, comparison 93 lines).

---

## Part 1 — Analysis

### 1.1 What each system actually is

The two are the *same idea* — a disciplined research-then-write pipeline that ships only "impressive AND true" with checkable proof — built twice. They even share vocabulary: the 9 Axioms ≈ lv1-writer's 8 core rules, the same A/B/C/D confidence grades, the same independent-inspector principle, the same "read the real artifact, never a summary" rule.

The difference the comparison nails: lv1-writer produces a **Market Overview** (clean, journalistic, skimmable), axiom produces an **Audited Intelligence Memorandum** (a thesis-driven, self-auditing document a third party could re-verify line by line). That gap is not language skill. It is **where the discipline lands**.

### 1.2 The root cause of the gap (one diagnosis, four symptoms)

**lv1-writer keeps its discipline in *process files*; axiom bakes its discipline into the *delivered artifact*.**

lv1-writer's contracts are excellent — arguably more sophisticated than axiom in two places (see 1.4). But the rigor lives in `runs/<id>/` receipts and `sources/library.md`, and the *shipped* document is allowed to be a clean prose summary with bare `[1][2]` footnotes. Axiom forces the rigor onto the page: every claim carries its grade inline, the methodology and enforcement rules open the document, the steelman is a required section, and the proof package (confidence table, assumptions, source index, declared gaps) ships *inside* the deliverable.

Concretely, the lv1-writer report:

- Uses bare citation numbers `[1]…[23]`, **no inline A/B/C/D grades** — yet lv1-writer's own rule 4 demands confidence labels on every factual claim. The grades existed in `library.md` and got dropped on the way to the reader.
- **Papered over a real data gap**: it merged front-end/back-end salary into one approximate table with a footnote, where axiom declared "I cannot separate lead-level front-end; I refuse to invent the number." lv1-writer's research contract *has* a `DECLARED GAP` mechanism — it just never surfaced into the prose.
- Reported "FPT giảm 497 vị trí (~1%)" **flat**, where axiom hunted the conflict, found the viral "FPT sacked 26,000" rumor, reconciled 54,110 (consolidated) vs 85,991 (group incl. affiliates), and debunked it with a dated denial. lv1-research says "if sources conflict, keep both" but never mandates *hunting* for conflicts.
- Is **descriptive/horizontal** (cut the market into 5 slices, describe each) rather than **hypothesis-driven/vertical** (plant one mechanism, use it as a lens through every chapter). No central thesis, no steelman, no winners/losers synthesis — even though lv1-writer rule 7 explicitly requires steelman before any conclusion.

So most of the gap is not missing capability — it is **capability that doesn't reach the page** plus **a few genuinely missing forcing functions** (methodology section, conflict-hunting, hypothesis spine, mandatory steelman section).

### 1.3 lv1-writer — pros and cons

**Strengths (preserve these):**

- **Isolated subagents with a claim-ledger forcing function.** `lv1-research` replaces the vague stop condition ("I have enough") with a *count*: enumerate the claims the piece needs first, then resolve every row to A/B or an explicit DECLARED GAP. This is a cleaner anti-thin-research device than axiom's prose research brief.
- **A coverage gate that can send research back.** The `REDO-RESEARCH` verdict (distinct from `FIX-IT`) lets the inspector reject work that is *true but built on too few sources* and route it back to research, not the draft. Axiom has no equivalent — its loop only re-runs assembly.
- **Document-type classification already exists** — in *review* mode. The review checklist (Pass 3) classifies type / core promise / intended reader / key contracts and generates type-specific checks. The seed for tone detection is already in the codebase; it just isn't wired into the *writing* path.
- **Smoother reader-facing prose.** The comparison scores lv1-writer higher on readability and flow. This is a real asset for non-specialist/PR audiences and must not be sacrificed.
- **Clean plugin packaging**: init/sync, library management, orphan flagging, safe re-runs, marker-bounded CLAUDE.md edits.

**Weaknesses (the gap):**

- Confidence grades don't ship inline in the prose.
- The proof package (manifest) lands in `runs/`, not in the delivered document.
- No methodology / enforcement / recency-window section in the output.
- No active conflict/pitfall hunting in research.
- No hypothesis-spine option; default structure is descriptive.
- Steelman is a rule but not an enforced output section.
- DECLARED GAPs don't surface into the body as honest "I don't have this" statements.
- **One fixed tone.** `writing-instruction.md` hard-codes a single voice (science-grounded narrative nonfiction) at init and is never changed automatically. There is no per-task genre detection or tone selection — the user's central request.

### 1.4 axiom — pros and cons

**Strengths (port these):**

- **Methodology + enforcement section up front** (§2): explicit analysis window, "every headline number must be in-window," "metrics without an in-window comparison are dropped, not guessed," and named red-line exclusions (the Robert Walters ban). This is what creates "evidential weight."
- **Inline confidence grades** on every claim in the body, not just an appendix.
- **Hypothesis-driven spine**: one central mechanism stated early, every chapter opened with the same lens question ("What is AI doing to X?") and closed with a "Winners vs Losers" synthesis.
- **Steelman as a required section** (§5): assemble the strongest opposing case from the data, then break it.
- **Active conflict resolution**: the FPT 26,000 debunk, JobOKO vs TopCV 16.92% kept un-conflated, Vingroup vs Navigos salary tiers kept separate.
- **Honest gaps declared twice** — once up front, once inline where the reader expects the number.
- **A full proof appendix that ships**: 52-row confidence table, assumptions list, source index with in-window flags, data-limits section, receipts, and a coach-pass changelog.

**Weaknesses (do not blindly copy):**

- **Heavy, high cognitive load.** The comparison marks it down on readability; "§"-coded, enforcement-rule-first, dense. Wrong register for a general audience or for fiction.
- **One implicit genre.** Axiom is tuned for the audited analytical memorandum. It has no tone switch either — it would render a romance or a children's book in the same forensic voice. So axiom does *not* solve the user's tone-detection ask; lv1-writer has to invent that.
- **Discipline-as-prose** (research brief in paragraphs) is less mechanically enforceable than lv1-writer's claim ledger.
- Proof apparatus is unconditional — appropriate for intelligence reports, absurd for a novel.

### 1.5 The synthesis target

The win condition is **not** "make lv1-writer write like axiom." It is: **keep lv1-writer's superior process machinery (claim ledger, coverage gate, isolated subagents, clean prose) and add axiom's artifact-level discipline (inline grades, methodology section, hypothesis spine, steelman, shipped proof package, conflict hunting) — but make all of that apparatus *conditional on a detected genre/tone*, so an intelligence report gets the full forensic treatment and a romance novel gets none of it.**

---

## Part 2 — Points to improve (prioritized)

Priority is by impact-on-gap × alignment-with-the-request.

**P0 — Three-knob genre/tone/authorship system + interactive init (the explicit ask).**
Add a tone system with **three independent knobs**: a **genre/tone profile** (voice, structure, register), a **rigor tier** (already exists: tiny/normal/full/high-stakes; controls how much proof apparatus ships), and an **authorship mode** — *AI-autonomous* (AI researches and drafts on its own) vs *interview-driven* (the user supplies the lived material; AI elicits it in batched question rounds). Init becomes **interactive**: it asks the user for topic, genre, and tone (the user can answer, or say "you decide"), proposes an authorship mode from the genre, and records all of it. Triage on each task confirms/overrides the genre, selects the tone profile, and the draft station composes the final voice. The inspector checks the draft *against the chosen profile and mode*. Reuse the existing review-mode classifier logic.

**P0 — Interview-driven authorship for experience/case-based books.**
For genres where the substance comes from the user's own life or chosen cases (personal experience, love/relationships, mindset, and often cognitive-behavior), AI must **not** invent or fully autonomously source the core material. Instead: gather a structured **intake interview** at the start (all open questions grouped into *one round*, not drip-fed), then, while drafting, whenever a concern or missing piece appears, pause and ask again — each time as a **single batched round** of grouped questions. AI may still research *around* the user's material (e.g. the science behind a felt experience), but the spine stays the user's. This authorship mode is selected at init and per-task.

**P0 — Surface confidence grades into the delivered prose.**
For non-fiction/rigorous genres, inline A/B grades on every factual claim in the body, not just in `library.md`. This is the single biggest visible difference and it is already required by rule 4 — it's an enforcement failure, not a new feature.

**P0 — Ship the proof package inside the document.**
Append the manifest (confidence table, assumptions, source index, declared gaps) to the delivered file for rigorous genres — not only to `runs/`. The reader should be able to verify without opening receipts.

**P1 — Methodology / enforcement section in the output.**
For full/high-stakes analytical work, open the document with a short methodology block: recency window, source spine, enforcement rules (in-window rule, drop-don't-guess), and red-line exclusions. Promote the triage "research contract" so it surfaces here.

**P1 — Hypothesis-spine structure mode + winners/losers synthesis.**
Offer a hypothesis-driven structure (state one central mechanism early; every section uses it as a lens; close with a synthesis) as a selectable shape for analytical/argumentative genres — alongside the existing descriptive shape.

**P1 — Steelman as an enforced output section.**
For any piece that reaches a contested conclusion, require a visible steelman section (strongest opposing case, then why the conclusion still stands or is revised). Make the inspector fail a draft that concludes without one.

**P1 — In-text honest-gap flagging.**
Pipe `01-research.md`'s DECLARED GAP rows into the body at the point the reader expects the missing number, plus a data-limits appendix entry. Forbid papering a gap over with a merged/approximate table presented as complete.

**P1 — Format follows content (per-section presentation choice).**
AI chooses the presentation format for each chapter/section from the *content type*, not a fixed template: a **table** when conveying multiple data points, stats, or numbers that invite comparison; **bullet points** when listing several discrete ideas or key points; **dashes/arrows** (e.g. `A → B → C`) when describing a loop, sequence, pipeline, or cause-effect chain; and **prose** when the reader should *experience* something (a person, an unfolding process, a reflection). The rule is a deliberate decision point — before writing a section, AI asks "what shape does this content want?" — not a license to over-format. This must respect the genre: fiction and narrative profiles default to prose and use structural markup sparingly; analytical/technical profiles lean on tables and diagrams freely.

**P2 — Active conflict/pitfall hunting in research.**
Add an explicit research step: search for the most-cited *wrong* or viral number in the topic and reconcile it. Make a resolved-conflicts list part of the research output and a thing the inspector checks.

**P2 — Tighten working-lessons reuse.**
Axiom carries a `working-lessons.md` read at session start (neutral≠timid; a label is a reason to commit harder; a numeric claim isn't a claim until the arithmetic is shown). lv1-writer has the same ideas scattered in rules; consolidating them into a short, always-read lessons file would raise consistency.

---

## Part 3 — File-by-file changes and new files

Notation: **[edit]** existing file, **[new]** new file. Paths are relative to `lv1-writer-plugin/`.

### 3.1 The tone/genre system (P0 — the core of the request)

**[new] `skills/lv1-writer/references/tone-detect.md`** — the detection contract, read during triage.

Defines how to classify a writing task into a genre and pick a tone profile. Proposed taxonomy (genre → default tone profile):

| Detected genre | Tone profile | Register | Proof apparatus | Default authorship |
|---|---|---|---|---|
| Intelligence / market / strategy report | `analytical-intelligence` | decisive, epistemic, hypothesis-driven (the axiom voice) | full: inline grades, methodology, steelman, appendix | AI-autonomous |
| Academic paper / literature review | `academic` | measured, formal, citation-dense | full, citation-style adapted | AI-autonomous |
| Pure-science / popular-science book | `popular-science` | vivid, case-led, mechanism-explaining | full→medium per tier | AI-autonomous (user may supply cases) |
| Cognitive-behavior / psychology book | `cognitive-behavior` | warm-rigorous, de-shaming, mechanism-first | medium (science cited; experience un-graded) | **hybrid** — user cases + AI science |
| IT-industry analysis / report | `analytical-intelligence` (IT-tuned) | decisive, data-led | full | AI-autonomous |
| Narrative nonfiction / long-form journalism | `narrative-nonfiction` | vivid, case-led, warm-rigorous | medium: grades in appendix, lighter inline | AI-autonomous |
| Personal experience / memoir | `personal-experience` | intimate, first-person, honest | none (lived truth, not citation) | **interview-driven** |
| Mindset / self-help | `mindset` | direct, encouraging, actionable | low (science cited where claimed) | **interview-driven / hybrid** |
| Love / relationships | `love` | warm, intimate, reflective | none–low | **interview-driven** |
| Persuasive essay / op-ed | `persuasive` | committed, argument-forward, steelman required | medium | AI-autonomous |
| Technical doc / runbook / spec | `technical` | precise, neutral, imperative | low: spec-style refs, no narrative | AI-autonomous |
| Literary fiction | `literary` | stylistic, voice-first | none — fiction rules (consistency/craft) replace truth/citation | interview-driven (user's vision) |
| Romance | `romance` | emotional, intimate, sensory | none — fiction rules | interview-driven |
| Children's / YA | `childrens` | simple, age-tuned, safe | none — fiction rules | interview-driven |

Detection signal order: (1) explicit user instruction always wins; (2) project `writing-instruction.md` if the user deliberately set it; (3) inferred from the task wording ("report"/"analysis"/"chapter of a romance novel"/"spec"…) and any handed-over sources; (4) when genuinely ambiguous, triage asks the user one question rather than guessing. The contract reuses the review checklist's Pass-1 classification block (type / core promise / intended reader / key contracts) so detection logic isn't duplicated.

Crucial rule to encode here: **the tone profile decides which honesty machinery applies.** Fiction profiles *switch off* A/B/C/D citation grading and the proof package (you cannot cite a source for an invented scene) and *switch on* fiction contracts instead — internal consistency, motivation-driven action, earned endings, consistent POV (these already exist in the review checklist's narrative examples). Non-fiction profiles keep the full grading + proof machinery, scaled by rigor tier.

**[new] `skills/lv1-writer/assets/tone-profiles/` (one file per profile)** — each is a self-contained voice spec in the same shape as the current `writing-instruction-template.md` (Spine & shape / Truth-or-fiction-contract / Detail / Tone & voice / Structure / Never-do). The current `writing-instruction-template.md` content becomes `narrative-nonfiction.md` almost verbatim. The `analytical-intelligence.md` profile should encode the axiom moves explicitly: hypothesis spine, per-section lens question, winners/losers synthesis, inline grades, mandatory steelman, methodology-first, honest-gap-in-text.

Each profile carries its own structural guidance inline (the Spine & shape / Structure sections) — there is no separate per-genre scaffold; the profile is the whole specification of how that genre reads and is shaped. Priority order, matching your focus: `popular-science`, `cognitive-behavior`, `analytical-intelligence` (IT-tuned) first; then `personal-experience`, `mindset`, `love`; then `academic`, `persuasive`, `technical`, and the fiction set (`literary`, `romance`, `childrens`).

**[edit] `skills/lv1-writer/references/triage.md`** — add a "Genre & tone" decision block to `00-triage.md`:

```
## Genre & tone
- Detected genre: <from tone-detect.md>
- Tone profile: <profile id from assets/tone-profiles/, or "project default (writing-instruction.md)">
- Rigor tier: <tiny|normal|full|high-stakes>
- Proof apparatus level: <none (fiction) | low | medium | full>
- Structure shape: <descriptive | hypothesis-driven>
- Why this profile: <one line; or the one question to ask the user if ambiguous>
```

Reference `tone-detect.md` from triage so the detection runs every task.

**[edit] `skills/lv1-writer/references/draft.md`** — change the Tone section so the draft station composes voice from, in precedence order: (1) explicit user instruction, (2) the tone profile named in `00-triage.md` (read `assets/tone-profiles/<id>.md`), (3) project `writing-instruction.md` overrides layered on top. Today it only reads `writing-instruction.md`. Add: if the profile is a fiction profile, follow fiction contracts and *do not* attach citation grades; if non-fiction, apply inline grading + the apparatus per the rigor level set in triage.

**[edit] `skills/lv1-writer/SKILL.md`** — in `## Setup`, keep `writing-instruction.md` as the *project default override* but clarify it is now optional: if absent, the per-task detected profile drives tone. In `## The pipeline`, add the genre/tone decision to the triage step description. Document the two-knob model (profile × rigor) in `## Effort tier`.

**[edit] `skills/lv1-writer-init/SKILL.md`** — make init **interactive**. Instead of silently writing the science-narrative tone, init asks the user a short, grouped set of questions (one round, via the AskUserQuestion mechanism in Cowork, or plain prompts in Claude Code):

1. **Topic** — what is the book/project about? (free text, or "decide later per task")
2. **Genre/format** — pick from the taxonomy table, or "you decide" (then AI infers per task from each request).
3. **Tone** — accept the genre's default tone profile, describe a custom tone, or "you decide".
4. **Working mode** — *auto* (let AI run autonomously wherever the genre allows) or *interactive* (the user wants to be involved / interviewed). This is the only authorship question asked; AI derives the concrete approach from genre + this choice (see §3.1b).

Init then seeds `manuscript/writing-instruction.md` from the chosen profile (still never overwritten on re-run), records topic + genre + working mode in `CLAUDE.md`, and — if the resolved approach is interview-driven or hybrid — runs the **intake interview** (§3.1b) immediately so the project starts with the user's material captured. Report what was chosen and the available profiles. If the user said "you decide" for genre, leave `writing-instruction.md` unwritten and let per-task detection drive tone.

### 3.1b Authorship approach + the interview workflow (P0)

AI resolves the concrete **authorship approach** from two inputs: the detected genre and the user's *auto / interactive* working-mode choice. Mapping: research/analytical/technical genres in *auto* → AI-autonomous; experience/case-based genres (personal, love, mindset), or any genre in *interactive* → interview-driven; cognitive-behavior → hybrid (user cases + AI-cited science). The user is never asked to name "autonomous vs interview-driven" directly — only auto vs interactive — and AI states the resolved approach so it's visible.

**[new] `skills/lv1-writer/references/interview.md`** — the contract for interview-driven and hybrid authorship. Defines:

- **Batching rule (the core of your ask).** Never drip questions one at a time. At any point where input is needed, collect *every* open question, group them by theme, and ask them as **one round**. Only after the user answers (or skips) does the next round form. This applies at three moments: (1) the **intake interview** at init/task start, (2) **mid-draft gaps** — when a section can't be written truthfully without the user's material, pause and batch the questions blocking that section, (3) a **pre-inspection confirmation** round for anything still assumed.
- **What AI may and may not do per approach.** *Interview-driven:* the spine (cases, feelings, events, opinions) must come from the user; AI may research *around* it (e.g. the cognitive science explaining a described experience) and must label which parts are the user's lived account vs AI-added context. AI never invents a personal anecdote. *Hybrid (e.g. cognitive-behavior):* user supplies the cases/experience; AI supplies and cites the science; the two are visibly distinguished. *AI-autonomous:* the existing pipeline — AI researches and drafts, asking the user only on genuinely blocking ambiguities.
- **Where answers are stored.** Intake answers and each round's responses are saved to `manuscript/intake.md` (or `runs/<id>/`), so the draft station and inspector can trace which prose rests on user-supplied material — the experience-equivalent of the source library.
- **A skip is a declared gap.** If the user can't or won't answer something load-bearing, it's recorded as a gap (mirroring research's DECLARED GAP), not invented around.

**[edit] `skills/lv1-writer/references/triage.md`** — record the resolved approach in `00-triage.md` (one line: genre + working mode → approach).

**[edit] `skills/lv1-writer/SKILL.md`** — add the approach to the pipeline: on interview-driven/hybrid tasks, the "gather material" step routes through `interview.md` (user material) *in addition to or instead of* `lv1-research` (web facts). The draft step reads `manuscript/intake.md` as a first-class source.

**[edit] `agents/lv1-inspect.md`** — for interview-driven/hybrid drafts, add a check: is every personal/experiential claim traceable to `intake.md` (not invented), and are AI-added context vs user-lived account kept visibly distinct? An invented anecdote is a REJECT (the fiction-free equivalent of a fabricated source).

### 3.2 Surface the discipline into the artifact (P0/P1)

**[edit] `skills/lv1-writer/references/draft.md`** — add an "Apparatus by rigor level" subsection. For `full`/`high-stakes` non-fiction:

1. Open with a **Methodology & enforcement** section (recency window, source spine, enforcement rules, red-line exclusions) — sourced from the triage research contract.
2. Carry **inline A/B grades** on every factual claim in the body.
3. Surface every **DECLARED GAP** from `01-research.md` in-text where the number would go, phrased as an explicit refusal-to-invent.
4. End with the **proof package appended to the document itself** (not only the manifest in `runs/`): confidence table, assumptions, source index with in-window flags, data-limits.

For `narrative-nonfiction`/medium: grades live in a closing notes/appendix, inline only on load-bearing claims; methodology compressed to a one-paragraph "About the sources" note.

**[edit] `skills/lv1-writer/SKILL.md`** — step 7 (Ship): for full/high-stakes non-fiction, the manifest content is **appended to the delivered file**, not only written to `runs/<id>/05-manifest.md`. The run manifest stays as the receipt; the reader-facing doc carries its own proof.

**[new] `skills/lv1-writer/references/structure-shapes.md`** — document the two structure modes and when to use each: **descriptive** (current; horizontal slices — good for overviews/PR/skimmable audiences) and **hypothesis-driven** (axiom-style — state a central mechanism early, use it as a per-section lens, close with winners/losers or equivalent synthesis — good for strategy/analysis/contested topics). `outline.md` selects the shape from the triage decision.

**[edit] `skills/lv1-writer/references/outline.md`** — read the structure shape from `00-triage.md`; when hypothesis-driven, require the outline to state the central thesis/mechanism and show how each section applies it, plus a synthesis section. When the conclusion is contested, require a steelman section in the outline.

**[edit] `skills/lv1-writer/references/draft.md`** — add a "Format follows content" rule (generalizing the tone template's existing "match list form to the cognitive job" line into all formats): before writing each section, AI picks the presentation form from the content — **table** for multiple data points/stats/numbers that invite comparison; **bullets** for a list of discrete ideas/key points; **dashes/arrows** (`A → B → C`) for a loop, sequence, pipeline, or cause-effect chain; **prose** for content the reader should experience. A deliberate decision point per section, gated by genre (narrative/fiction → prose-default, sparse markup; analytical/technical → tables and diagrams used freely). The inspector (§3.4) flags both under-formatting (a wall of prose hiding a comparison that wants a table) and over-formatting (bullets where the reader needed a narrative).

### 3.3 Research — conflict hunting + steelman (P1/P2)

**[edit] `agents/lv1-research.md`** — add two steps:

- **Conflict/pitfall hunt.** Beyond "if sources conflict, keep both," actively search for the most-cited *wrong* or viral number on the topic and reconcile it (the FPT-26,000 pattern). Add a **Resolved conflicts** section to `01-research.md` listing each conflict, both figures, and the reconciliation.
- **Steelman material.** When the task will reach a contested conclusion, gather the strongest *opposing* evidence during research (not at draft time), so the steelman is built from real sourced data, not invented for symmetry.

Add both to the `01-research.md` template.

### 3.4 Inspection — enforce the new contracts (P1)

**[edit] `agents/lv1-inspect.md`** — extend the five pipeline-mode checks so the new discipline is actually enforced, gated by the genre recorded in `00-triage.md`:

- **Tone conformance** — does the draft match the tone profile chosen in triage? (A romance read forensically, or a report written breezily, is a FIX-IT.)
- **Apparatus presence** — for full/high-stakes non-fiction: are inline grades present, is the methodology section there, did declared gaps surface in-text, is the proof package appended? Missing apparatus → FIX-IT.
- **Steelman presence** — a contested conclusion with no steelman section → FIX-IT.
- **Conflict handling** — were the research's resolved conflicts reflected, or was a known-disputed number reported flat? → FIX-IT.
- For **fiction** genres, *suppress* the citation/grade checks and instead run the fiction contracts (consistency, motivation, earned ending, POV) — reuse the review checklist's narrative examples so a novel isn't failed for lacking sources.

### 3.5 Constitution + lessons (P2)

**[edit] `skills/lv1-writer/SKILL.md`** (Core rules / The bar) — make explicit, as axiom's working-lessons do, that a confidence label is a reason to commit *harder*, not a hedge; and that a numeric/analytical claim is not a claim until its working is shown. (Rule 4 gestures at this; sharpen it.)

**[new] `skills/lv1-writer/references/working-lessons.md`** (or fold into CLAUDE.md template) — a short, always-read lessons file mirroring axiom's: neutral ≠ timid; label = commit harder; show the arithmetic; user's lived experience is a signal to investigate, not a conclusion to confirm; steelman ≠ false balance. Reference it from the draft and inspect contracts.

**[edit] `skills/lv1-writer/assets/claude-md-template.md`** — add one line noting the project now supports per-task genre/tone detection and where profiles live, so the project stays self-documenting.

### 3.6 Suggested sequencing

1. **Tone/authorship system + interactive init** (3.1, 3.1b) — the explicit ask; everything else keys off the genre, tone, and working mode recorded at init/triage. The profile files (each carrying its own structural guidance) are built here.
2. **Artifact-surfacing** (3.2) — biggest visible quality jump for the non-fiction genres; mostly enforcement of existing rules.
3. **Research conflict-hunting + steelman material** (3.3).
4. **Inspector enforcement** (3.4) — last, because it polices 3.1–3.3; without it the new rules are advisory.
5. **Lessons/constitution polish** (3.5) — ongoing.

### 3.7 What NOT to change

Keep the claim-ledger forcing function, the REDO-RESEARCH coverage gate, the isolated research/inspect subagents, the init/library-sync machinery, and the smoother default prose. These are lv1-writer's edge over axiom; the plan adds axiom's artifact discipline *on top of* them, gated by genre, rather than replacing them.

---

## One-paragraph summary

The gap between the two reports is not writing skill — it is that axiom forces its rigor onto the delivered page (inline grades, methodology-and-enforcement section, hypothesis spine, mandatory steelman, conflict-debunking, a shipped proof package) while lv1-writer leaves that same rigor in its process files and ships a clean summary. lv1-writer is actually ahead on process machinery (claim ledger, coverage-gate, isolated agents). So the plan keeps that machinery and adds: (1) a **three-knob system** — genre/tone profile × rigor tier × authorship mode — set through an **interactive init** that asks topic/genre/tone (or "you decide") and proposes an authorship mode from the genre; (2) an **interview-driven authorship mode** for experience/case-based books (personal, love, mindset, cognitive-behavior), where the user's material is the spine and AI elicits it in **batched question rounds** — one grouped round at intake and one more whenever a gap surfaces mid-draft — never inventing a personal anecdote; and (3) the axiom-grade proof apparatus shipped *inside* the document, but only at the rigor level the detected genre warrants (fiction switches it off and turns on craft contracts instead). AI resolves the concrete authorship approach from the genre plus the user's single auto/interactive choice — no separate skeleton scaffolds; each tone profile carries its own structural guidance. The inspector is upgraded last to enforce all of it, including that personal claims trace to the user's intake, not to invention.
