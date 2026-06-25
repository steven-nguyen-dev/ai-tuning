# lv1-compass — Review Checklist (document-specific)

Generated cold from the plugin's own files before verification. Generic template items
that don't apply have been dropped.

## Document classification

```
Document type   : Claude plugin — a set of instruction documents (constitution, two
                  SKILL.md orchestrators, six references, two subagents, templates,
                  manifest) that together define a runnable assistant+advisor system.
Core promise    : A slim, non-drifting work assistant+advisor where both skills and both
                  subagents share ONE constitution, run the described flow, and ship only
                  work that is "impressive AND true" with every fact graded + sourced.
Intended reader : (a) The runtime — Claude executing the skills/agents; (b) a human
                  installing/auditing the plugin.
Key contracts   : 1. The shared discipline is single-sourced in core/ and cannot drift
                     between roles or copies.
                  2. The subagents (research, inspect) can actually read what they're told
                     to read at runtime.
                  3. The effort tier actually gates which stations run (constitution's own
                     rule: "a tier that exists only as a label is decoration").
                  4. The inspector judges the real shipped artifact, not a summary (R5).
                  5. Modern plugin layout, no commands/ directory.
                  6. No vague step: each station has a concrete goal, input, output.
```

## 3A · Structure / layout contracts
- [ ] `.claude-plugin/plugin.json` present, valid JSON, required `name` field. — PASS
- [ ] No `commands/` directory (explicit requirement). — PASS
- [ ] `skills/<name>/SKILL.md` for each skill; frontmatter has name+description. — PASS
- [ ] `agents/*.md` each have valid frontmatter (name, description, tools). — PASS
- [ ] Every relative path a skill/agent is told to read resolves at runtime. — **FAIL** (F1)
- [ ] Office-format deliverable (docx/pptx/xlsx) is inspected as shipped. — **FAIL** (F2)

## 3B · Integrity contracts (what the plugin must not do)
- [ ] The discipline is single-sourced — no station keeps a "second inline copy." — **FAIL** (F3)
- [ ] No grade definition diverges between core and the subagents. — **FAIL** (F7)
- [ ] No fabricated/unverifiable claim about how the runtime resolves files. — partial (README flags skills' path but not the agents'/inspect's, F1)

## 3C · Consistency contracts (what it promises to hold constant)
- [ ] Effort tiers gate stations, not just apparatus (constitution's own rule). — **FAIL** (F4)
- [ ] The fast lane (tiny) has a defined station set and honors "source every claim." — partial (F5)
- [ ] Pipeline diagram in SKILL.md matches the numbered steps. — PASS
- [ ] Advisor delegation to lv1-research has a defined output location. — partial (F6)
- [ ] Terminology consistent (A/B/C/D grades, register names, verdict sets). — PASS
