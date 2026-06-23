---
name: lv1-writer-review
description: Independently review an existing document the disciplined lv1-writer way — generate a document-specific checklist from the review template, then review the document against it and write review-feedback.md. Use for an already-written document, not for producing new prose. Trigger with "lv1-writer review this doc", "review [file] with lv1-writer", "fact-check this document", or "critique this draft".
---

# lv1-writer — Review an existing document

Independently review a document that already exists (it need not have come through the
writing pipeline). Delegate to the **lv1-inspect** subagent in **standalone review mode**.

Target resolution:
- If the user named a file or folder, review that.
- If no target is given, default to `manuscript/` — ask which document if it's ambiguous.

`lv1-inspect` must, in this order:

1. **Generate the checklist first.** Read
   `skills/lv1-writer/assets/ai-review-checklist-template.md`. Do a cold read (Pass 1) of
   the target document, classify its type / core promise / key contracts, and write a
   concrete, document-specific `review-checklist.md` beside the document — filling Pass 3
   (3A structure / 3B integrity / 3C consistency) and deleting the template's generic
   examples that don't apply.
2. **Review with that checklist.** Run Pass 1 + Pass 2 + the mechanics checks against
   `review-checklist.md`, opening any source/reference files the document relies on, and
   spot-check the load-bearing claims against primary sources.
3. **Write `review-feedback.md`** beside the document, in the template's exact output
   format: one VERDICT (PASS / FIX-IT / REJECT), document type + core promise, then every
   Critical/Major finding ranked worst-first with exact location, issue, type (Factual
   error / Quality gap), and a concrete fix.

The inspector is **read-only** — it writes only `review-checklist.md` and
`review-feedback.md`; it must never edit the document it is judging.