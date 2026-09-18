# 00 — Prerequisites

> Complete this before the workshop starts. It takes about 10-15 minutes.

---

## Tools you need

| Tool | Version | Download |
|------|---------|----------|
| Node.js | 20+ | https://nodejs.org |
| JDK | 25 (latest LTS) | https://adoptium.net |
| Gradle | 9.7.1 (latest) | https://gradle.org/install |
| VS Code **or** IntelliJ IDEA | Latest | See below |
| GitHub account with Copilot access | — | https://github.com/settings/copilot |

> **Note:** The backend project uses the **Gradle Wrapper** (`gradlew` / `gradlew.bat`), so you normally don't need Gradle installed globally to *run* the project. Installing it yourself is still useful for troubleshooting, regenerating the wrapper, or running `gradle` commands directly — follow the section below if you hit wrapper issues.

Each section below has a **Windows** and a **macOS** tab. Follow the one that matches your machine.

### 1. Node.js 20+

**Windows**
```powershell
winget install OpenJS.NodeJS.LTS
```
Or download the installer from https://nodejs.org and run it.

**macOS**
```bash
brew install node@20
brew link --overwrite node@20
```
Or download the installer from https://nodejs.org.

Verify with:
```bash
node -v   # should print v20.x or higher
npm -v
```

### 2. JDK 25 (latest LTS)

**Windows**
```powershell
winget install EclipseAdoptium.Temurin.25.JDK
```
Then set `JAVA_HOME` if it isn't set automatically:
```powershell
setx JAVA_HOME "C:\Program Files\Eclipse Adoptium\jdk-25"
```
(Reopen your terminal after `setx`.)

**macOS**
```bash
brew install --cask temurin@25
```
Then point `JAVA_HOME` at it (add to `~/.zshrc` or `~/.bash_profile`):
```bash
export JAVA_HOME=$(/usr/libexec/java_home -v 25)
```

Verify with:
```bash
java -version   # should print 25.x
```

### 3. Gradle 9.7.1 (latest)

You only need this if you want the `gradle` command available globally (e.g. to regenerate the wrapper or debug build issues). The workshop itself uses the wrapper committed in `backend/gradlew`.

**Windows**
```powershell
choco install gradle
```
Or via [SDKMAN for Windows/WSL](https://sdkman.io/):
```bash
sdk install gradle 9.7.1
```

**macOS**
```bash
brew install gradle
```
Or via [SDKMAN](https://sdkman.io/):
```bash
sdk install gradle 9.7.1
```

Verify with:
```bash
gradle -v   # should show Gradle 9.7.1 and JVM 25
```

> **Troubleshooting the wrapper:** If `./gradlew` fails with something like *"'gradle' is not recognized"* or *"cannot find gradle-wrapper.jar"*, the wrapper files are missing or broken. Once you have Gradle installed globally, regenerate them from the `backend/` folder:
> ```bash
> gradle wrapper --gradle-version 9.7.1
> ```
> This creates/updates `gradlew`, `gradlew.bat`, and `gradle/wrapper/gradle-wrapper.{jar,properties}`. Commit all four files.

### 4. VS Code or IntelliJ IDEA

**VS Code — Windows & macOS**

Install from https://code.visualstudio.com, then install the **GitHub Copilot** extension from the marketplace:
```
ext install GitHub.copilot
```
Also install **GitHub Copilot Chat** if it doesn't come bundled.

**IntelliJ IDEA — Windows & macOS**

Install from https://www.jetbrains.com/idea/, then install the **GitHub Copilot** plugin via *Settings → Plugins → Marketplace*.

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

**Windows (PowerShell)**
```powershell
git clone https://github.com/your-username/copilot-ws.git
cd copilot-ws
```

**macOS (Terminal)**
```bash
git clone https://github.com/your-username/copilot-ws.git
cd copilot-ws
```

### 3. Start both the frontend and backend

**Frontend — Windows & macOS**
```bash
cd frontend
npm install
npm run dev
```
→ Opens at **http://localhost:5173**

**Backend — Windows**
```powershell
cd backend
.\gradlew.bat bootRun
```

**Backend — macOS**
```bash
cd backend
./gradlew bootRun
```
→ API available at **http://localhost:8080/api**
→ H2 console at **http://localhost:8080/h2-console**

> **H2 console settings:** JDBC URL `jdbc:h2:mem:webshopdb`, User `sa`, Password *(empty)*

> **First run is slower:** the wrapper downloads Gradle 9.7.1 automatically the first time you run `gradlew`. This needs an internet connection and can take a minute or two.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `'gradle' is not recognized` / `'java' is not recognized` | Tool not installed or not on `PATH` | Reinstall via the steps above and open a **new** terminal window |
| `./gradlew` fails immediately with no useful error | Wrapper files missing/corrupted (`gradle/wrapper/gradle-wrapper.jar`) | Run `gradle wrapper --gradle-version 9.7.1` from `backend/` with Gradle installed globally, then commit the regenerated files |
| Build fails with a Java version error (e.g. "Unsupported class file major version") | Wrong `JAVA_HOME` or multiple JDKs installed | Confirm `java -version` reports 25, and that `JAVA_HOME` points to the JDK 25 install |
| `npm install` fails with engine warnings | Node.js version too old | Confirm `node -v` reports 20 or higher |
| Port `5173` or `8080` already in use | Another process is using the port | Stop the other process, or change the port in `vite.config.ts` / `application.yml` |

---

## Expected result

| URL | What you should see |
|-----|---------------------|
| http://localhost:5173 | Frontend shell — "No products loaded yet" |
| http://localhost:8080/api/products | JSON array of 10 products |
| http://localhost:8080/h2-console | H2 database browser |

If both are running — you're ready! Head to [Part 1 →](./01-copilot-instructions.md)
