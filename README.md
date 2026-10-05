# Obsidian LLM Brain

A clone-and-go Obsidian vault that **Claude maintains for you**. You drop in sources and ask questions; Claude writes the summaries, links pages together, keeps an index, and logs every change.

One file — [`vault/CLAUDE.md`](vault/CLAUDE.md) — holds the rules, so every Claude session behaves the same way.

---

## Why use it

- **Knowledge compounds.** Each new source gets linked to what you already know, so answers improve over time instead of starting from zero each chat.
- **Claude keeps your context.** Your goals, style and notes live in files Claude reads every session.
- **Answers cite your own notes.** Claude reads the index, then the relevant pages, and answers with `[[links]]`.
- **Sources stay untouched.** `raw/` is read-only for Claude, so you can always check a summary against the original.
- **Contradictions are flagged, not overwritten.**
- **Full audit trail.** Every operation is appended to `log.md`.
- **You own the data.** Plain Markdown files on your disk.

---

## How it works

| Layer | Location | Who writes it | Rule |
| --- | --- | --- | --- |
| Raw sources | `raw/` | You | Immutable — Claude reads, never edits |
| The wiki | `wiki/` | Claude | Claude creates, updates and cross-links pages |
| The schema | `CLAUDE.md` | Both | Changed only when you agree |

---

## What you need

- [Obsidian](https://obsidian.md) (free)
- A Claude tool that can read and write files in a folder:
    - **Claude Cowork** (desktop app) — connect the vault folder, or
    - **Claude Code** — run `claude` from inside the vault folder
- ~15 minutes

---

## Quick start

1. **Get the template.**
   ```bash
   git clone https://github.com/Husseinmalki/obsidian-llm-wiki.git
   ```
   Or click **Code → Download ZIP** and unzip it.
2. **Copy the `vault/` folder** to where you want your notes to live, and rename it (e.g. `Second Brain`).
3. **Open it in Obsidian:** *Open folder as vault* → select the folder.
4. **Edit `CLAUDE.md`:**
    1. Replace every `[Your name]`.
    2. Fill in **section 7 — Standing Context** (goals, schedule, style). This is what makes Claude's output feel like yours.
5. **Fill in `wiki/me/Goals.md`.**
6. **Connect Claude to the vault folder.**
    - Cowork: add the folder in the desktop app.
    - Claude Code: `cd "path/to/Second Brain"` then `claude`.
7. **Do a test ingest.**
    1. Save any article as a `.md` file in `raw/`.
    2. Tell Claude: `ingest raw/<file>.md`
    3. Check you got: a page in `wiki/sources/`, a new entry in `index.md`, a new entry in `log.md`.

Done — that's the whole setup.

---

## Folder structure

```
vault/
├── CLAUDE.md        # the rules Claude follows
├── index.md         # catalog of every page
├── log.md           # append-only history
├── Home.md          # friendly entry point
├── raw/             # your sources — Claude never edits
│   └── assets/      # images saved with sources
├── wiki/
│   ├── me/          # identity, goals, learning style
│   ├── work/        # job, business, projects
│   ├── tech/        # tools, workflows
│   ├── sources/     # one summary per raw source
│   └── topics/      # concepts that span areas
├── Attachments/     # images pasted into notes
└── .obsidian/       # pre-set: pasted images go to Attachments/
```

Rename or add `wiki/` subfolders to fit your life — then update section 2 of `CLAUDE.md` to match.

---

## Daily use

| You say | Claude does |
| --- | --- |
| `ingest raw/article.md` (or paste text + "ingest this") | Discusses takeaways, writes a source summary, updates related pages, `index.md` and `log.md` |
| Any question, e.g. *"what do I know about Terraform state?"* | Reads `index.md` → relevant pages → answers with `[[links]]`; offers to save new synthesis |
| `lint the wiki` (monthly) | Finds contradictions, orphans, stale claims, missing pages; fixes what you approve |

See recent activity:

```bash
grep "^## \[" log.md | tail -5
```

---

## The 5 rules that make it work

1. `raw/` is read-only for Claude.
2. Every page change updates `index.md` — an unindexed page is invisible to queries.
3. `log.md` is append-only.
4. No orphan pages; contradictions get a warning callout, never a silent overwrite.
5. The vault root holds only `CLAUDE.md`, `index.md`, `log.md`, `Home.md`.

---

## Troubleshooting (least → most likely)

| Symptom | Possible causes | Fix |
| --- | --- | --- |
| Claude ignores the rules | 1. File misnamed (`claude.md`) 2. Claude opened on the parent folder, not the vault | Name it exactly `CLAUDE.md` at the vault root; open Claude on the vault folder itself |
| Pages land in the vault root | Section 2 doesn't cover the topic | Add a subfolder to section 2 |
| Answers miss pages you know exist | `index.md` out of date | Run `lint the wiki` |
| Output feels generic | Section 7 left as placeholders | Fill in goals, schedule, style |
| Your edit to a wiki page got overwritten | `wiki/` is Claude-owned | Put personal notes in `raw/`, or tell Claude a page is yours |

---

## Back up your vault (recommended)

Claude edits many files, so keep version history:

```bash
cd "path/to/Second Brain"
git init
git add .
git commit -m "Vault founded"
```

> **Keep your personal vault in a PRIVATE repo.** It will contain your notes, goals and sources.

---

## Optional extras

Once the basics run smoothly:

- **Study folders** — e.g. `Cert Notes/`, one page per topic. Add them to section 2 of `CLAUDE.md`.
- **Scheduled tasks** — a daily lesson or weekly review that Claude writes into the vault (log it with a `lesson` operation).
- **Task list** — a `wiki/me/TASKS.md` that Claude manages: one main thing + up to 3 others per day.
- **Chat check-ins** — a bot (e.g. Telegram) that sends reminders and passes replies to Claude. Add a rule that Claude never grants access from a chat request.

---

## Credits

Inspired by Andrej Karpathy's "LLM wiki" idea — an LLM that maintains a personal knowledge base rather than answering from scratch each time.

## License

[MIT](LICENSE)
