---
name: vault-review
description: Run a structured review of the vault across the three GTD/PARA cadences — daily shutdown, weekly review, and monthly area check. Reads across `my/Tasks 💼.md`, recent daily notes, `@Area` hubs, and active `+project` pages, then produces a concise summary: what shipped, what slipped, what's committed next, and recommendations (items to archive, promote, or surface into the right bucket). Use this skill whenever the user asks to "review my day/week/month", "do a shutdown", "weekly review", "month-end check", "what did I get done", "what's outstanding", "prepare my plan for tomorrow/next week", or any phrasing that implies reflecting across multiple notes rather than editing one — even if they don't name GTD or a specific cadence.
---

# Vault Review

## When to use

Invoke this skill when the user wants a structured look across the vault rather than an edit to a single note. Representative phrasings:

- "Let's do a daily shutdown."
- "Run my weekly review."
- "Month-end check on my areas."
- "What did I actually ship this week?"
- "What's still open on my projects?"
- "Prepare a plan for tomorrow / next Monday."

Do **not** use this skill for:

- Adding or routing a single task → `gtd-task-workflow`.
- Filing a new knowledge note → `second-brain-organize`.
- Pulling together scattered content on a specific topic or regenerating a hub page → `vault-synthesize`.

## Pick the cadence first

Ask or infer which cadence the user wants — the scope changes a lot.

| Cadence | Window | Primary sources | Time budget |
|---|---|---|---|
| **Daily shutdown** | today | `my/Tasks 💼.md`, today's `my/daily/YYYY-MM-DD.md`, yesterday's daily note | ~5 minutes |
| **Weekly review** | last 7 days | above + the week's daily notes, active `+project` pages, `my/workflow/Recurring Tasks 🔁.md`, `Ticklers 🗓️.md` | ~20 minutes |
| **Monthly** | last 30 days | above + `@Area` hubs under `second-brain/areas/`, `+project` pages under `second-brain/projects/`, archive candidates | ~45 minutes |

If the user is ambiguous ("review my stuff"), default to **weekly** — it's the highest-leverage cadence.

## Read order (always this order)

The goal is to read just enough to produce the summary, not to re-read the whole vault. Stop as soon as you have enough signal.

1. **`my/Tasks 💼.md`** — the authoritative task list. Note what's in `Done`, `Now`, `Today`, `This Week`, `Later`, `Wait/Delegated`, `Someday/Maybe`.
2. **Daily notes in scope** — today for daily; last 7 date-named files for weekly; last ~30 for monthly. Use `ls my/daily/ | sort | tail -N` rather than globbing.
3. **Active `+project` pages** — the ones linked from the `### 💻 @Projects` section of `Tasks 💼.md`. Skim for status, blockers, and recently completed next actions.
4. **`@Area` hubs** (monthly only) — read each `second-brain/areas/@Area 🔵.md` once. Note areas that feel stale or have drifted off their goals.
5. **Horizons reference** (monthly only) — `my/workflow/Horizons of Focus 🌅.md` for whether current commitments still align with the annual/quarterly focus.

Use `rg` to scan efficiently — e.g., `rg -l '+project-slug' my/daily/` to find which days touched a project.

## What to produce

The output is a written summary, not edits to the vault. The user decides what to change based on it. Keep it scannable.

### Daily shutdown

```markdown
# Daily shutdown — YYYY-MM-DD

## Shipped today
- …

## Carried over / slipped
- …

## Tomorrow's Now (1) & Today candidates
- …

## Captures worth routing
- (items from today's daily note that look like real tasks or knowledge notes — suggest bucket, don't route automatically)

## Flags
- (anything stale, anything that looks blocked, anything urgent nobody's holding)
```

### Weekly review

```markdown
# Weekly review — week of YYYY-MM-DD

## Shipped
- Grouped by @Area / +project.

## Slipped or still open
- From This Week → call out each one, and whether it should stay This Week, move to Later, or go Someday.

## Waiting / delegated status
- Each `⏳` item with a nudge suggestion if it's been stuck.

## Projects snapshot
- For each active +project: one-line status (progressing / blocked / stale / near complete).

## Recurring + ticklers to pull in
- From Recurring Tasks 🔁 and Ticklers 🗓️ — which land in next week's This Week.

## Next week's candidates
- 3–7 items the user should commit to This Week. Don't commit for them.

## Recommendations
- Items to archive, promote, downgrade, or surface to an @Area page.
```

### Monthly

```markdown
# Monthly review — YYYY-MM

## Shipped (month)
- Headline outcomes grouped by @Area.

## @Area health
- One line per area: on-track / neglected / overloaded, with the evidence.

## Project portfolio
- Active, stalled, and done-this-month. Flag anything that should be archived.

## Horizons alignment
- Do the active commitments still reflect H2 Areas of Focus and H3 Goals? Call out drift.

## Someday/Maybe review
- 3–5 items worth reconsidering (to promote or to drop).

## Recommendations for next month
- The smallest set of changes that would have the biggest impact.
```

## Principles

- **Summarize, don't rewrite.** The user reviews; the vault stays intact unless they ask for a specific edit afterward.
- **Be honest about slippage.** If something's been in `This Week` for three weeks, say so and recommend demoting it.
- **Respect the buckets.** Don't invent new status labels — use the vocabulary the user already has (`Now`, `Today`, `This Week`, `Later`, `Wait/Delegated`, `Someday/Maybe`).
- **Cite sources.** When surfacing a pattern (e.g., "three days in a row you flagged this"), link to the daily notes.
- **Flag, don't decide.** For routing suggestions, recommend and let the user confirm. Exception: if the user says "do it", proceed.

## Follow-ups the user may ask for

After the summary, the user often wants one of:

- "Apply the recommendations" → hand off to `gtd-task-workflow` for task moves and `second-brain-organize` for PARA changes.
- "Regenerate the +project page" → hand off to `vault-synthesize`.
- "Start tomorrow's daily note" → create `my/daily/YYYY-MM-DD.md` with the `#Journal` tag and seed it with the items you identified.

## References

- `my/Tasks 💼.md` — task state.
- `my/workflow/Horizons of Focus 🌅.md` — strategic frame.
- `my/workflow/Recurring Tasks 🔁.md`, `Ticklers 🗓️.md` — pickers for weekly/monthly.
- `AGENTS.md` (repo root) — conventions (buckets, priorities, emojis, wiki-links).
- `.agents/skills/gtd-task-workflow/SKILL.md` — hand-off for task edits.
- `.agents/skills/vault-synthesize/SKILL.md` — hand-off for hub-page regeneration.
