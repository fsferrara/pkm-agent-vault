# Connectors

## Status

The vault ships with a pre-wired [`.mcp.json`](.mcp.json) listing MCP servers for common SaaS tools. **No skill in this repo depends on them** — the included skills (`gtd-task-workflow`, `second-brain-organize`, `vault-update`) operate vault-local only and never reach out over MCP.

The connector list is here so you can:

1. Attach your own accounts (Slack, Notion, Asana, etc.) and ask the agent ad-hoc questions.
2. Write additional skills that pull context from those tools into the vault.

If you don't want MCP at all, delete `.mcp.json`.

## How tool references will work in future skills

If you add a skill that uses an external tool, refer to it by category rather than product so the skill stays portable:

- `~~chat` — whatever chat tool is connected (Slack, Teams, Discord, …).
- `~~email` / `~~calendar` — mail and calendar (Microsoft 365, Google, …).
- `~~knowledge base` — Notion, Confluence, Guru, Coda, …
- `~~project tracker` — Asana, Linear, Jira, monday.com, ClickUp, Shortcut, …
- `~~office suite` — Microsoft 365, Google Workspace, …

The agent resolves the placeholder to whichever MCP server is configured in that category. Skills should describe the workflow ("post an update to `~~chat`"), not the product.

## Connectors currently wired in `.mcp.json`

| Category | Placeholder | Included servers | Other options |
|----------|-------------|-----------------|---------------|
| Chat | `~~chat` | Slack | Microsoft Teams, Discord |
| Email | `~~email` | Microsoft 365 | Google |
| Calendar | `~~calendar` | Microsoft 365 | Google |
| Knowledge base | `~~knowledge base` | Notion | Confluence, Guru, Coda |
| Project tracker | `~~project tracker` | Asana, Linear, Atlassian (Jira/Confluence), monday.com, ClickUp | Shortcut, Basecamp, Wrike |
| Office suite | `~~office suite` | Microsoft 365 | Google Workspace |
