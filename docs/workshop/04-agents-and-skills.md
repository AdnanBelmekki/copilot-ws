# 04 — Custom Prompts, Agents & MCP

**Time:** ~15 minutes  
**Features:** Custom prompts, Code review agent, Custom agents (`.github/agents/`), Playwright MCP, environment setup

**Learning goals:**
- Learn how to turn inline prompts into version-controlled `.github/prompts/*.prompt.md` files
- Understand built-in review agents and how to build custom agents in `.github/agents/*.agent.md`
- See how MCP servers connect Copilot to external tools (browser, databases, APIs)
- Observe how pre-configured environments enable complex automation

> **Note:** This part demonstrates higher-level Copilot automation patterns — moving from reusable prompt templates to autonomous custom agents, external MCP tools, and pre-configured environments.

---

## 📝 1. Creating Reusable Prompts

You've used detailed prompts in Parts 02 and 03:
- Backend test generation prompt (Part 02, Step 7)
- Component review prompt (Part 03, Step 7)

**The problem:** Every time you need these, you copy and paste them from your notes or previous chat history.

**The solution:** Save them as reusable prompt templates in `.github/prompts/*.prompt.md`.

### Example: Convert an inline prompt to reusable

**Before (copy/paste every time):**
```
@backend-engineer Generate a test for POST /api/orders using MockMvc that:
[... full prompt text ...]
```

**After (saved as `.github/prompts/test-backend-api.prompt.md`):**
```
/test-backend-api generate a test for POST /api/orders
```
*(or use natural language: "Use the test-backend-api prompt...")*

**Try it:**

1. Open the starter file: `.github/prompts/test-backend-api.prompt.md`
2. Replace the TODO sections with the useful parts of the backend testing prompt from Part 02:
   ```markdown
   ---
   name: test-backend-api
   description: Generate backend API tests with MockMvc
   ---
   
   @backend-engineer Generate a test for [ENDPOINT] using MockMvc.
   ```

   Adapt the wording and add the guidelines you found most useful. The goal is to encode your own conventions, not copy a finished answer.

3. In Copilot Chat, type `/test-backend-api` at the beginning of your message:
   ```
   /test-backend-api generate a test for DELETE /api/products/{id}
   ```
   *(You can also use natural language: `Use the test-backend-api prompt to generate a test for...`)*

Copilot will load the prompt file and use it as a template. You can complete the `component-review.prompt.md` starter in the same way using `/component-review` or the review prompt from Part 03.

> 💡 **What this teaches:** Prompts can be version-controlled, shared with your team, and evolved over time. It's a way to codify your QA and coding standards.

### Your prompts as team conventions

Other reusable prompts you might create:
- **Design validation:** Review components against the designer skill + color palette
- **Security audit:** Check code for common vulnerabilities
- **Performance review:** Identify N+1 queries, memory leaks, expensive renders
- **Accessibility check:** Validate WCAG compliance

---

## 🔍 2. Built-in Review Agent & Custom Agents

Now that you've structured your prompts into templates, let's explore **Agents** — specialized AI personas equipped with instructions, tools, and multi-step processes.

> 💡 **Skills vs. Agents:**
> - **Skills** (`.github/skills/`): Reusable **domain expertise** (e.g., `backend-engineer`, `designer`). They teach Copilot *what* context, conventions, and patterns to follow when generating code.
> - **Agents** (`.github/agents/`): Autonomous **workflow runners** (e.g., `@todo`, `@code-review`). They give Copilot tools (`codebase`, `terminal`) and multi-step procedures to execute complete tasks.

### Built-in Code Review Agent

The **code review agent** analyses your staged or unstaged changes and surfaces only issues that genuinely matter: bugs, security problems, and logic errors. It deliberately ignores style and formatting.

**Try it:**

1. Make sure you have unstaged changes from Parts 2 and 3
2. Open Copilot Chat
3. Type:
   ```
   Review my changes
   ```
   or select the `code-review` agent if available in your IDE.

**What to observe:**
- The agent looks at diffs, not just individual files
- It cross-references your changes with the rest of the codebase
- It won't comment on indentation or variable naming — only real issues

---

### Segway: Build Your Own Custom Agents — `.github/agents/`

Built-in agents handle general tasks like code review, but what if your team needs a **specialized reviewer or implementation agent** tailored to your repository?

You can define **Custom Agents** in `.github/agents/name.agent.md`.

#### How custom agents work

Each agent file contains:
- **YAML Frontmatter:** Configures the agent name, description, and available tools (`codebase`, `terminal`, etc.)
- **System Instructions:** Defines the agent's specific role, checklist, prioritization rules, and output format.

