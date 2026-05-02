---
name: vault-update
description: Unified cross-vault skill that runs cadence reviews (daily shutdown, weekly review, monthly area check), topic retrieval ("what do I know about X"), hub-page refreshes ("rebuild the +project / @Area overview"), stale-task triage, and memory-gap fill (decode unknown people, projects, acronyms). Reads across `my/todo.md`, daily notes, `+project` pages, `@Area` hubs, the hot-cache sections of `my/daily/@Journal 📔.md`, and the deep memory in `my/contacts/`, `second-brain/projects/`, `my/@Glossary 📖.md`, and `my/@Company 🏢.md`. Use this skill whenever the user asks to "review my day/week/month", "do a shutdown", "weekly review", "what did I ship", "prepare tomorrow's plan", "what have I written about X", "pull everything on …", "refresh the +X page", "rebuild the @Area hub", "connect the dots on …", "update", "triage", "clean up tasks", or simply runs `/vault-update` — including any phrasing that implies looking across multiple notes, decoding shorthand, or consolidating scattered mentions into one place.
---

# Vault Update

The single surface for reflecting across the vault. Replaces the old `vault-review` + `vault-synthesize` skills and absorbs the triage + memory-gap-fill pattern from the productivity-plugin `update` command. **No external sync** — this skill only reads and writes files inside the vault.

## When to use

Pick this skill when the request is about looking *across* the vault rather than editing one file. Representative phrasings:

- Cadence: "daily shutdown", "plan tomorrow", "weekly review", "what did I ship", "month-end check", "area check".
- Retrieval: "what have I written about X", "pull everything on …", "show me my notes on …", "connect the dots on …".
- Refresh: "refresh the +X page", "rebuild the @Area overview", "the hub page is stale".
- Triage / memory: "update my vault", "clean up tasks", "triage", bare `/vault-update`.

## Do not use

Defer to a more specific skill when the request is narrower:

- Adding, routing, or reformatting a single task → `gtd-task-workflow`.
- Filing a new knowledge note or archiving a project → `second-brain-organize`.

If multiple apply, run `vault-update` first for the overview, then hand off.

## Pick the mode

Infer the mode from phrasing; ask only if ambiguous. Default when fully ambiguous is **weekly cadence** (highest leverage).

| User says… | Mode |
|---|---|
| "daily shutdown", "plan tomorrow", "what did I do today" | `cadence:daily` |
| "weekly review", "what did I ship this week" | `cadence:weekly` |
| "monthly", "month-end", "area check" | `cadence:monthly` |
| "what have I written about X", "pull everything on …", "what do I know about …" | `topic:retrieval` |
| "refresh the +X page", "rebuild the @Area", "hub page is stale" | `topic:refresh` |
| "update", "triage", "clean up tasks", bare `/vault-update` | `triage` (stale tasks + memory gaps) |

## Lazy bootstrap (run on every invocation, before the chosen mode)

Check that the memory scaffolding exists. If any of these are missing, append/create them **before** running the requested mode. Tell the user what you scaffolded.

1. **Hot cache** — `my/daily/@Journal 📔.md` must contain the four sections `## People`, `## Terms`, `## Projects`, `## Preferences` below the existing `## Context Content` section. If any are absent, append the template from `references/hot-cache-template.md`.
2. **Glossary** — `my/@Glossary 📖.md` must exist. If not, create it from `references/glossary-template.md`.
3. **Company context** — `my/@Company 🏢.md` must exist. If not, create it from `references/company-template.md`.

Individual `my/contacts/<name>.md` and `second-brain/projects/+<slug>.md` files are **not** preemptively created — they are offered on-demand during `triage` memory-gap fill.

## Read order (by mode)

Stop reading as soon as you have enough signal; don't re-read the whole vault.

### cadence:daily

1. `my/todo.md` — current bucket state.
2. Today's `my/daily/YYYY-MM-DD.md` (or note it doesn't exist yet).
3. Yesterday's daily note.

### cadence:weekly

1. `my/todo.md`.
2. Last 7 daily notes — use `ls my/daily/ | sort | tail -7` rather than globbing.
3. Active `+project` pages linked from `my/todo.md` (usually under `### [[@Projects 💻|💻 @Projects]]` or similar).
4. `my/workflow/Recurring Tasks 🔁.md` and `my/workflow/Ticklers 🗓️.md` (pickers for next week).

