---
name: vault-synthesize
description: Pull together scattered mentions of a topic, project, area, or person from across the vault (daily notes, meeting notes, `+project` pages, `@Area` hubs, resources, archive) and produce either a retrieval answer ("what have I written about X") or a regenerated hub-page section that consolidates the findings. Uses ripgrep + wiki-link tracing to find connections the user may have forgotten. Use this skill whenever the user asks "what have I written about…", "what do I know about…", "show me everything on…", "find my notes on…", "refresh / regenerate / update the overview for [project/area]", "what links to this", "connect the dots on…", or wants an `@Area` / `+project` hub page rebuilt from scattered source material — even if they don't name retrieval or synthesis explicitly.
---

# Vault Synthesize

## When to use

Invoke this skill for two related tasks that share the same underlying mechanic (find-and-aggregate across the vault):

- **Retrieval**: the user wants a read-only answer built from existing notes. "What have I written about performance budgets?" "Pull everything on Jordan." "What do my notes say about the write-book project?"
- **Dashboard refresh**: the user wants a hub page regenerated. "Refresh the `@Career 🧑‍💼` page." "Rebuild the overview of `+launch-website` from the scattered mentions." "The `@Health 💪` page is stale — bring it up to date."

Do **not** use this skill for:

- Filing a new note → `second-brain-organize`.
- Editing a single task or bucket → `gtd-task-workflow`.
- Summarizing time windows (day/week/month) rather than a topic → `vault-review`.
- Creating a mind map from existing text → `mindmap-generator`.

If the user is ambiguous, ask: "Do you want the findings as a summary to read, or should I rewrite the `@X`/`+X` page itself?"

## Sources to search (in priority order)

Wherever a topic can hide, look. The vault has six homes for content and a topic often spans several:

1. **Active task state** — `my/Tasks 💼.md` (search for the `+project`, `@Area`, or bare keyword).
2. **Daily notes** — `my/daily/YYYY-MM-DD.md` files. These are where fresh context lives and hub pages go stale by comparison.
3. **Meetings** — `my/meetings/**/*.md`.
4. **Project pages** — `second-brain/projects/+slug.md` and any subfolder notes inside `second-brain/projects/<slug>/`.
5. **Area hubs and sub-notes** — `second-brain/areas/<name>/**` and `second-brain/areas/@Area 🔵.md`.
6. **Resources and archive** — `second-brain/resources/**` and `second-brain/archive/**`. Last because they're less likely to be current, but they often hold the original reference material.

## Search mechanics

Use ripgrep. The vault is markdown with wiki-links, so the same topic appears under multiple forms.

For a topic `X`, search for all plausible forms in one pass:

```bash
rg -n --glob '*.md' \
   -e 'X' \
   -e '\[\[.*X.*\]\]' \
   -e '#X' \
   /Users/fferrara/github/fsferrara/brain
```

Adapt to the exact form of the thing:

- Project: `rg -n '\[\[\+project-slug\]\]' …` — wiki-links are the strongest signal.
- Area: `rg -n '\[\[@Area Name 🔵\]\]' …` — include the emoji, it's part of the link target.
- Person or free-form keyword: plain `rg -i`.

Then **follow the links**. If a meeting note mentions the project, read the meeting note. If a daily note mentions a person, check whether that person has a contact page under `my/contacts/`.

Stop when new reads don't add anything new. You don't need every mention — you need representative ones.

## What to produce

### Retrieval output

A structured answer the user can read, not a file. Keep it short enough to skim.

```markdown
# What the vault says about [topic]

## Summary
2–4 sentences. What the user seems to believe or have committed to, based on the notes.

## Most recent state
What the freshest note (usually a daily note) says. Date it.

## Timeline (if useful)
- YYYY-MM-DD — one line — source link
- YYYY-MM-DD — one line — source link

## Commitments currently open
- Open tasks tagged with the topic, from `my/Tasks 💼.md`, with their bucket.
- Delegated items that mention the topic.

## Related notes
- [[+project-slug]]
- [[@Area 🔵]]
- [[meeting-or-resource-title]]

## Gaps or contradictions
- Anything inconsistent across sources (e.g. "the +project page says 'blocked on Alex' but the latest daily note says 'Alex confirmed Friday'").
```

Use wiki-links for everything internal, with the exact filename including emojis.

### Dashboard refresh output

A rewritten section of an existing hub page. Rules:

- **Confirm the target file before writing.** Show the path (`second-brain/projects/+slug.md` or `second-brain/areas/@Area 🔵.md`) and the section you plan to replace.
- **Preserve the file's existing frontmatter, headings, and unrelated sections.** Only update the overview/status section unless the user asked for more.
- **Keep it a hub, not an archive.** The hub page should link out to sources, not duplicate their content. If you find yourself copying paragraphs, link instead.
- **Note the regeneration date** at the top of the refreshed section so staleness is visible next time: `_Refreshed YYYY-MM-DD from N daily notes, M meetings, K tasks._`

Template for a refreshed `+project` overview:

```markdown
## Overview
_Refreshed YYYY-MM-DD._

**Goal.** One sentence.

**Current status.** Two or three sentences based on the freshest daily notes and active tasks.

**Open next actions.**
- `- [ ]` items from `my/Tasks 💼.md` tagged with this project.

**Recent activity.**
- YYYY-MM-DD — one-line update — [[source]]
- YYYY-MM-DD — …

**Related.**
- [[@Area 🔵]]
- [[supporting-resource]]
```

For an `@Area` refresh, use the same skeleton but swap "Goal" for "Standard I'm maintaining" and add a "Linked projects" section that enumerates the active `+project` pages connected to it.

## Principles

- **Evidence, not opinion.** Every claim in the output should be traceable to a file. Link it.
- **Fresh beats deep.** When daily notes and the hub page disagree, trust the daily notes — that's where real activity is captured. Surface the disagreement.
- **Don't invent.** If the vault doesn't say something, say "no mentions" rather than filling in a plausible-sounding summary.
- **Respect the capture layer.** Daily notes are messy on purpose. Don't reformat them or suggest moving their content — just read them.
- **Ask before destructive rewrites.** A hub-page refresh is editing a user file; show the planned change and ask before writing if the diff is large.

## Handoffs

After synthesizing, the user often wants one of:

- "These are tasks" → `gtd-task-workflow` to route them into `Tasks 💼.md`.
- "Archive this project" → `second-brain-organize`.
- "Let's review the week too" → `vault-review`.
- "Turn this into a mind map" → `mindmap-generator`.

## References

- Every file under `my/**` and `second-brain/**`.
- `AGENTS.md` (repo root) — conventions for wiki-links, emoji preservation, hub-page patterns.
- `.agents/skills/vault-review/SKILL.md` — related skill for cadence-based summaries.
