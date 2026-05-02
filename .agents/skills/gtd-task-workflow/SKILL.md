---
name: gtd-task-workflow
description: Capture, clarify, and route tasks through the GTD (Getting Things Done) workflow used in this vault. Formats each task as `- [ ] (Priority) description @Context +project due:YYYY-MM-DD` and places it in the correct bucket (Completed, Now/Pomodoro, Today, This Week, Later, Waiting For, Someday/Maybe — plus an optional Ticklers) inside `my/todo.md`. Unprocessed captures land in `my/inbox.md`. Use this skill whenever the user mentions adding a todo, processing an inbox item, clarifying a capture, planning the day or week, promoting/demoting tasks between buckets, delegating, reviewing Horizons of Focus, or asks "what should I do with X" — even if they don't name GTD explicitly.
---

# GTD Task Workflow

## When to use

Invoke this skill for any request that touches personal task management in `my/`. Representative phrasings:

- "Add a task to call the dentist."
- "Dump these items in my inbox: …"
- "Process my inbox."
- "What should I do with this email from Sara?"
- "Plan today / this week."
- "Move these to Later / Someday."
- "I'm waiting on Alex to approve the PR — track it."
- "Review my Horizons of Focus."

Do **not** use this skill for knowledge notes or reference material (that's `second-brain-organize`) or for daily-journal narrative (covered by the Daily Notes section of `AGENTS.md`).

## Where tasks live

- **`my/inbox.md`** — free-form unprocessed capture. Items here are raw one-liners; they get clarified and routed into `my/todo.md` during processing.
- **`my/todo.md`** — the single file containing every active and historical bucket. All task additions and moves happen here.
- **`my/workflow/Horizons of Focus 🌄.md`** — strategic layer (H5 Purpose → H1 Projects). Consult when the user is reviewing priorities or deciding whether something deserves to become a project.
- **`my/workflow/Recurring Tasks 🔁.md`** and **`Ticklers 🗓️.md`** — pickers for weekly/monthly reviews. Tasks from here get copied into the live buckets.

> For stale-task triage, memory-gap fill, or any cross-vault review (daily/weekly/monthly cadence, topic retrieval, hub-page refresh), defer to `vault-update`.
>
> When a task mentions a name / acronym / `+slug` you don't recognize, decode it first via `.agents/skills/memory-management/SKILL.md` (hot cache → deep memory → ask) rather than guessing.

## The workflow: Capture → Clarify → Organize

### 1. Capture

New captures land in `my/inbox.md` as free-form one-liners or disorganized notes — **anything goes**. No formatting, no labels, no priorities, no buckets at this stage; the goal is friction-free capture. Categorization happens later, during Clarify.

> `🗓 Ticklers` is an optional section that doesn't exist in `my/todo.md` by default. Add it in its canonical position the first time it's used; don't leave stub empty sections behind.

### 2. Clarify (one item at a time)

For each item in `my/inbox.md`, answer in order:

1. **What is it?** Is it actionable at all?
2. **Desired outcome?** What does "done" look like?
3. **Priority?** See the Eisenhower matrix below.
4. **Alignment?** Which `@Area` or `+project` does it serve? Tag it.
5. **Delegatable?** If yes, assign and track.
6. **Time required?** If < 2 minutes — just do it and mark `[x]`.
7. **Deadline?** If there's a hard date, add `due:YYYY-MM-DD`.

### 3. Organize (route to the right bucket)

| Situation | Destination |
|---|---|
| < 2 minutes to complete | Do it now, add to `## ✅ Completed` with `[x]` |
| Delegated or blocked on someone | `## ⏳ Waiting For (delegated)` with a `wait:<label>` tag |
| Multi-step work | Create or update a `+project` page in `second-brain/projects/`, then link the next physical action back into a bucket |
| Committed for today | `## 📆 Today` |
| Committed for this week | `## 🗓 This Week` |
| Actionable and committed, but not within the next week | `## 🗒 Later` |
| Idea without commitment yet (or not yet actionable) | `## 🤔 Someday/Maybe` |
| Not actionable and not worth keeping | Delete |

Maintain **one** item under `## 🍅 Now/Pomodoro (1)` — the current focus. Two only if absolutely necessary.

## Task format (todo.txt-flavored)

```markdown
- [ ] (A) Task description [[@Hub 🔵]] [[+project-slug]] due:YYYY-MM-DD
```

Rules:

- Leading `- [ ]` for open, `- [x]` for done.
- Priority is optional but, when present, uses a single uppercase letter in parens: `(A)` `(B)` `(C)` `(D)`.
- `[[@Hub 🔵]]` is a wiki-link to an existing hub page in the vault (e.g. `[[@Meetings 💼]]`, `[[@Projects 💻]]`, `[[@Journal 📔]]`). It is **always** a wiki-link — bare `@tokens` like `@computer` or `@home` are not contexts in this vault, they're broken links to files that don't exist. Only use a hub that already exists as a file; if none fits, drop the context slot rather than invent one. New hubs are created deliberately, not as a side effect of filing a task.

  Good: `- [ ] (B) Prep agenda for 1:1 [[@Meetings 💼]] [[+q2-planning]]`
  Bad:  `- [ ] (B) Prep agenda for 1:1 @meetings +q2-planning` (bare token, not a link)
  Bad:  `- [ ] (B) Pick TUI framework @computer +tui-tetris` (no `@Computer` hub exists)
- `[[+project-slug]]` is a wiki-link to an existing project page under `second-brain/projects/` (e.g. `[[+launch-website]]`, `[[+tui-tetris]]`). It is **always** a wiki-link — bare `+tokens` like `+launch-website` are broken references in this vault. Only use a project that already exists as a file; if none fits, drop the project slot rather than invent one. New projects are created deliberately (see `second-brain-organize`), not as a side effect of filing a task.

  Good: `- [ ] (B) Ship landing-page draft [[@Projects 💻]] [[+launch-website]]`
  Bad:  `- [ ] (B) Ship landing-page draft [[@Projects 💻]] +launch-website` (bare token, not a link)
  Bad:  `- [ ] (B) Pick TUI framework [[@Projects 💻]] +tui-tetris` (must be `[[+tui-tetris]]` — the file exists)
- `due:YYYY-MM-DD` uses ISO dates. No other date format.
- Preserve the emojis in section headings and wiki-links exactly — they are part of the link target.

## Priority (Eisenhower matrix)

- **(A)** Urgent & Important — do today (deadlines, emergencies).
- **(B)** Important, not urgent — schedule (planning, skill development, prevention).
- **(C)** Urgent, not important — batch or delegate (most meetings, many emails).
- **(D)** Neither — minimize or delete.

Only a handful of `(A)` tasks should coexist. If everything is `(A)`, nothing is.

## Bucket semantics (order in the file)

Always respect this order — Foam/Obsidian users navigate by scrolling. The **seven canonical buckets** in `my/todo.md` are:

1. `## ✅ Completed`
2. `## 🍅 Now/Pomodoro (1)`
3. `## 📆 Today`
4. `## 🗓 This Week`
5. `## 🗒 Later`
6. `## ⏳ Waiting For (delegated)`
7. `## 🤔 Someday/Maybe`

One optional bucket exists in the vocabulary but isn't in the file by default — add it the first time the user needs it, in its canonical position:

- `## 🗓 Ticklers` — between `🗒 Later` and `⏳ Waiting For (delegated)`, for date-triggered reminders.

Keep the headings and emojis exactly as they appear. If a heading is missing (other than the optional one above), don't invent a new one — the file uses this exact set.

## Horizons of Focus (strategic reviews)

When the user asks for a review or wants to reshape priorities, read `my/workflow/Horizons of Focus 🌄.md` and work top-down:

- **H5 Purpose** (life mission) — rarely touched.
- **H4 Vision** (3–5 years) — quarterly.
- **H3 Goals** (1–2 years) — quarterly.
- **H2 Areas of Focus** (12 months) — monthly. These are the `@Area` pages.
- **H1 Projects & Priorities** (< 12 months) — weekly. These are the `+project` pages.
- **Ground** — actionable tasks in `my/todo.md`.

A task without a connection to at least an `@Area` or `+project` is a candidate for deletion or `Someday/Maybe`.

## Routine cadence

The buckets form a pipeline; work flows toward `Today` and eventually `Completed`. The drag direction is always down the list (toward higher commitment, toward "now"):

```
Someday/Maybe  →  Later  →  This Week  →  Today  →  Now/Pomodoro  →  Completed
```

- **Daily (every morning)**: plan `📆 Today` by dragging tasks from `🗓 This Week`. Keep `🍅 Now/Pomodoro (1)` honest — one focus at a time. Clear `my/inbox.md` before end of day.
- **Weekly**:
  - Pull from `Recurring Tasks 🔁` and `Ticklers 🗓️` into the live buckets.
  - Promote from `🗒 Later` → `🗓 This Week` — pick what you actually commit to doing this week.
  - Review `🤔 Someday/Maybe`: **delete what you'll never do**, **promote to `🗒 Later`** the ideas you now want to commit to (and process them as real tasks — priority, hub, project, deadline).
  - Move stale `🗒 Later` items down to `🤔 Someday/Maybe` if you're no longer committed.
  - Archive (or delete) the `✅ Completed` block.
- **Monthly**: review `H2 Areas` pages and confirm the active `+project` set.
- **Quarterly**: review `H3 Goals`, `H4 Vision`, and `🤔 Someday/Maybe`.

## References

- `my/todo.md` — live data.
- `my/workflow/Horizons of Focus 🌄.md` — strategic layer.
- `my/workflow/Recurring Tasks 🔁.md`, `Ticklers 🗓️.md` — pickers for weekly/monthly reviews.
- `AGENTS.md` (repo root) — vault-wide conventions, links, emoji handling.
- `.agents/skills/vault-update/SKILL.md` — cross-vault reviews, triage, memory gap-fill.
- `.agents/skills/memory-management/SKILL.md` — decoding shorthand (people, projects, acronyms) referenced in tasks.
