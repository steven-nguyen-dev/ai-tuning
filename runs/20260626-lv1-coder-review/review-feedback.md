# Review: lv1-coder — Analysis & Improvement Plan

**Run:** 20260626-lv1-coder-review  
**Mode:** review (lv1-advisor)  
**Tier:** full  
**Subject:** lv1-coder-installer + all nine asset templates  
**Benchmark:** lv1-compass plugin architecture (as observed in lv1-compass-plugin/)  
**Verdict:** FIX-IT — the pipeline logic is sound and the installer works, but seven structural and instruction gaps weaken the discipline relative to what lv1-compass achieved.

---

## What was read

- `lv1-coder-installer/SKILL.md` — the installer  
- `lv1-coder-installer/assets/lv1-coder-rules-template.md` — operating contract  
- `lv1-coder-installer/assets/lv1-coder-skill-template.md` — pipeline runner  
- `lv1-coder-installer/assets/triage-skill-template.md`  
- `lv1-coder-installer/assets/plan-skill-template.md`  
- `lv1-coder-installer/assets/build-skill-template.md`  
- `lv1-coder-installer/assets/researcher-agent-template.md`  
- `lv1-coder-installer/assets/inspector-agent-template.md`  
- `lv1-coder-installer/assets/library-template.md`  
- `lv1-coder-installer/assets/claude-md-template.md`  
- `lv1-compass-plugin/core/constitution.md`  
- `lv1-compass-plugin/core/working-lessons.md`  
- `lv1-compass-plugin/agents/lv1-research.md`  
- `lv1-compass-plugin/skills/lv1-assistant/SKILL.md`

---

## What works well [A]

The core concept is right and the installer works. The line `triage → research → plan → build → inspect → ship` is correct. The inspector runs in a separate context and reads the real code — that independence is mechanical, not a promise, which is exactly how lv1-compass does it. The operating contract (`lv1-coder.md`) is imported into every project context via CLAUDE.md, which is the right mechanism. Confidence labels (A/B/C/D), the fix-it loop capped at 3 rounds, and "never fake a step" are all present and correct.

---

## Findings — ranked by impact

### F1 · Researcher lacks the claim-budget forcing function [A] — HIGH IMPACT

**Location:** `assets/researcher-agent-template.md`, entire approach  
**What it does:** lv1-coder's researcher has open sections: Key findings, Approaches considered, Pitfalls, Conflicts/gaps. The agent decides when "I have enough" — which is a feeling, not a stop condition.  
**What lv1-compass does:** The researcher enumerates every claim the task needs *first* (the "claim budget"), then sources each row one by one. A row is closed only when it's A/B with a fetched source or explicitly marked DECLARED GAP. "I couldn't find it" is a declared gap, written down — never a quiet omission.  
**Why it matters:** Without a pre-enumerated budget, the researcher can stop after finding five comfortable facts, leaving the hard claims — the ones most likely to be wrong — unsourced. The claim-budget is the single most important structural forcing function in lv1-compass.  
**Fix:** Restructure the researcher template to: (1) enumerate all claims the task needs *before* searching, (2) source each row to a fetched-and-read source, (3) close only when every row is A/B or DECLARED GAP. Adopt the ledger table format from lv1-compass.

---

### F2 · Missing REDO-RESEARCH verdict path [A] — HIGH IMPACT

**Location:** `assets/lv1-coder-skill-template.md` (line 6: "FIX-IT / REJECT / PASS"), `assets/inspector-agent-template.md`  
**Problem:** The inspector can only return FIX-IT (build problem) or REJECT or PASS. If research was thin — the builder was forced to guess because the researcher didn't surface the right facts — the inspector is stuck returning FIX-IT on the build, which sends work back to build when the root problem is the research.  
**What lv1-compass does:** A third verdict, REDO-RESEARCH, sends the work back to research with the named gaps. The inspector can say "the build is honest but the research base is too thin to support these claims" — a different diagnosis from "the implementation is wrong."  
**Fix:** Add REDO-RESEARCH to the inspector template and to the runner skill's loop. The loop becomes: FIX-IT → back to build, re-inspect from scratch; REDO-RESEARCH → back to research with named gaps, then re-build, re-inspect; REJECT → stop.

