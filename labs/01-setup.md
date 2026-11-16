# Setup: get ready for the labs

Complete setup before F1. You need repo access and a GitHub account with a working Copilot licence.

## 1. Choose your environment

Use **Windows DevBox/local** as the primary option, or **Codespaces** as the alternative. Complete one option, then check the app and Copilot below.

### Option A: Local Node.js and npm (Windows)

**Recommended.** Run directly in Windows on your DevBox or laptop. No Docker or Make is needed.

#### Install Node.js and Python

Use your organisation's approved installers. Skip installation for tools already available on your DevBox.

- Install [VS Code](https://code.visualstudio.com/) and [Git](https://git-scm.com/downloads).
- Install **Node.js 24.x**, not 26, using the Windows `.msi` from the [Node.js 24 downloads](https://nodejs.org/download/release/latest-v24.x/). Keep npm and **Add to PATH** enabled.
- **Python 3.12.x** is recommended if SQLite needs native compilation. Install in Windows, not only WSL. If needed and WinGet is permitted:
  ```powershell
  winget install --id Python.Python.3.12 --exact --source winget
  ```
  Verify it in a fresh terminal with `py -3.12 --version`. An existing approved, working Python 3 setup does not need replacing. See [native-build help](troubleshooting.md#native-module-build-tools) if compilation fails.

#### Prepare your workspace

Fully close and reopen VS Code after installing tools. Open PowerShell and check:

```powershell
node --version
npm.cmd --version
git --version
```

Node should report `v24.x`. Then clone the `workshop` branch from the folder where you want your checkout:

```powershell
git clone --branch workshop https://github.com/shireendan/octocat-supply.git
Set-Location .\octocat-supply
code .
```

Already cloned? Open that checkout instead. Keep the **repo root** open in VS Code, with `app` and `labs` visible, and confirm the status bar shows `workshop`.

#### Install dependencies and build

In VS Code, open **Terminal > New Terminal** using PowerShell at the repo root. On managed Windows machines that need company certificates, first run `$env:NODE_USE_SYSTEM_CA = "1"` in this terminal. This uses Windows' trusted certificates; see [certificate help](troubleshooting.md#enterprise-certificate-trust) if errors remain.

Stop any running app servers before reinstalling. Run these **one at a time; stop if any command fails**:

```powershell
npm.cmd --prefix .\app\api ci --include=optional
npm.cmd --prefix .\app\frontend ci --include=optional
npm.cmd --prefix .\app\api run build
npm.cmd --prefix .\app\frontend run build
```

### Option B: GitHub Codespaces

Use this alternative if you cannot prepare the Windows environment. Node.js, Python and the other tools are provided automatically.

1. Use the GitHub account with your Copilot licence to [open this repo in Codespaces](https://codespaces.new/shireendan/octocat-supply/tree/workshop).
2. Keep branch `workshop`, choose at least **4 cores and 8 GB RAM**, then select **Create codespace**.
3. Wait for `make -C app install && make -C app build` to finish installing dependencies and building the app.
4. Confirm the status bar shows `workshop`.

## 2. Check that the app runs

Use the commands for your chosen environment. No separate database server or `.env` file is needed.

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

## 3. Check Copilot

1. Install or enable **GitHub Copilot** in local VS Code; Codespaces includes it.
2. Sign in with your licensed GitHub account, open **Chat**, send `hi` and check that Copilot replies.
3. Keep the default model and session type unless your facilitator says otherwise.

**Ready?** Continue to [F1: Prompting Fundamentals](F1-prompting-fundamentals.md).

**Need help or reference commands?** Open [Troubleshooting and reference](troubleshooting.md), or ask a facilitator. Do not continue past a failed setup command.
