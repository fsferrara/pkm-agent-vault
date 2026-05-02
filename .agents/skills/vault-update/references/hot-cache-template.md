# Hot-cache template

Sections to append to `my/daily/@Journal 📔.md` if missing. Preserve the existing `#context#journal` tag line, the `#Journal :> …` lead, and the `## Context Content` section above these.

Target total length of the four tables: **~80 lines**. If the cache grows beyond that, demote the least-used rows to their deep-memory files (`my/contacts/`, `second-brain/projects/`, `my/@Glossary 📖.md`).

```markdown
## People

Top ~30 contacts. Fuller profiles in `my/contacts/<name>.md`.

| Who | Role / how we work together |
|-----|-----------------------------|
|     |                             |

→ Full contact pages: `my/contacts/` · Hub: [[@Contacts 👥]]

## Terms

Acronyms and shorthand. Full glossary in [[@Glossary 📖]].

| Term | Meaning |
|------|---------|
|      |         |

→ Full glossary: [[@Glossary 📖]]

## Projects

Currently-active projects. Details on each `+project` page.

| Name | What |
|------|------|
|      |      |

→ Project pages: `second-brain/projects/` · Hub per area: [[@Projects 💻]]

## Preferences

My own working preferences — how I like meetings, messages, focus time, etc.

-
```

## Promotion / demotion rules

- **Promote** a row into the hot cache when the entity is referenced ≥ 3 times across recent tasks or daily notes.
- **Demote** (remove the hot-cache row; keep the deep-memory file) when the project ships, the person stops being a frequent contact, or the term falls out of use.
- The hot cache should reflect the last ~30 days of activity — it is not an archive.
