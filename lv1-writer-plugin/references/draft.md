# Draft — Assembly

Write the real prose, at full effort, following the outline. Save it to `manuscript/`
and a copy to `runs/<id>/03-draft.md`.

## Work one chapter at a time (prefer rule)

**The unit of work is a single chapter file, not the whole book.** Draft, revise, and
hand to inspection **one `manuscript/<chapter>.md` at a time** (e.g. `ch01.md`,
`ch02.md`). Load only the chapter(s) the current task touches plus `02-outline.md` for
where they sit — do **not** read the entire manuscript into context just to write or fix
one chapter. This keeps the working set small, the prose focused, and inspection precise.
Read another chapter only when the task genuinely needs cross-chapter continuity (a
callback, a timeline, a running argument), and then read just the spans you need. Operate
on the whole book at once only when the task is explicitly book-wide (a full-manuscript
consistency pass, a global rename).

## Tone — compose the voice (in precedence order)

Read `00-triage.md` for the chosen tone profile and authorship approach, then compose
the voice from these layers, highest precedence first:

1. **Explicit user instruction** for this task always wins.
2. **The tone profile** named in `00-triage.md` — read `${CLAUDE_PLUGIN_ROOT}/assets/tone-profiles/<id>.md`
   and follow its Spine / Truth-or-fiction contract / Detail / Tone / Structure /
   Never-do. This is the genre's voice and shape.
3. **Project overrides** in `inputs/writing-instruction.md`, layered on top, if it
   exists (the user's project-wide tone edits).

If no profile is set and no `writing-instruction.md` exists, write in clear, accurate,
plain prose. Also read `${CLAUDE_PLUGIN_ROOT}/discipline/working-lessons.md` and `${CLAUDE_PLUGIN_ROOT}/references/anti-tells.md`
before drafting.

**Load only the assigned profile.** Read the single tone profile named in `00-triage.md`
— do not re-load `tone-detect.md` or the other profiles into the drafting context. The
genre is already decided; the rest of the taxonomy is noise that encourages register
bleed (an analytical tic creeping into a memoir, or vice versa).

**Write out of the AI register.** `${CLAUDE_PLUGIN_ROOT}/references/anti-tells.md` lists the words and
constructions that mark prose as machine-written (generic-AI filler, therapy-speak, hype,
empty pivots). Avoid them as reflexive filler in every genre; for warm/intimate profiles
(love, personal-experience, mindset, cognitive-behavior) a slide into therapy-speak is an
automatic FIX-IT at inspection.

The profile is a style + structure layer, not a replacement for the honesty rules
below. **The profile also decides whether those rules apply at all:** a fiction profile
(literary, romance, childrens) switches A/B/C/D grading and the proof package **off** and
runs its craft contracts instead; non-fiction and hybrid profiles keep grading on. Where
a profile's own grading ladder ("echo / consistent with / well-evidenced / replicated")
and the project's A/B/C/D grades seem to pull apart, A/B/C/D decides whether a claim
ships at all; the ladder only governs which verb phrases a claim that already cleared
that bar.

## Apparatus by rigor level (non-fiction & hybrid)

Match how much proof machinery the prose carries to the rigor tier set in triage:

- **full / high-stakes** — open with a **Methodology & enforcement** section (recency
  window, source spine, enforcement rule, red-line exclusions, from the triage research
  contract); carry **inline A/B grades** on every factual claim in the body; **surface
  every DECLARED GAP** from `01-research.md` in-text where the number would go, phrased
  as an explicit refusal to invent; and **append the proof package to the delivered
  file itself** (confidence table, assumptions, source index with in-window flags,
  data-limits) — not only to `runs/<id>/05-manifest.md`.
- **normal / medium** — grades in a closing notes/appendix, inline only on load-bearing
  claims; methodology compressed to a one-paragraph "About the sources" note.
- **tiny** — grades available in `sources/library.md`; keep the prose clean.
- **fiction** — none of the above; follow the profile's craft contracts.

For **hybrid** (cognitive-behavior, mindset): grade and cite the science; treat the
user's material from `inputs/intake.md` as experience-truth (ungraded); keep the two
visibly distinct in the prose.

## Format follows content

Before writing each section, pick its presentation form from the *content type*, not a
fixed template:

- **Table** — multiple data points, stats, or numbers that invite comparison.
- **Bullets** — a list of discrete ideas or key points (or, in mindset/how-to, steps
  the reader will *do*).
- **Dashes / arrows** (`A → B → C`) — a loop, sequence, pipeline, or cause-effect chain.
- **Prose** — content the reader should *experience* (a person, an unfolding process, a
  reflection, an argument).

This is a deliberate decision point ("what shape does this content want?"), not a
license to over-format. Respect the genre: fiction and narrative/experience profiles
default to prose and use markup sparingly; analytical and technical profiles use tables,
diagrams, and arrows freely. The inspector flags both under-formatting (a wall of prose
hiding a comparison that wants a table) and over-formatting (bullets where the reader
needed a narrative).

**Honor the profile's density posture** (the `Density & pacing` line in its header):
- **compress** (technical, analytical-intelligence, academic) — shortest path to the
  claim; lead with tables/lists; no narrative throat-clearing.
- **expand** (literary, romance, love, personal-experience) — stay inside the scene;
  render the beat before moving on; don't summarize what should be lived. *Expansion is
  rendered detail, never filler — never pad to hit a length.*
- **balanced** (the rest) — length follows the idea.

There is **no minimum word count per section** — a quota only produces padding (the exact
filler `anti-tells.md` targets). The inspector flags a *rushed* expansive scene and a
*padded* compressive section alike.

## Interview-driven & hybrid drafts

If the authorship approach is interview-driven or hybrid, read `inputs/intake.md`
as a first-class source and draft the lived material from it — **never invent a
personal anecdote, scene, quote, or feeling the user didn't give**. A gap the user
skipped stays a gap (keep the section leaner); don't fill it with invention. See
`${CLAUDE_PLUGIN_ROOT}/references/interview.md`; an invented anecdote is a REJECT at inspection.

## Honesty (non-negotiable — non-fiction & hybrid; fiction follows its craft contracts)

1. **Every factual claim** carries a confidence grade and ties to a source in
   `sources/library.md`. No source, no claim. "D" guesses never ship.
2. **Mark interpretation as interpretation** (rule 6). Your reading, argument, or
   creative choice is honest judgment — never disguise it as established fact.
3. **Show the working** for any numeric or analytical claim, or don't make the
   precise claim.
4. **Don't change scope quietly.** If new material breaks the outline, stop, note it,
   and flag for re-plan.

## Self-review (the best-self pass) before handing off to inspection

Check: clarity (anything muddy or over-hedged?); specificity where it matters; the
whole scope covered; a stronger framing left on the table?; grades and sources real
and consistent?; a genuine, true "wow" the reader didn't ask for — or merely adequate?
Fix what you find. This is your last chance to catch something before the inspection
pass, which owes the draft nothing.

## On a FIX-IT send-back

Read `04-inspection.md`, address **every** finding, note exactly what changed, and
don't argue with the inspection findings — their job is to be harder to satisfy than
you are, not easier.

Report a one-line confidence count (e.g. "18 claims: 13xA, 4xB, 1xC