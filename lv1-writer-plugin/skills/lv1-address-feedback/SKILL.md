---
name: lv1-address-feedback
description: Respond to a human review of the manuscript. Reads the reviews in feedback/ (user-review.md, editor-review.md), verifies every point against the real draft and sources before agreeing, grades each, and writes a prioritized improvement plan into the matching *-response.md. It does not rewrite the manuscript — accepted items are handed to lv1-author. Trigger with "address feedback", "respond to the review", "go through the editor's notes", or "lv1-address-feedback".
---

# lv1-address-feedback

The third entry point of lv1-writer. `lv1-author` **writes**, `lv1-reviewer` **generates**
a review, and this skill **responds** to a review a human wrote — verifying each point
against the actual artifact before accepting it, then turning the verified points into an
improvement plan.

**Read first, every run:** `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md` and
`${CLAUDE_PLUGIN_ROOT}/discipline/working-lessons.md`. The bar, the A/B/C/D grades, the `[A]`
provenance test, and the "neutral ≠ timid / the reviewer's position is a signal to
investigate, not a command to obey" lessons govern this skill. It applies them; it does
not restate them.

## What it does and does not do

- **Does:** read the human review(s), break them into discrete points, verify each against
  the real manuscript + sources, grade it, decide **Accept / Partially accept /
  Reject-with-evidence**, and write a prioritized improvement plan into the response file.
- **Does not:** rewrite the manuscript. The deliverable is the response file. Applying the
  accepted changes is a separate, explicit `lv1-author` pass (offer it at the end).

This separation is deliberate: judging feedback and editing prose are different jobs, and
you should not silently change prose the user hasn't approved.

## Step 1 — Find the reviews

Look in `feedback/` for human-written reviews:

- `feedback/user-review.md` → response goes to `feedback/user-review-response.md`
- `feedback/editor-review.md` → response goes to `feedback/editor-review-response.md`

Process **each review that has real content** (ignore an untouched template stub). If the
user named a specific review or a different file, use that — the matching response file is
`<name>-response.md` beside it. If `feedback/` is empty or holds only template stubs, say
so and stop — there is nothing to address. If both reviews exist, process both; you will
reconcile any conflict between them in Step 4.

## Step 2 — Re-read the real artifact (this is what makes the response honest)

Do not respond from memory of the draft or from the reviewer's characterization of it.
Open the actual files fresh from disk, the same way `lv1-inspect` does:

- the prose being reviewed — the specific `manuscript/<chapter>.md` the review targets
  (read only the chapter(s) under review unless a point is cross-chapter; see the chapter
  prefer rule in `references/draft.md`);
- `sources/library.md` — the graded source blocks;
- the latest `runs/<id>/01-research.md` — the claim ledger, if the piece came through the
  pipeline;
- `inputs/intake.md` — for interview-driven / hybrid work, the user's lived material;
- `runs/<id>/00-triage.md` — the genre / tone profile and rigor tier, so you judge each
  point against the contract the piece was actually written to (don't fault a romance for
  missing citations, or excuse a high-stakes report that lacks them).

## Step 3 — Verify each point, then classify it

Split each review into discrete, individually-addressable points. For every point:

1. **Locate** what in the draft it refers to (exact section / paragraph / sentence). If
   you can't locate it, say so rather than guessing what the reviewer meant.
2. **Verify** against the real artifact and the source hierarchy, in order:
   the manuscript itself → `sources/library.md` + the research ledger → external
   authoritative sources via search **only if the point asserts an external fact not in
   the library** → otherwise state plainly that no source was found. Grade any factual
   assertion (the reviewer's or your own) **A/B/C/D**; a claim is `[A]` only if you can
   point to the source text in this session. Never invent a source to support or rebut a
   point.
3. **Classify** the point:
   - **Accept** — the point is correct and supported; it should change the draft.
   - **Partially accept** — part holds; name precisely which part and which part doesn't.
   - **Reject-with-evidence** — the point is wrong or unsupported. Say so plainly and cite
     the basis (the source block, the arithmetic, the triage contract). A confident
     reviewer who is factually wrong gets a sourced rebuttal, not agreement. **Pushback
     does not override evidence** — and neither does a reviewer's tone or seniority.
   - **Out of scope / judgment call** — a matter of taste or a change that contradicts the
     recorded tone/scope. Surface it, give your read, and leave the call to the user.

Matters of genuine taste are not "wrong" — say "this is a judgment call" rather than
manufacturing a verdict.

## Step 4 — Reconcile conflicts (when both reviews exist)

If the user review and the editor review disagree on the same point, **do not silently
pick one.** State both, give your verified read of which the evidence supports, and flag
the conflict for the user to resolve. Note it in both response files.

## Step 5 — Write the response file

Write `<name>-response.md` in `feedback/`, following
`${CLAUDE_PLUGIN_ROOT}/assets/feedback-templates/review-response-template.md`. It contains:

1. A one-line **summary verdict** (e.g. "9 points: 5 accepted, 2 partial, 2
   rejected-with-evidence").
2. A **prioritized improvement plan** — the accepted / partially-accepted items as
   concrete, located actions, worst/most-important first, each pointing at the exact
   `manuscript/<chapter>.md` span to change.
3. A **per-point ledger** — for every review point: the point quoted, its location, the
   verification (what the real artifact/source says, with grade), the verdict, and the
   action or the sourced rebuttal.
4. Any **conflicts** between reviews, left open for the user.

Be precise and terse — a vague response is useless to whoever applies it. Do not edit the
manuscript.

## Step 6 — Hand off

Tell the user the verdict summary and offer to run `lv1-author` to implement the accepted
items (that pass rewrites the named chapters, re-inspects, and logs its decisions). If the
piece came through a pipeline run, you may also drop a copy of the response under
`runs/<id>/` for the record.
