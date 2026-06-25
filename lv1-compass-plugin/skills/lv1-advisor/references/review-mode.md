# Review mode — verdict on an existing artifact

The user hands over a document (a memo, report, analysis, plan, deck, or their own draft)
and wants it judged. Output is a **verdict + located findings + concrete fixes** — never an
edit of their artifact.

## How it runs

Delegate to the **`lv1-inspect` subagent in standalone review mode**. Its full contract —
generate the document-specific checklist, cold-read, verify against sources, write the
located `review-feedback.md` (PASS / FIX-IT / REJECT) — lives in `agents/lv1-inspect.md`.
**Do not restate that procedure here**; this file owns only the parent-side concerns below.

When you delegate, carry into the subagent's task prompt: the path to the artifact, the
constitution + working-lessons content, and the **contents of
`assets/review-checklist-template.md`** (you can read it from this skill; the subagent
cannot reach it from its own working path). The inspector is **read-only** — it judges, it
never edits the document.

## What you do with the result

Relay the verdict and the ranked findings plainly. If the user wants the findings *fixed*,
that's a make task — route the fix to `lv1-assistant` (or do it only if they explicitly ask
you to edit), keeping the independence of this review intact: the thing that judged it
shouldn't also quietly rewrite it and re-bless its own work.

If the artifact relies on sources you don't have and facts can't be checked, say so — a
declared limit, not a silent pass. If a load-bearing claim needs fresh facts to verify,
delegate that to `lv1-research` before landing the verdict.
