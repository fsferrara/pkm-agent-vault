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

### Adding Instructions

Instructions help customize GitHub Copilot's behavior for specific processes, tasks, or domains.

1. **Create your instruction file**: Add a new `.md` file in the `instructions/` directory
2. **Follow the naming convention**: Use descriptive, lowercase filenames with hyphens (e.g., `pitch.instructions.md`)
3. **Structure your content**: Start with a clear heading and organize your instructions logically
4. **Test your instructions**: Make sure your instructions work well with GitHub Copilot

#### Example instruction format

```markdown
---
description: 'Instructions for customizing GitHub Copilot behavior for specific technologies and practices'
---

# Your Process/Task/Domain Name

## Instructions

- Provide clear, specific guidance for GitHub Copilot
- Include best practices and conventions
- Use bullet points for easy reading

## Additional Guidelines

- Any additional context or examples
```

### Adding Prompts

Prompts are ready-to-use templates for specific development scenarios and tasks.

1. **Create your prompt file**: Add a new `.prompt.md` file in the `prompts/` directory
2. **Follow the naming convention**: Use descriptive, lowercase filenames with hyphens and the `.prompt.md` extension (e.g., `generate-mind-map.prompt.md`)
3. **Include frontmatter**: Add metadata at the top of your file (optional but recommended)
4. **Structure your prompt**: Provide clear context and specific instructions

#### Example prompt format

```markdown
---
mode: 'agent'
tools: ['codebase', 'terminalCommand']
description: 'Brief description of what this prompt does'
---

# Prompt Title

Your goal is to...

## Specific Instructions

- Clear, actionable instructions
- Include examples where helpful
```

### Adding Chat Modes

Chat modes are specialized configurations that transform GitHub Copilot Chat into domain-specific assistants or personas for particular activity scenarios.

1. **Create your chat mode file**: Add a new `.chatmode.md` file in the `chatmodes/` directory
2. **Follow the naming convention**: Use descriptive, lowercase filenames with hyphens and the `.chatmode.md` extension (e.g., `mentor.chatmode.md`)
3. **Include frontmatter**: Add metadata at the top of your file with required fields
4. **Define the persona**: Create a clear identity and expertise area for the chat mode
5. **Test your chat mode**: Ensure the chat mode provides helpful, accurate responses in its domain

#### Example chat mode format

```markdown
---
description: 'Brief description of the chat mode and its purpose'
model: 'gpt-5'
tools: ['codebase', 'terminalCommand']
---

# Chat Mode Title

You are an expert [domain/role] with deep knowledge in [specific areas].

## Your Expertise

- [Specific skill 1]
- [Specific skill 2]
- [Specific skill 3]

## Your Approach

- [How you help users]
- [Your communication style]
- [What you prioritize]

## Guidelines

- [Specific instructions for responses]
- [Constraints or limitations]
- [Best practices to follow]
```

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