### cadence:monthly

Everything in weekly, plus:

1. Last ~30 daily notes.
2. Each `@Area 🔵.md` hub under `second-brain/areas/`.
3. `my/workflow/Horizons of Focus 🌅.md` for alignment check.

### topic:retrieval / topic:refresh

Use ripgrep across `my/**` and `second-brain/**`. The vault has six homes for content; a topic often spans several.

Plausible forms in one pass:

```bash
rg -n --glob '*.md' \
   -e 'X' \
   -e '\[\[.*X.*\]\]' \
   -e '#X' \
   /Users/fferrara/github/fsferrara/pkm-agent-vault
```

Adapt to the exact form:

- Project: `rg -n '\[\[\+project-slug\]\]' …` — wiki-links are the strongest signal.
- Area: `rg -n '\[\[@Area Name 🔵\]\]' …` — include the emoji, it's part of the link target.
- Person or free-form keyword: plain `rg -i`.

Follow links. A meeting note mentioning the project is worth reading; a daily note mentioning a person is worth cross-checking against `my/contacts/`. Stop when new reads don't add anything.

Source priority: active tasks (`my/todo.md`) → daily notes (`my/daily/`) → meetings (`my/meetings/`) → project pages (`second-brain/projects/+slug.md` and subfolders) → area hubs (`second-brain/areas/**`) → resources and archive last.

### triage

1. `my/todo.md` — stale-task scan (see §Triage rules).
2. Hot cache in `my/daily/@Journal 📔.md` + deep memory (see §Memory lookup) — decode entities in today's + this-week's tasks and today's daily note.
3. List unknown entities (people / projects / acronyms) as gap-fill candidates.

## Memory lookup (all modes, when decoding shorthand)

Always decode entities (people, projects, acronyms, codenames) before acting on a request. Tiered lookup:

1. **Hot cache** — the `## People`, `## Terms`, `## Projects`, `## Preferences` tables in `my/daily/@Journal 📔.md`. Covers ~90% of daily needs.
2. **Deep memory — people** — `my/contacts/<name>.md`.
3. **Deep memory — projects** — `second-brain/projects/+<slug>.md`.
4. **Deep memory — terms / company** — `my/@Glossary 📖.md`, `my/@Company 🏢.md`.
5. **Ask the user** — if still unknown, ask. Once they answer, write it to the appropriate deep-memory file. If the entity then gets referenced ≥ 3 times across recent tasks / daily notes, promote a one-line entry to the hot cache.

## Triage rules (mode: `triage`)

Scan `my/todo.md` for stale patterns and surface them for the user to decide:

- Task in any bucket with `due:YYYY-MM-DD` in the past → flag as past-due.
- Task in `🗒 Later` unchanged for ~30 days → suggest demote to `🤔 Someday/Maybe` or delete.
- Task in `⏳ Waiting For (delegated)` with no `wait:<who>` label → ask who it's waiting on.
- Task with no `@Context`, no `+project`, and no priority → ask for at least one signal or offer to drop to `🤔 Someday/Maybe`.
- Task in `📆 Today` or `🍅 Now/Pomodoro (1)` that hasn't moved in 3+ days → ask to re-plan.

For each flagged item, offer a single-keystroke disposition (done / reschedule / demote / drop / keep). Do **not** edit `my/todo.md` without confirmation — hand off the actual edits to `gtd-task-workflow` once the user says which to apply.

## Memory-gap fill (mode: `triage`, optionally after `cadence:*`)

After the task scan, decode every entity in today's + this-week's tasks and today's daily note against the memory lookup chain. Group the unknowns:

```
Found entities I don't have context for:

People:
  1. "Alice" — in task "sync with Alice on +phoenix"
     → Who is Alice?

Projects:
  2. "+phoenix" — in same task
     → What is +phoenix?

Terms:
  3. "PSR" — in today's daily note
     → What does PSR stand for?
```

For each answer the user gives:

