# Tone profile — mindset

- **Genre:** mindset / self-help
- **Register:** direct, encouraging, actionable — change something for the reader
- **Proof apparatus:** low — claims about *how the mind/behaviour works* are cited and graded; the user's stories are not
- **Default authorship:** **interview-driven / hybrid** — the user's experience and philosophy lead; AI grounds and structures
- **Structure shape:** problem/promise → principle → story → practice, per chapter
- **Density & pacing:** **balanced** — story and principle get room; the practice is tight and scannable. Length follows the idea, never a motivational-padding quota

> Move the reader from understanding to doing — built on the user's real experience and
> philosophy, kept honest by real evidence wherever a factual claim is made. Read
> order: Requirements → Two kinds of claim → Tone → Structure → Review. **If two rules
> conflict, Truth wins.**

---

## Two kinds of claim (keep them honest)

- **The user's experience and philosophy** — the stories, the hard-won principles, the
  worldview. Gather via `${CLAUDE_PLUGIN_ROOT}/references/interview.md` into `manuscript/intake.md`. Treated
  as the author's voice; not graded; **never invented** by AI.
- **Factual claims about how things work** — "the brain does X", "studies show Y",
  "habit forms in Z". These are **cited and graded** (A/B/C/D), scoped honestly, and
  must not over-claim. Self-help drifts into pseudoscience exactly here — hold the line:
  if the science is thin or contested, say so or drop the precise claim.

Keep the two **visibly distinct with one consistent device** (as in cognitive-behavior):
the user's story/principle in plain prose; a cited mechanism flagged by a consistent marker
(a bold lead-in or a brief citation), never blurred into the story as if the story proved
it. A paragraph break alone isn't enough.

## Tone & voice

- Direct and warm; speak *to* the reader, motivate without hype or guru-posturing.
- Encouraging, not shaming — frame struggle as common and workable.
- Concrete over abstract: a real example beats an inspirational slogan.
- Avoid grand unfalsifiable promises ("this will change your life"); promise the
  specific, doable shift.
- **Read `${CLAUDE_PLUGIN_ROOT}/references/anti-tells.md`.** Hype and therapy-speak (`game-changer`,
  `unlock your potential`, `do the work`, `show up for yourself`…) are an automatic
  FIX-IT — concrete and direct beats inspirational filler.

## Structure & format

- Per-chapter beat: **the problem (and the promise) → the principle → a real story
  (user's or a documented one) → a concrete practice the reader can do today**.
- One idea per chapter; build cumulatively.
- **Format follows content** (see `${CLAUDE_PLUGIN_ROOT}/references/draft.md`): prose for story and
  principle; **bullets or numbered steps for a practice the reader will *do*** (this is
  the one genre where actionable lists earn their place); arrows for a simple
  before→after or loop. Don't bury an action in a paragraph.

## Process

1. Interview the user for their stories, principles, and the change they want for the
   reader.
2. Research and grade any factual mechanism claims; source into the library.
3. Draft to the beat from `intake.md` (experience) + library (science).
4. Verify: every "research shows" claim traces to a real source and is scoped honestly;
   every story traces to `intake.md` or a documented source.

## Never do this

- Dress an opinion or anecdote up as established science.
- Over-claim a mechanism ("rewires your brain") beyond what evidence supports.
- Invent the user's stories or a "client" anecdote.
- Make grand, unfalsifiable promises, or shame the reader for struggling.
