# AI Document Review — Universal Checklist Template

> **How to use this template**
> Give this file to an AI reviewer alongside the target document (and its source/reference
> files, if any). The AI reads the document cold in Pass 1, derives the document-specific
> contracts in the middle section, then verifies claims and mechanics in Passes 2–3.
> Every finding must name the exact location and a concrete fix. No vague findings allowed.

---

## Before you begin — reviewer obligations

- [ ] Read the document **fresh, in full**, before consulting any sources or prior review notes.
- [ ] Judge every claim by what the document actually says — not what it was *meant* to say.
- [ ] Every finding states: **exact location** (section, paragraph, sentence) · **what is wrong** ·
      **concrete fix** · **severity**.
- [ ] Separate findings into two labeled types:
  - **Factual error** — "this is wrong / unsupported; here is the correct source."
  - **Quality gap** — "this is not wrong, but would be stronger if…"
- [ ] Rank all findings. Lead with the worst.

**Severity scale**
| Level | Meaning |
|---|---|
| **Critical** | Factually wrong · unsupported · fabricated · logically invalid · unsafe · ethically compromised |
| **Major** | Weakens the core argument or purpose; misleads the reader |
| **Minor** | Style, clarity, navigation, formatting |

**Final verdict (choose exactly one)**
| Verdict | Condition |
|---|---|
| **PASS** | No Critical or Major findings; only cosmetic issues remain |
| **FIX-IT** | One or more Major findings; list every one; re-review from scratch after fixes |
| **REJECT** | Any Critical finding — fabricated source, fabricated data, invalid logic presented as proof, unsafe content — stop and escalate immediately |

---

## Pass 1 — Cold read (document only; no sources yet)

The goal of Pass 1 is to evaluate the document purely as a reader experiences it:
structure, argument, voice, and clarity — before checking whether any claim is actually true.

### 1A · Purpose and scope

- [ ] The document's **purpose is stated or immediately apparent** from the opening.
- [ ] The document **does what it claims** to do in its title, abstract, introduction, or stated
      scope. If it promises a tutorial, it teaches. If it promises a design, it specifies.
- [ ] **Scope is bounded** — the document does not silently expand or contract its remit
      mid-way without acknowledgment.
- [ ] The **intended audience** is clear (or correctly assumed), and the level of assumed
      knowledge is consistent throughout.

### 1B · Structure and logic

- [ ] There is a **single coherent through-line** — one organizing argument, question, or
      problem — that every section serves.
- [ ] **Each section earns its place**: it introduces something new *or* resolves something
      opened earlier. No section merely repeats a prior one in different words.
- [ ] **Dependencies are in order**: the reader is never required to know something before
      it is introduced. (Especially critical for tutorials, textbooks, and design docs.)
- [ ] **Transitions are load-bearing**: the join between sections explains *why* the next
      section follows — it is not just a heading change.
- [ ] **Nothing is hand-waved**: where the argument must make a conceptual leap, that leap
      is named and argued for, not glossed.
- [ ] The **conclusion or closing section** follows from the body — it does not introduce
      new claims, and it does not overstate what the body established.

### 1C · Engagement and accessibility

> *These items scale by document type. Apply at the level appropriate to the form:
> narrative pull for a trade book or case study; clarity and parsability for a guideline
> or spec; intellectual motivation for a scientific paper.*

- [ ] The **opening earns the reader's attention**: it presents a question, tension, gap,
      or problem — not a definition or table of contents summary.
- [ ] **Concrete before abstract**: the document grounds each idea in a specific example,
      case, datum, or scenario before stating the general principle.
- [ ] **The reader is never lost**: at every point, a competent member of the target
      audience knows where they are in the argument and why.
- [ ] **Pacing is appropriate**: the document does not dwell on the obvious or rush past
      what is hard. Mark every spot that drags, repeats, or rushes.
- [ ] **Section endings are deliberate**: each section closes on its strongest point or
      opens the next question — it does not trail off.

### 1D · Voice and register

