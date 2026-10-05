# CLAUDE.md — LLM Wiki Schema

This vault is an **LLM-maintained wiki**. Claude is the wiki agent: it writes and maintains all wiki content. [Your name] curates sources, asks questions, and directs the analysis. Every session that touches this vault follows this schema.

---

## 1. The Three Layers

| Layer           | Location    | Who writes it            | Rule                                                  |
| --------------- | ----------- | ------------------------ | ----------------------------------------------------- |
| **Raw sources** | `raw/`      | [Your name] (drops files in) | IMMUTABLE — Claude reads, never edits             |
| **The wiki**    | `wiki/`     | Claude                   | Claude owns it: creates, updates, cross-references    |
| **The schema**  | `CLAUDE.md` | Co-evolved               | Update only when [Your name] agrees a convention should change |

---

## 2. Folder Conventions

```
vault/
├── CLAUDE.md            # this schema
├── index.md             # content catalog — update on EVERY ingest/new page
├── log.md               # append-only chronological record
├── Home.md              # human-friendly entry point (mirrors index highlights)
├── raw/                 # immutable sources (articles, PDFs, clippings, transcripts)
│   └── assets/          # images downloaded with sources
├── wiki/
│   ├── me/              # identity, goals, learning style
│   ├── work/            # job, business, side projects
│   ├── tech/            # tools, workflows, productivity
│   ├── sources/         # one summary page per ingested raw source
│   └── topics/          # concept/entity pages that span domains
└── Attachments/         # pasted images from notes
```

Rules:

- New knowledge pages go in the matching `wiki/` subfolder. If none fits, propose a new subfolder — don't dump in root.
- Root contains ONLY: CLAUDE.md, index.md, log.md, Home.md. Nothing else, ever.
- Filenames: descriptive Title Case (`ARM Templates.md`). Source summaries: same name as the raw file.
- If a scheduled task writes into a folder, that folder's path is load-bearing — never move or rename it without updating the task.

## 3. Page Format

Every wiki page Claude creates:

```markdown
---
tags: [domain, type]
created: YYYY-MM-DD
updated: YYYY-MM-DD
source: raw/<file> (only for source-summary pages)
---

# Page Title

One-line purpose statement.

(body — bullet points and step-by-step over prose walls;
sequential, concrete, no skipped steps)

---

## Related
- [[Other Page]] — why it's related
```

- Every page links to at least one other page. No orphans.
- Contradictions: when a new source conflicts with an existing claim, do not silently overwrite. Add a `> [!warning] Contradiction` callout naming both sources, and flag it in chat.

## 4. Operations

### Ingest (trigger: "ingest …" or a new file appears in raw/)

1. Confirm the source file is in `raw/` (if pasted into chat, save it there as markdown).
2. Read it fully. Discuss key takeaways in chat BEFORE writing.
3. Write a summary page in `wiki/sources/` (frontmatter `source:` points to the raw file).
4. Update every wiki page the source touches — entity pages, topic pages, cross-references. A single ingest may touch many pages; list them in chat.
5. Update `index.md` (add/update entries).
6. Append to `log.md`.

### Query (trigger: any question against accumulated knowledge)

1. Read `index.md` first to locate relevant pages — don't grep blindly.
2. Read those pages, synthesize, answer with `[[wiki-links]]` as citations.
3. If the answer produced new synthesis worth keeping (comparison, analysis, decision), offer to file it as a page in `wiki/topics/` — explorations should compound.

### Lint (trigger: "lint the wiki" — also suggest it roughly monthly)

1. Check: contradictions, stale claims, orphan pages (no inbound links), missing cross-references, concepts mentioned ≥3 times without their own page, index entries that don't match reality, root-level strays.
2. Report findings as a checklist; fix what [Your name] approves.
3. Append a lint entry to `log.md`.

## 5. index.md Rules

- Catalog of every wiki page: link + one-line summary, grouped by category.
- Update it in the same session as ANY page creation or significant page change. An unindexed page is a bug.

## 6. log.md Rules

- Append-only. Never edit past entries.
- Entry prefix format (parseable): `## [YYYY-MM-DD] <operation> | <title>` where operation ∈ `ingest`, `query`, `lint`, `maintenance`.
- One or two lines per entry: what was done, which pages were touched.
- `grep "^## \[" log.md | tail -5` = last 5 operations.

## 7. Standing Context  ← EDIT THIS SECTION

- Goal: [e.g. "job-ready as a DevOps engineer by DD MMM YYYY"] → [[Goals]]
- Schedule: [e.g. "work 9–5; study in the evening"]
- How I learn: [e.g. "Pluralsight for cert X; self-paced labs for the rest"]
- Style: [e.g. "concise, direct, bullet points, step-by-step, no fluff"]
- Debugging: [e.g. "list possible causes least → most likely, then test"]
- Life areas this vault covers: [e.g. "career, side business, health"]
