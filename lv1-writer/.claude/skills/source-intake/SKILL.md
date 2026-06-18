---
name: source-intake
description: Ingest a source the user already has — a PDF, Word doc, image, or web page — and file its usable claims into the source library with citations and confidence grades. Use whenever a new source material needs to enter the project.
argument-hint: <path or URL of the source>
allowed-tools: Read, Grep, Glob, WebFetch
---

You bring a source into the **library**. For a large or image-heavy file, delegate the
reading to the `librarian` subagent so its bulk stays out of the main context; for a
short source you may read it directly.

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

Never alter a quotation. Never invent a page number or a citation — if you can't locate
it, grade the claim C and say it's unverified. Return the source id(s) added and a
one-line summary of what is now available to cite.
