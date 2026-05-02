---
name: memory-management
description: Two-tier memory system that decodes the user's shorthand — names, nicknames, acronyms, codenames, internal terms — so Claude can act on requests like a colleague would. Covers looking up a person/project/term ("who is Todd", "what is PSR", "what's +phoenix"), recording a new fact ("remember X = Y", "add this person", "capture this term"), and promoting/demoting entries between the hot cache in `my/daily/@Journal 📔.md` and deep memory in `my/contacts/`, `second-brain/projects/`, `my/@Glossary 📖.md`, and `my/@Company 🏢.md`. Use this skill whenever the user wants to decode shorthand, look up who someone is or what a term means, teach Claude a new person/project/acronym, or reorganize the hot cache — even if they don't say "memory".
---

# Memory Management

The two-tier memory that lets the agent decode the user's shorthand ("ask todd about PSR for phoenix") into full context (Todd Martinez, Pipeline Status Report, the +phoenix migration). Hot cache is a compact summary inside the daily-notes hub; deep memory is the full knowledge base, spread across the vault's existing homes — no parallel `memory/` tree.

## When to use

Invoke this skill when the request is about the memory layer itself:

- **Lookup** — "who is Alice", "what does PSR stand for", "what's +phoenix about", "do I have a note on <name>".
- **Capture** — "remember: PSR = Pipeline Status Report", "add Alice to my contacts", "Todd's nickname is T", "start a project page for phoenix".
- **Hot-cache maintenance** — "promote Alice to the hot cache", "drop the phoenix row — it shipped", "the hot cache is too long".
- **Decode before acting** — any time the user's message contains an unfamiliar name / acronym / `+slug` that needs resolving before the agent can do the real task.

## Do not use

Defer to a more specific skill when the request is broader:

- Stale-task scans, cross-vault review, cadence reviews, hub refreshes → `vault-update` (which calls into this skill for the decode step).
- Task capture or routing → `gtd-task-workflow`.
- Filing a knowledge note or archiving a project → `second-brain-organize`.

## Architecture

```
my/daily/@Journal 📔.md     ← hot cache (four tables, ~80 lines total)
my/contacts/<name>.md       ← deep memory: people
second-brain/projects/+<slug>.md  ← deep memory: projects
my/@Glossary 📖.md          ← deep memory: acronyms, codenames, nicknames
my/@Company 🏢.md           ← deep memory: teams, tools, recurring processes
```

### Hot cache — `my/daily/@Journal 📔.md`

Four tables appended below the existing `## Context Content` section. Target total length: **~80 lines**.

- `## People` — top ~30 contacts.
- `## Terms` — most-used acronyms and shorthand.
- `## Projects` — currently-active `+project` entries.
- `## Preferences` — working preferences (meeting style, focus windows, etc.).

If any of the four sections are missing, append them from `.agents/skills/vault-update/references/hot-cache-template.md`.

### Deep memory

| Tier | Location | Notes |
|---|---|---|
| People | `my/contacts/<name>.md` | Hub: `[[@Contacts 👥]]`. Lowercase-hyphenated filenames. |
| Projects | `second-brain/projects/+<slug>.md` | Leading `+`, lowercase slug. |
| Glossary (acronyms, codenames, nicknames) | `my/@Glossary 📖.md` | Created lazily by `vault-update`. |
| Company context (teams, tools, processes) | `my/@Company 🏢.md` | Created lazily by `vault-update`. |

## Lookup order

Always decode every unfamiliar name / acronym / `+slug` in the user's request before acting. Stop as soon as you find a match.

1. **Hot cache** — the four tables in `my/daily/@Journal 📔.md`. Covers ~90% of daily decoding.
2. **Deep memory — people** — `my/contacts/<name>.md`.
3. **Deep memory — projects** — `second-brain/projects/+<slug>.md`.
4. **Deep memory — terms / company** — `my/@Glossary 📖.md`, then `my/@Company 🏢.md`.
5. **Ask the user** — once they answer, write the answer to the correct deep-memory file (see §Capture).

Example:

```
User: "ask todd about PSR for +phoenix"

Lookup:
  todd     → hot cache ## People        → Todd Martinez, Finance lead ✓
  PSR      → hot cache ## Terms         → Pipeline Status Report ✓
  +phoenix → hot cache ## Projects      → DB migration, Q2 launch ✓
```

