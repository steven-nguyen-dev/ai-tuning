# manuscript/

Your plain-text drafts live here, **one file per chapter or piece** (e.g. `ch01.md`,
`ch02.md`). This is a prefer rule, not a suggestion: the pipeline drafts, revises, and
inspects **one chapter file at a time** rather than loading the whole book, so keep each
chapter in its own file. The draft station writes here. Keep prose plain and
source-backed — every factual claim should trace to `../sources/library.md`.

`_about.md` holds the project description (topic and angle). The **assembled full book** is
written to the **project root** as `<book-title>.md` (a generated build output — say
"assemble the book" to rebuild it from the chapters); don't edit it by hand. If init split
an existing single-file book into chapters, the original is kept under `manuscript/_archive/`
— nothing is deleted.
