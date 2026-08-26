# 00 — Prerequisites

> Complete this before the workshop starts. It takes about 10 minutes.

---

## Tools you need

| Tool | Version | Download |
|------|---------|----------|
| Node.js | 20+ | https://nodejs.org |
| JDK | 21 | https://adoptium.net |
| VS Code **or** IntelliJ IDEA | Latest | See below |
| GitHub account with Copilot access | — | https://github.com/settings/copilot |

### VS Code
Install the **GitHub Copilot** extension from the marketplace:
```
ext install GitHub.copilot
```
Also install **GitHub Copilot Chat** if it doesn't come bundled.

### IntelliJ IDEA
Install the **GitHub Copilot** plugin via *Settings → Plugins → Marketplace*.

---

## Verify your Copilot access

Open VS Code (or IntelliJ) and look for the Copilot icon in the status bar (bottom right).  
It should show a ✅ or your GitHub avatar — not a ⚠️ or ✖️.

If it shows an error, sign in via *GitHub Copilot: Sign In* from the command palette.

---

## Fork and clone the repo

### 1. Fork the repo

Go to **https://github.com/Ivarafa/copilot-ws** and click **Fork** (top-right corner). This creates your own copy where you can push changes.

### 2. Clone your fork

```bash
git clone https://github.com/your-username/copilot-ws.git
cd copilot-ws
```

### 3. Start both the frontend and backend

**Frontend:**
```bash
cd frontend
npm install
npm run dev
```
→ Opens at **http://localhost:5173**

**Backend:**
```bash
cd backend

# Linux / macOS
./gradlew bootRun

# Windows
.\gradlew.bat bootRun
```
→ API available at **http://localhost:8080/api**  
→ H2 console at **http://localhost:8080/h2-console**

> **H2 console settings:** JDBC URL `jdbc:h2:mem:webshopdb`, User `sa`, Password *(empty)*

---

## Expected result

| URL | What you should see |
|-----|---------------------|
| http://localhost:5173 | Frontend shell — "No products loaded yet" |
| http://localhost:8080/api/products | JSON array of 10 products |
| http://localhost:8080/h2-console | H2 database browser |

If both are running — you're ready! Head to [Part 1 →](./01-copilot-instructions.md)
