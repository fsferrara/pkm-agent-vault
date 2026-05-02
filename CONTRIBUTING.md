# Contributing

Thank you for your interest in contributing!  
We welcome contributions of all types, including:

- 📝 Documentation improvements
- 💻 Markdown contributions
- 🐛 Bug reports and fixes
- 🎨 Design improvements
- 💡 Ideas and suggestions
- 🤔 Answering questions
- 📢 Promoting the project

## How to Contribute

### Adding Skills

Skills customize the agent's behavior by teaching it domain-specific workflows, formats, or personas. The coding agent invokes a skill automatically when the user's request matches the skill's `description`.

1. **Create the skill directory**: `.agents/skills/<skill-name>/` with a single `SKILL.md` inside. Use a descriptive, lowercase, hyphenated name (e.g., `weekly-review`, `meeting-capture`).
2. **Write the frontmatter**: `name` and `description` are required. The `description` is the primary triggering mechanism — state concretely what the skill does and list the phrasings/contexts that should invoke it. Lean slightly "pushy" to combat undertriggering.
3. **Write the body**: use imperative voice, explain the *why* behind rules, and keep the file under ~500 lines. Include a `## When to use` section, the workflow steps, and any exact formats/templates the skill must produce.
4. **Cross-reference live files**: link to the specific vault files the skill reads or writes (e.g. `my/Tasks 💼.md`, `second-brain/projects/`) rather than restating their conventions.
5. **Test the trigger**: open a fresh coding agent session and issue a natural-language request *without naming the skill*. If the agent doesn't invoke it, refine the description's "use this whenever…" clause.

#### Example SKILL.md

```markdown
---
name: example-skill
description: One paragraph saying what this skill does AND when to invoke it. List 3–5 example phrasings the user might say. Close with "Use this skill whenever …" to encourage triggering even when the user doesn't name the skill.
---

# Example Skill

## When to use
- "concrete user phrasing"
- "another realistic phrasing"

## Workflow
1. Step one — explain why.
2. Step two — explain why.

## Output format
Exact template or rules the skill must produce.

## References
- Links to live vault files this skill reads or writes.
```

Look at the existing skills under `.agents/skills/` for working examples.

## Submitting Your Contribution

1. **Fork this repository**
2. **Create a new branch** for your contribution
3. **Add your change** following the guidelines above
4. **Test your change**
5. **Submit a pull request** with:
   - A clear title describing your contribution
   - A brief description of what your instruction/prompt does
   - Any relevant context or usage notes

## License

By contributing to this repository, you agree that your contributions will be licensed under the MIT License.
