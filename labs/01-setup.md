# Setup: open your codespace (do this before F1)

⏱ About 10 minutes, ideally during the 09:00 opening.

## 1. Create your codespace
1. Open this link (you need to be signed in to GitHub with the account your Copilot licence is on):
   **https://codespaces.new/shireendan/octocat-supply/tree/workshop**
2. Keep branch `workshop` and choose a machine with at least **4 cores and 8 GB RAM**, then choose **Create codespace**.
3. The first start takes a few minutes, because it installs and builds the app for you. Wait until the terminal shows that `make install && make build` has finished.

> To come back later, use **https://codespaces.new/shireendan/octocat-supply/tree/workshop?quickstart=1** or open github.com/codespaces.
> The codespace opens in your browser. If you prefer VS Code on your laptop, that works too.

## 2. Check that you're on the `workshop` branch
The status bar (bottom left) shows `workshop`. You can also check in the terminal:
```bash
git branch --show-current
```

## 3. Get the answer key (once)
```bash
git fetch origin main
```

## 4. Check that the app runs
1. In the terminal:
   ```bash
   make dev-api
   ```
2. Wait for `Database initialized successfully` and `Server is running on port 3000`.
3. Open the **Ports** tab → port **3000** → open it in the browser → add **`/api-docs`** to the end of the URL.
   You should see the Swagger page with sections such as *Branches* and *Orders*.
4. Stop the server with **Ctrl+C**.

Optional: `make dev` starts the API **and** the website (port **5137**). The dev container forwards both ports.

## 5. Check Copilot
1. Open the **Chat** view (the Copilot icon at the top or in the side bar).
2. At the bottom of the Chat view, find:
   - the **agent picker** (Ask / Agent / Plan);
   - the **model picker** (keep the default unless your facilitator says otherwise).
3. Choose **Ask** and type `hi`. You should get an answer.
4. If the chat shows a session type (for example "Copilot"), keep the default.

## 6. Compare and catch up (you'll use these in every lab)
| I want to… | Do this |
|---|---|
| See the original of a file | On GitHub, switch to branch **`main`** and open the file. Or: `git show origin/main:<file>` |
| Compare a file I **edited** | `git diff origin/main -- <file>` |
| Compare a file I **re-created** | `git add <file>` then `git diff origin/main -- <file>` |
| Compare side by side | `git show origin/main:<file> > /tmp/ref.md && code --diff /tmp/ref.md <file>` |
| Catch up (take the original) | `git checkout origin/main -- <file>` |

## 7. Ground rules
- Don't commit or push unless a lab tells you to; all work stays in your codespace.
- Always review what an agent changed before you click **Keep**. **Undo** is always available.
- The repo's Copilot hooks write `.agent.log` (session events and prompts) and `.agent-audit/<sessionId>/session-start.json` and `session-stop.json` (audit snapshots). Ignore these files. Automatic checkpoint commits are disabled.

## Troubleshooting
| Problem | Try |
|---|---|
| Port 3000 isn't listed | Run `make dev-api` again and wait for `Server is running on port 3000` |
| The browser shows 502 on port 3000 | The server isn't up yet: wait 10 seconds and refresh |
| Chat says you have no access | Tell your facilitator: your Copilot licence may be on another account |
| Everything feels slow | The first build may still be running: check the terminal |
