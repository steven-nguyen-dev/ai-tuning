# sources/library.md — knowledge base navigator

This file is the **index** of the project's reference material. The `researcher`
subagent owns this directory: when it needs external knowledge (web pages, PDFs,
books, internal documents), it distills the *needed* knowledge into a focused
`sources/<topic>.md` and registers it here. Raw material is **never** dumped
verbatim into the repo — every entry below points to a distilled note.

You (the user) can ask the assistant at any time to add a new source: paste a URL,
point at a local file, or describe a topic to research. The researcher will produce
a `sources/<topic>.md`, link it here, and grade the facts it extracted.

## How to use this file

- **Before researching anything new, read this index first.** A topic that is already
  distilled does not need to be fetched again.
- **Keep it organized.** Group entries under section headers (add new sections as the
  base grows). Each entry is one line: a link to the note plus a short hook.
- **Keep entries honest.** If a note is stale or wrong, update or remove it — do not
  silently leave bad information in place.

## Entry format

```
- [<topic title>](./<topic-slug>.md) — <one-line hook: what it covers, primary source>
```

## Sections

### Project / domain knowledge
- *(none yet — first research run will populate this)*

### Libraries, frameworks, tools
- *(none yet)*

### External docs and references
- *(none yet)*

### Internal documents
- *(none yet — paste a path or URL to add one)*

---

*This file is maintained by the `researcher` subagent. To add a source, ask the
assistant: "Add `<url-or-path>` to the knowledge base."*
