# Setup: get ready for the labs

Welcome! Before F1, let's open your workspace, check that OctoCAT runs and make sure Copilot is ready.

**Choose Codespaces or local Node.js/npm.** Codespaces is recommended, not mandatory. Both options cover the same labs; complete the app and Copilot checks before the training rather than spending lab time on installation.

Both options require access to this repo and a GitHub account with a working Copilot licence. Local setup also requires permission to install the tools and download packages. If installation or enterprise network restrictions prevent setup, contact IT or ask a facilitator about pairing with someone who has a working environment.

## 1. Choose your environment

Complete **one** option, then continue with the shared app and Copilot checks.

### Option A: GitHub Codespaces (recommended)

A **codespace** gives you VS Code, this repo and its tools in the cloud, accessible through your browser.

1. Use the GitHub account with your Copilot licence to [open this repo in Codespaces](https://codespaces.new/shireendan/octocat-supply/tree/workshop).
2. Keep branch `workshop`, choose at least **4 cores and 8 GB RAM**, then select **Create codespace**.
3. Wait for `make install && make build` to finish installing dependencies and building the app.
4. Confirm the status bar shows `workshop`, or run `git branch --show-current` in the terminal. If `workshop` isn't listed, follow [branch recovery](#recovery-workshop-branch-missing).

Returning? Use the [resume link](https://codespaces.new/shireendan/octocat-supply/tree/workshop?quickstart=1) or [your codespaces](https://github.com/codespaces). Browser and desktop VS Code both work.

### Option B: Local Node.js and npm (Windows)

Run directly on your Windows computer in a normal local VS Code window, not a remote devbox. **No Docker or Make is needed.** The lab sheets use `make` shortcuts for Codespaces; use the [local command equivalents](#local-windows-equivalents-for-lab-commands) when following them.

1. Install [VS Code](https://code.visualstudio.com/), [Git](https://git-scm.com/downloads) and **Node.js 24.x**. npm comes with Node.js; it does not need a separate installer. If you already have Node.js, check its version in the next step before deciding whether to install it.

   For Node.js, use the [official Node.js 24 downloads](https://nodejs.org/download/release/latest-v24.x/), choose the Windows `.msi` for your computer's architecture, and leave npm and **Add to PATH** enabled. Do not assume the installer labelled "latest LTS" is still version 24.
2. Fully close and reopen VS Code after installation, then open a new PowerShell terminal.

   Check the tools:
   ```powershell
   node --version
   npm.cmd --version
   git --version
   ```
   Node.js should report `v24.x`. If Node.js or npm is not recognised, follow the [PATH troubleshooting](#nodejs-installed-but-not-recognised-in-powershell) before continuing. `npm.cmd` avoids PowerShell's `npm.ps1` execution-policy issue without changing your security policy.
3. Clone the `workshop` branch. From the folder where you want the checkout, run:
   ```powershell
   git clone --branch workshop https://github.com/shireendan/octocat-supply.git
   Set-Location .\octocat-supply
   code .
   ```
   Already cloned? Open that folder in VS Code instead; do not clone over it or discard existing changes. All remaining local commands start from the **repo root**, the folder containing `api` and `frontend`, unless stated otherwise.
4. In VS Code, choose **Terminal > New Terminal** and use PowerShell. Stop any running app servers before reinstalling dependencies. Run these commands **one at a time**, resolving any error before moving to the next:
   ```powershell
   npm.cmd --prefix .\api ci --include=optional
   npm.cmd --prefix .\frontend ci --include=optional
   npm.cmd --prefix .\api run build
   npm.cmd --prefix .\frontend run build
   ```

   `--prefix` selects the project without changing folders. `ci` replaces its installed dependencies using the locked versions, and `--include=optional` includes platform-specific packages such as Rollup. Your source files and lockfiles are unchanged. This clean install also replaces incompatible packages left from another operating system; do not copy installed dependencies between Windows and Linux.
5. Install or enable **GitHub Copilot** in VS Code and sign in with the account that has your Copilot licence. Continue with the app and Copilot checks below.

No separate database server or `.env` file is needed for the default local app. The API initializes and seeds its SQLite database automatically when started.

### Confirm your branch

For every option, confirm the status bar shows `workshop`, or run:

```bash
git branch --show-current
```

If `workshop` is missing, use the recovery guidance below. Do not switch branches over unsaved work or force a switch.

## Recovery: workshop branch missing

Already on `workshop`? Skip this recovery section.

`origin` is the usual name for the GitHub remote. In your **workspace terminal**, run `git remote get-url origin`. Only if it reports **No such remote** and `git status --short` is empty, run:

```bash
git remote add origin https://github.com/shireendan/octocat-supply.git
git fetch origin
git switch --track origin/workshop
```

If `origin` exists, there are local changes, or the switch fails, ask a facilitator. Don't replace a remote or force the switch.

## 2. Check that the app runs

The **API** handles OctoCAT's business records. **Swagger** is its interactive documentation, where you can try operations in your browser.

### Codespaces

1. In a terminal at the repo root, run:
   ```bash
   make dev
   ```
2. Wait for `Database initialized successfully`, `Server is running on port 3000` and the website's Vite startup message on port **5137**.
3. In **Ports**, open **3000**, then add **`/api-docs`** to the browser URL. Look for *Branches* and *Orders* in Swagger.
4. In **Ports**, open **5137** and check that the product catalog loads.

If you only need the API for a lab, use `make dev-api` instead of `make dev`.

### Local Node.js and npm

1. In **Terminal 1**, at the repo root, start the API:
   ```powershell
   npm.cmd --prefix .\api run dev
   ```
   Wait for `Database initialized successfully` and `Server is running on port 3000`.
2. Open **Terminal > New Terminal**. In **Terminal 2**, also at the repo root, start the website:
   ```powershell
   npm.cmd --prefix .\frontend run dev
   ```
3. Open http://localhost:3000/api-docs and look for *Branches* and *Orders* in Swagger.
4. Open http://localhost:5137 and check that the product catalog loads. Local users do not need a forwarded URL or the Ports tab.

For either option, leave the servers running while using the app. Stop them with **Ctrl+C** in their terminals when finished. On later sessions, repeat only your option's startup commands; you do not need to reinstall dependencies or build every time.

### Local Windows equivalents for lab commands

Run these from the repo root. When a lab says **Ports** then **3000**, use http://localhost:3000/api-docs locally instead.

| Codespaces command in a lab | Local Windows PowerShell |
|---|---|
| `make dev-api` | `npm.cmd --prefix .\api run dev` |
| `make dev-frontend` | `npm.cmd --prefix .\frontend run dev` |
| `make dev` | Run the two commands above in separate terminals |
| `make install` | `npm.cmd --prefix .\api install`, then `npm.cmd --prefix .\frontend install` |
| `make build` | `npm.cmd --prefix .\api run build`, then `npm.cmd --prefix .\frontend run build` |
| `make build-api` | `npm.cmd --prefix .\api run build` |
| `make build-frontend` | `npm.cmd --prefix .\frontend run build` |
| `make test-api` | `npm.cmd --prefix .\api test` |
| `make lint` | `npm.cmd --prefix .\api run lint`, then `npm.cmd --prefix .\frontend run lint` |

API tests watch for changes; press **q** to quit. For a single run, use:

```powershell
npm.cmd --prefix .\api test -- --run
```

When a lab asks for `make test`, use the API test command for its API exercise. `make test` is not a working full-suite command: after the API watcher exits, it calls a frontend `test` script that is not defined.

The initial local setup uses `ci` for a clean install from the lockfiles. Use `install` when deliberately changing dependencies, so the matching lockfile is updated.

## 3. Check Copilot

Copilot is your AI assistant in VS Code. Check that Chat responds before starting the labs.

Codespaces includes the Copilot extensions. For local VS Code, install or enable **GitHub Copilot** and sign in with the account that has your Copilot licence.

1. Open **Chat** using the Copilot icon.
2. Start a new chat, click **+**, then **Agent...**, then **Ask** to ask questions without editing files.
3. Send `hi` and check that Copilot replies.
4. Keep the default **model** and **session type** unless your facilitator says otherwise.

For the labs:
- **Agent** opens by default in a new chat and can edit files and run tools.
- **Plan** uses a separate control: click the **message icon at the bottom of Chat**, then choose **Plan**.

## 4. Get the reference files

Fetch once for local compare/catch-up commands; viewing `main` on GitHub needs no fetch, and fetching leaves your working files unchanged:

```bash
git fetch origin main
```

Use these when a lab asks; replace `<file>` with its path. **Catch up overwrites that file's edits; save a copy first.**

| I want to… | Do this |
|---|---|
| View a reference file | `git show origin/main:<file>` (or view it on GitHub's `main` branch) |
| Compare a file I **edited** | `git diff origin/main -- <file>` |
| Compare a file I **re-created** | `git add <file>` then `git diff origin/main -- <file>` |
| Compare side by side (Codespaces Bash) | `git show origin/main:<file> > /tmp/ref.md && code --diff /tmp/ref.md <file>` |
| Catch up (take the reference version) | `git checkout origin/main -- <file>` |

For side-by-side comparison in **local Windows PowerShell**, replace `<file>` in both lines below with the reference file's path:

```powershell
$reference = Join-Path $env:TEMP 'octocat-reference.md'
git show origin/main:<file> | Set-Content -Path $reference -Encoding utf8
code --diff $reference <file>
```

## 5. Ground rules

- Work on `workshop`. Don't commit or push unless a lab tells you to.
- Review the agent's changes before clicking **Keep**; use **Undo** to reject them.
- Ignore the generated `.agent.log` and `.agent-audit/` logs. Automatic checkpoint commits are disabled.

## Troubleshooting

| Problem | Try |
|---|---|
| Codespaces is unavailable | Follow the [local Node.js/npm setup](#option-b-local-nodejs-and-npm-windows) and complete its app and Copilot checks. |
| Local software installation is restricted or setup cannot finish in time | Ask a facilitator about pairing with someone who has a working setup; do not bypass organisation policies. |
| `node`, `npm` or `npm.cmd` is not recognised | Install Node.js 24.x, restart VS Code, and use the [PATH checks below](#nodejs-installed-but-not-recognised-in-powershell). |
| PowerShell says `npm.ps1` cannot be loaded | Use `npm.cmd` as shown above; do not change the execution policy. |
| Dependency installation or a build fails locally | Check that `node --version` reports 24.x, keep the full error, and ask a facilitator before continuing. Do not ignore a failed install or reuse Linux-installed dependencies on Windows. |
| Rollup's platform package is missing or SQLite reports `invalid ELF header` | Stop the servers and repeat the local `ci --include=optional` commands in [Option B](#option-b-local-nodejs-and-npm-windows). In Codespaces, run `npm ci --include=optional` from the affected project's directory. Do not delete the lockfiles or share installed dependencies between operating systems. |
| SQLite installation reports `node-gyp`, Python or Visual Studio build errors | A compatible prebuilt binary may be unavailable, requiring native build tools. Keep the full error and ask your facilitator or IT for the approved tools; the app itself is Node.js/TypeScript. |
| npm reports SSL/TLS, proxy or certificate errors | Local npm still needs approved package-registry access. Ask IT to check the proxy, certificates or internal package mirror. Do not disable certificate checks or bypass organisation policies. |
| `npm ci` reports a missing or out-of-sync lockfile | Check that you opened the intended `workshop` checkout. Keep the lockfiles and share the full error with a facilitator; use `npm install` only when intentionally updating dependencies. |
| Only `main` is listed / no `origin` | See [branch recovery](#recovery-workshop-branch-missing). |
| Port 3000 isn't listed in Codespaces | Start the API using the command for your environment and wait for `Server is running on port 3000`. Local users open http://localhost:3000/api-docs directly. |
| The browser shows 502 on a Codespaces port | The server isn't up yet: wait 10 seconds and refresh. Check the terminal for a startup error if it persists. |
| A local URL refuses the connection | Check the matching API or frontend terminal for errors and leave the server running. Use `http://`, not `https://`, for the local URLs. |
| Port 3000 or 5137 is already in use | Stop your earlier dev server with **Ctrl+C** in its terminal before restarting. Ask a facilitator if another application owns the port. |
| Chat says you have no access | Tell your facilitator: your Copilot licence may be on another account |
| Everything feels slow | The first build may still be running: check the terminal |

### Node.js installed but not recognised in PowerShell

The VS Code terminal can retain its old PATH after Node.js is installed. For the default Windows installer location, check that **both** commands return `True`:

```powershell
Test-Path "C:\Program Files\nodejs\node.exe"
Test-Path "C:\Program Files\nodejs\npm.cmd"
```

If either returns `False`, check where Node.js was installed or install Node.js 24.x with npm before continuing. If both return `True`, run this in **each PowerShell terminal** you will use:

```powershell
$env:Path = "C:\Program Files\nodejs;$env:Path"
node --version
npm.cmd --version
```

If you chose a different install directory, use that directory instead. This PATH update applies only to the current terminal. Fully close all VS Code windows and reopen VS Code to pick up the installer's PATH change for future terminals.

Checks passed? Continue to [F1: Prompting Fundamentals](F1-prompting-fundamentals.md). Otherwise, raise your hand.
