VERDICT: FIX-IT

DOCUMENT TYPE: Claude plugin (instruction-document system — constitution + 3 skills + 2 subagents + templates + manifest)
CORE PROMISE: A slim, non-drifting assistant+advisor for work where both skills and both subagents share one constitution and ship only work that is "impressive AND true," every fact graded and sourced.

Layout verdict (the structural ask): PASS. `.claude-plugin/plugin.json` is valid and minimal; skills live at `skills/<name>/SKILL.md` with proper frontmatter; subagents are `agents/*.md` with valid `name`/`description`/`tools`; there is no `commands/` directory, as required. `core/` is a non-standard but legal data directory. The structure is sound — every Major finding below is about runtime behavior and drift, not layout.

---

FINDINGS (ranked, worst first):

[MAJOR — borderline Critical at runtime] Location: agents/lv1-research.md line 17; agents/lv1-inspect.md lines 21 & 137
Issue: The subagents are told to read bare relative paths — `core/constitution.md` and `skills/lv1-advisor/assets/review-checklist-template.md`. A delegated subagent runs from the user's *project* working directory, but the plugin's `core/` and `skills/` live in the plugin install directory, and `lv1-compass-init` never copies them into the project (it creates only `sources/`, `runs/`, `CLAUDE.md`). So at runtime these paths very likely do not resolve, which means the subagents silently run *without* the constitution — the grades, the bar, and the failure-mode rules they're supposed to enforce never load. This is the single biggest threat to the stated promise that the discipline is shared and enforced. The README's "Honest limits" flags the *skills'* `../../core/` path as unverified but never mentions the subagents' bare paths, which are the more fragile of the two.
Type: Quality gap (functional correctness)
Fix: Use `${CLAUDE_PLUGIN_ROOT}/core/constitution.md` (and `${CLAUDE_PLUGIN_ROOT}/skills/lv1-advisor/assets/review-checklist-template.md`) in both agent files, or have each orchestrator pass the absolute plugin path when it delegates. Then add this to README "Honest limits" and confirm with a real install test.

[MAJOR] Location: skills/lv1-assistant/references/draft.md "Office-format handoff" (lines 51–65) ↔ agents/lv1-inspect.md "Pipeline mode" inputs (lines 44–49)
Issue: For docx/pptx/xlsx, draft.md produces the real file via the office skill and tells the inspector to read the markdown copy `03-draft.md`. The inspector's tools are only Read/Grep/Glob, so it judges the structured markdown — never the actual shipped .docx/.pptx/.xlsx. Formatting, slide structure, formula, and rendering errors in the file the user actually receives pass uninspected. That contradicts the constitution's rule 8 / R5: "judge the real artifact, never a summary of it." The coverage gate is blind exactly where office formats fail.
Type: Quality gap
Fix: State explicitly that for office formats the inspector verifies the structured copy AND the office skill's own validation/round-trip check runs on the produced file (e.g., re-extract text from the .pptx/.xlsx for the inspector), so the shipped binary is actually checked.

[MAJOR] Location: skills/lv1-advisor/references/review-mode.md (steps 1–4, lines 9–24) vs agents/lv1-inspect.md (line 21–22)
Issue: lv1-inspect.md asserts "This file is the single source of truth for the inspection contract. The parent skill … does not keep a second inline copy." But review-mode.md restates the inspector's internal procedure (read template → cold read → classify → review → write feedback) as numbered steps. That is exactly the second inline copy the agent file says doesn't exist — two descriptions that will drift when one changes. Violates key contract 1 (single-sourced discipline).
Type: Quality gap
Fix: In review-mode.md, replace steps 1–4 with a one-line delegation ("hand the artifact to lv1-inspect in standalone review mode; see agents/lv1-inspect.md for the contract") and keep only the parent-side concerns (what to do with the verdict, independence of fixes).

