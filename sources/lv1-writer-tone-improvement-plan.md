# lv1-writer tone system — improvement plan (round 2)

*Scope: verify the 7-point external critique of `tone-detect.md` and the profiles, keep
only what survives scrutiny, and turn the survivors into concrete file changes. Companion
to `lv1-writer-improvement-plan.md`. Paths are relative to `lv1-writer-plugin/`.*

The critique is sharp on architecture and right about several real failure modes. It is
also written in a confident pseudo-quantitative register that bolts invented mechanism
onto good instinct. The job here is to separate the two: take the failure mode, drop the
fake number, and write a fix the model can actually execute.

A recurring tell to watch for: **an LLM cannot read out a calibrated number about its own
internal state.** Any fix that says "calculate your confidence as 73%" or "the model has a
600-word breath" will be obeyed by *confabulating a number that justifies what it already
wanted to do*. Those fixes feel rigorous and enforce nothing. Behavioral, checkable rules
("if you can name a second plausible genre, you must ask") enforce something. Every
rewrite below is pushed to that form.

---

## Verdict at a glance

| # | Point | Real bug? | Proposed fix | Verdict |
|---|---|---|---|---|
| 1 | Eager Router under-asks on ambiguity | **Yes** | numeric entropy trigger (<20% delta / <75%) | **Adopt the bug, rewrite the fix** — replace the fake score with a criterial ask-gate |
| 2 | Genre vs source-material collision ("academic paper about my divorce") | **Yes — strongest point** | source data always wins, downgrade genre, log it | **Adopt, refine** — compose voice + material-appropriate apparatus; surface, don't silently override |
| 3 | `[A]` grade can be slapped on a pretrained recall | **Yes** | recall caps at `[B]`, `[A]` needs context/tool provenance | **Adopt the provenance test; keep the existing stricter cap** (recall already caps at `[C]`) |
| 4 | Warm profiles collapse into AI therapy-speak | **Yes** | per-profile `[FORBIDDEN LEXICON]`, instant fail | **Adopt as a shared anti-tell blocklist; soften the trigger** (flag clusters, don't hard-fail one word) |
| 5 | Hybrid "visibly distinct" is under-specified | **Partly** (already half-addressed) | force blockquote / bold-bracket separator | **Adopt a consistent-device rule; reject mandatory blockquotes** (they fight the "moving prose" goal) |
| 6 | Profiles lack Rigor Tier / Working Mode keys | Observation true, **conclusion wrong** | stuff all 8 triage keys into every profile | **Reject the fix** — those are task knobs, not genre constants. Adopt only a small profile→triage mapping note |
| 7 | No pacing/density control per genre | **Yes** | `Pacing_Density` var, 250-word/beat minimum | **Adopt density posture per profile; reject the word quota** (quotas induce padding) |
| — | Context contamination: drop `tone-detect.md` after triage | **Mostly already handled** | evict the taxonomy from the draft payload | **Adopt as a one-line hygiene note** — design already reads one profile from a separate file |

Net: 5 adopt-with-refinement, 1 adopt-the-test-only, 1 reject-the-fix-keep-a-footnote, 1
hygiene note. Nothing is adopted verbatim; nothing is dismissed wholesale.

---

## Point-by-point verification

### 1 — The "Eager Router" under-asks. Real bug; fix needs de-numbering.

**Verified.** The bias is real and well-known: a generative model rewarded for producing
text will resolve a near-tie by picking a genre and moving on, because "ask a question"
ends the generation it was about to enjoy. Current Step 4 — *"when genuinely ambiguous,
ask… not a guess"* — is a soft fallback, and "genuinely ambiguous" is exactly the kind of
self-judgment the bias erodes.

**But the proposed cure is theatre.** "Calculate your internal confidence (0–100%); if the
delta is < 20% or top < 75%, you are forbidden to assign a genre." The model has no
calibrated read on its own genre-posterior; asked for a percentage it will *generate one*,
and it will generate whatever lets it proceed. A fake threshold on a fake number enforces
nothing — it just adds a ritual.

**Extract:** keep the strengthened gate, make it *criterial* not numeric. Asking is cheap
and should be the **default** on an enumerated set of conditions, not a last resort the
model has to talk itself into.

> **Rewrite of Step 4 (`tone-detect.md`):** You do not need to be certain to proceed — but
> you must *stop and ask* when any of these is true: (a) you can name a second genre that a
> reasonable reader might assign to this same request; (b) the request mixes signals — e.g.
> a "report" that argues a thesis (analytical vs persuasive), a "guide" built on the user's
> own story (mindset vs memoir), a "science book" with no clear citation expectation
> (popular-science vs academic); (c) the requested genre and the supplied material disagree
> about whether claims get graded (see Rule 2 below). When any holds, output the genre as
> `UNRESOLVED` and serve **one batched round** of questions before writing a word. Picking a
> genre to avoid asking is the failure this contract exists to prevent.

This keeps the user's intent (force the ask) and discards the un-executable math.

### 2 — Genre vs source-material collision. The strongest point. Adopt, refine.

**Verified, and it's the most important one.** Signal order Rule 1 says *"explicit user
instruction always wins"* with no exception for when the requested apparatus is
incompatible with the material. The walked example is exact: "write an academic paper
about my messy divorce" routes to `academic.md` (Proof apparatus: **full**, A/B/C/D, "every
claim is cited", "never fabricate a citation"), but the spine is lived experience that
*has* no citation and must not be graded or dropped. `academic.md`'s own "Never do this"
list would force the model to treat un-citable lived detail as a defect.

**The fix is directionally right but too blunt.** "Source data always wins; override the
user's genre to the nearest hybrid; log it" silently overrules an explicit instruction —
which violates this system's own ethos (ambiguity → *ask*, don't guess) and would surprise
a user who had a good reason for the academic frame.

