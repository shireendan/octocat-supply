# Setup: get ready for the labs

Welcome! Before F1, let's open your workspace, check that OctoCAT runs and make sure Copilot is ready.

## 1. Open your codespace

A **codespace** gives you VS Code, this repo and its tools in the cloud, accessible through your browser.

1. Use the GitHub account with your Copilot licence to [open this repo in Codespaces](https://codespaces.new/shireendan/octocat-supply/tree/workshop).
2. Keep branch `workshop`, choose at least **4 cores and 8 GB RAM**, then select **Create codespace**.
3. Wait for `make install && make build` to finish installing dependencies and building the app.
4. Confirm the status bar shows `workshop`, or run `git branch --show-current` in the terminal. If `workshop` isn't listed, follow [branch recovery](#recovery-workshop-branch-missing).

Returning? Use the [resume link](https://codespaces.new/shireendan/octocat-supply/tree/workshop?quickstart=1) or [your codespaces](https://github.com/codespaces). Browser and desktop VS Code both work.

## Recovery: workshop branch missing

Already on `workshop`? Skip this recovery section.

`origin` is the usual name for the GitHub remote. In the **Codespace terminal**, run `git remote get-url origin`. Only if it reports **No such remote** and `git status --short` is empty, run:

```bash
git remote add origin https://github.com/shireendan/octocat-supply.git
git fetch origin
git switch --track origin/workshop
```

If `origin` exists, there are local changes, or the switch fails, ask a facilitator. Don't replace a remote or force the switch.

## 2. Check that the app runs

The **API** handles OctoCAT's business records. **Swagger** is its interactive documentation, where you can try operations in your browser.

1. In the repo root, run:
   ```bash
   make dev-api
   ```
2. Wait for `Database initialized successfully` and `Server is running on port 3000`.
3. In **Ports**, open **3000**, then add **`/api-docs`** to the browser URL. Look for *Branches* and *Orders* in Swagger.
4. Stop the server with **Ctrl+C**.

Optional: `make dev` also starts the website on port **5137**. Open it through Ports; stop with **Ctrl+C**.

## 3. Check Copilot

Copilot is your AI assistant in VS Code. Check that Chat responds before starting the labs.

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
| Compare side by side | `git show origin/main:<file> > /tmp/ref.md && code --diff /tmp/ref.md <file>` |
| Catch up (take the reference version) | `git checkout origin/main -- <file>` |

## 5. Ground rules

- Work on `workshop`. Don't commit or push unless a lab tells you to.
- Review the agent's changes before clicking **Keep**; use **Undo** to reject them.
- Ignore the generated `.agent.log` and `.agent-audit/` logs. Automatic checkpoint commits are disabled.

## Troubleshooting

| Problem | Try |
|---|---|
| Only `main` is listed / no `origin` | See [branch recovery](#recovery-workshop-branch-missing). |
| Port 3000 isn't listed | Run `make dev-api` again and wait for `Server is running on port 3000` |
| The browser shows 502 on port 3000 | The server isn't up yet: wait 10 seconds and refresh |
| Chat says you have no access | Tell your facilitator: your Copilot licence may be on another account |
| Everything feels slow | The first build may still be running: check the terminal |

Checks passed? Continue to [F1: Prompting Fundamentals](F1-prompting-fundamentals.md). Otherwise, raise your hand.
