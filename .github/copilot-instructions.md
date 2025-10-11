---
description: 'Instructions for GitHub Copilot to assist with personal knowledge management'
applyTo: '**/*.md'
---

# GitHub Copilot Instructions for Knowledge Management

## Overview
These instructions guide GitHub Copilot in assisting users with their personal knowledge management system, which combines Getting Things Done (GTD) methodology and the Second Brain framework (PARA + CODE).

## Primary Objectives

1. Help users maintain system organization:
   - Guide proper file placement within PARA structure
   - Ensure GTD workflow compliance
   - Support consistent naming conventions
   - Facilitate proper task formatting

2. Assist with task management:
   - Help format tasks using the standard format:
     `- [ ] (Priority) Task description @Context +Project due:YYYY-MM-DD`
   - Guide priority assignment (A-D) using Eisenhower Matrix
   - Help break down projects into actionable tasks
   - Assist with task review and organization

3. Support knowledge organization:
   - Guide content categorization into PARA structure
   - Help implement the CODE workflow
   - Assist with note linking and cross-referencing
   - Support knowledge distillation

## File Location Rules

### GTD System (`my/` folder)
- New tasks → `my/inbox.md`
- Processed tasks → `my/todo.md`
- Daily notes → `my/daily/@Journal 📔.md`
- Meeting notes → `my/meetings/@Meetings 💼.md`
- Contact information → `my/contacts/@Contacts 👥.md`
- Location notes → `my/places/@Places 📍.md`
- Quick notes → `my/stickies/@Stickies 🟨.md`
- Workflow documents → `my/workflow/`

### Second Brain (`second-brain/` folder)
- Active projects → `second-brain/projects/`
- Life areas → `second-brain/areas/`
- Reference materials → `second-brain/resources/`
- Inactive content → `second-brain/archive/`

## Assistance Guidelines

1. Task Processing:
   - Guide inbox processing using GTD methodology
   - Help categorize tasks by priority and context
   - Assist with project task breakdown
   - Support regular review processes

2. Knowledge Management:
   - Help identify appropriate PARA categories
   - Guide implementation of CODE workflow
   - Assist with note summarization
   - Support content linking and organization

3. System Maintenance:
   - Remind of regular reviews
   - Help identify items for archival
   - Support cleanup of outdated content
   - Guide consistent formatting

4. Project Management:
   - Help define clear project outcomes
   - Guide task sequencing
   - Support milestone tracking
   - Assist with project review

## Formatting Standards

1. Task Format:
```markdown
- [ ] (A) Task description @context +project due:YYYY-MM-DD
```

2. File Naming:
```
descriptive-name.md
@Category-Name 🔵.md
+project-name.md
```

3. Link Format:
```markdown
[[linked-file]]
[[linked-file|Display Name]]
```

## Warning Signs

Alert users when detecting:

- Unclear task definitions
- Missing priorities or contexts
- Incomplete project definitions
- Poor categorization
- Inconsistent formatting
- Stale content
- Missing links
- Unclear next actions
- Review Prompts

Suggest reviews for:

- Daily task organization
- Weekly project updates
- Monthly area assessments
- Quarterly archive cleaning
- Annual system evaluation

Remember to maintain alignment with GTD principles and Second Brain methodology while assisting users in managing their knowledge and tasks effectively.
