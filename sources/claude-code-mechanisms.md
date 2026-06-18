# Platform mechanisms (Claude Code / Cowork)

Grounded reference for how instructions actually load and run, so the design principles
rest on real behavior rather than assumption. Accurate as of mid-2026 — verify
version-specific details against current documentation.

## Always-on instructions (CLAUDE.md / project instructions)

- Auto-loaded at the start of every session and included in every turn.
- Also loaded into **every custom subagent's** context — the full memory hierarchy the
  main session loads reaches the workers too. (The built-in exploration/planning agents
  are the exception; they skip it to stay cheap.)
- Implication: this file is the most-paid text in the system. Keep it to short,
  always-true rules and shared vocabulary. Because workers already load it, they do not
  need to restate its rules — they only need their own delta.

## Skills (the home for procedures)

- A skill is a `SKILL.md` file (folder-based, so it can carry supporting files). Custom
  slash-commands and skills are the same mechanism; a file invoked as `/name` is a skill.
- The **body loads only when the skill is used**, so long procedures cost almost nothing
  until invoked — the opposite of always-on instructions.
- Selected by its **description**: the model reads descriptions to decide when to load a
  skill, so descriptions must be sharp and specific. The description is the always-present
  part; the body is the on-demand part.
- Invocation control: you can invoke explicitly with `/name`, let the model auto-invoke
  when relevant, or set the skill to explicit-only so it never auto-fires.
- Tooling: a skill can restrict which tools it may use, and can be run inside a subagent
  so its work happens in an isolated context.

## Subagents (the home for isolated workers)

- Each runs in its **own context window** with its own system prompt, its own tool
  permissions, and its own model. It works independently and returns only a **summary**
  to the caller.
- Because contexts are separate, two subagents cannot see each other's reasoning — this
  is what makes an independent reviewer genuinely independent.
- Since they don't share state, the durable channel between steps is the **file system**:
  each step writes its output to a file the next step (or a reviewer) reads directly.
- Loaded at session start; edits to a subagent file on disk need a session restart to
  take effect.
- Delegation can nest (a foreground worker can spawn another) **only** if it is granted
  the delegation tool; a worker without it cannot sub-delegate, which keeps a linear
  pipeline linear.

## Frontmatter (subagents and skills)

- **Required:** `name` and `description`. Everything else is optional.
- `tools` / `allowed-tools` — restrict the tool set (least privilege).
- `model` — `haiku`, `sonnet`, `opus`, a faster tier, a full model id, or `inherit`
  (the default). Use cheaper models for light stations and a stronger model for the
  hardest check to balance cost and quality.
- For skills: an argument hint, and a flag to disable automatic (model-driven) invocation.

## Hooks (real enforcement)

- Hooks fire on lifecycle events and can **block** an action. This is how to make a
  required step actually mandatory — e.g. prevent a run from completing unless the
  expected artifact (an inspection result) exists. A rule in a prompt is a request; a
  hook is enforcement.

## Cowork parallels

- Same underlying agent architecture: projects with selectable folders, folder-level
  instructions (a lightweight always-on file works), persistent memory, skills, and
  subagents. The same layering — contract in instructions, procedures in skills, isolated
  work in subagents — applies. Cowork adds first-class document handling and scheduled
  tasks, which suits research-and-writing workloads.

## Quick mapping: where does X go?

| You have… | Put it in… | Because… |
|---|---|---|
| An always-true rule / vocabulary | Always-on instructions | Must govern every task and every worker |
| A multi-step workflow used sometimes | A skill | Loads only when triggered |
| A step that must be independent | A subagent | Separate context = real independence |
| A step that reads bulky material once | A subagent | Keeps bulk out of the main context |
| A step that must never be skipped | A hook | Only mechanism that truly enforces |
| Human setup/usage notes | A README, kept separate | Not prompt surface |
