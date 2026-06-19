# ai-tuning

A collection of AI pipeline skills — one for writing, one for coding — built around
the same discipline: understand the real ask, do honest research, plan before
building, inspect independently, and ship only work that is impressive AND true.

Each sub-project is a self-contained skill with its own install instructions.

---

## lv1-writer

**Platform:** Claude Desktop / Cowork (upload-based skill)

A writing pipeline skill that runs tasks through **triage → research → plan → draft
(+ self-review) → inspect → ship**. Installed as a single skill upload — no project
layout required.

→ See [`lv1-writer/README.md`](./lv1-writer/README.md) for install and usage.

---

## lv1-coder-installer

**Platform:** Claude Code (globally-installed skill)

A coding pipeline skill that scaffolds **triage → research → plan → build (+
self-review) → inspect → ship** into any project via `/lv1-coder-installer init`.
Installed once globally; available in every project on the machine.

→ See [`lv1-coder-installer/README.md`](./lv1-coder-installer/README.md) for install
and usage.

---

## The shared discipline

Both pipelines enforce the same floor regardless of task size:

- Triage decides scope and effort before any work starts.
- Research is sourced and graded — no unsourced claims ship.
- Inspection runs in an independent context; the maker never self-certifies.
- Grading is A/B/C/D. D never ships.