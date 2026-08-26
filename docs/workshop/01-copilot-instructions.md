# 01 — Copilot Instructions

**Time:** ~20 minutes  
**Features:** Repository-wide and path-specific instructions

---

## What is `copilot-instructions.md`?

It's a Markdown file at `.github/copilot-instructions.md` that GitHub Copilot automatically injects into **every** chat conversation and inline completion in your repository.

Think of it as a persistent system prompt that shapes Copilot's persona, knowledge, and coding style — without you having to re-explain the project every time.

> 💡 **Key insight:** Unlike a chat message, `copilot-instructions.md` is always active. You never need to remind Copilot what tech stack you use, what your domain model looks like, or what code style to follow.

---

## Step 1 — Read the existing instructions

Open `.github/copilot-instructions.md` in your editor and read through it.

Notice it defines:
- A **persona** ("senior full-stack engineer")
- A **domain glossary** (Product, CartItem, Order, SKU)
- **Frontend and backend conventions**
- **Testing guidance**

---

## Step 2 — Observe the effect

Open Copilot Chat and ask:

```
What are the naming conventions for this project?
```

Copilot should answer using the exact conventions from the instructions file — without you having copied them into the question.

Now try:

```
I want to add a discount code feature. Where should I put the logic?
```

Notice how Copilot references the service layer, repositories, and the correct package structure — all from the instructions.

---

## Step 3 — Add your own rule

Edit `.github/copilot-instructions.md` and add one or more of your own rules. Some ideas:

```markdown
## Extra Conventions
- Never use `var` in Kotlin — always use `val` or `lateinit var`.
- Always add JSDoc comments to exported TypeScript functions.
- Prefer `useReducer` over `useState` for complex state shapes.
- All API errors should be displayed to the user — never silently swallowed.
- Start every answer with a dad joke.
```

Save the file.

---

## Step 4 — Verify it works

Ask Copilot Chat:

```
How should I handle errors from the API in this project?
```

It should now reference your new rule. If you added the JSDoc rule, open `api.ts` and start typing a new function — Copilot should suggest JSDoc automatically.

## Step 5 — Add path-specific instructions

Repository-wide instructions are useful for rules that apply everywhere. Some conventions should apply only to a particular file or area of the codebase. These local instructions should add context that is genuinely specific to the matching files, rather than repeat a skill's domain knowledge.

Create or open this starter file:

`.github/instructions/database-resources.instructions.md`

Add YAML frontmatter with an `applyTo` pattern, followed by rules that should apply to every SQL resource file:

```markdown
---
applyTo: "backend/src/main/resources/**/*.sql"
---

## Database resource conventions
- Preserve compatibility with the schema and entity mappings.
- Use explicit column names in INSERT statements.
- Keep seed IDs stable because orders refer to products by ID.
- Avoid destructive statements unless the task explicitly requires them.
```

Now open `backend/src/main/resources/data.sql` and ask:

```
Add three products to the seed data and follow the database resource instructions.
```

Then open a Kotlin or TypeScript file outside the matching path and ask the same question. Compare the context Copilot uses in each location.

> **Key insight:** `copilot-instructions.md` is the shared baseline. Files in `.github/instructions/` add rules only when their `applyTo` pattern matches the current file.

**Other useful scopes:**
- `.github/instructions/frontend.instructions.md` with `applyTo: "frontend/**/*.{ts,tsx}"` if the rules add frontend workflow guidance rather than repeating the `frontend-engineer` skill.
- `.github/instructions/tests.instructions.md` with `applyTo: "**/*test*.*"` or a narrower test-file pattern
- `.github/instructions/ci.instructions.md` with `applyTo: ".github/workflows/**/*.yml"`
- `.github/instructions/git.instructions.md` with a narrow workflow or documentation pattern if you want Git-specific contribution guidance.

Do not duplicate the whole repository-wide file. Use path-specific instructions for rules that genuinely belong to one area.

---

## 💡 Workshop debrief

| Without instructions | With instructions |
|----------------------|-------------------|
| "Use React best practices" | "Use named exports, async/await, and Tailwind CSS" |
| Copilot guesses your stack | Copilot knows Product, CartItem, Order |
| Style varies per developer | Style is consistent across the team |

Instructions are version-controlled, team-shared, and always active — that's what makes them powerful.

Path-specific instructions add a second layer: broad rules provide the baseline, while matching `applyTo` patterns provide local context.

---

**Next:** [Part 2 — Complete the Backend →](./02-complete-the-backend.md)
