---
name: second-brain-organize
description: Organize personal knowledge in the `second-brain/` vault using the PARA method (Projects, Areas, Resources, Archive) and the CODE workflow (Capture, Organize, Distill, Express). Decides where a new note belongs, when to promote an area to a project or archive a completed project, and how to distill long captures over successive passes. Use this skill whenever the user wants to file, categorize, summarize, distill, link, or relocate a knowledge note — including phrasings like "where should this go", "file this in my vault", "start a new project/area page", "archive this", "turn these notes into a reference", "summarize this for later" — even if they don't say "PARA" or "second brain".
---

# Second Brain: PARA + CODE

## When to use

Invoke this skill for knowledge-management work in `second-brain/`. Typical phrasings:

- "File this article summary somewhere sensible."
- "Where should these meeting takeaways live?"
- "Start a project page for launching the new website."
- "This project's done — archive it."
- "Distill this long note down to the essentials."
- "Summarize the highlights of this page for future me."

Do **not** use this skill for:

- Task capture or routing → `gtd-task-workflow`.
- Daily journal entries → the Daily Notes section of `AGENTS.md`.
- Hub-page refreshes from scattered notes, topic retrievals, or cadence reviews → `vault-update` (mode: `topic:refresh` or `topic:retrieval` or `cadence:*`).

## Directory layout

```
second-brain/
├── projects/   # Active, time-bound initiatives with a defined outcome
├── areas/      # Ongoing responsibilities with no end date
├── resources/  # Topic-based reference material
└── archive/    # Completed / inactive items from any of the above
```

File-naming convention (see `AGENTS.md`):

- `+project-slug.md` for project pages (lowercase, hyphenated, leading `+`).
- `@Area Name 🔵.md` for area hubs (leading `@`, trailing emoji).
- Resources and archive entries use descriptive names; emojis are optional but common (e.g. `Marketing Strategy 📊.md`).
- Include dates only for time-sensitive content.

## PARA: choose by actionability, not topic

The same topic can live in different PARA buckets depending on how you're relating to it *right now*.

| Bucket | Test | Examples |
|---|---|---|
| **Projects** | Defined outcome + finite end date | `+launch-website`, `+home-renovation`, `+write-book` |
| **Areas** | Ongoing standard to maintain — no end date | `@Health 💪`, `@Finance 💰`, `@Career 🧑‍💼` |
| **Resources** | Topic of interest for future reference | `SQL query patterns`, `Markmap syntax`, a meeting-note template |
| **Archive** | Inactive from any of the above | Completed projects, obsolete reference material |

### Transitions

- **Project completes** → move the file to `archive/` (keep name so old links still resolve if possible).
- **Area becomes time-bound** → promote a focused chunk into a new `+project-slug.md` in `projects/`; the area page stays and links to the project.
- **Resource stops being useful** → move to `archive/`; don't delete, in case you need the reference later.
- **Archive item becomes relevant again** → move back to the appropriate active bucket.

### Decision procedure for a new note

1. Is there a concrete outcome and end date? → `projects/` (create or update `+slug.md`).
2. Is this an ongoing life/work area I want to steward? → `areas/` (create or update `@Area 🔵.md`).
3. Is this reference material I might want later? → `resources/` under a descriptive topic name.
4. Otherwise → don't create a file; consider deleting or pasting into today's daily note.

Never leave a new note at the root of `second-brain/` — always pick a bucket.

## CODE: the processing loop

### Capture

- Save only what **resonates** — roughly 10% of the source, not a verbatim copy.
- Always include the source reference (URL, book + page, person, meeting).
- Capture is quick and minimally processed — you are not organizing yet.

### Organize

- File into PARA immediately using the decision procedure above.
- Link to related notes with `[[wiki-links]]`. An isolated note is half useless.
- Use consistent naming; match nearby files rather than inventing a new style.

### Distill

Distillation is **progressive** — it happens on re-reads, not at capture.

- First pass: bold the key claims or names.
- Second pass: highlight the crucial insight or the single sentence that matters.
- Third pass: add a short summary at the top (1–3 lines) and, if useful, a "so what?" line.

Don't distill on first write. Capture now, distill later.

### Express

- Reuse distilled notes when producing new output (a blog post, a design doc, a meeting brief, a weekly review).
- Cross-link: when you write something new that leans on an existing note, drop a `[[link]]` both ways.

## Conventions

- Prefer updating an existing hub page (`@Area`, `+project`) over creating a new top-level note.
- Wiki-links, not markdown links, for internal cross-references: `[[+project-slug]]`, `[[@Area 🔵]]`, `[[note-name|Display Text]]`.
- Preserve emojis in filenames and links exactly — they are part of the link target.
- Keep captures short and linked. Expansion is the job of the Distill and Express passes.

## Maintenance cadence

- **Weekly**: review the active `+project` set; archive anything that shipped.
- **Monthly**: skim `@Area` pages; update their state and outstanding commitments.
- **Quarterly**: prune `resources/` and `archive/`; remove duplicates.

## References

- `second-brain/{projects,areas,resources,archive}/` — live data.
- `AGENTS.md` (repo root) — vault-wide conventions for file naming, linking, and emoji handling.
- `.agents/skills/vault-update/SKILL.md` — hub-page refreshes from scattered notes.
