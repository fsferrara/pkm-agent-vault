# AGENTS.md

This file provides guidance to coding agents when working with code in this repository.

## Repository Purpose

This is a **personal knowledge management (PKM) vault** built from markdown files. It is not a software project — there is nothing to build, lint, or test. Changes are prose edits to `.md` files that implement two complementary methodologies:

- **GTD (Getting Things Done)** under `my/` — task and workflow management
- **Second Brain (PARA + CODE)** under `second-brain/` — knowledge organization

The vault is designed to be opened in [Foam](https://foambubble.github.io/) (VS Code extension) or [Obsidian](https://obsidian.md/), which both render `[[wiki-links]]` and provide graph navigation. Configuration for each lives in `.vscode/` and `.obsidian/`.

## High-Level Structure

```
my/                     # GTD: personal task & workflow management
  todo.md               # Ground-level task list (main todo file)
  inbox.md              # Free-form unprocessed capture
  @Glossary 📖.md       # Deep memory: acronyms, codenames, nicknames
  @Company 🏢.md        # Deep memory: teams, tools, recurring processes
  daily/                # Daily journal entries (YYYY-MM-DD.md)
    @Journal 📔.md      # Daily-notes hub + hot-cache for memory
  meetings/             # Meeting notes
  contacts/             # Contact cards (deep memory: people)
  places/               # Location notes
  stickies/             # Quick ephemeral notes
  workflow/             # GTD reference docs (Horizons of Focus, Recurring, Ticklers)

second-brain/           # PARA: topic-based knowledge
  projects/             # Active, time-bound initiatives (prefix: +). Also deep memory: projects.
  areas/                # Ongoing responsibilities (prefix: @)
  resources/            # Reference material by topic
  archive/              # Completed / inactive items

templates/              # Foam templates for new notes (task, project, meeting, etc.)
attachments/            # Images and binary assets referenced from notes

.agents/                           # Coding Agent surface
  skills/                          # Intent-triggered skills (see "Skills" below)
```

## Skills

Five intent-triggered skills live under `.agents/skills/`. The coding agent invokes a skill when the user's request matches its `description`; you don't need to name the skill explicitly.

**Editing one note:**

- **`gtd-task-workflow`** — capture, clarify, route, and format tasks in `my/todo.md`. Triggers on todo/inbox/planning/delegation phrasings.
- **`second-brain-organize`** — file and distill knowledge notes under `second-brain/` via PARA + CODE. Triggers on "where does this go", filing, categorizing, summarizing, archiving.

**Working across the vault:**

- **`vault-update`** — the one cross-vault skill. Runs six modes chosen from the user's phrasing:
  - `cadence:daily` — daily shutdown + plan for tomorrow.
  - `cadence:weekly` — weekly review: shipped / slipped / waiting / projects snapshot / next-week candidates.
  - `cadence:monthly` — month-end: @Area health, project portfolio, Horizons alignment.
  - `topic:retrieval` — "what have I written about X" across the whole vault.
  - `topic:refresh` — regenerate a `+project` or `@Area` hub-page overview from scattered mentions.
  - `triage` — stale-task flagging in `my/todo.md` + memory-gap fill for unknown people, projects, acronyms.

  No external sync — vault-local only.

Each skill contains action-oriented, imperative instructions — step-by-step workflow, the exact formats/templates to produce, and cross-references to the live vault files. The methodology background (GTD horizons, PARA philosophy, etc.) is in this file.

## Core Conventions

### File naming

- `descriptive-name.md` for regular notes (lowercase, hyphenated).
- `@Category 🔵.md` for **areas / contexts** (leading `@`, trailing emoji). These are hub pages.
- `+project-slug.md` for **projects** (leading `+`, lowercase slug). These represent active work.
- Daily notes: `YYYY-MM-DD.md` inside `my/daily/`.
- Emojis are part of filenames and links — preserve them exactly when editing references.

### Linking

Use Foam/Obsidian wiki-link syntax, not standard markdown links, for internal cross-references:

```markdown
[[+project-slug]]
[[@Area 🔵]]
[[note-name|Display Text]]
```

### Task format (todo.txt-style)

```markdown
- [ ] (A) Task description @Context +project due:YYYY-MM-DD
```

Priorities follow the Eisenhower Matrix:

- `(A)` Urgent & Important — do now
- `(B)` Important, not urgent — schedule
- `(C)` Urgent, not important — delegate / batch
- `(D)` Neither — minimize or drop

### Todo list sections (in `my/todo.md`)

Tasks flow through these buckets in order. Keep the section headings and emojis intact. The seven canonical buckets reflect the current file:

1. `✅ Completed`
2. `🍅 Now/Pomodoro (1)` — single current focus
3. `📆 Today`
4. `🗓 This Week`
5. `🗒 Later`
6. `⏳ Waiting For (delegated)`
7. `🤔 Someday/Maybe`

`🗓 Ticklers` is optional — `gtd-task-workflow` adds it the first time the user needs it.

Unprocessed capture goes to `my/inbox.md`, not a todo bucket.

## GTD Workflow (editing `my/**`)

**Horizons of Focus** are layered priorities — skim `my/workflow/Horizons of Focus 🌅.md` before making strategic changes:

- **H5 Purpose** — core mission & values
- **H4 Vision** — 3–5 year outlook
- **H3 Goals** — 1–2 year outcomes
- **H2 Areas** — 12-month focus (linked from `@Area` pages)
- **H1 Projects** — <12-month commitments (linked from `+project` pages)
- **Ground** — actionable tasks in `my/todo.md`

**Capture → Process → Organize**: new items land in `my/inbox.md`, get clarified (outcome? priority? context? deadline?), then routed into one of the seven `my/todo.md` buckets:

- < 2 min → do it now, mark done (`✅ Completed`)
- Delegatable → `⏳ Waiting For (delegated)`
- Multi-step → create/update a `+project` page, link the task to it
- Timed → `📆 Today` / `🗓 This Week` / `🗒 Later`
- Not actionable → delete or move to `🤔 Someday/Maybe`

## Second Brain Workflow (editing `second-brain/**`)

**PARA** decides location; **CODE** decides process.

**PARA** — choose the category by actionability, not topic:

- **Projects**: has a defined outcome and end date
- **Areas**: ongoing standard to maintain (no end date)
- **Resources**: topic of interest / reference
- **Archive**: inactive from any of the above

When a project completes → move to `archive/`. When an area becomes time-bound → promote to `projects/`.

**CODE** — the processing loop:

1. **Capture** only what resonates (≈10% of source); include the source link.
2. **Organize** into PARA immediately; link to related notes.
3. **Distill** progressively — bold key points on re-read, summarize on next touch.
4. **Express** — reuse distilled notes when producing new output.

## Daily Notes

Daily notes live in `my/daily/YYYY-MM-DD.md`. They serve as a running journal for the day: ephemeral captures, scratch work, links to things done, and meeting/context dumps that may later be promoted into a hub page, a `+project`, or `second-brain/resources/`.

Conventions:

- Filename is the ISO date: `2026-05-02.md`. The daily-note hub is `my/daily/@Journal 📔.md`.
- The file is tagged `#Journal` (see `templates/daily-note.md`).
- Content is free-form — short narrative, bulleted observations, code snippets, or quoted logs are all fine.
- Link outward: reference `[[+project]]`, `[[@Area]]`, or meeting/contact notes rather than duplicating their content.
- Treat the daily note as **capture**, not archive. Anything durable should migrate into the relevant hub or `second-brain/` location during processing.

When an AI agent is asked to take or update daily notes:

- Append to **today's** note (create `my/daily/YYYY-MM-DD.md` if missing) rather than editing past dates.
- Keep the existing `#Journal` tag at the top; add new content below without rewriting what's already there.
- Use timestamps (`## 14:30` or `- 14:30 — …`) when logging events across the day so entries stay chronological.
- Summarize external material rather than pasting long logs verbatim; include a link to the source.
- Flag items that look like GTD tasks (action + owner) so the user can route them into `my/todo.md`; do not add them there automatically unless instructed.

There is no test suite, no linter, no build. Verification is reading the rendered note in Foam/Obsidian and confirming links resolve.

## Memory (two-tier)

The agent decodes your shorthand ("ask todd about PSR") by consulting a two-tier memory. **Hot cache** is a short, high-traffic summary living inside the daily-notes hub. **Deep memory** is the full knowledge base, spread across the vault's existing homes instead of a parallel `memory/` tree.

### Hot cache

`my/daily/@Journal 📔.md` — four tables appended below the existing `## Context Content` section:

- `## People` — top ~30 contacts.
- `## Terms` — most-used acronyms and shorthand.
- `## Projects` — currently-active `+project` entries.
- `## Preferences` — working preferences (meeting style, focus windows).

Target total for the four tables: ~80 lines. When it grows beyond that, demote to deep memory.

### Deep memory

| Tier | Location |
|---|---|
| People | `my/contacts/<name>.md` (hub: [[@Contacts 👥]]) |
| Projects | `second-brain/projects/+<slug>.md` |
| Glossary (acronyms, codenames, nicknames) | `my/@Glossary 📖.md` |
| Company context (teams, tools, processes) | `my/@Company 🏢.md` |

### Lookup order

1. Hot cache (`@Journal 📔.md` tables).
2. Deep memory — people / projects / glossary / company context.
3. Ask the user; on answer, write to deep memory.

### Promotion / demotion

- **Promote** a deep-memory entry to the hot cache when the entity is referenced ≥ 3 times across recent tasks or daily notes.
- **Demote** (remove the hot-cache row; keep the deep-memory file) when the project ships, a contact stops being frequent, or a term falls out of use.
- The `vault-update` skill runs the bootstrap, lookup, and gap-fill — it's the surface that keeps memory alive.

## When Editing

- For any non-trivial edit in `my/**` or `second-brain/**`, consult the matching Skill (`gtd-task-workflow` / `second-brain-organize`) and follow it.
- For cross-note reviews, retrievals, hub refreshes, or triage, use `vault-update`.
- Preserve existing emojis, wiki-links, priority markers, and section headings exactly — they are semantically meaningful to the user's system and to Foam's graph.
- Prefer updating the appropriate existing hub page (`@Area`, `+project`, `my/todo.md`) over creating new top-level notes.
- When adding a task, place it in the correct bucket; don't dump everything into `my/inbox.md` unless it's genuinely unprocessed.
- When capturing knowledge, decide PARA placement before writing — don't leave orphan notes at the root of `second-brain/`.
- Keep captures short and linked. Distillation happens over time, not at capture.