#### Example: The `@todo` implementation agent

Open `.github/agents/todo.agent.md` in your editor. Notice how it defines:
- **Frontmatter:** `name: todo`, `description: ...`, `tools: [codebase]`
- **Step-by-step workflow:** Discovers TODO comments, reports status, prioritizes stubs by dependency, and implements solutions adhering to backend and frontend conventions.

**Try it in Copilot Chat:**
```
@todo Find all unimplemented stubs in this project and show me the status report.
```

#### Creating a Custom Reviewer Agent

Just like the `@todo` agent, you can create custom reviewer agents for your team:
- `security-reviewer.agent.md` — Audits authentication, SQL injection, and secret leaks
- `a11y-reviewer.agent.md` — Audits WCAG compliance and keyboard navigation
- `performance-reviewer.agent.md` — Checks for N+1 queries and unnecessary re-renders

> 💡 **SDLC role:** Custom agents act as automated **Tech Leads, QA Engineers, and Domain Experts** — encoding your team's exact procedures into reusable agents.

#### Agent Plugins & Extension Marketplaces

Beyond building custom agents and skills in your repository, you can also explore **Agent Plugins** and extension marketplaces. These marketplaces offer downloadable, pre-packaged bundles of agents, skills, tools, and instructions tailored for specific tech stacks (such as Spring Boot, React, Azure, or Docker) and workflows (such as security auditing or API testing). Installing preset bundles allows teams to instantly equip Copilot with standardized domain expertise and capabilities across projects without having to author every agent or instruction from scratch.

---

## 🎭 3. Playwright MCP — Browser Automation from Chat

Beyond prompt templates and agents, Copilot can interact with external tools using the **Model Context Protocol (MCP)**.

The Playwright MCP server (pre-configured in `.mcp.json`) lets Copilot control a browser, take screenshots, interact with the UI, and validate user flows — all from chat.

**Try it:**

1. Make sure both frontend and backend are running (see [Part 00](./00-prerequisites.md))
2. Verify MCP is connected (look for a 🔌 or server icon in Copilot Chat sidebar)
3. Ask Copilot:
   ```
   Open the webshop at http://localhost:5173, take a screenshot, and describe what you see.
   ```

Copilot will:
- Launch a browser instance
- Navigate to the URL
- Capture a screenshot
- Describe the current state of the application

**Next, test a user flow:**
```
Navigate to http://localhost:5173, click "Browse Products", take a screenshot, then add the first product to the cart. Show me the cart state.
```

**Validate the checkout flow:**
```
Add a product to the cart, navigate to checkout, fill in the form with name "Test User" and email "test@example.com", submit the order, and confirm the order was created successfully by checking the backend.
```

> 💡 **SDLC role:** This is a **QA Engineer / Product Owner** tool — automated end-to-end testing from natural language, catching regressions before they reach users.

---

## ⚙️ 4. Environment Setup — `copilot-setup-steps.yml`

Open `.github/copilot-setup-steps.yml` — this file pre-installs dependencies needed for testing and browser automation when agents run in automated or cloud environments.

**Current setup includes:**
- Node.js + npm (for frontend dependencies)
- JDK 25 (for backend compilation)
- Playwright browsers (for automated testing)

**Try it in Copilot Chat:**
```
Explain what dependencies are installed in copilot-setup-steps.yml and why each one matters for this webshop project.
```

Or ask:
```
Add a step to copilot-setup-steps.yml that installs Playwright system dependencies for headless testing.
```

> 💡 **SDLC role:** This is a **DevOps / QA** tool — ensuring the test environment is ready, just like a CI machine setup.

---

## Summary — SDLC role map

| Feature | Who benefits most | What it replaces |
|---------|-------------------|------------------|
| Custom prompts (`.github/prompts/`) | Developers, QA | Copy/pasting prompt snippets from notes |
| Code review & Custom agents (`.github/agents/`) | Tech Lead, QA | Manual PR review & ad-hoc workflow scripts |
| Playwright MCP (`.mcp.json`) | QA, Product Owner | Manual testing, end-to-end validation |
| `copilot-setup-steps.yml` | DevOps, QA | Manual test environment setup |

---

**Finished the core workshop! 🎉**  
If you have time, explore the bonus tasks:

- [BONUS 01 — MCP Servers →](./BONUS-01-mcp-servers.md)
- [BONUS 02 — Testing →](./BONUS-02-testing.md)
- [BONUS 03 — CI/CD →](./BONUS-03-ci-cd.md)
- [BONUS 04 — Custom Instructions Deep Dive →](./BONUS-04-custom-instructions.md)