---

### F3 · Flat `assets/` directory — no separation by type [A] — MEDIUM IMPACT

**Location:** `lv1-coder-installer/assets/` (9 files, all at the same level)  
**Problem:** All nine templates — core rules, skill runners, station skills, subagents — are dumped in one flat directory. Someone reading or editing the installer cannot immediately see which files govern which layer. This also makes it harder to update a single layer without touching others.  
**What lv1-compass does:** `core/` (constitution, working-lessons, readability), `agents/` (lv1-research, lv1-inspect), `skills/lv1-assistant/references/` (triage, outline, draft, source-intake), `skills/lv1-advisor/assets/` and `references/`. Each layer has a home.  
**Fix:** Reorganize `assets/` to mirror lv1-compass:
```
assets/
├── core/
│   ├── lv1-coder-rules-template.md
│   └── working-lessons-template.md    ← new (see F4)
├── skills/
│   ├── lv1-coder/SKILL.md template
│   ├── triage/
│   │   ├── SKILL.md template
│   │   └── references/triage-guide.md  ← new depth (see F5)
│   ├── plan/
│   │   └── SKILL.md template
│   └── build/
│       ├── SKILL.md template
│       └── references/code-quality.md  ← new depth (see F5)
└── agents/
    ├── researcher-template.md
    └── inspector-template.md
```
Update the SKILL.md installer to reference the new paths.

---

### F4 · No `working-lessons.md` equivalent [A] — MEDIUM IMPACT

**Location:** missing from lv1-coder entirely  
**Problem:** lv1-compass has `core/working-lessons.md`, a growing document every station reads at the start of each run. It captures hard-won discipline: "neutral ≠ timid," "a confidence label is a reason to commit harder," "the user's stated position is a signal to investigate, not a mirror." These are the failure modes that survive rule-following in letter but not spirit. lv1-coder has no equivalent — there is no accumulation mechanism for coding-specific lessons.  
**Why it matters:** Without this file, every discovered failure mode has to be re-learned each time or retrofitted into the rules file, which bloats it. The lessons file keeps the rules clean and the discipline growing.  
**Fix:** Create `assets/core/working-lessons-template.md` with coding-specific initial lessons (e.g., "a passing test is not proof if the test is trivially satisfied," "don't change scope quietly — announce and re-plan," "D never ships — not even with a note"). Add a step to the installer that writes this to `.claude/working-lessons.md` and imports it in CLAUDE.md alongside the rules. All station skills should read it at the start of a run.

---

### F5 · Station skills are too thin — no `references/` depth [A] — MEDIUM IMPACT

**Location:** `triage` (25 lines), `plan` (32 lines), `build` (31 lines)  
**Problem:** Each station skill gives the agent a minimal format to fill in, but very little guidance on *how to decide*. Triage doesn't explain what makes a task high-stakes vs normal. Plan doesn't explain how to size the team or identify genuinely parallel work. Build doesn't describe what a good self-review catches.  
**What lv1-compass does:** Each skill has a `references/` subdirectory with detailed guidance files (triage.md, outline.md, draft.md, source-intake.md, register.md). The SKILL.md is thin (orchestrator logic); the depth lives in references, loaded only when needed.  
**Fix:** Add at minimum two reference files:
- `triage/references/triage-guide.md` — what determines lane (fast vs full), what makes a task high-stakes, what "done" looks like for coding tasks (passing tests, working command, etc.)
- `build/references/code-quality.md` — a coding equivalent of `readability.md`: what the self-review checks (edge cases, error handling, test coverage, scope drift, claims dressed beyond evidence)

The inspector should be pointed at the `code-quality.md` reference so it applies the same checklist.

---

### F6 · Numbering gap: `04` is missing from run folder [A] — LOW IMPACT (but confusing)

