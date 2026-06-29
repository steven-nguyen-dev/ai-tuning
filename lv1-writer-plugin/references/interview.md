# Interview — gathering the user's material (interview-driven & hybrid authorship)

Some genres can't be researched off the open web — their substance is the user's own
life, cases, philosophy, or creative vision. For those, the user is the primary source
and AI is the interviewer and craftsman. This contract governs how that material is
gathered, stored, and used.

## When this applies

The authorship approach is resolved in triage from genre + working mode (see
`${CLAUDE_PLUGIN_ROOT}/references/tone-detect.md`):

- **Interview-driven** — personal-experience, love, and fiction (literary, romance,
  childrens); or *any* genre when the user chose **interactive** working mode. The
  user's material is the spine; AI invents nothing.
- **Hybrid** — cognitive-behavior (and often mindset): the user supplies the lived
  cases/experience; AI supplies and cites the science around them; the two are kept
  visibly distinct in the prose.
- **AI-autonomous** — research/analytical/technical genres in **auto** mode don't use
  this contract except to ask a genuinely blocking question.

## The batching rule (non-negotiable)

**Never drip questions one at a time.** At any moment input is needed, collect *every*
open question, group them by theme, and ask them as **one round**. Only after the user
answers (or skips) does the next round form. Three moments call for a round:

1. **Intake interview** — at init (or task start). One comprehensive round that captures
   what the piece needs before drafting begins.
2. **Mid-draft gaps** — when a section can't be written truthfully without the user's
   material, pause and ask, in a single batched round, every question blocking that
   section. Don't guess and don't invent to keep moving.
3. **Pre-inspection confirmation** — a final round for anything still assumed, before
   the draft goes to inspection.

Use Cowork's AskUserQuestion mechanism when available (grouped multiple-choice + free
text); otherwise ask as a numbered list the user can answer in one reply.

## What to ask (by genre — adapt, don't recite)

- **personal-experience / memoir** — events, timeline, the people involved, feelings at
  the time and now, turning points, the arc they want, what they will and won't share.
- **love / relationships** — the relationships and moments, reflections, the throughline,
  privacy boundaries (whose details can appear, what to anonymize).
- **cognitive-behavior (hybrid)** — real situations and triggers, what the pattern feels
  like from inside, what they want the reader to recognize and be able to do.
- **mindset (hybrid)** — their stories, the hard-won principles, the change they want for
  the reader.
- **fiction** — premise, characters, setting, themes, arc, tone, heat/age level where
  relevant, and any non-negotiables. Surface real creative forks (a plot turn, a
  character choice) as a batched round rather than deciding silently.

## What AI may and may not do

- **Interview-driven:** the spine — cases, feelings, events, opinions, creative vision —
  comes from the user. AI may research *around* it (e.g. the science explaining a
  described experience, the real history behind a setting) and must label which parts are
  the user's account vs AI-added context. **AI never invents a personal anecdote,
  feeling, scene, or quote the user didn't give.**
- **Hybrid:** user material is experience-truth (not graded A/B/C/D); the science AI adds
  is fully graded and cited; the two stay visibly distinct on the page.
- A user's account is treated as true-as-experienced — rendered faithfully, not
  "corrected" or smoothed into a tidier story.

## Storage

Save intake answers and every round's responses to `manuscript/intake.md` (and/or
`runs/<id>/`). This is the experience-equivalent of the source library: the draft station
reads it as a first-class source, and the inspector uses it to confirm every
personal/experiential claim traces to the user's material rather than to invention.

```
# Intake — <project/task>
## <theme / chapter>
- Q: <question asked>
- A: <user's answer, verbatim or faithfully summarized>
- [GAP] <question the user skipped or couldn't answer — what's missing>
```

## A skip is a declared gap

If the user can't or won't answer something load-bearing, record it as a **gap** (mirror
research's DECLARED GAP) — keep the section leaner, or note the limit — **never invent
around it**. An invented anecdote is the fiction-free equivalent of a fabricated source:
the inspector treats it as a REJECT.
