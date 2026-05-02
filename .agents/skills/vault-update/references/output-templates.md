# Output templates

One markdown skeleton per `vault-update` mode. Pick the skeleton matching the inferred mode and fill it in. Keep outputs scannable — the user reviews; the vault stays intact unless explicitly asked to edit (and except for `topic:refresh`, which writes one hub page after preview).

All internal references use wiki-links with exact filenames, including emojis.

---

## Mode: `cadence:daily`

```markdown
# Daily shutdown — YYYY-MM-DD

## Shipped today
- …

## Carried over / slipped
- …

## Tomorrow's Now (1) & Today candidates
- …

## Captures worth routing
- (items from today's daily note that look like real tasks or knowledge notes — suggest a bucket, don't route automatically)

## Flags
- (anything stale, anything blocked, anything urgent nobody's holding)
```

---

## Mode: `cadence:weekly`

```markdown
# Weekly review — week of YYYY-MM-DD

## Shipped
- Grouped by @Area / +project.

## Slipped or still open
- From This Week → call out each one, and whether it should stay This Week, move to Later, or go Someday.

## Waiting / delegated status
- Each `⏳ Waiting For` item with a nudge suggestion if it's been stuck.

## Projects snapshot
- For each active +project: one-line status (progressing / blocked / stale / near complete).

## Recurring + ticklers to pull in
- From Recurring Tasks 🔁 and Ticklers 🗓️ — which land in next week's This Week.

## Next week's candidates
- 3–7 items the user should commit to This Week. Don't commit for them.

## Recommendations
- Items to archive, promote, downgrade, or surface to an @Area page.
```

---

## Mode: `cadence:monthly`

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

---

## Mode: `topic:retrieval`

```markdown
# What the vault says about [topic]

## Summary
2–4 sentences. What the user seems to believe or have committed to, based on the notes.

## Most recent state
What the freshest note (usually a daily note) says. Date it.

## Timeline (if useful)
- YYYY-MM-DD — one line — [[source]]
- YYYY-MM-DD — one line — [[source]]

## Commitments currently open
- Open tasks tagged with the topic, from `my/todo.md`, with their bucket.
- Delegated items that mention the topic.

## Related notes
- [[+project-slug]]
- [[@Area 🔵]]
- [[meeting-or-resource-title]]

## Gaps or contradictions
- Anything inconsistent across sources (e.g. "the +project page says 'blocked on Alex' but the latest daily note says 'Alex confirmed Friday'").
```

---

## Mode: `topic:refresh`

Rules:

- **Confirm the target file before writing.** Show the path (`second-brain/projects/+<slug>.md` or `second-brain/areas/@Area 🔵.md`) and the section you plan to replace.
- **Preserve existing frontmatter, headings, and unrelated sections.** Only update the overview/status section unless the user asked for more.
- **Keep it a hub, not an archive.** Link to sources rather than copying their content.
- **Note the regeneration date** at the top of the refreshed section so staleness is visible next time.

### Template — `+project` overview

```markdown
## Overview
_Refreshed YYYY-MM-DD from N daily notes, M meetings, K tasks._

**Goal.** One sentence.

**Current status.** Two or three sentences based on the freshest daily notes and active tasks.

**Open next actions.**
- `- [ ]` items from `my/todo.md` tagged with this project.

**Recent activity.**
- YYYY-MM-DD — one-line update — [[source]]
- YYYY-MM-DD — …

**Related.**
- [[@Area 🔵]]
- [[supporting-resource]]
```

### Template — `@Area` overview

Same skeleton but swap **Goal** for **Standard I'm maintaining** and add a **Linked projects** section enumerating the active `+project` pages connected to it.

---

## Mode: `triage`

```markdown
# Vault triage — YYYY-MM-DD

## Stale tasks in `my/todo.md`

### Past-due
- `- [ ] …` (bucket: 📆 Today, due: YYYY-MM-DD) — N days overdue
  → [d] done · [r] reschedule · [m] demote to 🗒 Later · [x] drop

### Stuck in Later (30+ days)
- `- [ ] …` — last touched YYYY-MM-DD
  → [m] demote to 🤔 Someday/Maybe · [x] drop · [k] keep

### Waiting For without owner
- `- [ ] …` — no `wait:<who>` label
  → who are we waiting on?

### Tasks with no context
- `- [ ] …` — no @Context, no +project, no priority
  → tag with @Area / +project, or demote to 🤔 Someday/Maybe

### Stalled in Today / Now
- `- [ ] …` — in 📆 Today / 🍅 Now for 3+ days
  → re-commit or move back to 🗓 This Week?

## Memory gaps

Entities in today's + this-week's tasks and today's daily note that aren't in the hot cache or deep memory:

### People
1. "Alice" — in task "sync with Alice on +phoenix"
   → Who is Alice? (will write `my/contacts/alice.md`)

### Projects
2. "+phoenix" — in same task
   → What is +phoenix? (will write `second-brain/projects/+phoenix.md`)

### Terms
3. "PSR" — in today's daily note
   → What does PSR stand for? (will append to [[@Glossary 📖]])

_No auto-writes. Confirm each and I'll add it._
```
