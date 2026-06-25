# lv1-writer build sequence — split into 5 runnable parts

*Companion to `lv1-writer-improvement-plan.md`. Each part is a self-contained run: open this file + the plan, do only that part's files, verify, stop. A fresh session can pick up the next part with no memory of the previous one. Section refs (§) point into the plan for the detail.*

All paths are relative to `lv1-writer-plugin/`. **[new]** = create, **[edit]** = modify. After each part, run its **Verify** line before stopping.

**Run order matters** for dependencies, but partial states are safe: until a profile file exists, `draft.md` falls back to `writing-instruction.md` or plain prose, so the plugin keeps working between parts. The system is only *fully* wired after Part 5.

---

## Part 1 — Routing & interactive init (the layer everything keys off)

Establishes the three-knob model (genre/tone × rigor × working mode) and the interactive init. No profiles yet — this is the plumbing that selects them.

- **[new] `skills/lv1-writer/references/tone-detect.md`** — genre→profile taxonomy + detection signal order + the "tone profile decides which honesty machinery applies" rule. (§3.1)
- **[new] `skills/lv1-writer/references/structure-shapes.md`** — descriptive vs hypothesis-driven shapes and when each applies. (§3.2)
- **[edit] `skills/lv1-writer/references/triage.md`** — add the "Genre & tone" + working-mode + resolved-approach + structure-shape block to `00-triage.md`. (§3.1, §3.1b)
- **[edit] `skills/lv1-writer-init/SKILL.md`** — make init interactive: ask topic / genre / tone / (auto|interactive), seed `writing-instruction.md` from the chosen profile, record choices in `CLAUDE.md`. (§3.1)
- **[edit] `skills/lv1-writer/SKILL.md`** — *Setup*, *The pipeline* (add the genre/tone/working-mode decision to triage step), and *Effort tier* (document the three-knob model). **Touch only these sections** — authorship routing, ship-apparatus, and core-rules edits to this file come in Part 4.

**Verify:** re-read the four touched files; confirm triage now records genre + tone + working mode + approach, and init asks the four questions. The taxonomy in `tone-detect.md` lists every profile name Part 2–3 will create.

---

## Part 2 — Priority tone profiles (science / cognitive / IT)

The token-heavy content, batch 1. Each profile follows the shape of the current `writing-instruction-template.md` (Spine & shape / Truth-or-fiction contract / Detail / Tone & voice / Structure / Process / Never-do) and carries its own structural guidance inline. (§3.1)

- **[new] `skills/lv1-writer/assets/tone-profiles/narrative-nonfiction.md`** — port the current `writing-instruction-template.md` content almost verbatim (this becomes the canonical home of the existing default voice).
- **[new] `skills/lv1-writer/assets/tone-profiles/analytical-intelligence.md`** — encode the axiom moves: hypothesis spine, per-section lens question, winners/losers synthesis, inline grades, mandatory steelman, methodology-first, honest-gap-in-text. Note the IT-tuned variant here too.
- **[new] `skills/lv1-writer/assets/tone-profiles/popular-science.md`** — vivid, case-led, mechanism-explaining.
- **[new] `skills/lv1-writer/assets/tone-profiles/cognitive-behavior.md`** — warm-rigorous, de-shaming, mechanism-first; mark it the **hybrid** authorship default (user cases + AI-cited science, kept visibly distinct).

**Verify:** four files exist under `assets/tone-profiles/`; each names its genre, register, proof-apparatus level, and default authorship, matching the row in `tone-detect.md`.

---

## Part 3 — Remaining tone profiles (personal / persuasive / technical / fiction)

Batch 2. The fiction profiles are shorter (no citation machinery — craft contracts instead). (§3.1)

- **[new] `assets/tone-profiles/personal-experience.md`** — intimate, first-person; interview-driven default.
- **[new] `assets/tone-profiles/mindset.md`** — direct, actionable; interview-driven / hybrid.
- **[new] `assets/tone-profiles/love.md`** — warm, reflective; interview-driven.
- **[new] `assets/tone-profiles/academic.md`** — measured, formal, citation-dense.
- **[new] `assets/tone-profiles/persuasive.md`** — committed, argument-forward, steelman required.
- **[new] `assets/tone-profiles/technical.md`** — precise, neutral, imperative; spec-style refs.
- **[new] `assets/tone-profiles/literary.md`**, **`romance.md`**, **`childrens.md`** — fiction profiles: truth/citation off, craft contracts (consistency, motivation, earned ending, POV, age-safety) on.

