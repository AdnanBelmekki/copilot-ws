# Reusable Prompts

This folder contains custom prompts that guide Copilot for common tasks in this project.

Students create these prompts as part of the workshop to learn how to encode team conventions into reusable templates.

## How to use a prompt

Prompts saved in `.github/prompts/*.prompt.md` can be invoked directly as a slash command at the beginning of a chat message:

```
/test-backend-api generate a test for POST /api/products
```

Or using natural language:
```
Use the test-backend-api prompt to generate a test for POST /api/products
```

Copilot will load the prompt file and apply its guidelines to your request.

## How to create your own

See [Part 04 — Agents & Skills](../../docs/workshop/04-agents-and-skills.md) for a walkthrough on creating your first prompt.

## Prompt file format

Filenames must follow this format: `name.prompt.md`

Each prompt file starts with YAML frontmatter:
```markdown
---
name: prompt-name
description: What this prompt is for
keywords:
  - keyword1
  - keyword2
---

Your prompt content goes here...
```

## Starter templates

The workshop provides starter templates for these prompts:
- `test-backend-api.prompt.md` — Generate backend API tests
- `component-review.prompt.md` — Review React components  
- `error-handling-review.prompt.md` — Audit error handling

Use the inline prompts in Parts 02 and 03 first. In Part 04, complete the starter files in this folder and turn those one-time prompts into reusable team conventions.

