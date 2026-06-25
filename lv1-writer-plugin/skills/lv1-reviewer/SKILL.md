---
name: lv1-reviewer
description: Independently review an existing document — generate a document-specific checklist, then review against it and write review-feedback.md. Use for an already-written document, not for producing new prose. Trigger with "review this doc", "fact-check this document", "critique this draft", or "lv1-reviewer".
---

# lv1-reviewer

Independently review a document that already exists (it need not have come through the
writing pipeline). Delegate to the **lv1-inspect** subagent in **standalone review mode**.

Target resolution:
- If the user named a file or folder, review that.
- If no target is given, default to `manuscript/` — ask which document if it's ambiguous.

**Before delegating**, read these two files from the plugin path and inject their contents
into the subagent's task prompt — lv1-inspect runs from the project folder and cannot
reach the plugin path on its own:

1. `assets/ai-review-checklist-template.md` — the inspector uses this directly from the
   prompt; it must not try to read it from disk.
2. `../../core/working-lessons.md` — the governing discipline for the inspection run.

`lv1-inspect` must, in this order:

1. **Generate the checklist first.** The review checklist template content is in your
   task prompt — the orchestrator read it from the plugin path and injected it (that file
   is not on your working path). Use it directly from your task prompt. Do a cold read
   (Pass 1) of the target document, classify its type / core promise / key contracts, and
   write a concrete, document-specific `review-checklist.md` beside the document —
   filling Pass 3 (3A structure / 3B integrity / 3C consistency) and deleting the
   template's generic examples that don't apply.
2. **Review with that checklist.** Run Pass 1 + Pass 2 + the mechanics checks against
   `review-checklist.md`, opening any source/reference files the document relies on, and
   spot-check the load-bearing claims against primary sources.
3. **Write `review-feedback.md`** beside the document, in the template's exact output
   format: one VERDICT (PASS / FIX-IT / REJECT), document type + core promise, then every
   Critical/Major finding ranked worst-first with exact location, issue, type (Factual
   error / Quality gap), and a concrete fix.

The inspector is **read-only** — it writes only `review-checklist.md` and
`review-feedback.md`; it must never edit the document it is judging.