- [ ] **One consistent voice** throughout — the register does not shift from authoritative
      to casual to sales-y without reason.
- [ ] The **tone is appropriate** to the document's purpose and audience. (Clinical
      guideline: neutral, precise. Trade book: warm, direct. Research paper: measured.)
- [ ] **No register contamination**: the document does not lecture where it should inform,
      sell where it should explain, or hedge where it should commit.
- [ ] **No scaffolding visible to the reader**: no author notes, file paths, placeholder
      text, draft comments, or internal labels left in.
- [ ] **Claims are calibrated**: words like "always," "never," "universally," "proven,"
      "the best" are used only where the evidence fully supports them. Flag every instance
      that seems overstated — mark for verification in Pass 2.

---

## Pass 2 — Claim verification (with source/reference material open)

> Open every reference, source file, cited paper, or standard the document relies on.
> The question is not "does a source exist" but "does this source actually establish
> this specific claim as stated."

### 2A · Citation integrity

- [ ] **Every factual claim has a traceable source** — a citation, a data reference, a
      specification number, or an acknowledged derivation.
- [ ] Each cited source **actually establishes the specific claim** it is cited for.
      A source is not valid because it is topically related.
- [ ] **Scope is not blurred**: a finding from a subgroup is not stated as a population
      finding; a single case study is not stated as general evidence; a lab result is not
      stated as a clinical outcome.
- [ ] **Magnitudes are preserved**: any number, rate, percentage, or statistical qualifier
      in the text matches what the source actually reports — no inflation, no rounding
      that changes meaning, no converting a range into a point estimate without saying so.
- [ ] **Quotes are exact**: every quoted passage matches the source word for word,
      including ellipsis and brackets used honestly.
- [ ] **Sources are real and correctly attributed**: author, year, venue, and title are
      accurate for every entry in the reference list.
- [ ] **Every in-text citation appears in the reference list** and vice versa — no orphans
      in either direction.

### 2B · Honesty and representation

- [ ] **Interpretation is labeled as interpretation** — it is not presented as an
      established finding unless it is one.
- [ ] **Contested or debated findings are flagged as such** at the point of use —
      not relegated to a footnote and not omitted.
- [ ] **The strongest counter-argument or alternative explanation** is stated and
      engaged with — not ignored or strawmanned.
- [ ] **Confidence is graded honestly**: language like *suggests*, *is consistent with*,
      *is well-evidenced*, and *is replicated* is used at the level the evidence warrants.
      The verb must match the strength of the support.
- [ ] **No invented detail**: all names, figures, quotes, outcomes, and scenarios are
      either documented in sources or explicitly marked as illustrative/hypothetical.
- [ ] **Caveats are sized to the claim**: material caveats on load-bearing claims get
      explicit treatment; incidental caveats are woven inline. No caveat is omitted;
      none is a generic boilerplate hedge that says nothing specific.

### 2C · Logical validity

- [ ] Every **causal claim** is either supported by causal evidence (RCT, mechanism,
      established theory) or explicitly qualified as correlational / associative.
- [ ] Every **analogy or borrowed mechanism** (cross-domain, cross-population,
      cross-species) is labeled as an analogy and argued for — not presented as
      a direct measurement.
- [ ] **Inferences are valid**: conclusions drawn from data do not exceed what the
      data can support given the study design, sample, or method used.
- [ ] **No circular reasoning**: the conclusion does not appear as a premise.
- [ ] **No false dichotomy**: where two options are presented as exhaustive, confirm
      that no obvious third option is excluded.

---

## Pass 3 — Document-type contracts

> **This section is generated by the AI reviewer after Pass 1.**
> On completing the cold read, the reviewer identifies what type of document this is
> and what implicit or explicit promises it makes to its reader. Those promises become
> binding checklist items below. The reviewer must populate this section before
> proceeding — generic items that do not apply to this document must be removed.

**Document classification**
The reviewer records the document type and the contracts it carries:

