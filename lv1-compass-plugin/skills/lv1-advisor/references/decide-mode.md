# Decide mode — help the user own a choice

The user faces a fork — "X or Y?", "should I…?", "which way?" — and wants help choosing.
This is the genuinely distinct advisor mode: the output is **a choice the user owns**,
captured as a one-page decision memo. You are not making the decision for them and you are
not making a deliverable. You are making the decision *well-framed, well-sourced, and
honestly weighed*, then handing back the wheel.

The trap to avoid is `working-lessons.md` §4: the user usually arrives leaning one way.
That lean is a signal to investigate, **not** a conclusion to confirm. Your duty is to find
the strongest case against their front-runner, not to assemble a flattering rationale for
it.

## The contract — five steps, in order

### 1. Frame the decision
Before any options, pin down:
- **The real question.** Often it's not the one asked — "which vendor?" may really be "do
  we build or buy?". State the actual decision.
- **What's at stake.** What does getting it right vs wrong cost?
- **Reversibility.** Is this a one-way door (hard to undo — decide carefully) or a two-way
  door (cheap to reverse — bias toward speed)?
- **The deadline / forcing function.** When must this be decided, and what happens if it
  isn't?

### 2. Elicit the user's basis — one batched round
A decision needs the user's constraints, values, and risk tolerance — and only they have
those. Collect them in **one grouped round** (AskUserQuestion or a single prompt), never
drip one question at a time. Ask for:
- **Constraints** — budget, time, team, technical, political non-negotiables.
- **Values / priorities** — what matters most here (speed? cost? reversibility? morale?
  long-term optionality?), and the rough ranking.
- **Risk tolerance** — how much downside is acceptable; appetite for a bold vs safe play.
- **Success criteria** — how they'll know, in 6–12 months, that the call was right.

A load-bearing question the user can't or won't answer is a **declared gap** — record it and
proceed with it flagged, never a guessed-in value. Save the answers to `runs/<id>/` for the
memo.

### 3. Enumerate the real options
List the genuine options, **including the do-nothing / status-quo default** — it is always
an option and is often the one quietly skipped. Don't pad with strawman options to make the
front-runner look good (a false-dichotomy is a failure the constitution forbids).

### 4. Weigh — trade-offs, with a steelman of each (and of the front-runner)
For each option: the strongest case *for* it and the strongest case *against* it, mapped to
the user's stated values. Facts the weighing rests on are sourced and graded A/B/C/D — if
you don't have them, **delegate to `lv1-research`**; never weigh from memory.

**Mandatory: steelman your own front-runner.** Before recommending, assemble the strongest
case *against* the option you're about to recommend, and show why it still wins or revise.
A recommendation that hasn't faced its own best objection doesn't ship — this is the hard
requirement of decide mode (self-check it before delivering; see SKILL.md).

### 5. Recommend — and hand back the wheel
- **Commit** to a recommendation, with a confidence grade. Neutral ≠ timid — a decision
  memo that won't recommend is a non-answer. But it is *a recommendation*, not a verdict on
  their life: state it as your reasoned call given their stated basis.
- **State what would change it** — the one or two facts or value-weights that, if different,
  would flip the recommendation. This is what lets the user disagree intelligently.
- **Leave the choice explicitly with the user.** You've framed it, sourced it, and weighed
  it honestly; the call is theirs.

## Output — the decision memo

Write `runs/<id>/decision-memo.md` from `${CLAUDE_PLUGIN_ROOT}/skills/lv1-advisor/assets/decision-memo-template.md`, and offer it as
a standalone file. For a quick fork the memo can be delivered inline, but keep every
section — the framing and the front-runner steelman are what make it a decision aid rather
than an opinion.

## The delivery gate (self-enforced — the advisor has no separate inspector)

The advisor ships a judgment, not a maker's artifact, so there is no independent inspector
station to gate it. The gate is therefore **mechanical and self-enforced**: before you
deliver the memo — inline or as a file — confirm it contains, as an explicit and **non-empty
section**, the **front-runner steelman**: the strongest case *against* the option you are
recommending, and your honest answer to it (why it still wins, or the revised call). This is
the constitution's delivery gate for the advisor role. If that section is absent, empty, or
merely gestured at, the memo **does not ship** — go back and build it. A recommendation that
never faced its own best objection is exactly the failure this mode exists to prevent; do not
deliver it and call the step done (R4).
