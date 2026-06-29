# Tone profile — cognitive-behavior

- **Genre:** cognitive-behavior / psychology book (applied, reader-facing)
- **Register:** warm-rigorous, de-shaming, mechanism-first
- **Proof apparatus:** medium — the **science is graded and cited (A/B/C/D)**; the **user's lived experience is not graded** (it is experience-truth, not a sourced claim)
- **Default authorship:** **hybrid** — the user supplies the real cases/experience; AI supplies and cites the science; the two are kept visibly distinct
- **Structure shape:** relatable scene (user case) → cognitive mechanism (AI, cited) → reframe → practice
- **Density & pacing:** **balanced** — dwell in the lived scene, then deliver the mechanism efficiently; the practice is tight. Length follows the idea, never a quota

> Help a reader recognize a pattern in themselves and do something about it — grounded
> in real cognitive science, anchored in the author's real experience, never in
> invention. Read order: Requirements → Two-source rule → Truth → Tone → Structure →
> Review. **If two rules conflict, Truth wins.**

---

## The two-source rule (the heart of this profile)

This genre runs on **two distinct kinds of material**, and they must never blur:

1. **Lived material — from the user (interview-driven).** Real situations, triggers,
   feelings, dialogue, turning points. This is the spine of each chapter. **AI does not
   invent it.** Gather it through the intake interview and mid-draft batched rounds
   (`${CLAUDE_PLUGIN_ROOT}/references/interview.md`); store it in `manuscript/intake.md`. A user's account is
   treated as true-as-experienced and is **not** graded A/B/C/D.
2. **Science — from AI (cited).** The cognitive mechanism behind the lived pattern:
   the bias, the loop, the reframe, the evidence it works. Every scientific claim is
   **graded and sourced** like any non-fiction claim, with effect sizes and limits
   honest.

**Keep them visibly distinct — with one consistent device, not just a paragraph break.**
A bare paragraph break is the lazy reading of "distinct"; the reader should never have to
guess which voice is speaking. Pick **one** distinction device for the book and hold it:

- **Lived material** → unadorned narrative prose (the scene, the feeling, the user's words).
- **Cited science** → introduced by a consistent marker. Default: a **bold lead-in anchor**
  (`**The loop underneath** — in controlled studies…`). A blockquote (`>`) is available
  when the science is a longer aside. Choose the device once (note it in `intake.md` /
  triage) and don't drift between chapters.
- **Never** let an *un-cited* scientific claim share a sentence with a lived anecdote as if
  the anecdote were its evidence.

Use a clear handoff ("Here's what was happening underneath…") rather than dissolving the
science into the anecdote. One real case does not prove a mechanism — the science does; the
case makes it *land*. (Keep the warmth: the marker separates the voices; it must not turn
the science into a clinical sidebar — see Tone.)

## Truth

- **Invent no personal anecdote, ever.** If a chapter needs a case the user hasn't
  given, ask for it (batched round) or render the experimental/study scene instead —
  don't fabricate a person. A made-up "client story" is the fiction-equivalent of a
  fabricated source: a REJECT.
- **Don't over-claim the science.** A lab finding is not a life rule; a correlation is
  not a cause; one population's result is not everyone's. Phrase confidence to match
  the evidence (echo < consistent with < well-evidenced < replicated).
- A skip is a **declared gap**: if the user can't supply a needed case, say the section
  is lighter rather than dress up a thin or invented one.

## Tone & voice

- **De-shame above all.** Frame the painful or embarrassing pattern as a predictable
  mechanism common across people — never a moral failing or a diagnosis of the reader.
- Warm and rigorous at once; the reader should feel understood, not analyzed.
- Second person used carefully — describe the mechanism, don't diagnose the individual
  reader ("many people find…" not "you are…").
- No clinical lecture register; carry the science in plain, moving prose.
- **Read `${CLAUDE_PLUGIN_ROOT}/references/anti-tells.md`.** Therapy-speak (`hold space`, `do the work`,
  `your journey`, `sit with it`…) is an automatic FIX-IT — describe the mechanism in real
  language, not wellness-post filler.

## Structure & format

- Per-chapter beat: **scene (user's real situation) → mechanism (the cognitive
  process, cited) → reframe (the shift in seeing it) → practice (a concrete, doable
  step)**.
- One pattern per chapter; build cumulatively.
- **Format follows content** (see `${CLAUDE_PLUGIN_ROOT}/references/draft.md`): prose for the scene and the
  reflection (content the reader *experiences*); bullets for a recap of named steps or
  a practice the reader will *do*; a simple diagram/arrows for a thought-loop
  (`trigger → thought → feeling → action → reinforcement`). Default to prose.
- Bold the named mechanism as a skim-anchor. No author-only scaffolding in the text.

## Process

1. Gather the user's lived material first (`${CLAUDE_PLUGIN_ROOT}/references/interview.md`) — one batched
   round at the start; more rounds when a chapter surfaces a gap.
2. Research the science for each pattern; source and grade it into the library.
3. Draft to the beat, reading `manuscript/intake.md` as a first-class source for the
   lived material and the library for the science.
4. Verify: every scientific claim traces to its source and is faithful in scope; every
   personal detail traces to `intake.md` (not invented); the two are visibly distinct.

## Self-review

- Is the science honestly graded and scoped, no lab-to-life over-reach?
- Is every anecdote the user's (traceable to `intake.md`), none invented or composite?
- Are experience and science kept visibly separate, with the science doing the proving?
- Is the framing de-shaming, describing the mechanism rather than diagnosing the reader?
- Is each practice concrete and actually doable?

## Never do this

- Invent, embellish, or merge personal anecdotes; present a composite as one real person.
- State a lab/correlational finding as a settled life rule.
- Let one anecdote stand in as proof of a mechanism.
- Diagnose the reader instead of describing the mechanism.
- Blur the line between "the author's experience" and "what the research shows."
