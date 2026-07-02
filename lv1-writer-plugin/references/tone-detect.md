# Tone & genre detection — run during triage

Pick the voice before writing a word. A market report and a romance chapter are not
written in the same register, and the same forensic apparatus that makes an
intelligence memo trustworthy would wreck a memoir. This contract decides, for each
task, three things: the **genre**, the **tone profile** that genre maps to, and the
**authorship approach** (resolved with the user's auto/interactive choice — see
`interview.md`). Record the result in `00-triage.md`.

## The taxonomy (genre → profile → apparatus → authorship)

| Detected genre | Tone profile | Register | Proof apparatus | Default authorship |
|---|---|---|---|---|
| Intelligence / market / strategy report | `analytical-intelligence` | decisive, epistemic, hypothesis-driven | full: inline grades, methodology, steelman, appendix | AI-autonomous |
| IT-industry analysis / report | `analytical-intelligence` (IT-tuned) | decisive, data-led | full | AI-autonomous |
| Academic paper / literature review | `academic` | measured, formal, citation-dense | full, citation-style adapted | AI-autonomous |
| Pure-science / popular-science book | `popular-science` | vivid, case-led, mechanism-explaining | full→medium per tier | AI-autonomous (user may supply cases) |
| Cognitive-behavior / psychology book | `cognitive-behavior` | warm-rigorous, de-shaming, mechanism-first | medium (science cited; experience un-graded) | **hybrid** — user cases + AI science |
| Narrative nonfiction / long-form journalism | `narrative-nonfiction` | vivid, case-led, warm-rigorous | medium: grades in appendix, lighter inline | AI-autonomous |
| Persuasive essay / op-ed | `persuasive` | committed, argument-forward | medium; steelman required | AI-autonomous |
| Technical doc / runbook / spec | `technical` | precise, neutral, imperative | low: spec-style refs, no narrative | AI-autonomous |
| Personal experience / memoir | `personal-experience` | intimate, first-person, honest | none (lived truth, not citation) | **interview-driven** |
| Mindset / self-help | `mindset` | direct, encouraging, actionable | low (science cited where claimed) | **interview-driven / hybrid** |
| Love / relationships | `love` | warm, intimate, reflective | none–low | **interview-driven** |
| Literary fiction | `literary` | stylistic, voice-first | none — fiction rules replace truth/citation | interview-driven (user's vision) |
| Romance | `romance` | emotional, intimate, sensory | none — fiction rules | interview-driven |
| Children's / YA | `childrens` | simple, age-tuned, safe | none — fiction rules | interview-driven |

Each profile lives at `${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/<id>.md` and carries its own structural
guidance — there is no separate scaffold. If a profile file doesn't exist yet, fall
back to `inputs/writing-instruction.md`, or to clear, accurate plain prose.

## Detection signal order

Decide the genre by the first signal that applies:

1. **Explicit user instruction always wins** — *unless its apparatus contradicts the
   material* (see "When voice and material disagree" below). "Write this as an academic
   paper" / "this is a romance novel" — take it.
2. **Project default**, if the user deliberately set one — `inputs/writing-instruction.md`
   seeded from a chosen profile at init, or the genre recorded in `CLAUDE.md`.
3. **Inferred from the task wording and any handed-over sources** — "report",
   "analysis", "chapter of my memoir", "spec", a dataset vs a lived anecdote.
4. **When ambiguous, you must stop and ask** — one batched round of questions (see
   `interview.md`). You do not need to be certain to proceed, but asking is the
   **default**, not a last resort, whenever any of these holds:
   - you can name a **second genre** a reasonable reader might assign to this same request;
   - the request **mixes signals** — a "report" that argues a thesis (analytical vs
     persuasive), a "guide" built on the user's own story (mindset vs memoir), a "science
     book" with no clear citation expectation (popular-science vs academic);
   - the requested genre and the supplied material **disagree about whether claims get
     graded** (see "When voice and material disagree" below).

   When any holds, record the genre as `UNRESOLVED` in `00-triage.md` and serve the batched
   round before writing a word. Picking a genre *to avoid asking* is the exact failure this
   contract exists to prevent — do not confabulate a confidence number to justify
   proceeding; judge against the conditions above.

## The rule that matters most: the profile decides which honesty machinery applies

The tone profile is not only a voice — it switches the truth apparatus on or off:

- **Non-fiction profiles** (analytical-intelligence, academic, popular-science,
  cognitive-behavior, narrative-nonfiction, persuasive, technical) keep the full
  A/B/C/D grading and the proof package, **scaled by the rigor tier** (see the Effort
  tier section of `SKILL.md`). A report at `high-stakes` ships methodology + inline
  grades + appendix; a `normal` piece ships grades in a closing note.
- **Fiction profiles** (literary, romance, childrens) **switch grading and the proof
  package off** — you cannot cite a source for an invented scene — and switch on
  **craft contracts** instead: internal consistency, motivation-driven action, earned
  endings, consistent POV, age-appropriateness. Invention is the medium, not a fault.
- **Experience profiles** (personal-experience, love) treat the user's lived account
  as the truth: it is not graded against external sources, but it also must not be
  invented (see `interview.md`). Any *external* claim woven in (a statistic, a study)
  still carries a grade.
- **Hybrid** (cognitive-behavior, sometimes mindset): the user's cases are
  experience-truth; the science around them is fully graded and cited; the two are
  kept visibly distinct in the prose.

## When voice and material disagree

Sometimes an explicit genre's default apparatus contradicts the *nature of the supplied
material* — a citation-graded genre (academic, analytical-intelligence) applied to a
lived/experiential spine ("write an academic paper about my messy divorce"), or a fiction
genre applied to a real dataset the user wants represented faithfully. You cannot grade a
marriage A/B/C/D, and you must not delete the user's life because it has no JSTOR citation.

Do **not** silently honor the request (and start grading lived experience), and do **not**
silently override the user's genre. Resolve by **composition** — the voice and the truth
apparatus are separate knobs, so split them:

- **Keep the requested voice/register** (academic → measured, formal, structured).
- **Switch the apparatus to match the material**: a lived/experiential spine follows
  experience-truth rules (true-as-lived, ungraded, **never invented** — see `interview.md`);
  a real dataset keeps grading **on**. Any *external* fact woven into a lived spine (a
  statistic, a study) still carries a grade.
- **Record the split** in `00-triage.md`, e.g. `Voice: academic · Apparatus:
  experience-truth (lived spine) — composed, not stock academic`, and **state it to the
  user in one line**.

If the user's intent is genuinely unclear — do they want analysis *of* the divorce, or the
divorce *rendered*? — that is an ambiguity → ask (signal 4).

## Filling `00-triage.md` from the profile

From the chosen profile, copy across: `Register` → the tone line, `Proof apparatus` →
proof-apparatus level, `Structure shape` → structure shape, `Default authorship` → the
*starting point* for the authorship line. The remaining triage fields are **not** profile
properties:

- **Rigor tier** comes from the task's stakes (see the Effort/rigor section of `SKILL.md`),
  not the genre — the same romance can be drafted at `normal` or `high-stakes`.
- **Working mode** (`auto | interactive`) comes from the user's choice; it combines with
  `Default authorship` to resolve the final approach (AI-autonomous | interview-driven |
  hybrid).

Do not expect the profile header to supply rigor tier or working mode — by design it
cannot, and that orthogonality is deliberate.

## Output into `00-triage.md`

Record one line each: detected genre, tone profile, rigor tier, proof-apparatus level
(none | low | medium | full), structure shape (see `structure-shapes.md`), working
mode (auto | interactive), resolved authorship approach (AI-autonomous |
interview-driven | hybrid), any **voice/apparatus composition** (per the section above),
and one line of why — or the one question to ask if ambiguous.
