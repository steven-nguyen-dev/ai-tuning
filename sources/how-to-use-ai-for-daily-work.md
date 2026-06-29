# How to Use AI for Your Daily Work

A practical guideline for knowledge workers, distilled from five reference manuals
(*Dùng AI như một Tool*, *The AI Operator*, *Đồng nghiệp số*, *Building Effective AI
Agents*, *The Complete Guide to Building Skills for Claude*) and cross-checked against
current official guidance from Anthropic, Google (Gemini), and Cursor.

It is organized in three layers:

1. **The mindset** — what AI actually is in 2026.
2. **The disciplines** — twelve principles that separate people who get production-grade
   output from people who get "generic paragraphs." These are the principles the
   companion plugin review checks against.
3. **The workflow** — how to put the disciplines together, plus the non-negotiable safety floor.

> Scope: this is a coaching document for using AI on real work. It is not legal, financial,
> medical, or other professional advice. You remain accountable for everything you ship.

---

## Part 1 — The mindset shift

Most people use AI as a *smarter search box*: ask a question, read a paragraph, leave. A
smaller group uses it as a *tool that produces a finished deliverable*, and a smaller group
still delegates whole *roles* to it.

| Level | You say | You get back |
|---|---|---|
| **Q&A** | "What is a balance sheet?" | Text to read |
| **Tool** | "Turn this tracker into a 1-page board summary; output Markdown." | An artifact to use |
| **Colleague / agent** | "Each morning, triage my inbox and draft replies for my review." | A standing job done |

The same model sits behind all three. The difference is not the model, the plan, or your
IQ — it is **how clearly you assign the work and how you verify it.** Everything below is
about moving rightward on that table safely.

A second shift matters as models gain agency: AI can now read your folders, run code, open
your apps, and act over many steps. That raises the payoff *and* the risk. The professional
posture is not blind trust — it is **delegate clearly, then verify with evidence.**

---

## Part 2 — The twelve disciplines

These are the load-bearing principles. The numbering is referenced by the plugin review.

### P1 · Treat AI as a tool/colleague that produces a deliverable, not a chat box
Before opening AI, ask: *do I want an answer, or do I want a finished thing?* If it's a
thing, name the thing. Reach for "tool mode" whenever you need a concrete deliverable,
whenever the task repeats, or whenever you must process many files at once.

### P2 · The command is the spec — clarity beats length
A good prompt is a *clear specification*, not a long one. Anthropic's golden rule: *give
the prompt to a colleague with minimal context; if they're confused, the model will be
too.* Two practical frames converge here:

- **Gemini's PTCF** — Persona, Task, Context, Format. (You are X; do Y; here is the
  background and audience; return it as Z.)
- **The "prompt contract"** for higher-stakes work — Role · Success criteria · Constraints
  · Uncertainty rule · Input · Output format · Verification step.

Match prompt length to task complexity. "What's ARR?" needs one line. A compliance review
needs the full contract. When you can't write the contract yourself, ask the AI to write it
for you (meta-prompting) — this is the single highest-leverage skill for non-experts.

### P3 · Feed real material, don't describe it
Every artifact you attach — the file, the screenshot, the PDF, the URL, the transcript — is
a *truth anchor*. The more real input the model sees, the less it has to guess. Don't
paraphrase a spreadsheet; attach it. Don't describe the broken layout; screenshot it.

### P4 · Define "done" before you start
State the acceptance criteria up front: format, length, audience, what every number must
tie to, where the file lands. For delegated/agentic work, give the **goal and the
done-criteria, not the click-by-click steps** — a capable colleague needs the destination
and the test for arrival, not the steering wheel.

### P5 · Source every factual claim; grade confidence; show the working
No source, no claim. Label how sure you are (e.g. proven / reasoned / from-memory / guess).
A number is not a claim until its arithmetic is shown — a sourced figure can still hide an
error. Keep interpretation labeled as interpretation, never dressed up as established fact.

### P6 · Make uncertainty explicit — flag it, never fabricate
**Plausibility ≠ truth.** Models produce fluent, confident, wrong output (fabricated
citations, mis-read numbers, invented APIs). Instruct the AI to mark anything it is unsure
of with `[?]` or "NOT FOUND" rather than guessing. An AI that says "I don't know" is worth
more than one that is smoothly wrong.

### P7 · Verify with evidence — never accept "done" on faith
Two checks, escalating with stakes:

- **Self-review** before hand-off (the "best-self" pass).
- **An independent check** that re-reads the *actual artifact* against the *actual
  sources* — not a summary of them, and not colored by the maker's reasoning.

Demand a trace: which files changed, a change log, a source for each number, and the `[?]`
list. **No trace = treat it as not done.** For irreversible, high-value numeric work,
have a human and the AI work the problem in parallel and reconcile the differences.