If a lookup needs rich detail (e.g. drafting an email to Todd), follow through to `my/contacts/todd.md` or `second-brain/projects/+phoenix.md` — the hot cache is for parsing, the deep file is for execution.

## Capture

When the user says "remember X = Y", "X is a person / project / term", or supplies context on an unknown entity, write to the right deep-memory file. **Never auto-add** — confirm with the user before creating or editing any of these files.

### Person → `my/contacts/<name>.md`

Use `templates/contact.md` when present (the template has `#contact` tag + Company / Location / Email / Related resources / Notes sections). Otherwise a minimal stub:

```markdown
#contact

- Role: <role>
- How we know each other: <team / project / history>
- Nicknames: <comma-separated>

## Notes

-
```

Filename: lowercase, hyphenated (`todd-martinez.md`, not `Todd Martinez.md`). Add the contact to the `## People` table in `@Journal 📔.md` only if the promotion rule applies (see below).

### Project → `second-brain/projects/+<slug>.md`

Use `templates/project-home.md` when present (it has `#project` tag + Quick links / Next Tasks / Milestones / Pages / References / Quick Notes). Otherwise a minimal stub:

```markdown
#project

- Goal: <desired outcome>
- Status: <active / paused / shipped>
- Related area: [[@Area 🔵]]
- Related people: [[<name>]]

## Next Tasks

-
```

Filename: leading `+`, lowercase slug (`+phoenix.md`). If the project is currently active, add a row to the `## Projects` table in `@Journal 📔.md`.

### Term / acronym / nickname → `my/@Glossary 📖.md`

Append a row to the relevant section (Acronyms / Internal Terms / Nicknames / Project Codenames). If the term is used ≥ 3 times recently, also add it to `## Terms` in `@Journal 📔.md`.

### Company / team / tool context → `my/@Company 🏢.md`

Append to the matching section (Tools, Teams, Processes). Rarely promoted to the hot cache — company context belongs in deep memory.

### Preference → `## Preferences` in `@Journal 📔.md`

Preferences live only in the hot cache (no deep-memory equivalent). Add a short bullet.

## Promotion / demotion

Keep the hot cache at ~80 lines total across the four tables — it is **not** an archive.

- **Promote** a deep-memory entry into the hot cache when the entity is referenced ≥ 3 times across recent tasks (`my/todo.md`) or the last ~30 daily notes. Add one row; keep the deep file as the source of truth.
- **Demote** (remove the hot-cache row; keep the deep-memory file) when:
  - A project ships or is archived.
  - A contact stops being a frequent interlocutor.
  - A term falls out of use (not seen in recent tasks / daily notes).

Promotion and demotion are run opportunistically during lookups, or explicitly during `vault-update`'s `triage` mode.

## Never auto-add

Do not create or edit files in these locations without the user confirming the specific entry first:

- `my/contacts/**`
- `second-brain/projects/+*.md`
- `my/@Glossary 📖.md`
- `my/@Company 🏢.md`
- The four hot-cache tables in `my/daily/@Journal 📔.md`

Appending the four *section headers* themselves (lazy bootstrap) is allowed and should be announced to the user; adding *rows* is not.

## References

### Files this skill reads or writes

- `my/daily/@Journal 📔.md` — hot cache host.
- `my/contacts/<name>.md` — deep memory: people.
- `second-brain/projects/+<slug>.md` — deep memory: projects.
- `my/@Glossary 📖.md` — deep memory: terms.
- `my/@Company 🏢.md` — deep memory: company / teams / tools.
- `templates/contact.md`, `templates/project-home.md` — capture templates.
- `.agents/skills/vault-update/references/hot-cache-template.md` — hot-cache section template.

### Related skills

- `.agents/skills/vault-update/SKILL.md` — cross-vault reviews + `triage` mode runs the gap-fill against this skill's rules.
- `.agents/skills/gtd-task-workflow/SKILL.md` — calls into this skill before routing tasks that mention unknown entities.
- `.agents/skills/second-brain-organize/SKILL.md` — calls into this skill before filing notes that reference unknown people / projects.

### Conventions

- `AGENTS.md` (repo root) — vault-wide conventions (buckets, priorities, emojis, wiki-links).