**The cleaner resolution is already latent in your architecture.** The whole design
decouples **voice** from **truth apparatus**. So a collision doesn't require picking a
winner — it requires *composing*: keep the requested **voice/register** (academic: measured,
formal, structured) and let the **material** set the **apparatus** (lived spine =
experience-truth, ungraded, never invented; any external fact woven in = still graded).
That is precisely the hybrid contract. Then **surface it**, don't bury it.

> **New block in `tone-detect.md`, "When voice and material disagree":** If an explicit
> genre's default apparatus contradicts the nature of the supplied material — a
> citation-graded genre (academic, analytical-intelligence) applied to a lived/experiential
> spine, or a fiction genre applied to a real dataset the user wants represented faithfully
> — do **not** silently honor or silently override the request. Resolve by **composition**:
> keep the requested *voice*, switch the *apparatus* to match the material (lived spine →
> experience-truth rules; real data → grading on). Record the split in `00-triage.md`
> (`Voice: academic · Apparatus: experience-truth (lived spine) — composed, not stock
> academic`) and state it to the user in one line. If the user's intent is genuinely unclear
> (do they want analysis *of* the divorce, or the divorce *rendered*?), that is an
> ambiguity → ask (Rule 4).

This honors the explicit request where it's coherent, refuses the incoherent part
(grading a marriage), and stays loud about the choice.

### 3 — "Hallucinated Primary." Real; adopt the provenance test, keep the stricter cap.

**Verified in spirit.** A model genuinely cannot feel the difference between "I read this in
the context window" and "this is a strong association in my weights," so it will attach
`[A]` to a confident recall. Worth hardening.

**One correction to the critique:** it proposes capping recall at `[B]`. The existing grade
table is *already stricter* — recall maxes at **`[C]` "From memory… held back until
confirmed"**, and `[A]` is "a primary source you fetched and read." So don't *weaken* to
`[B]`. The valuable part of the point is the **operational test** for `[A]`: make the
boundary physical and checkable rather than a vibe.

> **Sharpen the `[A]` row (SKILL.md grade table) and echo in `analytical-intelligence.md`
> and `academic.md`:** A claim earns `[A]` only if the supporting source text is **present
> in this session** — fetched and read this run, handed over by the user, or returned by a
> tool call you actually made — such that you could quote it now. A claim you "know" from
> training but did not pull this session is **not** `[A]`; grade it `[C]` (memory) until
> confirmed, or `[B]` only if it is a sound *derivation* from material that is present. If
> you cannot point to where the source sits, it is not an `[A]`.

Cheap, checkable, and consistent with the existing ladder.

### 4 — "Instagram Therapist." Real; adopt as a shared blocklist, soften the trigger.