**Verify:** every profile named in `tone-detect.md` now has a file; fiction profiles explicitly switch off A/B/C/D grading and switch on craft contracts.

---

## Part 4 — Authorship/interview workflow + craft surfacing

Wires the interview mode and pushes the discipline onto the page. (§3.1b, §3.2, §3.3, §3.5)

- **[new] `skills/lv1-writer/references/interview.md`** — batching rule (one grouped round, recurring at intake / mid-draft gaps / pre-inspection), what AI may/may-not do per approach, `manuscript/intake.md` storage, skip = declared gap. (§3.1b)
- **[new] `skills/lv1-writer/references/working-lessons.md`** — neutral≠timid; label = commit harder; show the arithmetic; lived experience is a signal not a conclusion; steelman ≠ false balance. (§3.5)
- **[edit] `skills/lv1-writer/references/draft.md`** — tone composition (profile → writing-instruction overrides → explicit user instruction); apparatus-by-rigor (methodology section, inline grades, in-text gaps, appended proof package); **format-follows-content** rule (table/bullets/arrows/prose by content type, gated by genre). (§3.2)
- **[edit] `skills/lv1-writer/references/outline.md`** — read structure shape; require thesis + per-section lens + synthesis when hypothesis-driven; require a steelman section when the conclusion is contested. (§3.2)
- **[edit] `agents/lv1-research.md`** — conflict/pitfall hunt (+ Resolved-conflicts section) and steelman-material gathering. (§3.3)
- **[edit] `skills/lv1-writer/SKILL.md`** — *authorship routing* in the pipeline (interview-driven/hybrid route through `interview.md`; draft reads `intake.md`), *ship step* appends the proof package to the delivered file, *core rules* sharpen the label-as-commitment / show-the-working point. **Different sections than Part 1.**
- **[edit] `skills/lv1-writer/assets/claude-md-template.md`** — one line noting genre/tone/working-mode support and where profiles live.

**Verify:** draft.md now contains the format-follows-content rule and the apparatus-by-rigor ladder; interview.md's batching rule is unambiguous; research contract has a Resolved-conflicts output.

---

## Part 5 — Inspector enforcement + final consistency pass

Last, because it polices everything above. Without it the new rules are advisory. (§3.4)

- **[edit] `agents/lv1-inspect.md`** — add, gated by the genre recorded in `00-triage.md`: tone conformance, apparatus presence (grades/methodology/in-text gaps/appended proof), steelman presence on contested conclusions, conflict handling, format-follows-content (flag under- and over-formatting), and — for interview-driven/hybrid — every personal claim traceable to `intake.md` (invented anecdote = REJECT). For fiction, suppress citation checks and run craft contracts instead.
- **Final consistency pass (no file):** grep the plugin for references to files that should now exist (every profile in `tone-detect.md`, `interview.md`, `structure-shapes.md`, `working-lessons.md`); confirm SKILL.md's two edit rounds didn't collide; confirm `writing-instruction-template.md` and `narrative-nonfiction.md` agree. Consider a dry-run: init a throwaway project, pick one autonomous genre and one interview-driven genre, and check the routing.

**Verify:** the inspector references each new contract; the dry-run routes an analytical task to autonomous + full apparatus and a personal task to interview-driven + no grades.

---

## At-a-glance

| Part | Theme | New | Edit |
|---|---|---|---|
| 1 | Routing & init | tone-detect, structure-shapes | triage, init/SKILL, SKILL (setup/pipeline/effort) |
| 2 | Priority profiles | narrative-nonfiction, analytical-intelligence, popular-science, cognitive-behavior | — |
| 3 | Remaining profiles | personal-experience, mindset, love, academic, persuasive, technical, literary, romance, childrens | — |
| 4 | Interview + craft | interview, working-lessons | draft, outline, lv1-research, SKILL (authorship/ship/rules), claude-md-template |
| 5 | Inspector + consistency | — | lv1-inspect (+ final pass) |

Each part is one safe run. If a part still feels too large mid-run, stop at a file boundary and note where you stopped — the next run resumes from this file.
