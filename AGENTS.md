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
  Tasks 💼.md           # Inbox + Ground-level task list (main todo file)
  daily/                # Daily journal entries (YYYY-MM-DD.md)
  meetings/             # Meeting notes
  contacts/             # Contact cards
  places/               # Location notes
  stickies/             # Quick ephemeral notes
  workflow/             # GTD reference docs (Horizons of Focus, Recurring, Ticklers)

second-brain/           # PARA: topic-based knowledge
  projects/             # Active, time-bound initiatives (prefix: +)
  areas/                # Ongoing responsibilities (prefix: @)
  resources/            # Reference material by topic
  archive/              # Completed / inactive items

templates/              # Foam templates for new notes (task, project, meeting, etc.)
attachments/            # Images and binary assets referenced from notes

.agents/                           # Coding Agent surface
  skills/                          # Intent-triggered skills (see "Skills" below)
```

## Skills

Six intent-triggered skills live under `.agents/skills/`. The coding agent invokes a skill when the user's request matches its `description`; you don't need to name the skill explicitly.

**Editing one note:**

- **`gtd-task-workflow`** — capture, clarify, route, and format tasks in `my/Tasks 💼.md`. Triggers on todo/inbox/planning/delegation phrasings.
- **`second-brain-organize`** — file and distill knowledge notes under `second-brain/` via PARA + CODE. Triggers on "where does this go", filing, categorizing, summarizing, archiving.

**Authoring artifacts:**

- **`async-meeting`** — draft a structured three-phase async-meeting document (Presentation → Discussion → Conclusion) with owners and deadlines.
- **`mindmap-generator`** — convert text into a markmap.js-compatible hierarchical mind map with the required frontmatter.

**Working across the vault:**

- **`vault-review`** — cadence-driven summary across multiple notes: daily shutdown, weekly review, monthly area check. Reports what shipped/slipped and what to commit next.
- **`vault-synthesize`** — gather scattered mentions of a topic, project, or area from across the vault and either answer a retrieval question or regenerate a hub page from them.

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

### Todo list sections (in `my/Tasks 💼.md` or `my/todo.md`)

Tasks flow through these buckets in order. Keep the section headings and emojis intact:

1. `📥 Inbox` — unprocessed capture
2. `🍅 Now (1)` — single current focus
3. `📆 Today`
4. `🗓 This Week`
5. `🗒 Later`
6. `🗓 Ticklers` — date-triggered reminders
7. `⏳ Wait/Delegated`
8. `🤔 Someday/Maybe`
9. `✅ Done`

## GTD Workflow (editing `my/**`)

**Horizons of Focus** are layered priorities — skim `my/workflow/Horizons of Focus 🌅.md` before making strategic changes:

- **H5 Purpose** — core mission & values
- **H4 Vision** — 3–5 year outlook
- **H3 Goals** — 1–2 year outcomes
- **H2 Areas** — 12-month focus (linked from `@Area` pages)
- **H1 Projects** — <12-month commitments (linked from `+project` pages)
- **Ground** — actionable tasks in `Tasks 💼.md`

**Capture → Process → Organize**: new items land in the Inbox section, get clarified (outcome? priority? context? deadline?), then routed:

- < 2 min → do it now, mark done
- Delegatable → `Wait/Delegated`
- Multi-step → create/update a `+project` page, link the task to it
- Timed → `Today` / `This Week` / `Later`
- Not actionable → delete or move to `Someday/Maybe`

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
- Flag items that look like GTD tasks (action + owner) so the user can route them into `Tasks 💼.md`; do not add them there automatically unless instructed.

There is no test suite, no linter, no build. Verification is reading the rendered note in Foam/Obsidian and confirming links resolve.

## When Editing

- For any non-trivial edit in `my/**` or `second-brain/**`, consult the matching Skill (`gtd-task-workflow` / `second-brain-organize`) and follow it.
- Preserve existing emojis, wiki-links, priority markers, and section headings exactly — they are semantically meaningful to the user's system and to Foam's graph.
- Prefer updating the appropriate existing hub page (`@Area`, `+project`, `Tasks 💼.md`) over creating new top-level notes.
- When adding a task, place it in the correct bucket; don't dump everything into Inbox unless it's genuinely unprocessed.
- When capturing knowledge, decide PARA placement before writing — don't leave orphan notes at the root of `second-brain/`.
- Keep captures short and linked. Distillation happens over time, not at capture.
