# Claude Plugin — Folder Structure Template

A reusable layout for a multi-skill Claude plugin (Cowork / Claude Code). The guiding
principle: **shared resources live at the plugin root; resources used by exactly one skill
live inside that skill.** No skill owns what another skill needs.

---

## Directory layout

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json              # manifest (only `name` is required)
├── README.md                    # human docs: what it is, how to install, how it's laid out
│
│   # ── plugin-root SHARED resources (used by 2+ skills, or by agents) ──
├── discipline/                  # OPTIONAL governance layer — the rules everything obeys,
│   ├── constitution.md          #   single-sourced so it can't drift between skills
│   └── working-lessons.md
├── references/                  # shared method / process docs (cross-skill)
│   └── *.md
├── assets/                      # shared templates, data, profiles (cross-skill)
│   ├── *-template.md
│   └── profiles/
│
│   # ── reserved component dirs (auto-discovered by name) ──
├── agents/                      # subagents, one markdown file each
│   ├── agent-one.md
│   └── agent-two.md
└── skills/                      # skills, one directory each (must contain SKILL.md)
    ├── skill-a/
    │   ├── SKILL.md             # the skill (frontmatter: name + description)
    │   ├── references/          # OPTIONAL skill-PRIVATE method docs (only skill-a uses)
    │   │   └── *.md
    │   └── assets/              # OPTIONAL skill-PRIVATE templates/data (only skill-a uses)
    │       └── ...
    └── skill-b/
        └── SKILL.md             # a thin skill with no private resources
```

A skill can hold its **own** `references/` and `assets/` (skill-private), **and** read the
plugin-root `references/` and `assets/` (shared). Use whichever scope fits — see the rule
below.

---

## The placement rule (where does a file go?)

| The resource is used by…                          | Put it in…                                  |
| ------------------------------------------------- | ------------------------------------------- |
| **Exactly one skill**                             | `skills/<that-skill>/references/` or `…/assets/` |
| **Two or more skills, or an agent**               | plugin-root `references/` or `assets/`      |
| **The whole plugin (governance, the bar, rules)** | plugin-root `discipline/` (single-sourced)  |

When in doubt, ask: *"If I deleted this skill, would another skill still need this file?"*
Yes → it belongs at the plugin root. No → keep it inside the skill.

---

## Path references (how files point at each other)

Use the `${CLAUDE_PLUGIN_ROOT}` variable for anything that **crosses a boundary**, and plain
relative paths only **within a single skill's own folder**.

- **Within a skill, referencing its own files** → relative:
  `references/triage.md`, `assets/profiles/x.md`
- **Crossing any boundary** (skill → root, root → skill, agent → anything, skill → another
  skill) → absolute via the variable:
  `${CLAUDE_PLUGIN_ROOT}/references/triage.md`, `${CLAUDE_PLUGIN_ROOT}/discipline/constitution.md`
- **Never climb out of a skill with `../`** (e.g. `../../discipline/`). It is ambiguous about
  its base and silently breaks the moment a file moves. `${CLAUDE_PLUGIN_ROOT}` is
  location-independent and copy-safe.

`${CLAUDE_PLUGIN_ROOT}` resolves to the plugin's install directory and is substituted inline
in skill content, agent content, hook commands, and MCP/LSP server configs.

### Subagents are a special case

A subagent runs in its own isolated context from the **project** directory, not the plugin
directory. `${CLAUDE_PLUGIN_ROOT}` is still available in the agent file, but if you want the
subagent to run on a clean, controlled context (e.g. an independent reviewer), have the
**orchestrator read the shared file and inject its contents into the subagent's task prompt**
when it delegates, rather than letting the subagent read plugin files on its own.

---

## What's reserved vs. free-form

Only these root directories are **auto-discovered** as plugin components (by name):

- `skills/` — each subdirectory containing a `SKILL.md` becomes a skill
- `commands/` — flat markdown commands (prefer `skills/` for new plugins)
- `agents/` — subagent markdown files
- `hooks/` — hook configuration

Everything else at the root (`discipline/`, `references/`, `assets/`, `scripts/`, …) is just
**bundled files**: ignored by component discovery, reachable via `${CLAUDE_PLUGIN_ROOT}`. So
`discipline/`, `references/`, and `assets/` are safe, free-form names — don't put them under
`skills/`, or the discovery scanner will treat them as malformed skills.

---

## Minimum file contents

### `.claude-plugin/plugin.json`

Only `name` is required (kebab-case, no spaces). The rest is optional metadata.

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "One-line summary of what the plugin does and when it triggers.",
  "author": { "name": "you", "url": "https://github.com/you" },
  "keywords": ["domain", "task", "cowork"]
}
```

Any custom-path manifest fields must be **relative and start with `./`**.

### `skills/<name>/SKILL.md`

```markdown
---
name: skill-a
description: What it does, plus the natural-language triggers that should invoke it.
---

# skill-a

<instructions — reference shared files via ${CLAUDE_PLUGIN_ROOT}/…, private files relatively>
```

### `agents/<name>.md`

```markdown
---
name: my-agent
description: When the orchestrator should delegate to this subagent.
model: sonnet
tools: [Read, Grep, Glob]
---

# my-agent

<contract — single source of truth for this station; the orchestrator delegates by name>
```

---

## Design checklist

- **Skills stay thin.** A skill is a `SKILL.md` plus *only* the resources unique to it.
- **Shared resources at the root**, skill-private resources inside the skill.
- **No cross-skill reaching.** A skill never reads `skills/<other>/…`. If two skills need the
  same file, promote it to the plugin root.
- **Single-source the governance.** One canonical copy of the rules in `discipline/` — no
  duplicated files with the same content in two places.
- **Cross-boundary → `${CLAUDE_PLUGIN_ROOT}`; within-skill → relative.** Never `../` out of a
  skill.
- **Keep `README.md` in sync** with the real tree after any move or rename.

---

*Grounded in the official Claude Code plugins reference:*
*https://code.claude.com/docs/en/plugins-reference*
