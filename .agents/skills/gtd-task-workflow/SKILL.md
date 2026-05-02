---
name: gtd-task-workflow
description: Capture, clarify, and route tasks through the GTD (Getting Things Done) workflow used in this vault. Formats each task as `- [ ] (Priority) description @Context +project due:YYYY-MM-DD` and places it in the correct bucket (Inbox, Now, Today, This Week, Later, Wait/Delegated, Someday/Maybe, Done) inside `my/Tasks 💼.md`. Use this skill whenever the user mentions adding a todo, processing an inbox item, clarifying a capture, planning the day or week, promoting/demoting tasks between buckets, delegating, reviewing Horizons of Focus, or asks "what should I do with X" — even if they don't name GTD explicitly.
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

- **`my/Tasks 💼.md`** — the single file containing every active and historical bucket. All task additions and moves happen here.
- **`my/workflow/Horizons of Focus 🌅.md`** — strategic layer (H5 Purpose → H1 Projects). Consult when the user is reviewing priorities or deciding whether something deserves to become a project.
- **`my/workflow/Recurring Tasks 🔁.md`** and **`Ticklers 🗓️.md`** — pickers for weekly/monthly reviews. Tasks from here get copied into the live buckets.

## The workflow: Capture → Clarify → Organize

### 1. Capture

New items land under the `### 📥 Inbox` heading with no priority, no context, and no bucket. One-liner is fine — the goal is friction-free capture.

### 2. Clarify (one item at a time)

For each Inbox item, answer in order:

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
| < 2 minutes to complete | Do it now, add to `### ✅ Done` with `[x]` |
| Delegated or blocked on someone | `### ⏳ Wait/Delegated` with a `wait:<label>` tag |
| Multi-step work | Create or update a `+project` page in `second-brain/projects/`, then link the next physical action back into a bucket |
| Committed for today | `### 📆 Today` |
| Committed for this week | `### 🗓 This Week` |
| Actionable, but later than this week | `### 🗒 Later` |
| Idea, no commitment | `### 🤔 Someday/Maybe` |
| Not actionable and not worth keeping | Delete |

Maintain **one** item under `### 🍅 Now (1)` — the current focus. Two only if absolutely necessary.

## Task format (todo.txt-flavored)

```markdown
- [ ] (A) Task description @Context +project due:YYYY-MM-DD
```

Rules:

- Leading `- [ ]` for open, `- [x]` for done.
- Priority is optional but, when present, uses a single uppercase letter in parens: `(A)` `(B)` `(C)` `(D)`.
- `@Context` refers to an area hub page (e.g. `@Work 💼`, `@Home 🏠`, `@Learning 📚`). Written inline without brackets or as a wiki-link `[[@Area 🔵]]` — follow the style already present in the file.
- `+project` links to a `+project-slug` page (e.g. `[[+launch-website]]`).
- `due:YYYY-MM-DD` uses ISO dates. No other date format.
- Preserve the emojis in section headings and wiki-links exactly — they are part of the link target.

## Priority (Eisenhower matrix)

- **(A)** Urgent & Important — do today (deadlines, emergencies).
- **(B)** Important, not urgent — schedule (planning, skill development, prevention).
- **(C)** Urgent, not important — batch or delegate (most meetings, many emails).
- **(D)** Neither — minimize or delete.

Only a handful of `(A)` tasks should coexist. If everything is `(A)`, nothing is.

## Bucket semantics (order in the file)

Always respect this order — Foam/Obsidian users navigate by scrolling:

1. `### ✅ Done`
2. `### 📥 Inbox`
3. `### 🍅 Now (1)`
4. `### 📆 Today`
5. `### 🗓 This Week`
6. `### 🗒 Later`
7. `### 🗓 Ticklers` — date-triggered reminders (pointers to tickler/recurring pages)
8. `### ⏳ Wait/Delegated`
9. `### [[@Projects 💻|💻 @Projects]]` — the list of active `+project` pages
10. `### 🤔 Someday/Maybe`

Keep the headings and emojis exactly as they appear. If a heading is missing, add it in its canonical position rather than inventing a new one.

## Horizons of Focus (strategic reviews)

When the user asks for a review or wants to reshape priorities, read `my/workflow/Horizons of Focus 🌅.md` and work top-down:

- **H5 Purpose** (life mission) — rarely touched.
- **H4 Vision** (3–5 years) — quarterly.
- **H3 Goals** (1–2 years) — quarterly.
- **H2 Areas of Focus** (12 months) — monthly. These are the `@Area` pages.
- **H1 Projects & Priorities** (< 12 months) — weekly. These are the `+project` pages.
- **Ground** — actionable tasks in `my/Tasks 💼.md`.

A task without a connection to at least an `@Area` or `+project` is a candidate for deletion or `Someday/Maybe`.

## Routine cadence

- **Daily**: plan `Today`, keep `Now (1)` honest, clear Inbox before end of day.
- **Weekly**: pull from `Recurring Tasks 🔁` and `Ticklers 🗓️`, promote from `Later` to `This Week`, move stale `Later` items to `Someday/Maybe`, archive the `Done` block.
- **Monthly**: review `H2 Areas` pages and confirm the active `+project` set.
- **Quarterly**: review `H3 Goals`, `H4 Vision`, and `Someday/Maybe`.

## References

- `my/Tasks 💼.md` — live data.
- `my/workflow/Horizons of Focus 🌅.md` — strategic layer.
- `my/workflow/Recurring Tasks 🔁.md`, `Ticklers 🗓️.md` — pickers for weekly/monthly reviews.
- `AGENTS.md` (repo root) — vault-wide conventions, links, emoji handling.
