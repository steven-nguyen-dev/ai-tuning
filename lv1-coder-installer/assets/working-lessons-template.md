# .claude/working-lessons.md — coding lessons

Hard-won lessons for every station to read at the start of a run. These are not new
rules; they are the spirit behind the rules — failure modes that recur when rules are
followed in letter but not in spirit.

---

## 1. Triage default is fast lane — escalate for a reason, not a feeling

The default answer for research and plan is **no**. Adding stations because "we might
need them" wastes tokens and adds nothing when the codebase already answers the
question. A valid reason names a specific gap: "latest stable version of dep X is
unknown" or "this touches three services and the interaction isn't documented." "I
haven't tried yet" is not a valid reason — research fires after the codebase has been
read and a gap was found, not before.

## 2. A passing test is not proof if the test is trivially satisfied

A test that checks `assert result is not None` or always returns True proves nothing.
A real test fails when the code is wrong. If your test cannot fail, you do not have a
test — you have decoration. Grade a claim "A" only when you've confirmed the test
actually can fail.

## 3. Scope drift is the build's responsibility to announce, not the inspector's to catch

If you notice during build that the scope needs to change, stop and announce it. Do
not quietly add or drop scope and hope the inspector misses it. An unannounced scope
change is a FIX-IT — and the round costs more tokens than a one-line scope note would
have.

## 4. Research fires for specific gaps, not uncertainty in general

"I'm not sure" is not a reason to trigger research. Research fires when a specific
external fact is needed that the codebase cannot answer — a version, an API behavior,
a deprecation. Uncertainty about your own approach is resolved by reading the code,
not the web. If design docs exist that cover the area, they are the research.

## 5. Neutral ≠ timid

When the evidence points clearly to a conclusion, say so. Writing "this may indicate
a potential issue" when the test output already shows a failure is misleading. A
confidence label is a reason to commit harder, not to hedge.

---

*Add to this file when a lesson is violated — do not delete old ones.*
