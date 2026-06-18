# Sources — Instruction-design knowledge base

A general, reusable reference for designing AI agent instruction systems (for Claude
Code and Cowork). Distilled from reviewing a real multi-agent pipeline into principles
that apply to any instruction set — what to put where, what wastes tokens, what
backfires, and how the underlying platform actually loads and runs your instructions.

Read in this order:

1. **`instruction-design-principles.md`** — the architecture: how to layer a system,
   single-source your rules, split facts from procedures, and pick the right runtime.
2. **`instruction-failure-modes.md`** — the anti-patterns: redundancy and the rules
   that quietly push the model the wrong way.
3. **`claude-code-mechanisms.md`** — grounded reference for the platform primitives
   (instructions, skills, subagents, frontmatter, hooks) the principles rely on.

These are working notes, accurate as of mid-2026. Platform behavior changes — verify
version-specific details against current documentation before relying on them.
