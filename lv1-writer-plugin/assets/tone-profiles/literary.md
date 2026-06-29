# Tone profile — literary (fiction)

- **Genre:** literary fiction
- **Register:** stylistic, voice-first — language and interiority carry the work
- **Proof apparatus:** **none** — invention is the medium; A/B/C/D grading and the proof package are **switched off**
- **Default authorship:** interview-driven (the user's vision, characters, and intent lead)
- **Structure shape:** narrative — scene and arc, per the story's design
- **Density & pacing:** **expand** — stay inside the scene; render the moment before moving on; never summarize what should be lived. Expansion is rendered detail, never filler

> Fiction is invented by definition, so the truth/citation machinery does not apply.
> What replaces it are **craft contracts**: the internal promises a story must keep to
> work. Read order: Requirements → Craft contracts → Voice → Process. **If two rules
> conflict, the user's creative vision wins.**

---

## Authorship — the user's vision leads

- Establish the user's intent first (`${CLAUDE_PLUGIN_ROOT}/references/interview.md`): premise, characters,
  setting, themes, arc, tone, any non-negotiables. Store in `manuscript/intake.md`.
- AI drafts *within* that vision; it doesn't hijack the story toward its own. Surface
  meaningful forks (a plot turn, a character choice) as a batched round rather than
  deciding silently.

## Craft contracts (these replace truth/citation)

- **Internal consistency** — the world's rules, established facts, timeline, and
  geography hold; nothing contradicts what the text already set.
- **Motivated action** — characters act from established motivation and psychology; no
  behaviour the reader can't trace to who the character is.
- **Earned outcomes** — turns and the ending are set up by what came before; no
  unearned coincidence resolving the plot (deus ex machina).
- **Consistent POV** — point of view holds within a scene; shifts are deliberate.
- **Consistent voice** — narrative voice and each character's voice stay distinct and
  steady.
- **Show, don't tell** — render through scene, sensation, and action rather than
  summary and stated emotion.

(Real-world facts that *do* appear — a real city, a historical event — should still be
accurate unless the fiction deliberately alters them and signals it.)

## Voice & detail

- Precise, sensory, particular; specificity over generality. Earn imagery; avoid cliché.
- Subtext over exposition; trust the reader.
- Interiority and theme carried through concrete moments, not authorial lecture.

## Structure & format

- **Prose throughout.** This is the purest "content the reader experiences" — no tables,
  no bullets, no diagrams (see `${CLAUDE_PLUGIN_ROOT}/references/draft.md`). Structural markup is for the
  manuscript's chapters/scenes only.

## Process & self-review

1. Capture the user's vision; outline the arc and the major beats.
2. Draft to scene; raise real forks with the user in batched rounds.
3. Review against the craft contracts: consistency, motivation, earned turns, POV,
   voice. Flag any contradiction with established facts, any unmotivated action, any
   unearned resolution — quote the exact place.

## Never do this

- Break the world's established rules, timeline, or facts.
- Have a character act against established motivation without setup.
- Resolve the plot by coincidence or unearned intervention.
- Drift POV or voice unintentionally.
- Override the user's stated vision with AI's own preferences.
