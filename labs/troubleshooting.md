# Troubleshooting and reference

[Back to setup](01-setup.md) · [Labs index](README.md)

You do not need to read this guide before starting. Use the relevant section if a setup command fails or a lab asks you to compare files or run tests.

## Common problems

| Problem | Try |
|---|---|
| Local software installation is restricted or setup cannot finish in time | Check whether [Codespaces](01-setup.md#option-b-github-codespaces) is available, or ask a facilitator about pairing. Do not bypass organisation policies. |
| Codespaces is unavailable | Follow the primary [Windows setup](01-setup.md#option-a-local-nodejs-and-npm-windows). |
| `choco` is unavailable or installation is blocked on the managed DevBox | Ask IT for the approved Chocolatey/ProGet route. Do not install Chocolatey yourself, change feeds or switch to public installers. |
| `node`, `npm` or `npm.cmd` is not recognised | Follow the [approved installation route](01-setup.md#install-nodejs-and-python), restart VS Code, and use the [PATH checks](#nodejs-installed-but-not-recognised-in-powershell). |
| PowerShell says `npm.ps1` cannot be loaded | Use `npm.cmd` as shown in setup; do not change the execution policy. |
| Dependency installation or a build fails locally | Check `node --version`: managed DevBoxes use 26.x with the [SQLite update](#node-26-and-sqlite); the other Windows setup uses 24.x. Keep the full error and ask a facilitator before continuing. |
| Rollup's platform package is missing or SQLite reports `invalid ELF header` | Stop the servers and repeat the clean-install commands in [setup](01-setup.md#install-dependencies-and-build). In Codespaces, run `npm ci --include=optional` from the affected project's directory. Do not delete lockfiles or share installed dependencies between Windows and Linux. |
| SQLite installation reports `node-gyp`, Python or Visual Studio build errors | Fix any earlier certificate/download error first, then check the [native-build requirements](#native-module-build-tools). |
| npm reports `unable to get local issuer certificate`, SSL/TLS or proxy errors | Follow [enterprise certificate trust](#enterprise-certificate-trust), then ask IT about remaining CA-chain, proxy or package-mirror requirements. |
| npm warns that the Node engine is unsupported | Check which package is named. For `better-sqlite3` on Node 26, use the [SQLite update](#node-26-and-sqlite). For other warnings, ask a facilitator; do not delete lockfiles or make arbitrary dependency changes. |
| `npm ci` reports a missing or out-of-sync lockfile | Check that you opened the intended `workshop` checkout. Keep the lockfiles and share the full error with a facilitator. |
| Only `reference_solution` is listed / no `origin` | See [branch recovery](#recovery-workshop-branch-missing). |
| `reference_solution` is missing | Your template copy needed **Include all branches**. Keep your work and see [reference-file help](#get-the-reference-files). |
| `origin` points to the shared template | Keep any local work and ask a facilitator before changing remotes. The labs use your own generated repo. |
| Products do not load, but the website opens | Keep both the API and frontend terminals running. Check http://localhost:3000/api/products in a browser on the machine running the app. A successful earlier Swagger request does not mean the API is still running. |
| Port 3000 is not listed in Codespaces | Start the API and wait for `Server is running on port 3000`. Local Windows users open http://localhost:3000/api-docs directly. |
| The browser shows 502 on a Codespaces port | Wait for server startup, then refresh. Check the terminal for an error if it persists. |
| A local URL refuses the connection | Check the matching server terminal and leave it running. Use `http://`, not `https://`, for the local URLs. On a remote DevBox, open the URL in the DevBox's browser. |
| Port 3000 or 5137 is already in use | Stop your earlier dev server with **Ctrl+C** in its terminal before restarting. Ask a facilitator if another application owns the port. |
| Chat says you have no access | Tell your facilitator: your Copilot licence may be on another account. |
| Everything feels slow | The first build may still be running; check the terminal. |

### Node.js installed but not recognised in PowerShell

VS Code can retain its old PATH after Node.js is installed. For the default Windows installer location, check that both commands return `True`:

```powershell
Test-Path "C:\Program Files\nodejs\node.exe"
Test-Path "C:\Program Files\nodejs\npm.cmd"
```

If either returns `False`, check the installation location or use the [approved installation route](01-setup.md#install-nodejs-and-python) with npm included. If both return `True`, run this in each PowerShell terminal you will use:

```powershell
$env:Path = "C:\Program Files\nodejs;$env:Path"
node --version
npm.cmd --version
```

Use your actual installation directory if different. This update applies only to the current terminal. Fully close all VS Code windows and reopen VS Code to pick up the installer's PATH change for future terminals.

### Enterprise certificate trust

This guidance is for native Windows Node/npm on a managed laptop or DevBox, not a blanket prerequisite for all participants or for Codespaces.

`unable to get local issuer certificate` means Node cannot verify a download's certificate chain. A company proxy or HTTPS-inspection service may use a certificate authority (CA) already trusted by Windows but not by Node's default certificate bundle. It is a trust-configuration problem, not proof that the download is deliberately blocked.

If your organisation's approved certificates are already trusted by Windows, run this before npm installation in the same PowerShell terminal:

```powershell
$env:NODE_USE_SYSTEM_CA = "1"
```

This is supported in Node.js 24.6.0 and newer 24.x releases, and in Node.js 26.x. It adds Windows' trusted CAs to Node's existing trust roots, keeps HTTPS verification enabled, and leaves `NODE_OPTIONS` unchanged. It affects only processes started from that terminal and their children; repeat it in new terminals when needed.

If the error remains, ask IT to check the approved certificate chain and proxy configuration for the failing download host. npm packages, SQLite's prebuilt binaries and Node's build headers can use different hosts.

If IT provides a PEM-format CA bundle, use its actual path. For example, from the repo root:

```powershell
$env:NODE_EXTRA_CA_CERTS = "C:\certs\company-ca-bundle.pem"
```

Then retry the dependency command for your Node version from [setup](01-setup.md#install-dependencies-and-build).

The path is an example, not a file included in this repo. Use only the approved bundle supplied by IT. Node reads it when a new process starts, and it can be used alongside `NODE_USE_SYSTEM_CA`. Follow IT's proxy or internal-mirror instructions where required; do not disable certificate verification or bypass organisation controls.

Changing Node versions or installing Python does not fix certificate trust. See [Node.js enterprise network configuration](https://nodejs.org/learn/http/enterprise-network-configuration) for background.

### Native-module build tools

The app runs on Node.js/TypeScript. Python is only needed when a native dependency, such as the SQLite connector, must be compiled because a compatible prebuilt binary is unavailable or its download failed.

- Use the [Windows Python installation guidance](01-setup.md#install-nodejs-and-python). Python installed only in WSL is not automatically usable by Windows npm.
- On Node 26, apply the [SQLite update](#node-26-and-sqlite) before troubleshooting compilation of the older connector.
- Compilation also needs approved Windows C++ build tools. Ask IT for the appropriate Visual Studio **Desktop development with C++** workload; Windows on ARM may need additional ARM64 components.
- Fix certificate/download problems first. Compilation may itself need HTTPS downloads, so Python alone does not resolve a certificate error.

See the [node-gyp installation requirements](https://github.com/nodejs/node-gyp#installation). Do not install additional build tools or change organisation policies without approval.

### Node 26 and SQLite

The original API lockfile pins an older `better-sqlite3`. For the managed DevBox's Node 26, run from the repo root:

```powershell
npm.cmd --prefix .\app\api update better-sqlite3 --include=optional
npm.cmd --prefix .\app\api ls better-sqlite3
```

Node 26 needs a recent 12.x release declaring Node 26 support, such as **12.10.0 or newer**. The update stays within the API's existing dependency range and intentionally updates its lockfile; keep that file for later installs. If your configured registry still supplies an older version or the update fails, share the full output with IT or a facilitator. Do not change registries or disable HTTPS verification. Updating SQLite does not fix a separate certificate-trust problem.

## Recovery: workshop branch missing

Already on `workshop`? Skip this section.

In the workspace terminal, run `git remote get-url origin`. Only if it reports **No such remote** and `git status --short` is empty, run the following, replacing `<YOUR-OWNER>` with the owner of **your generated repo**:

```bash
git remote add origin "https://github.com/<YOUR-OWNER>/octocat-supply-participants.git"
git fetch origin
git switch --track origin/workshop
```

If `origin` exists, there are local changes, or the switch fails, ask a facilitator. Do not replace a remote or force the switch.

## Local Windows equivalents for lab commands

Run these from the repo root. When a lab says **Ports** then **3000**, local users open http://localhost:3000/api-docs instead.

| Windows PowerShell | Codespaces |
|---|---|
| `npm.cmd --prefix .\app\api run dev` | `make -C app dev-api` |
| `npm.cmd --prefix .\app\frontend run dev` | `make -C app dev-frontend` |
| Run the two commands above in separate terminals | `make -C app dev` |
| `npm.cmd --prefix .\app\api install`, then `npm.cmd --prefix .\app\frontend install` | `make -C app install` |
| `npm.cmd --prefix .\app\api run build`, then `npm.cmd --prefix .\app\frontend run build` | `make -C app build` |
| `npm.cmd --prefix .\app\api run build` | `make -C app build-api` |
| `npm.cmd --prefix .\app\frontend run build` | `make -C app build-frontend` |
| `npm.cmd --prefix .\app\api test` | `make -C app test-api` |
| `npm.cmd --prefix .\app\api run lint`, then `npm.cmd --prefix .\app\frontend run lint` | `make -C app lint` |

API tests watch for changes; press **q** to quit. For a single run, which is also preferable when an agent runs tests:

**Windows PowerShell:**
```powershell
npm.cmd --prefix .\app\api test -- --run
```

**Codespaces:**
```bash
npm --prefix app/api test -- --run
```

`ci` replaces installed dependencies with the locked versions, including platform-specific packages, without changing source files or lockfiles. The Node 26 API setup instead uses `update better-sqlite3` and intentionally updates the API lockfile. Follow the [version-specific setup](01-setup.md#install-dependencies-and-build) for the first installation. Use `install` when deliberately changing other dependencies so the matching lockfile is updated.

## Get the reference files

In your template-generated repo, `origin` points to your own account or organisation. Fetch once for local compare/catch-up commands; this also works when a Codespace initially fetched only `workshop`. Fetching leaves your working files unchanged:

```bash
git fetch origin reference_solution:refs/remotes/origin/reference_solution
```

If Git says the remote branch is missing, you may have skipped **Include all branches** when creating your repo. Keep your work and ask a facilitator; do not reset or recreate it. You can still [view the template's reference files](https://github.com/shireendan/octocat-supply-participants/tree/reference_solution) on GitHub.

Replace `<file>` below with the file's path. **Catch up overwrites that file's edits; save a copy first.**

| I want to… | Do this |
|---|---|
| View a reference file | `git show origin/reference_solution:<file>` or view it on your repo's `reference_solution` branch on GitHub |
| Compare a file I **edited** | `git diff origin/reference_solution -- <file>` |
| Compare a file I **re-created** | `git add <file>` then `git diff origin/reference_solution -- <file>` |
| Compare side by side (Codespaces Bash) | `git show origin/reference_solution:<file> > /tmp/ref.md && code --diff /tmp/ref.md <file>` |
| Catch up (take the reference version) | `git checkout origin/reference_solution -- <file>` |

For side-by-side comparison in Windows PowerShell, replace `<file>` in both lines:

```powershell
$reference = Join-Path $env:TEMP 'octocat-reference.md'
git show origin/reference_solution:<file> | Set-Content -Path $reference -Encoding utf8
code --diff $reference <file>
```

## Optional: version details for setup support

If a facilitator asks you to compare working setups, run these from the repo root after dependencies have installed successfully:

```powershell
node --version
npm.cmd --version
npm.cmd --prefix .\app\api ls better-sqlite3
```

These commands only display installed versions. You do not need to record them for normal setup.

## Copilot controls and lab ground rules

- Use the [Chat-controls overview in setup](01-setup.md#4-check-copilot-and-learn-the-chat-controls) for the session target, Ask/Agent/Plan, model and permissions.
- Work on `workshop`. Do not commit or push unless a lab tells you to.
- Review changes before choosing **Keep**; use **Undo** to reject them.
- Ignore generated `.agent.log` and `.agent-audit/` logs. Automatic checkpoint commits are disabled.

## Returning to Codespaces

Open [your codespaces](https://github.com/codespaces) and resume the one belonging to **your own repo**. To create another, follow the [Codespaces setup](01-setup.md#option-b-github-codespaces) from that repo, not from the shared template. Browser and desktop VS Code both work.
