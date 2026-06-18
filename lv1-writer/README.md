# lv1-writer

A self-contained discipline pack for book and research writing in Claude Cowork. It
turns finding, keeping, and using source material into a disciplined pipeline — triage,
source curation, research, outline, draft, independent inspection — and ships only work
that is **impressive AND true**, with every fact traceable to a source.

## Install

Copy the contents of this folder into your Cowork project folder:

```
my-book/
├── CLAUDE.md           ← the operating rules (loads automatically)
├── .claude/
│   ├── skills/         ← the pipeline stations you invoke
│   │   ├── lv1/        ← /lv1 orchestrator
│   │   ├── triage/
│   │   ├── source-intake/
│   │   ├── outline/
│   │   └── draft/
│   └── agents/         ← isolated workers (own context + model)
│       ├── librarian.md
│       ├── researcher.md
│       └── inspector.md
├── sources/            ← the kept source material
│   ├── library.md      ← the index: every source + every claim, cited and graded
│   ├── pdf/  word/  images/  web/
└── manuscript/         ← your plain-text drafts
```

## Use

Run the whole line:

```
/lv1 Draft a 2,000-word chapter on the fall of the Western Roman economy
```

Hand over a source to file it into the library:

```
Use source-intake on sources/pdf/heather-2005.pdf
```

The orchestrator runs each station, writes receipts to `runs/<id>/`, keeps every fact
tied to `sources/library.md`, and delivers the draft with its confidence list and
assumptions. On a `tiny` task it runs a fast lane (triage → draft → inspect).

## What makes the check real

The inspector runs in its own context and reads the actual draft and the source library
— not a summary. Independence is mechanical, not a promise. Factual claims are checked
against real sources; interpretation is allowed, but only when it is honestly labeled as
interpretation rather than dressed up as fact.
