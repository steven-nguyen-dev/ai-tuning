# Instruction failure modes

The recurring ways an instruction set wastes tokens or pushes the model the wrong way.
The first group is redundancy (costs money); the second is backfire (costs correctness).
Backfire matters more — a wrong rule is worse than a wasteful one.

## Redundancy patterns

**The duplication multiplier.** A rule restated in the always-on file *and* in every
worker is paid once per turn and again in every worker context. The cost scales with the
number of workers, so a few duplicated paragraphs can dominate a run's instruction
budget. Fix: state once, reference everywhere (see single-sourcing).

**Re-teaching vs. reinforcing.** Not all repetition is waste. Re-explaining a concept the
model already has in context is pure cost; a short pointed reminder at the point of use
can aid adherence. Cut the re-explanations; keep the in-context applications.

**Procedures stranded in the always-on file.** A multi-step workflow living in the
always-on instructions is paid on every task, including the ones that never use it. Move
it to an on-demand skill.

**Human docs treated as prompt surface.** READMEs and install notes are for people. If
they sit where the agent loads them as instructions, they add tokens and noise. Keep them
clearly separate.

**Mixed languages.** Instructions written in two languages cost more tokens than one and
can reduce instruction-following. Pick one language for the machine-facing text.

## Backfire patterns

**Absolute rules that break a legitimate sub-domain.** A rule that is right for most of
the work can be wrong for part of it. The classic case: "every claim needs a cited
source," applied to creative or interpretive output that has no external source. Taken
literally it either blocks the work or invites *fabricated* citations to satisfy the
rule — producing the exact dishonesty the rule was meant to prevent. Fix: **scope** the
rule to where it applies (empirical claims), and give the other sub-domain its own
honesty rule (e.g. "label interpretation as interpretation").

**Qualifiers used as hedges.** Confidence labels, "may suggest," "this raises questions"
— meant to signal calibration — get used to *avoid committing* to a conclusion the
evidence actually supports. A label should be a reason to state the finding *more*
precisely, not a shield against stating it. Build that intent into the rubric, or the
labels quietly become a license to be vague.

**Anti-collaboration rules stated too strongly.** "Use the fewest agents / keep it
simple" is good guidance that, pushed too hard, suppresses *legitimate* parallelism or
specialization the platform is good at. Frame it as "no idle or for-show workers," not
"fewer workers is always better."

**Soft checks dressed as hard gates.** A self-check the same agent performs on its own
work, described as an "automatic gate that will not open," oversells a soft control. The
agent that wants to finish is also the one "guarding" the exit. Either enforce it with a
real mechanism (a hook) or name it honestly as a self-check.

**Low-yield ceremony on every task.** A station that always runs but rarely changes the
outcome (e.g. a step that only asks "is this your best?") taxes every task for little
gain and often duplicates a later check. Fold it into an adjacent step by default; make
it a standalone station only for high-stakes work.

**Over-cautious defaults.** "When unsure, do the heavier thing" combined with a loud
"don't cut corners" rule makes the safe default drift toward running everything. That is
the opposite of efficient. Default to the lighter path; require a reason to escalate.

**Relying on "read this first" with no loading mechanism.** An instruction to "read file
X at the start of every session" depends on the agent choosing to obey, and nothing loads
X automatically. Important content should load by mechanism (an import or the always-on
file), not by hope. If it isn't loaded, assume it won't be read.

**Near-duplicate rules under different names.** Long rule lists often contain the same
idea twice (a positive rule and its mirror-image "don't"), inflating the count without
adding coverage. Merge them; fewer, distinct rules are easier to follow and cheaper to
carry.
