# sources/library.md — knowledge base navigator

This file is the **index** of the project's reference material. Sources are registered
here by the research station as they are fetched/verified, so later stations (draft,
inspection) can check claims against one shared library instead of re-deriving them.

## Entry format

```
### <short title>
- URL: <url>
- Grade: A/B/C
- Used in run: runs/<id>/
- Claim(s) supported: <one-line summary of what this source backs>
```

## Sections

### Claude Code / Agent SDK — subagent mechanics (from runs/20260701T044042Z-axiom-framework-review)

### Create custom subagents (Claude Code Docs)
- URL: https://code.claude.com/docs/en/sub-agents
- Grade: B (accessed via search synthesis, not raw page fetch, this run)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: subagent frontmatter fields (name/description required; tools/model optional), isolated context window per subagent, description-field-as-delegation-trigger, `Agent`/Task-tool nesting restriction and its recent relaxation (v2.1.172, depth 5), `disallowedTools` mechanism to block a subagent from spawning further subagents.

### Subagents in the SDK (Claude Code Docs)
- URL: https://code.claude.com/docs/en/agent-sdk/subagents
- Grade: B (search synthesis, not raw-fetched)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: subagent as a named, isolated Claude instance with its own system prompt/context/tools/permission mode; only final message returns to parent.

### How and when to use subagents in Claude Code (Anthropic/Claude blog)
- URL: https://claude.com/blog/subagents-in-claude-code
- Grade: B (search synthesis)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: corroborates implicit (description-matching) and explicit (@-mention, name) subagent invocation mechanisms.

### Slash Commands in the SDK (Claude Code Docs)
- URL: https://code.claude.com/docs/en/agent-sdk/slash-commands
- Grade: B (search synthesis)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: slash command mechanics; basis for "model-driven, not deterministically guaranteed" characterization of subagent invocation from a command.

### Claude Code changelog
- URL: https://code.claude.com/docs/en/changelog
- Grade: B (changelog line reported identically across 5 independent secondary sources; not raw-fetched this run)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: "Sub-agents can now spawn their own sub-agents (up to 5 levels deep)" — shipped v2.1.172, reported ~June 10, 2026. This reverses a ~2-year-standing "subagents cannot spawn subagents" rule.

### Configure the sandboxed Bash tool (Claude Code Docs)
- URL: https://code.claude.com/docs/en/sandboxing
- Grade: B (search synthesis)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: sandbox restricts Bash-tool writes outside working directory/session temp dir by default; `Edit()` vs `Write()` permission-rule distinction for extending write access. Plausible (unconfirmed) mechanism behind subagent file-write-persistence bug reports.

### Making Claude Code more secure and autonomous with sandboxing (Anthropic engineering blog)
- URL: https://www.anthropic.com/engineering/claude-code-sandboxing
- Grade: B (search synthesis)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: corroborates sandboxing mechanism and rationale.

### GitHub anthropics/claude-code issue #9458 (subagent Write tool operations don't persist)
- URL: https://github.com/anthropics/claude-code/issues/9458
- Grade: B (issue existence/title corroborated across 6 independently-numbered related issues; not raw-fetched this run)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: subagent `Write`/`Edit` operations can silently fail to persist to filesystem under "partial sandboxing" conditions — directly relevant to any framework (including AXIOM) that relies on file-based handoff between sequential subagent stages. Related issues: #4462, #18995, #13890, #40321, #23821 (same repo).

### Model configuration (Claude Code Docs)
- URL: https://code.claude.com/docs/en/model-config
- Grade: B (search synthesis)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: valid `model:` aliases (`haiku`, `sonnet`, `opus`, `best`, `sonnet[1m]`, `opus[1m]`, `opusplan`) and `inherit`; aliases resolve to "latest" per tier via environment variables, not fixed IDs.

### Building Effective AI Agents (Anthropic Research)
- URL: https://www.anthropic.com/research/building-effective-agents
- Grade: B (phrasing corroborated identically across ≥5 independent secondary sources; not raw-fetched this run)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: workflows ("predefined code paths," predictable/consistent) vs. agents ("dynamically direct their own processes," flexible/model-driven) distinction; five workflow patterns including evaluator-optimizer; recommendation to prefer simplicity over agentic complexity when possible.

### How we built our multi-agent research system (Anthropic engineering blog)
- URL: https://www.anthropic.com/engineering/multi-agent-research-system
- Grade: B (search synthesis, independently corroborated by Simon Willison's direct read-through)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: multi-agent systems are non-deterministic by design (same query, different valid paths run-to-run); evaluation built around outcomes (LLM-as-judge rubric) rather than fixed tool-call sequences; token usage explains ~80% of performance variance; multi-agent (Opus lead + Sonnet subagents) outperformed single-agent Opus by 90.2% on internal eval.

### Anthropic: How we built our multi-agent research system (Simon Willison's notes)
- URL: https://simonwillison.net/2025/Jun/14/multi-agent-research-system/
- Grade: B (independent human read-through of the Anthropic primary essay)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: independent corroboration of non-determinism framing and "same prompt may lead to different paths."

### Be clear, direct, and detailed (Claude prompt-engineering docs)
- URL: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/be-clear-and-direct
- Grade: B (search synthesis)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: "brilliant but new employee" framing; be specific about output/format; explain instruction motivation; sequential numbered steps when order matters.

### Use XML tags to structure your prompts (Claude prompt-engineering docs)
- URL: https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags
- Grade: B (search synthesis)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: recommended XML tag structure for complex prompts (`<role>`, `<context>`, `<task>`, `<instructions>`, `<output_format>`, etc.) improves clarity, accuracy, parseability; no canonical tag set but consistency matters.

### Platform mechanisms (Claude Code / Cowork) — internal project note
- URL: D:\Claude-Projects\Projects\ai-tuning\sources\claude-code-mechanisms.md (local file, pre-existing in this project)
- Grade: B (internal distilled reference, itself sourced from platform docs; treated as a reputable secondary within this project)
- Used in run: runs/20260701T044042Z-axiom-framework-review/
- Claim(s) supported: corroborates subagent isolation, filesystem as the durable inter-step channel, frontmatter fields, and — importantly — states "Delegation can nest ... only if it is granted the delegation tool; a worker without it cannot sub-delegate, which keeps a linear pipeline linear," which aligns with and supports Conflict 1's reconciliation (nesting is opt-in/gated, not automatic, even after the v2.1.172 change).

### Libraries, frameworks, tools
- *(none yet beyond the above)*

### Internal documents
- *(none yet — paste a path or URL to add one)*

---

*This file is maintained by the research station of each run. To add a source, ask the
assistant: "Add `<url-or-path>` to the knowledge base."*
