# Setup: get ready for the labs

Complete setup before F1. You need repo access and a GitHub account with a working Copilot licence.

## 1. Create your own repository

Use your own copy so your changes do not affect the shared workshop template.

1. Sign in to GitHub and open the [participant template](https://github.com/shireendan/octocat-supply-participants).
2. Select **Use this template > Create a new repository**. Do not fork or choose **Open in a codespace** here.
3. Set **Owner** to your account or the organisation named by your facilitator, **Repository name** to `octocat-supply-participants`, and **Visibility** to **Private** unless instructed otherwise.
4. Select **Include all branches**. You need both `workshop` and `reference_solution` for the labs.
5. Select **Create repository from template**. In your new repo's branch selector, check that both branches exist, then select `workshop`.

Below, replace `<YOUR-OWNER>` with the account or organisation you chose. Your repo is `https://github.com/<YOUR-OWNER>/octocat-supply-participants`; use it throughout the labs, not the shared template.

## 2. Choose your environment

Use **Windows DevBox/local** as the primary option, or **Codespaces** as the alternative. Complete one option, then check the app and Copilot below.

### Option A: Local Node.js and npm (Windows)

**Recommended.** Run directly in Windows on your DevBox or laptop. No Docker or Make is needed.

#### Install Node.js and Python

Skip tools already installed. On a **managed DevBox**, use the configured Chocolatey command in PowerShell:

```powershell
choco install vscode git nodejs
```

The managed installation uses the organisation's **ProGet feed** and handles the required installation rights. That feed currently provides **Node.js 26.x**. Do not use WinGet or public MSI installers on this DevBox, or change Chocolatey sources; ask IT if the command is unavailable or fails.

On **other Windows machines**, use approved installers for [VS Code](https://code.visualstudio.com/), [Git](https://git-scm.com/downloads) and [Node.js 24.x](https://nodejs.org/download/release/latest-v24.x/) with npm and **Add to PATH** enabled.

**Python 3.12.x** is only needed if SQLite requires native compilation. Obtain it through your approved software process or IT, install it in Windows rather than only WSL, and verify with `py -3.12 --version` in a fresh terminal. Do not replace an existing approved, working Python 3 setup. See [native-build help](troubleshooting.md#native-module-build-tools) if compilation fails.

#### Prepare your workspace

Fully close and reopen VS Code after installing tools. Open PowerShell and check:

```powershell
node --version
npm.cmd --version
git --version
```

Expect `v26.x` from the managed DevBox feed, or `v24.x` for the other Windows setup. An existing Node 24 installation can use the Node 24 commands below. Then clone **your new repo's** `workshop` branch from the folder where you want your checkout. Replace `<YOUR-OWNER>` before running:

```powershell
git clone --branch workshop "https://github.com/<YOUR-OWNER>/octocat-supply-participants.git"
Set-Location .\octocat-supply-participants
code .
```

Already cloned your own copy? Reuse that checkout. Keep the **repo root** open in VS Code, with `app` and `labs` visible, and confirm the status bar shows `workshop`. `git remote get-url origin` should show your account or organisation, not `shireendan`.

#### Install dependencies and build

In VS Code, open **Terminal > New Terminal** using PowerShell at the repo root. On managed Windows machines that need company certificates, first run `$env:NODE_USE_SYSTEM_CA = "1"` in this terminal. This uses Windows' trusted certificates; see [certificate help](troubleshooting.md#enterprise-certificate-trust) if errors remain.

Stop any running app servers before reinstalling. Run these **one at a time; stop if any command fails**:

**First install API dependencies: choose one command matching `node --version`.**

**Node 26.x (managed DevBox):** update SQLite instead of installing the older locked API version:

```powershell
npm.cmd --prefix .\app\api update better-sqlite3 --include=optional
```

This installs a newer SQLite package and updates the API lockfile for Node 26. Keep that updated lockfile.

**Node 24.x:** install the locked API dependencies:

```powershell
npm.cmd --prefix .\app\api ci --include=optional
```

**Then, for either Node version:**

```powershell
npm.cmd --prefix .\app\frontend ci --include=optional
npm.cmd --prefix .\app\api run build
npm.cmd --prefix .\app\frontend run build
```

### Option B: GitHub Codespaces

Use this alternative if you cannot prepare the Windows environment. Node.js 24.x, Python and the other tools are provided automatically; the Node 26 SQLite update is not needed here.

1. On GitHub, open **your new repo**, not the shared template, and select branch `workshop`.
2. Choose **Code > Codespaces** and create a codespace on `workshop`, using at least **4 cores and 8 GB RAM**. Use **New with options** if you need to choose the machine.
3. Wait for `make -C app install && make -C app build` to finish installing dependencies and building the app.
4. Confirm the status bar shows `workshop`.

## 3. Check that the app runs

Use the commands for your chosen environment. No separate database server or `.env` file is needed.

First, from the repo root in either environment, fetch the reference configuration:

```bash
git fetch origin reference_solution:refs/remotes/origin/reference_solution
```

This leaves your branch and files unchanged. If the branch is missing, use the [reference-file help](troubleshooting.md#get-the-reference-files).

### Local Node.js and npm

Use two PowerShell terminals at the repo root:

1. **Terminal 1 - API:**
   ```powershell
   npm.cmd --prefix .\app\api run dev
   ```
   Wait for `Database initialized successfully` and `Server is running on port 3000`.
2. **Terminal 2 - website:**
   ```powershell
   npm.cmd --prefix .\app\frontend run dev
   ```
3. Open http://localhost:3000/api-docs, execute **GET `/api/branches`**, and check for **200** with JSON data.
4. Open http://localhost:5137/products and check that product cards load.

**Keep both terminals running.** On a remote DevBox, open the URLs in the DevBox's browser.

### Codespaces

1. In a terminal at the repo root, run:
   ```bash
   make -C app dev
   ```
2. Wait for the API startup message on **3000** and the website's Vite startup message on **5137**.
3. In **Ports**, open **3000**, add `/api-docs` to the URL and try **GET `/api/branches`**. Expect **200** with JSON data.
4. In **Ports**, open **5137**, choose **Products** and check that product cards load.

Stop servers with **Ctrl+C** when finished. Next time, repeat only the startup commands, not installation.

## 4. Check Copilot and learn the Chat controls

1. Install or enable **GitHub Copilot** in local VS Code; Codespaces includes it.
2. Sign in with your licensed GitHub account. Open **Chat** with **Ctrl+Alt+I**, then select **New Chat (+)**.
3. Find these controls around the chat input. Their position and labels can vary by VS Code version.

| Control | Where to look | What it controls / workshop choice |
|---|---|---|
| **Session target / harness** | First selector below the input | The runtime running the session, not the model. Choose **Copilot** when available; keep the same target during the labs. This is separate from choosing Windows or Codespaces. |
| **Agent / mode** | Agent picker or **Agent...** near the input | How Copilot works: **Ask**, **Agent** or **Plan**, explained below. Custom agents created in F2 appear in the agent picker too. |
| **Model and reasoning effort** | Model name inside or below the input | Which model answers and how much reasoning it uses. Keep **Auto** or the approved default; use the same model for before/after comparisons. A context-size number is not another model. |
| **Permissions** | Permissions selector, sometimes inside the mode menu | Which tool actions need approval. Use **Manual permissions** when available, or your organisation's required settings. Review requests; do not enable **Allow all** or **Autopilot**. |

- **Ask:** answer questions and inspect code with read-only tools, without changing files.
- **Agent:** edit files and use tools to carry out the requested task. Some versions also show **Interactive** as the working mode; that does not make the session read-only.
- **Plan:** research and propose an approach before implementation. Choose **Plan**, or use `/plan` where offered. Do not approve implementation when a lab asks for a plan only.

Select **Ask** in the agent picker for question-only steps, even if a separate mode control says **Interactive**. If a required control is missing, ask a facilitator. **New Chat** clears conversation context, not your files; use **Add Context** or `#` to attach files the prompt depends on.

**Try it once in Ask:** send `Which folders contain the API and frontend? Do not change any files.` Expect `app/api` and `app/frontend`, with no file edits. For later Agent tasks, inspect tool calls and changed files before choosing **Keep** or **Undo**.

Optional reference: [VS Code Chat guide](https://code.visualstudio.com/docs/chat/chat-overview).

**Ready?** Continue to [F1: Prompting Fundamentals](F1-prompting-fundamentals.md).

**Need help or reference commands?** Open [Troubleshooting and reference](troubleshooting.md), or ask a facilitator. Do not continue past a failed setup command.