[MAJOR] Location: core/constitution.md "Effort tiers" (lines 76–86) vs skills/lv1-assistant/SKILL.md pipeline (steps 1–7)
Issue: The constitution states "the tier must change the spend … A tier that exists only as a label is decoration — it must actually gate which stations run." But only `tiny` changes the flow (skip research/outline). `normal`, `full`, and `high-stakes` run the identical station set; their only stated differences are apparatus weight and "extra scrutiny at research and at the independent check" (constitution line 83), which is never operationalized — nothing says what `high-stakes` does that `full` does not. By the constitution's own rule, three of the four tiers are currently decoration.
Type: Quality gap
Fix: Make the gating concrete and verifiable, e.g. high-stakes = mandatory second research pass (or raised min-distinct-sources), inspector re-reads even on a PASS, and the redo caps differ; full = research contract + appended proof package; normal = single research pass, grades in appendix. Tie each tier to a station/threshold the inspector can check.

[MAJOR] Location: agents/lv1-research.md grade table (line 77, B row) vs core/constitution.md grade table (line 60, B row)
Issue: The B-grade definition diverges. Constitution: "B — Reasoned … Derived with sound logic from verified material." Research agent: "B — Reasoned … Sound logic from verified material, **or a reputable secondary citing a primary**." The "reputable secondary" clause exists only in the subagent, so a secondary-sourced claim earns B in research but would not under the constitution's definition the inspector enforces. This is real drift inside the very mechanism (grades) the plugin promises is single-sourced — and it can cause research↔inspect disagreement on what ships.
Type: Factual error (internal inconsistency)
Fix: Pick one definition and single-source it. Either add the secondary-source clause to the constitution, or remove it from lv1-research.md and have the agent reference the constitution's table rather than restating it.

[MINOR] Location: skills/lv1-assistant/SKILL.md step 2 (line 60), "On tiny … skip to draft after triage"
Issue: Two gaps. (a) The fast lane's station set after triage is undefined — it says "skip to draft" but never states whether self-review, the inspect subagent, and ship still run. (b) Skipping research removes the only station that sources facts, yet constitution rule 5 ("no source, no claim") still binds; a tiny task that makes any factual claim then has no place to ground it.
Type: Quality gap
Fix: Spell out the fast-lane stations (e.g. triage → draft → self-review → ship, inspect optional), and add: "if a tiny task carries a factual claim, escalate out of the fast lane."

[MINOR] Location: skills/lv1-advisor/references/challenge-mode.md (line 21) & decide-mode.md (line 47) vs SKILL.md "Receipts" (lines 59–63)
Issue: Challenge/decide modes may delegate to lv1-research, which writes to `runs/<id>/01-research.md`; but the advisor only creates a `runs/<id>/` folder for full/high-stakes. For a *quick* challenge/decision that still needs research, the research subagent's output location is undefined.
Type: Quality gap
Fix: State that delegating to lv1-research always creates a run folder (even in quick mode), or that research returns inline and skips the disk write when no run id exists.

[MINOR] Location: README.md "Honest limits" (line 133)
Issue: The illustration quotes `../../core/constitution.md` from the README's own position. The README sits at the plugin root, where `../../` points *above* the plugin — a misleading path to show at root level (it's really the skill-relative path).
Type: Quality gap
Fix: Clarify it's the *skill-relative* reference, e.g. "the reference from each skill (`../../core/…`, resolving to `<plugin-root>/core/`)."

---

CONTRACTS CHECKED (Pass 3):
- Layout / no commands / valid manifest / skill+agent frontmatter — PASS.
- Pipeline diagram matches numbered steps — PASS.
- Terminology (grades, register names, verdict sets per mode) — consistent — PASS, except the B-grade drift (F7).
- Single-sourced discipline — FAIL: review-mode duplicates the inspection contract (F3); B-grade definition diverges (F7).
- Subagents can read what they're told to read — FAIL (F1).
- Inspector judges the real artifact — FAIL for office formats (F2).
- Tiers gate stations — FAIL (F4).
- Fast-lane / advisor-research output locations defined — partial (F5, F6).

NOTES FOR RE-REVIEW (FIX-IT):
- F1 is the load-bearing one: re-review only after an actual install test confirms both subagents load `core/constitution.md` at runtime. If they cannot, F1 escalates to Critical (the shared-discipline promise fails silently).
- After fixing F3 and F7, grep the whole tree for any other place a grade table, the bar, or a station contract is restated, to confirm the "single-sourced" claim now holds.
- No fabricated sources, unsafe content, or invalid logic-as-proof was found — hence FIX-IT, not REJECT.