**Verified.** Prompt a model for "warm, intimate, de-shaming" and it slides into the
LinkedIn-therapist register — *tapestry, hold space, journey, lean in, give yourself
grace*. `love.md`, `personal-experience.md`, `mindset.md`, and `cognitive-behavior.md` all
sit in that gravity well. A banned-phrase list is a cheap, proven mitigation.

**Two refinements.** (a) The tells aren't unique to warm genres — analytical writing has its
own (*delve, supercharge, in today's fast-paced world, it's worth noting that*). Make a
**shared** anti-tell blocklist all profiles reference, plus a **warmth-specific** extension.
(b) "Use any banned word → draft fails inspection" is too brittle: *journey*, *navigate*,
*crucial*, *foster* have honest literal uses (a memoir may *be* about a journey). Make the
inspector flag **clusters / characteristic usage**, not a single literal occurrence — the
goal is killing the register, not banning a dictionary.

> **New shared file `references/anti-tells.md`** (read by draft, checked by inspect):
> a blocklist of AI-tell words and constructions, grouped: *generic-AI* (`delve, tapestry,
> testament, navigate the complexities, it's worth noting, in today's fast-paced,
> supercharge, unlock, foster, beacon, realm`), *therapy-speak* (`hold space, lean into,
> sit with, your journey, do the work, give yourself grace, show up for yourself, inner
> child` unless the user's own material uses them), *hype* (`game-changer, revolutionary,
> seamless`). Rule: avoid these as reflexive filler; a literal, earned use is fine.
> Inspector check 6 (tone conformance) flags a *cluster* of tells or any therapy-speak in a
> warm draft as a FIX-IT, not a single incidental word.

Each warm profile's Tone section gains one line: *"Read `references/anti-tells.md`; the
therapy-speak register is an automatic FIX-IT here."*

### 5 — Hybrid prose-stitching. Partly real; already half-solved; adopt a softened rule.

**Partly verified.** "Keep the science and the lived material visibly distinct" *is* under-
specified, and a bare paragraph break is the lazy reading of it. But the critique overstates
the current state: `cognitive-behavior.md` already requires a **clear handoff phrase**
("Here's what was happening underneath…"), already says *don't dissolve the science into the
anecdote as if the anecdote proved it*, and already **bolds the named mechanism as a skim-
anchor**. So a distinction device exists; it just isn't mandatory or consistent.

**And the proposed fix has a cost the critique ignores.** The same profile asks for the
science "in plain, moving prose," *not* a "clinical lecture register." Forcing every
scientific beat into a `**[The Cognitive Engine]**` bracket or a blockquote risks exactly
the textbook-sidebar feel the warm genre is trying to avoid — trading "jarring" for
"clinical." So mandate *consistency*, not a specific heavy device.

> **Tighten `cognitive-behavior.md` (and apply the pattern to `mindset.md`):** Choose **one**
> distinction device for the book and apply it consistently so the reader always knows which
> voice is speaking: the user's lived material in unadorned narrative prose; each cited
> scientific mechanism introduced by a consistent marker — a **bold lead-in anchor**
> (`**The loop underneath** — controlled studies show…`) is the default; a blockquote is
> available when the science is a longer aside. **Never** let an *un-cited* scientific claim
> share a sentence with a lived anecdote as if the anecdote were its evidence. Decide the
> device once (record it in `intake.md`/triage) and don't drift between chapters.

Adopts the discipline, keeps the warmth, names the trade-off.

### 6 — "Triage Manifest Inheritance Mismatch." Observation true, fix wrong. Reject, footnote.

**The factual claim checks out:** every profile header carries exactly five keys — `Genre,
Register, Proof apparatus, Default authorship, Structure shape` — and none carries `Rigor
Tier` or `Working Mode`. (Verified across all 13 files; the headers are already uniform
with each other.)

**The conclusion does not follow.** Rigor tier and working mode are **task- and user-level
knobs, deliberately orthogonal to genre** — that orthogonality is the three-knob design the
critique elsewhere praises. The *same* romance can be drafted at `normal` or `high-stakes`;
working mode is the user's `auto/interactive` choice, not a property of the genre. Baking
those keys into a profile would **re-couple** what the architecture correctly separated and
make the profile lie (a fixed "Working Mode: interactive" can't represent a user who chose
auto). So the model isn't pulling them "out of thin air" — it pulls them from the **task and
the user**, which is where they live. The profile already contributes its two legitimate
fields: `Proof apparatus` → apparatus level, `Default authorship` → the genre's input to
resolving working mode.

**The only real kernel** is that the profile→triage derivation is *implicit*. Fix that with
documentation, not by corrupting the headers.

> **Add a short mapping note to `tone-detect.md` ("Filling `00-triage.md` from the
> profile"):** From the chosen profile, copy `Register`→tone line, `Proof apparatus`→apparatus
> level, `Structure shape`→structure shape, `Default authorship`→starting point for the
> authorship line. The remaining triage fields are **not** from the profile: **rigor tier**
> comes from the task's stakes (SKILL.md effort tier) and **working mode** from the user's
> auto/interactive choice; the two combine with `Default authorship` to resolve the final
> approach. Do not expect the profile to supply rigor or working mode — by design it can't.

Reject the "8 keys in every file" change explicitly so a later builder doesn't "fix" it.

### 7 — "Token Budget Vacuum." Premise false, need real. Adopt density posture, reject quota.

**The premise is invented.** There is no "600–750 word natural exhalation breath." Output
length is governed by the token limit and the prompt, not a physiological constant of the
model. Discard that framing entirely.

**The need underneath is real.** Without explicit guidance the model under-writes expansive
genres (a romance or memoir beat comes out rushed and summarized) and over-writes terse ones
(a runbook turns into rambling prose). Several profiles already gesture at this implicitly
(`technical` compresses via tables; `love`/`personal-experience` linger in prose). Making it
explicit is a genuine quality lever.

**But the proposed "minimum 250 words per beat" quota backfires.** A word-count floor
induces *padding* — the exact fluff this whole system exists to prevent (the anti-tells of
Point 4 thrive in mandated word counts). Use a **density posture**, executed by structural
choices, not a tape measure.

> **Add a "Density & pacing" line to each profile header (a 6th key) and one sentence in its
> Structure section** — not a triage variable, not a word count:
> - `technical`, `analytical-intelligence`, `academic` → **compress**: lead with tables /
>   lists / figures; shortest path to the claim; no narrative throat-clearing.
> - `romance`, `literary`, `love`, `personal-experience` → **expand**: stay inside the scene;
>   render the beat (sensory, emotional) before moving on; don't summarize what should be
>   lived. *Expansion means more rendered moment, never more filler.*
> - `popular-science`, `narrative-nonfiction`, `cognitive-behavior`, `mindset`,
>   `persuasive`, `childrens` → **balanced**: open a point with a concrete case, then make it
>   efficiently; length follows the idea.
>
> Inspector check 10 (format-follows-content) already flags over-/under-formatting; extend
> its note to flag a *rushed* expansive scene and a *padded* compressive section.

Keeps the real lever, drops the counter-productive quota and the fake biology.

### Pro-tip — context contamination. Mostly already handled; one hygiene line.

**Largely mitigated by the existing design.** The contamination vector described ("the LLM
sees all the other profiles sitting in the text file and cross-pollinates") assumes the
profiles live in one big file. They don't — each is a **separate file**, and the draft step
reads only `assets/tone-profiles/<id>.md` (the single assigned profile). `tone-detect.md` is
a triage-time reference, not a draft-time one. `lv1-research` and `lv1-inspect` already run
as **isolated subagents** with their own context. So the specific failure is mostly designed
out.

**Residual worth a line:** the draft station runs in the orchestrator's context, so be
explicit that it loads the one assigned profile and does not re-read the full taxonomy while
drafting.

> **One line in `references/draft.md`:** When drafting, read only the single tone profile
> named in `00-triage.md`. Do not re-load `tone-detect.md` or other profiles into the
> drafting context — the genre is already decided; the other profiles are noise that
> encourages register bleed.

---

## Consolidated file-by-file changes

**[edit] `references/tone-detect.md`**
- Rewrite **Step 4** of the signal order into the criterial ask-gate (Point 1): enumerate the
  conditions that *force* a `UNRESOLVED` + one batched round; name asking as the default, not
  the fallback.
- Add **"When voice and material disagree"** block (Point 2): compose voice + material-set
  apparatus, record the split in triage, surface it to the user; if intent is unclear, ask.
- Add **"Filling `00-triage.md` from the profile"** mapping note (Point 6): which triage
  fields come from the profile vs. from task/user; state plainly that rigor tier and working
  mode are *not* profile properties.

**[edit] SKILL.md — Confidence grades table**
- Sharpen the **`[A]` definition** to the in-session-provenance test (Point 3); keep `[C]`
  for memory. Echo the one-liner in `analytical-intelligence.md` and `academic.md`.

**[new] `references/anti-tells.md`** (Point 4)
- Shared AI-tell / therapy-speak / hype blocklist, grouped, with the "literal earned use is
  fine; reflexive filler is not" rule. Referenced by `draft.md` and by inspector check 6.

**[edit] warm profiles** `love.md`, `personal-experience.md`, `mindset.md`,
`cognitive-behavior.md`
- One Tone-section line each pointing to `anti-tells.md` and marking therapy-speak an
  automatic FIX-IT (Point 4).

**[edit] `cognitive-behavior.md`** (and pattern into `mindset.md`) (Point 5)
- Upgrade "visibly distinct" to a **consistent distinction device** (default: bold lead-in
  anchor; blockquote optional), chosen once and held; forbid un-cited science sharing a
  sentence with a lived anecdote as pseudo-proof.

**[edit] all 13 profiles** (Point 7)
- Add a 6th header key **`Density & pacing:`** (compress | balanced | expand) and one Structure-
  section sentence on what that posture means for the genre. *Note:* this **is** a genuine
  genre constant (unlike rigor/working mode), so it belongs in the header — it describes how
  the genre reads, not a per-task choice.

**[edit] `references/draft.md`** (pro-tip)
- One line: while drafting, load only the assigned profile; do not re-load the taxonomy.

**[edit] `agents/lv1-inspect.md`**
- Extend **check 6 (tone conformance)** to flag a cluster of anti-tells / any therapy-speak in
  a warm draft (Point 4).
- Extend **check 10 (format-follows-content)** to flag a rushed expansive scene or a padded
  compressive section against the profile's density posture (Point 7).
- Add to the gating note that a voice/apparatus *composition* recorded in triage (Point 2) is
  honored — don't fail a composed-academic memoir for ungraded lived material.

**Explicitly NOT doing**
- No numeric confidence/entropy thresholds anywhere (Point 1 — un-executable).
- No `[B]`-cap weakening of memory grades (Point 3 — current `[C]` cap is stricter).
- No rigor-tier / working-mode keys forced into profiles (Point 6 — re-couples the design).
- No minimum-words-per-beat quota (Point 7 — induces padding).

---

## Sequencing

1. **`tone-detect.md`** (Points 1, 2, 6) — the router is upstream of everything; fix it first.
2. **`anti-tells.md` + grade-`[A]` sharpening** (Points 3, 4) — small, self-contained, high
   visible-quality return.
3. **Profile edits** — density key across all 13 (Point 7); warm-profile anti-tell lines and
   the cognitive-behavior/mindset distinction-device upgrade (Points 4, 5).
4. **`draft.md`** one-liner (pro-tip).
5. **`lv1-inspect.md`** last (Points 2, 4, 7) — it polices the rules above; without it they're
   advisory. Mirrors the original plan's "inspector enforcement comes last."

Each step is independently shippable; partial states stay safe (a missing `anti-tells.md`
just means the draft falls back to plain good taste, as today).

## One-paragraph summary

The critique correctly diagnoses six real weaknesses — an ask-gate that loses to the model's
bias toward proceeding, an explicit-genre rule with no provision for material that can't be
graded, an `[A]` grade the model attaches to pretrained recall, warm profiles that decay into
therapy-speak, an under-specified "visibly distinct" in hybrids, and no pacing control — and
one non-issue (profiles "missing" task-level keys they shouldn't carry). But its fixes lean on
mechanisms the model doesn't have: it can't read out a calibrated confidence percentage, has no
fixed word-count "breath," and its profiles already standardize on a uniform header. So the plan
keeps every real bug and re-engineers each fix into something a transformer can actually obey:
criterial ask-conditions instead of fake entropy; voice/apparatus *composition* (the system's own
decoupling) instead of a silent genre override; an in-session provenance test for `[A]` instead of
a weaker cap; a shared anti-tell blocklist flagged by cluster instead of a brittle one-word fail;
one consistent distinction device instead of mandatory blockquotes; a density *posture* per genre
instead of a padding quota; and a documented profile→triage mapping instead of corrupting the
profile headers. The inspector is upgraded last to enforce the new tone, anti-tell, composition,
and pacing rules — because until it does, all of this is advisory.
