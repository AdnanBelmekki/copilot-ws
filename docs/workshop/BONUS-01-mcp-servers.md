# BONUS 01 — Playwright MCP: Browser Automation & Testing

**Self-paced bonus task**  
**Feature:** Playwright MCP server for end-to-end testing

**Learning goals:**
- Understand how Model Context Protocol (MCP) servers extend Copilot's capabilities
- See how Copilot can control external tools (browsers, databases, APIs) via MCP
- Learn to use MCP for automation, validation, and testing workflows
- Explore how MCP turns natural language requirements into executable actions

> **Note:** Playwright MCP demonstrates how Copilot can interact with real systems. The testing scenarios are realistic examples of end-to-end validation — a pattern that applies to any web application.

---

## What is Playwright MCP?

Playwright MCP is an MCP server that lets Copilot control a real browser, take screenshots, interact with pages, and run test validations — all from chat.

**Why it matters:** MCP servers turn Copilot from a code-only assistant into a **full-stack automation platform** that can interact with your running application.

---

## Pre-configured server

Open `.mcp.json` — the Playwright server is pre-configured:

| Server | What it does |
|--------|-------------|
| `playwright` | Launch browsers, navigate pages, interact with UI elements, take screenshots, run test assertions |
| `filesystem` | Read and write files in your workspace (for saving screenshots, reports) |

---

## Task 1 — Visual Inspection

Use Playwright MCP to inspect the application. Make sure the frontend and backend are running, then ask:

```
Open http://localhost:5173, wait for products to load, take a screenshot, and describe the visible products and controls.
```

Observe that Copilot can launch a browser, navigate to localhost, inspect the page, and return a screenshot-backed description.

---

## Task 2 — A Simple User Interaction

Use one short flow to learn how browser actions are chained:

```
Open http://localhost:5173, wait for products to load, click the first "Add to Cart" button, and report the cart contents.
```

This is the basic smoke check. The longer checkout journey appears only in Task 5.

---

## Task 3 — Form Validation

Check invalid input without mixing it with a successful checkout flow:

```
Open http://localhost:5173/checkout, submit the form with an empty email, and report the validation feedback and console errors.
```

Then try one invalid email such as `invalid@` and compare the result. This demonstrates that MCP can validate behavior, not just take screenshots.

---

## Task 4 — Visual Regression

Create one baseline and compare it after a UI change:

```
Open http://localhost:5173 and save a screenshot as "baseline-products-page.png" in the workspace.
```

After changing `ProductCard`, ask:

```
Take a screenshot of the current products page and compare it with "baseline-products-page.png". Report any visible differences.
```

The filesystem MCP server can be used to save and retrieve the screenshot.

---

## Task 5 — End-to-end Checkout

Now combine several actions into one meaningful workflow:

```
Execute this workflow and report success or failure at each step:
1. Open http://localhost:5173
2. Wait for products to load
3. Add two products to the cart
4. Navigate to checkout
5. Fill in name "Test User" and email "test@webshop.com"
6. Submit the order
7. Capture the confirmation
8. Verify the order through http://localhost:8080/api/orders
```

This is the advanced flow. It demonstrates multi-step interaction and browser-to-API verification without repeating the basic cart exercise.

---

## Task 6 — Accessibility and Performance

Use Playwright for two focused quality checks:

```
Open http://localhost:5173/checkout and check keyboard navigation, associated form labels, focus visibility, image alt text, and obvious color-contrast issues. Report each finding.
```

```
Open http://localhost:5173 and measure the time until the products page is usable. Report the timing and any console errors.
```

These checks demonstrate quality validation without suggesting that Playwright MCP is a full load-testing tool.

---

## 💡 Key Capabilities

Playwright MCP handles:
- **Browser control** — Launch, navigate, and close browsers
- **Screenshots** — Capture full pages and selected elements
- **Interactions** — Click, type, submit forms, and scroll
- **Assertions** — Verify text, elements, states, and URLs
- **Inspection** — Query DOM content and attributes
- **Timing** — Measure page and interaction timing

---

## 💡 Workflow Integration

**During development:** Run Task 1 after UI changes for quick visual validation.  
**During QA:** Use Tasks 3–6 for validation and end-to-end checks.  
**During debugging:** Repeat the smallest failing task and inspect the page state.  
**During documentation:** Save screenshots from Task 1 or Task 4.  

---

## Next Steps

Combine Playwright MCP with other tools:
- Use **Filesystem MCP** (in `.mcp.json`) to save test reports and screenshots
- Use **Copilot `/tests` command** to auto-generate Playwright test files from your manual testing scenarios
- Use **Code Review Agent** to validate test coverage of your changes