- **Person** → write `my/contacts/<name>.md` (use `templates/contact.md` if present; otherwise a minimal stub: name, role, how-we-know-each-other, nicknames). If mentioned ≥ 3 times recently, add a row to the `## People` table in `@Journal 📔.md`.
- **Project** → write `second-brain/projects/+<slug>.md` (use `templates/project.md` if present; otherwise a minimal stub: goal, status, related area, related people). If active, add a row to the `## Projects` table in `@Journal 📔.md`.
- **Term / acronym** → append a row to `my/@Glossary 📖.md`. If used ≥ 3 times recently, also add to `## Terms` in `@Journal 📔.md`.
- **Company / team / tool context** → append to the right section of `my/@Company 🏢.md`.

**Never auto-add** to `my/todo.md`, `my/contacts/`, or project pages without the user confirming each one.

## Output per mode

See `references/output-templates.md` for the full skeletons. In summary:

- `cadence:daily` → short shutdown report: shipped, slipped, tomorrow's Now (1), captures worth routing, flags.
- `cadence:weekly` → shipped / slipped / waiting / projects snapshot / recurring + ticklers to pull in / next-week candidates / recommendations.
- `cadence:monthly` → shipped / @Area health / project portfolio / Horizons alignment / Someday review / recommendations.
- `topic:retrieval` → summary / most-recent-state / timeline / open commitments / related notes / gaps-or-contradictions.
- `topic:refresh` → proposed replacement section for the hub page, with `_Refreshed YYYY-MM-DD from N daily notes, M meetings, K tasks._` header, previewed as a diff before writing.
- `triage` → stale-task flags (with disposition options) + memory-gap list (with answer prompts).

## Principles

- **Summarize, don't rewrite.** Cadence and retrieval modes produce a written summary; the user decides what to change. `topic:refresh` is the only mode that edits an existing vault file, and only after a diff preview and confirmation.
- **Fresh beats deep.** When a daily note and a hub page disagree, trust the daily note and surface the disagreement.
- **Cite sources.** Every claim should trace to a file. Use wiki-links with exact filenames (including emojis) for internal references.
- **Respect existing vocabulary.** Use the user's bucket names (`🍅 Now/Pomodoro (1)`, `📆 Today`, `🗓 This Week`, `🗒 Later`, `⏳ Waiting For (delegated)`, `🤔 Someday/Maybe`, `✅ Completed`), priorities `(A)-(D)`, and `@Context`/`+project` conventions — don't invent new labels.
- **Ask before destructive writes.** Any file edit bigger than appending a table row gets a preview first.
- **Never auto-add** entries to `my/todo.md`, `my/contacts/`, project pages, or glossary without the user confirming.
- **Don't invent content.** If the vault doesn't say something, say "no mentions" rather than generating plausible-sounding prose.

## Handoffs

After the summary, the user often wants one of:

- "Apply these changes to `todo.md`" → `gtd-task-workflow`.
- "Archive this project" / "File this note" → `second-brain-organize`.

## References

### Files this skill reads or writes

- `my/todo.md` — canonical task list.
- `my/daily/YYYY-MM-DD.md` — daily notes.
- `my/daily/@Journal 📔.md` — hot-cache host (appended to, not rewritten).
- `my/contacts/<name>.md` — deep memory: people.
- `my/workflow/Horizons of Focus 🌅.md`, `Recurring Tasks 🔁.md`, `Ticklers 🗓️.md` — strategic + pickers.
- `second-brain/projects/+<slug>.md` — deep memory: projects, and refresh target.
- `second-brain/areas/@Area 🔵.md` — area hubs, refresh target.
- `my/@Glossary 📖.md` — deep memory: terms.
- `my/@Company 🏢.md` — deep memory: company / teams / tools.

### Related skills

- `.agents/skills/gtd-task-workflow/SKILL.md` — task-level edits.
- `.agents/skills/second-brain-organize/SKILL.md` — PARA filing.

### Local references

- `references/hot-cache-template.md` — sections to append to `@Journal 📔.md`.
- `references/glossary-template.md` — boilerplate for `@Glossary 📖.md`.
- `references/company-template.md` — boilerplate for `@Company 🏢.md`.
- `references/output-templates.md` — one markdown skeleton per mode.

### Conventions

- `AGENTS.md` (repo root) — vault-wide conventions (buckets, priorities, emojis, wiki-links, two-tier memory).
