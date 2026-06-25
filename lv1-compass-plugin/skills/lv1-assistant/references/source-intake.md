# Source-intake — bringing a source into the library

Files a source the user already has — a PDF, Word doc, image, or web page — into
`sources/library.md` with a citation and a confidence grade per claim.

For a large or image-heavy file, don't dump its full text into the conversation — read it
in passes (by section or page range) and pull out only what's usable, so its bulk doesn't
flood the rest of the task's context. For a short source, just read it directly.

Steps:
1. Read the source. Capture a full citation (author, title, date, page/URL, file path).
2. Pull out the claims this project can actually use — quotes, figures, facts, dates.
3. Grade each claim A/B/C/D. A primary source you read = A or B; a passing mention or
   second-hand figure = C until confirmed.
4. Append to `sources/library.md`:

```
## <Sxx> — <author, title, date> — <type: pdf|word|image|web> — <path/url>
- [A] <claim> — p./loc <…>
- [B] <claim, reasoned> — from <Sxx>
```

If the source is a file (not a URL) and there's no folder yet for its type under `sources/`
(`pdf/`, `word/`, `images/`, `web/`), create that one folder now — don't pre-create the
others.

Never alter a quotation. Never invent a page number or a citation — if you can't locate it,
grade the claim C and say it's unverified. A made-up page number is worse than no page
number, because it survives unnoticed until a reader checks it.

Writing to `sources/library.md` is a **verified write** (constitution → Write integrity):
after appending, re-read the file and confirm the block landed. If it's locked, use the
`sources/library.pending.md` fallback and flag it — never leave the spine missing the source
and never drop a probe/scratch file in `sources/`.

Return the source id(s) added and a one-line summary of what's now available to cite.

## Library sync (used by `lv1-compass-init`)

Setup runs this every time, even if `sources/library.md` already exists — the goal is to
keep the library in sync with whatever's actually in `sources/`.

1. List the **source files** under `sources/` (recursively, across `pdf/`, `word/`,
   `images/`, `web/`, and any genuine loose source directly in `sources/`). **Exclude the
   library's own bookkeeping files** — `library.md`, `library.pending.md`, and anything
   matching `library*.md` — from this scan; the library must never try to ingest itself. A
   loose file in `sources/` that is clearly not a source (e.g. a stray `test` or scratch
   file) is **flagged in your report, not silently intaken** as a source.
2. For each file **not yet represented** in `sources/library.md` (matched by file path),
   run the intake steps above and append its block.
3. For each file **already represented**, leave its existing block alone — don't re-read and
   re-grade something already cited just because init ran again.
4. For each source block whose path is a **local file** (type `pdf`/`word`/`image`) that
   **no longer exists** under `sources/`, don't delete the block — mark it `[ORPHANED —
   source file missing]` so the gap is visible. Skip this for `web` sources (the path is a
   URL).
5. Re-save `sources/library.md` with the legend and all current blocks.

This sync never discards a graded claim. It only adds what's new in `sources/` and flags
local files that have gone missing from it.