```
Document type   : [e.g. scientific paper / novel / technical spec / clinical guideline / 
                   system design / textbook chapter / case study / policy document / other]
Core promise    : [What this document commits to deliver — in one sentence]
Intended reader : [Who this is for and what they already know]
Key contracts   : [List the 3–6 specific promises this document makes that a reviewer 
                   must hold it to — derived from the document itself, not from this template]
```

### 3A · Structure contracts (derived from document type)

These items are written by the AI reviewer based on the classification above.
Examples by type — **replace with document-specific items**:

*Scientific paper:* Methods are reproducible; statistical choices are justified; null
results are reported; Figure/Table captions are self-contained.

*Novel / narrative:* Each character's actions follow from established motivation; the
ending is earned by prior setup; point-of-view is consistent within scene.

*Textbook:* Learning objectives are stated and met; each chapter builds on the prior;
worked examples match the difficulty of the exercises.

*Clinical guideline:* Every recommendation is graded by evidence level; contraindications
are listed where relevant; the scope of population is explicit.

*System design document:* Every component has a stated owner; failure modes are addressed;
interface contracts are specified; scalability assumptions are named.

*High-level architecture / strategy doc:* Trade-offs between alternatives are stated
explicitly; non-goals are named; open questions are listed and owned.

> The reviewer writes the actual items here, not the examples above.

- [ ] *(AI reviewer writes item 1 — specific to this document)*
- [ ] *(AI reviewer writes item 2)*
- [ ] *(AI reviewer writes item 3)*
- [ ] *(Add as many as the document's contracts require)*

### 3B · Integrity contracts (what this document must not do)

Again, derived from the document. Common forms:

- [ ] *(AI reviewer writes: e.g. "No patient data is identifiable" / "No undisclosed
      conflict of interest" / "No regulatory claim without cited standard" / 
      "No component dependency omitted from the diagram")*

### 3C · Consistency contracts (things the document promises to hold constant)

- [ ] *(AI reviewer writes: e.g. "Terminology is consistent throughout — the same
      concept is not called by two different names" / "The narrative POV does not
      shift without purpose" / "Version numbers are consistent across all sections")*

---

## Mechanics and navigation

- [ ] **Table of contents** (if present) matches actual headings exactly — no stale title.
- [ ] **Section/chapter numbering** is contiguous and consistent.
- [ ] **Cross-references** (e.g. "see Section 4.2," "as shown in Figure 3") resolve
      correctly — the target exists and contains what is promised.
- [ ] **Figures, tables, and diagrams** are numbered, captioned, and cited in the text.
      Every figure referenced in the text exists; no figure exists without a text reference.
- [ ] **Definitions** — technical terms are defined at or before first use and used
      consistently thereafter.
- [ ] **Formatting consistency** — heading hierarchy, list style, and emphasis conventions
      are applied consistently throughout.
- [ ] **No author-only artifacts** visible to the reader: no file paths, `TODO` comments,
      template placeholder text, draft annotations, or version-control markers.
- [ ] **Document metadata** (title, author, date, version) is accurate and present where
      the format requires it.

---

## Closing the review

- [ ] **Section 3 (document-type contracts) is fully populated** before closing — if it
      is still in template form, the review is incomplete.
- [ ] **One verdict recorded**: PASS · FIX-IT · REJECT — with justification.
- [ ] **Every Critical and Major finding** is listed with: exact location · what is wrong
      · concrete fix · severity label.
- [ ] **If FIX-IT**: after fixes are applied, re-review from scratch using a clean copy
      of this template. Do not re-use the prior pass's tick-marks.
- [ ] **If REJECT**: do not proceed to suggest fixes. State the Critical finding and
      stop. Escalate to the document owner.
- [ ] **Load-bearing claims** — the two or three claims the entire document rests on —
      are spot-checked against primary sources directly, not just the document's own
      summary of them.

---

## Output format for the AI reviewer

When submitting a review, structure the output as follows:

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
[list the document-specific contracts that were verified, and their status]

NOTES FOR RE-REVIEW (FIX-IT only):
[what to pay special attention to in the next pass]
```