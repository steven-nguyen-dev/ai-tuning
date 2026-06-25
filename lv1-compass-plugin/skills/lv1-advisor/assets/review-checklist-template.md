# Work-Document Review — Checklist Template

> **How to use this template**
> The `lv1-inspect` subagent reads this alongside the target document (and its
> source/reference files, if any). It reads the document cold first, derives the
> document-specific contracts in the middle section, then verifies claims and mechanics.
> Every finding must name the exact location and a concrete fix. No vague findings allowed.

This template is tuned for **work documents** — analyses, reports, memos, briefs, strategy
docs, specs, and decks. It is not for fiction.

---

## Before you begin — reviewer obligations

- [ ] Read the document **fresh, in full**, before consulting any sources or prior notes.
- [ ] Judge every claim by what the document actually says — not what it was *meant* to say.
- [ ] Every finding states: **exact location** (section, paragraph, sentence) · **what is
      wrong** · **concrete fix** · **severity**.
- [ ] Separate findings into two labeled types:
  - **Factual error** — "this is wrong / unsupported; here is the correct source."
  - **Quality gap** — "this is not wrong, but would be stronger if…"
- [ ] Rank all findings. Lead with the worst.

**Severity scale**
| Level | Meaning |
|---|---|
| **Critical** | Factually wrong · unsupported · fabricated · logically invalid · unsafe |
| **Major** | Weakens the core argument or purpose; misleads the reader |
| **Minor** | Style, clarity, navigation, formatting |

**Final verdict (choose exactly one)**
| Verdict | Condition |
|---|---|
| **PASS** | No Critical or Major findings; only cosmetic issues remain |
| **FIX-IT** | One or more Major findings; list every one; re-review from scratch after fixes |
| **REJECT** | Any Critical finding — fabricated source/data, invalid logic presented as proof, unsafe content — stop and escalate |

---

## Pass 1 — Cold read (document only; no sources yet)

Evaluate the document purely as a reader experiences it — structure, argument, voice,
clarity — before checking whether any claim is true.

### 1A · Purpose and scope
- [ ] The **purpose is stated or immediately apparent** from the opening.
- [ ] The document **does what it claims** in its title/intro/stated scope.
- [ ] **Scope is bounded** — it doesn't silently expand or contract mid-way.
- [ ] The **intended reader** is clear, and the assumed knowledge level is consistent.

### 1B · Structure and logic
- [ ] A **single coherent through-line** every section serves.
- [ ] **Each section earns its place** — introduces something new or resolves something open.
- [ ] **Dependencies are in order** — the reader never needs something not yet introduced.
- [ ] **Transitions are load-bearing** — they explain *why* the next section follows.
- [ ] **Nothing is hand-waved** — conceptual leaps are named and argued, not glossed.
- [ ] The **conclusion follows from the body** — no new claims, no overstatement.

### 1C · Engagement and accessibility
- [ ] The **opening earns attention** — a question, tension, gap, or problem, not a TOC summary.
- [ ] **Concrete before abstract** — grounded in a specific example before the general principle.
- [ ] **The reader is never lost** — they always know where they are in the argument and why.
- [ ] **Pacing is appropriate** — doesn't dwell on the obvious or rush the hard part.

### 1D · Voice and register
- [ ] **One consistent voice** — no drift from authoritative to casual to sales-y.
- [ ] The **tone fits** the purpose and reader (analytical: decisive; briefing: neutral, precise).
- [ ] **No register contamination** — doesn't sell where it should explain, or hedge where it
      should commit.
- [ ] **No scaffolding visible** — no file paths, placeholders, draft comments, internal labels.
- [ ] **Claims are calibrated** — "always", "never", "proven", "the best" only where supported.
      Flag every overstatement for Pass 2.

---

## Pass 2 — Claim verification (with source material open)

> Open every reference the document relies on. The question is not "does a source exist" but
> "does this source actually establish this specific claim as stated."

### 2A · Citation integrity
- [ ] **Every factual claim has a traceable source.**
- [ ] Each cited source **actually establishes the specific claim** — topical relation isn't enough.
- [ ] **Scope is not blurred** — a subgroup finding isn't stated as a population finding.
- [ ] **Magnitudes are preserved** — numbers/rates/percentages match what the source reports.
- [ ] **Quotes are exact** — word for word, with honest ellipsis/brackets.
- [ ] **Sources are real and correctly attributed.**

