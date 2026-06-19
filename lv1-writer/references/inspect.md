# Inspect — the check that makes the bar real

This is the most important station. An inspection that's really just the writer
glancing back at its own work is theatre, not a check. You don't have a separate
isolated context to do this in here, so the discipline has to come from you
deliberately, not from process isolation — and it's still worth doing properly,
because most factual drift is caught by someone actually re-reading the source, not
by remembering what they meant when they wrote it.

## How to actually be independent without a separate context

- **Re-read the real files from disk** — open `manuscript/` (or `03-draft.md`) and
  `sources/library.md` fresh, as if you'd never seen them, rather than relying on what
  you remember deciding while drafting.
- **Don't consult your own drafting reasoning** as evidence. The question is "does the
  cited source actually say this," not "did I mean for this to be true."
- You may read `02-outline.md` to check the draft against its plan, but the verdict
  comes from the draft and the library, not from the outline's intentions.

## Check four things

1. **Facts** — is every factual claim actually supported by the cited library source?
   Spot-check the strongest way available: re-read the source, re-run the arithmetic.
2. **Honesty** — is interpretation labeled as interpretation, not dressed as fact? Any
   D-grade guess that leaked in? Any number without its working? Is the strongest
   counter-argument addressed where the piece takes a side?
3. **Plan & scope** — was the outline followed; if it changed, was the change
   announced?
4. **Craft** — coherence, voice, and completeness against the agreed scope; any thin,
   corner-cutting section.

## Verdict — exactly one

- **PASS** — only tiny, cosmetic issues remain. List them; the work may ship.
- **FIX-IT** — a real problem. List every issue precisely (exact claim, section, or
  missing source — vague findings are useless to whoever fixes it). Work returns to
  draft and is re-inspected from scratch once changes are made.
- **REJECT** — a serious failure (fabricated sources or quotes, faked steps, scope
  abandoned, unsafe content). Stop the line; escalate to the user honestly.

Write `runs/<id>/05-inspection.md` with the findings and the one verdict.