**Location:** `assets/lv1-coder-rules-template.md`, Receipts section  
**Problem:** The receipts list is: `00-triage.md`, `01-research.md`, `02-plan.md`, `03-deliverable…`, `05-inspection.md`. The `04` slot is empty. Nothing in the instructions names what `04` would be, yet `05-inspection.md` implies something came before it. This creates ambiguity and a gap the inspector has to silently skip over.  
**Fix:** Either:  
  (a) Renumber inspection to `04-inspection.md` (cleaner — it's the fourth major artifact), or  
  (b) Explicitly name `04-build-summary.md` as the build step's receipt (the self-review output), making the build's output a first-class artifact the inspector can read alongside the code.  
Option (b) is better aligned with lv1-compass's pattern where each station writes its own receipt.

---

### F7 · No source-intake procedure for user-provided documents [A] — LOW IMPACT

**Location:** missing  
**Problem:** lv1-compass has `references/source-intake.md` — a specific procedure for when the user hands over a file, PDF, or URL mid-pipeline. The researcher registers it in `sources/library.md`, grades the facts it contains, and makes them available to the pipeline. lv1-coder has no equivalent. The researcher template mentions "if the user asks you to add a source" but gives no procedure.  
**Fix:** Add a brief `source-intake` procedure to the researcher agent template (or a standalone reference file the researcher reads) that mirrors lv1-compass's intake behavior: distill, grade, register in library.md.

---

## Steelman of the current design

The flat-assets, thin-station-skills approach is not just carelessness — it has a real justification: lv1-coder installs into *another* project as local skills and agents. The simpler and shorter each installed file is, the less cognitive overhead the developer faces when they open `.claude/skills/triage/SKILL.md` to understand what's running. The lv1-compass pattern works when the skills are in a plugin and referenced read-only; lv1-coder's skills become *the developer's own files*, and there's an argument for keeping them legible over deep.

This steelman holds for F3 (flat assets) in the installer itself — the user never reads the installer's assets directory, so there's no legibility argument against hierarchical organization there. But it partially defends F5 (thin skills): the installed station skills are meant to be readable by the developer who owns the project.

The resolution: the installer's `assets/` should be hierarchical (F3), but the installed station skills can stay terse — as long as the critical gaps (F1, F2, F4) are fixed. The depth-via-references pattern (F5) should be applied only to the most decision-heavy stations (triage and build), not all three.

---

## Revised context (addendum from user)

Three constraints that reshape the improvement plan:

1. **AI is already strong at coding.** Research is not a default step — it's a rare trigger. It fires only when (a) the AI cannot resolve the issue from the codebase, or (b) a specific external fact is missing that the AI cannot know (e.g., latest stable version of a dependency, a third-party API change).
2. **Normal coding tasks need none of the heavy pipeline.** Most tasks should be: triage → build → inspect. The full research + plan path is the exception, not the default.
3. **Token efficiency is a first-class constraint.** Every station that runs costs tokens. Triage's job is to eliminate unnecessary stations, not just classify difficulty. When good system design documents already exist in the project, they are the research — do not re-research what's already documented.

This changes the priority order. The pipeline's architecture problem is not that research is weak (F1) — it's that research is treated as a default step when it should be a conditional branch. That is the highest-impact fix.

---

## Improvement plan — ordered by impact

### P0 — Redesign the pipeline default: research and plan are conditional, not default [NEW]

**Current:** `triage → research → plan → build → inspect → ship`  
**Target:** `triage → [research?] → [plan?] → build → inspect → ship`

The default lane for any coding task is **triage → build → inspect**. Research and plan are opt-in, decided by triage.

**Triage decision logic (make this explicit in the triage skill):**

*Skip research if:*
- The task can be solved from the existing codebase, comments, and design docs alone
- It's a standard coding operation (refactor, add a feature per existing patterns, fix a bug visible in the code)
- Good system design documents exist in the project that cover the relevant area
- The AI can reason to a confident plan without external facts

*Trigger research if:*
- A specific external fact is needed that the AI cannot know reliably (latest stable version of a dep, a third-party API change, a framework-specific behavior)
- The AI has tried to solve the issue and cannot determine the right approach from the code alone
- A high-stakes or irreversible change depends on a fact that must be verified (e.g., database migration behavior under load)

*Skip plan if:*
- Single-file or small change with clear scope
- The deliverable and approach are unambiguous from triage

*Write a plan if:*
- Multi-file or multi-step change
- Task involves coordinating work across multiple components
- High-stakes tier

**In the triage output (`00-triage.md`), add three explicit fields:**
```
- Research needed? yes | no — reason: <one line>
- Plan needed? yes | no — reason: <one line>
- Stations that will run: triage → [research →] [plan →] build → inspect
```

Triage must justify skipping or including each station. "No" should be the default answer for both. A justification of "I always run research" is not valid.

**Token efficiency rules for every station:**
- Triage: read the task + any existing design docs, decide in ≤3 tool calls. Do not read files unrelated to the task.
- Research (when triggered): read `sources/library.md` first; if a relevant `sources/<topic>.md` covers it, stop there — do not re-fetch. Web search only for facts not in the knowledge base.
- Build: read only the files the plan identifies. Do not sweep the whole repo.
- Inspector: read only the deliverable and the plan/triage receipts. Do not re-read unrelated code.

---

### P1 — Researcher: add claim-budget forcing function
- Rewrite `researcher-agent-template.md` section 1 to require claim enumeration *before* searching
- Adopt the ledger table: `# | Claim | Grade | Source | Corroborated?`
- Add DECLARED GAP as a valid row resolution (never silent omission)
- Estimated scope: rewrite ~40 lines of the researcher template

### P2 — Runner + inspector: add REDO-RESEARCH verdict
- In `lv1-coder-skill-template.md`, add a third verdict branch after FIX-IT: REDO-RESEARCH → back to research with named gaps, cap at 2 redos
- In `inspector-agent-template.md`, add the REDO-RESEARCH verdict option and describe when it applies (honest build, thin research)
- Estimated scope: ~10 lines added to each template

### P3 — Installer: reorganize `assets/` into `core/`, `skills/`, `agents/`
- Move files into the three subdirectories (no content change, just relocation)
- Update `SKILL.md` installer copy-paths to match new structure
- Estimated scope: 9 file moves + 9 path edits in SKILL.md

### P4 — Add `working-lessons-template.md` for coding
- Write `assets/core/working-lessons-template.md` with 4–6 initial coding-specific lessons
- Add installer step: copy to `.claude/working-lessons.md`, add `@.claude/working-lessons.md` to CLAUDE.md
- Add "read working-lessons.md first" instruction to each station skill (one line each)
- Estimated scope: new 30-line file + 4 one-line additions

### P5 — Fix numbering gap (renumber inspection to `04-inspection.md`)
- Add `04-build-summary.md` as the build station's receipt, OR renumber inspection to `04`
- Update all references in rules template, runner, inspector template
- Estimated scope: 3 files, ~5 edits each

### P6 — Add code-quality reference for build + inspector
- Write `assets/skills/build/references/code-quality.md` (~25 lines): edge cases, error handling, test adequacy, scope drift, claims vs evidence
- Point build SKILL.md and inspector template at this file
- Estimated scope: new 25-line file + 2 one-line additions

### P7 — Add source-intake procedure to researcher
- Add a short source-intake section to `researcher-agent-template.md` (or a referenced file)
- Mirror lv1-compass: distill → write `sources/<topic>.md` → register in library.md
- Estimated scope: ~15 lines added to researcher template (this is already partially there; P7 just makes it explicit and procedural)

---

## Confidence summary

All findings are [A] — grounded in files read in this session, directly compared. No claim is from memory. The steelman (§ above) is judgment [interpretation], not a factual claim.

**Overall verdict: FIX-IT (revised priority).** P0 is now the highest-leverage change — making research and plan conditional branches rather than default steps, with explicit triage decision logic and token-efficiency rules. P1 (claim-budget) applies only when research *is* triggered. P2 (REDO-RESEARCH) remains valid but becomes lower priority in a world where research rarely runs. P3–P7 are architectural parity and polish.

**Revised execution order:** P0 → P3 (restructure assets to make P0 changes easier) → P4 (working-lessons) → P6 (code-quality reference) → P1 (researcher depth) → P2 (REDO-RESEARCH) → P5, P7 (cleanup).
