# AI-Powered PKM Vault for Knowledge Workers

A personal knowledge-management vault built from markdown, designed to be opened in [Foam](https://foambubble.github.io/) or [Obsidian](https://obsidian.md/) **and** to be edited by a coding agent that understands your workflow.

Two methodologies, one vault:

- **GTD (Getting Things Done)** under [`my/`](my/) — task and workflow management.
- **Second Brain (PARA + CODE)** under [`second-brain/`](second-brain/) — knowledge organization.

## Start here

- [`AGENTS.md`](AGENTS.md) — primary instructions for coding agents. Read this first; it is the source of truth for vault conventions (file naming, wiki-links, emoji handling, task format, memory model).
- [`CONTRIBUTING.md`](CONTRIBUTING.md) — how to add new skills.
- [`CONNECTORS.md`](CONNECTORS.md) — MCP connector conventions (tool-agnostic placeholders).

## Layout

```
my/                    # GTD: personal task & workflow management
  todo.md              # Ground-level task list
  inbox.md             # Free-form unprocessed capture
  daily/               # Daily journal (YYYY-MM-DD.md) + @Journal hot-cache hub
  contacts/            # People (deep memory)
  meetings/            # Meeting notes
  places/              # Location notes
  stickies/            # Quick ephemeral notes
  workflow/            # Horizons of Focus, Recurring Tasks, Ticklers

second-brain/          # PARA: topic-based knowledge
  projects/            # Active, time-bound initiatives (+slug)
  areas/               # Ongoing responsibilities (@Area 🔵)
  resources/           # Reference material by topic
  archive/             # Completed / inactive items

templates/             # New-note templates (task, project, meeting, etc.)
attachments/           # Images and binary assets
.agents/skills/        # Intent-triggered skills for the coding agent
```

## Agent skills

Three skills live under [`.agents/skills/`](.agents/skills/). The coding agent picks one automatically based on your request — you don't need to name the skill.

- **`gtd-task-workflow`** — edit `my/todo.md`: capture, clarify, route tasks into the seven buckets.
- **`second-brain-organize`** — file and distill knowledge notes under `second-brain/` using PARA + CODE.
- **`vault-update`** — cross-vault operations: daily / weekly / monthly reviews, topic retrieval, hub-page refresh, stale-task triage, memory-gap fill.

## Two-tier memory

The agent decodes shorthand ("ask Todd about PSR") by consulting a two-tier memory described in `AGENTS.md`:

- **Hot cache** — top-of-mind people, terms, projects, and preferences in `my/daily/@Journal 📔.md`.
- **Deep memory** — full entries under `my/contacts/`, `second-brain/projects/`, `my/@Glossary 📖.md`, and `my/@Company 🏢.md`. (The glossary and company files are created lazily on the first `vault-update` run.)

## Editor setup

Configuration for Foam lives under [`.vscode/`](.vscode/). Obsidian users can open the vault directly; wiki-links and daily notes just work.

## License

[MIT](LICENSE.md).