### 2B · Honesty and representation
- [ ] **Interpretation is labeled as interpretation** — not presented as established fact.
- [ ] **Contested findings are flagged at the point of use** — not buried, not omitted.
- [ ] **The strongest counter-argument is stated and engaged** — not ignored or strawmanned.
- [ ] **Confidence is graded honestly** — the verb matches the strength of support.
- [ ] **No invented detail** — every figure, quote, and scenario is documented or marked
      illustrative.
- [ ] **Caveats are sized to the claim** — material caveats on load-bearing claims get explicit
      treatment; no generic boilerplate hedges.

### 2C · Logical validity
- [ ] Every **causal claim** is supported by causal evidence or qualified as correlational.
- [ ] Every **analogy / borrowed mechanism** is labeled and argued, not presented as measurement.
- [ ] **Inferences are valid** — conclusions don't exceed what the data supports.
- [ ] **No circular reasoning**; **no false dichotomy** (confirm no obvious third option excluded).

---

## Pass 3 — Document-type contracts

> **Generated by the reviewer after Pass 1.** On completing the cold read, identify the
> document type and the promises it makes; those become binding checklist items. Populate
> this section before proceeding — remove generic items that don't apply.

**Document classification**
```
Document type   : [e.g. market/strategy analysis / business memo / technical spec /
                   system design / policy doc / readout deck / status brief / other]
Core promise    : [What this document commits to deliver — in one sentence]
Intended reader : [Who this is for and what they already know]
Key contracts   : [The 3–6 specific promises this document makes that a reviewer must hold
                   it to — derived from the document itself, not this template]
```

### 3A · Structure contracts (derived from document type)
Examples by type — **replace with document-specific items**:

*Market / strategy analysis:* a central thesis is stated and defended; trade-offs between
options are explicit; winners/losers (or "what this means") synthesis, not a flat recap;
non-goals named.

*Business memo / brief:* the ask or recommendation is up front; the reasoning is traceable;
the reader can act on it without a follow-up meeting.

*System design document:* every component has a stated owner; failure modes are addressed;
interface contracts are specified; scalability assumptions are named.

*Technical spec:* every requirement is testable; edge cases are enumerated; open questions
are listed and owned.

> The reviewer writes the actual items here, not the examples above.

- [ ] *(item 1 — specific to this document)*
- [ ] *(item 2)*
- [ ] *(add as many as the document's contracts require)*

### 3B · Integrity contracts (what this document must not do)
- [ ] *(e.g. "No figure without a cited source" / "No undisclosed assumption presented as
      fact" / "No component dependency omitted from the diagram")*

### 3C · Consistency contracts (what it promises to hold constant)
- [ ] *(e.g. "Terminology is consistent — the same concept isn't called two names" /
      "Figures reconcile across sections" / "The recency window is applied uniformly")*

---

## Mechanics and navigation
- [ ] **Cross-references** resolve — the target exists and contains what's promised.
- [ ] **Figures/tables** are numbered, captioned, and cited in the text; none orphaned.
- [ ] **Definitions** — technical terms defined at or before first use, used consistently.
- [ ] **Formatting consistency** — heading hierarchy, list style, emphasis applied consistently.
- [ ] **No author-only artifacts** — no file paths, TODOs, placeholders, draft annotations.

---

## Closing the review
- [ ] **Pass 3 is fully populated** — if still in template form, the review is incomplete.
- [ ] **One verdict recorded** — PASS · FIX-IT · REJECT — with justification.
- [ ] **Every Critical and Major finding** listed with: exact location · what's wrong ·
      concrete fix · severity.
- [ ] **If FIX-IT**: after fixes, re-review from scratch with a clean copy — don't reuse ticks.
- [ ] **If REJECT**: state the Critical finding and stop; escalate to the document owner.
- [ ] **Load-bearing claims** — the two or three the document rests on — spot-checked against
      primary sources directly.

---

## Output format

```
VERDICT: [PASS / FIX-IT / REJECT]

DOCUMENT TYPE: [classification from Pass 3]
CORE PROMISE: [one sentence]

FINDINGS (ranked, worst first):

[SEVERITY] Location: [exact section/paragraph/sentence]
Issue: [what is wrong]
Type: [Factual error / Quality gap]
Fix: [concrete action]

[repeat for each finding]

CONTRACTS CHECKED (Pass 3):
[the document-specific contracts verified, and their status]

NOTES FOR RE-REVIEW (FIX-IT only):
[what to pay special attention to in the next pass]
```