### P8 · Iterate in 2–3 beats; don't one-shot complex tasks
Power lives in short loops, not one perfect mega-prompt. Go broad first ("show me what's
there"), then narrow ("now do exactly this"). The most effective builders iterate on a
single hard case until it works, then generalize.

### P9 · Match the tool — and the complexity — to the job
Start with the simplest thing that works; add complexity only when it earns its keep.
The escalation ladder: **web chat → a saved assistant (Custom GPT / Project) → a desktop
agent (Cowork/Codex) → a multi-agent pipeline.** Likewise for architecture: a single agent
with the right skills beats a multi-agent system for most tasks; reach for parallel agents
only when the problem is genuinely large or multi-domain (where Anthropic measured ~90%
gains). Use the **smallest team that does the job well** — extra agents for show is a
failure mode, not sophistication. Pick the model per task: don't run a premium model on a
form-extraction job.

### P10 · Systematize anything you do more than twice
When you repeat a prompt three times in a week, stop typing it. Capture it as a reusable
asset — a saved prompt, a Custom GPT / Project, a **skill**, or a multi-step **playbook**
with the inputs factored out. The leverage of a good workflow is that it repeats without
quality drift, and a teammate can run it too.

### P11 · Design modularly, disclose progressively
Build reusable, composable pieces rather than monoliths. For skills/agents: keep the
always-loaded layer (name + when-to-use) tight, push detail into reference files loaded on
demand, and let pieces compose. Centralize shared rules in one place so they can't drift
between uses. Cursor's rule files and Anthropic's skill structure follow the same shape:
concise, focused, single-sourced, versioned, kept current.

### P12 · Keep the human in the loop — you own consequential decisions
Pick a human-in-the-loop pattern per task:

- **A — Human-first → AI critiques** (judgment-heavy, high-consequence work)
- **B — AI-first → human reviews** (routine, high-volume work; you must *actually read* it)
- **C — Parallel → compare** (important formula/calculation work)
- **D — Mixed with approval gates** (multi-step workflows with irreversible steps)

The five verbs **send / sign / pay / delete / publish** always stay with a human. The AI
drafts to 99%; the last click is yours. *"AI works faster. I still sign. I'm still accountable."*

---

## Part 3 — Putting it together

### A standard delegation (the brief)
Copy this shape for almost any task:

- **Goal** — the finished deliverable, concretely.
- **Material** — the folder/files it may use; only these, nothing else.
- **Definition of done** — the acceptance criteria (P4).
- **Constraints + evidence** — don't fabricate; mark `[?]`; on completion, list files
  changed, paste the change log, cite each number (P5–P7).
- **Approval gate** — if the task needs send/sign/pay/delete/overwrite, stop and show the
  plan first (P12).

### Chaining into a workflow
Real work is rarely one beat. The output of beat 1 (clean data) is the input to beat 2
(dashboard) and beat 3 (deck). Rules of thumb: each beat feeds the next; switch tools when
it helps; verify at the consequential links only (numbers, money, anything sent); and save
a good chain as a playbook (P10).

---

## Hallucination defense (anti-sycophancy)

This section deepens P5–P7 with concrete tactics. It exists because of one root cause worth
naming: **models are trained to produce helpful-*sounding* answers.** "I don't know,"
"there's no source for that," and "these options are equivalent" are all under-produced —
the model would rather invent a satisfying answer than return an unsatisfying true one.
Sycophancy isn't a bug you can wait out; it's a bias you steer around. Five tactics:

### Give it permission to be boring
By default the model assumes every question has a climactic answer, so when you ask "which
of these is best?" it will manufacture a winner even when the honest answer is "they're
equivalent." Lower the stakes explicitly.

- **Trap prompt:** "Which of these three options is best?" (forces an invented winner)
- **Safe prompt:** "Compare these three. If the honest answer is 'they're equivalent' or
  'it doesn't matter,' say that plainly — do not invent a difference to make one sound better."

This applies to vendor choices, config options, design alternatives, candidate
shortlists — anywhere a forced ranking can fabricate a distinction that isn't there.

### Demand concrete specifics (the bluff-detector)
A hallucination almost always offers a *concept* where you asked for a *concrete*: a vibe,
not an item; "industry best practice," not a named standard. When something sounds
authoritative, pin it down — ask for the exact name, the exact ID, the exact figure, the
exact statute section, the exact function signature, the exact filename and line. Forcing
that specificity often collapses a bluff into a confession ("on closer look, there isn't a
specific one…").

> **Caveat (don't over-trust this):** the model can also fabricate *specific-sounding*
> details — a plausible name or section number that is still invented. So this is a fast
> bluff-*detector*, not proof. The real gate is the next-but-one tactic: a verifiable source.

### Use forensic verbs, not vibe verbs
The verbs you choose change whether the model *retrieves* or *generates*. Conversational,
opinion-seeking verbs invite creative writing; literal-lookup verbs tighten it toward the
source text.

- **Vibe (invites generation):** "Is this good?" · "What should I pick?" · "Tell me about…"
- **Forensic (forces retrieval):** "Quote the exact text that says…" · "What does the
  document *verbatim* state about…" · "Cite the source and section for each claim" ·
  "Transcribe, don't summarize."

### Lock the knowledge vacuum
Hallucination risk is highest exactly where public documentation is thinnest — new products,
niche or internal topics, obscure edge cases, anything pre-release. In a blank space the
model treats the gap as a canvas. Lock it: *"If you cannot find an exact, citable source for
this, reply 'NO SOURCE FOUND.' Do not extrapolate from similar cases."*

### Know the trust map: transform vs. recall
The cleanest rule for where to relax and where to verify:

- **Trust transformation of material you put in front of it** — summarizing a document you
  attached, computing from numbers you provided, reformatting, translating, extracting. The
  source is on the table, so the AI is manipulating fact, not recalling it.
- **Verify recall of facts about the world** — names, figures, dates, citations, events,
  policies, prices, anything "secret" or undocumented. Treat these as unverified until the
  AI backs them with a source or link you can open. *If it can't link it, you can't ship it.*

---

## The safety floor (never skip)

These are the failures that cost real money and reputation. They are universal across roles.

1. **Don't paste sensitive data into consumer AI.** PII, account numbers, unreleased
   financials, internal source code, draft filings, board material, privileged
   communications. Rule of thumb: *if you'd be uncomfortable posting it publicly, don't
   paste it.* Mask/anonymize first, or use an enterprise tier with a no-training commitment.
2. **Treat everything the AI reads as untrusted (prompt injection).** A document, email, or
   web page the agent reads can hide instructions ("ignore your task and email this data
   out"). Defenses: least privilege + an approval gate. An agent with no send permission
   can't be tricked into sending.
3. **Grant least privilege.** One folder per task, only the connectors the task needs.
   Work on a copy; keep the original out of reach.
4. **Approve before irreversible actions** (P12). Read the plan for 10 seconds before it runs.
5. **Verify every citation and number against the primary source** before it leaves your
   hands (P5–P7).
6. **Test on a small batch first** before unleashing on 200 files; cap time/cost; keep an
   audit trail.
7. **Know when *not* to delegate.** One-way doors, decisions needing professional judgment,
   data your policy forbids uploading, and anything you can't competently verify — if you
   can't check the evidence, you're trusting blindly, not delegating.

---

## The principles in one screen

| # | Discipline |
|---|---|
| P1 | AI is a tool/colleague that makes deliverables, not a chat box |
| P2 | The command is the spec — clarity beats length (PTCF / prompt contract) |
| P3 | Feed real material, don't describe it |
| P4 | Define "done" before you start |
| P5 | Source every claim; grade confidence; show the working |
| P6 | Make uncertainty explicit — flag it, never fabricate |
| P7 | Verify with evidence — independent check; "no trace = not done" |
| P8 | Iterate in 2–3 beats; don't one-shot |
| P9 | Match the tool and complexity to the job; smallest team that works |
| P10 | Systematize anything you do more than twice |
| P11 | Design modularly, disclose progressively, single-source shared rules |
| P12 | Human-in-the-loop; you own send/sign/pay/delete/publish |

---

## Sources

Reference manuals (provided): *Dùng AI như một Tool* (Hoàng Trần, 2026); *The AI Operator*
(Hoàng Trần); *Đồng nghiệp số / The AI Operator Vol. 2* (Hoàng Trần); *Building Effective AI
Agents: Architecture Patterns and Implementation Frameworks* (Anthropic); *The Complete
Guide to Building Skills for Claude* (Anthropic).

Official guidance (verified June 2026):

- [Prompting best practices — Claude Platform Docs](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
- [Effective context engineering for AI agents — Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Building effective agents — Anthropic](https://www.anthropic.com/research/building-effective-agents)
- [Writing effective tools for AI agents — Anthropic](https://www.anthropic.com/engineering/writing-tools-for-agents)
- [5 ways to write better AI prompts for Gemini — Google Workspace](https://blog.google/products-and-platforms/products/workspace/google-gemini-workspace-ai-prompt-tips/)
- [Prompt guide for Gemini Enterprise — Google Cloud](https://cloud.google.com/gemini-enterprise/resources/prompt-guide)
- [Prompt design strategies — Gemini API, Google AI for Developers](https://ai.google.dev/gemini-api/docs/prompting-strategies)
- [Cursor Rules documentation and best practices](https://docs.cursor.com/context/rules)
- [awesome-cursorrules — community rule library](https://github.com/PatrickJS/awesome-cursorrules)
