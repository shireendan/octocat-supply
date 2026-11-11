# F2 · Build Your Personal Agent: lab

[Labs index](README.md) · [Previous](F1-prompting-fundamentals.md) · [Next](F3-context-engineering.md)

## What will you do?

Build two focused AI teammates: an API Specialist for this project and a personal, read-only reviewer for your area. You'll compare a plan before and after customization, then check whether your reviewer respects its limits.

**Why it matters:** repeating your team's rules in every prompt is easy to get wrong. Custom agents make roles and boundaries reusable, visible and reviewable. [Microsoft Learn: why customize Copilot?](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/2-examine-benefits-customize-github-copilot)

**Your story:** OctoCAT needs a Cart API, and you've been asked for a plan that follows the team's conventions. You also want a reviewer for your daily work, but one that can only advise, not change your files.

Two parts:
- **Part A (⏱ 25 min):** rebuild the team's **API Specialist**, a *workspace* agent shared through the repo.
- **Part B (⏱ 20 min):** build **My Reviewer**, a *personal*, read-only agent for your own daily work.

**Missing on your branch:** `.github/agents/api-specialist.agent.md`. The 10 other agents in that folder are untouched, so have a look at them.
**Done when:** the agent picker shows your **API Specialist** and your **my-reviewer**.

## New words

| Term | What it means in this lab |
|---|---|
| **[Custom agent](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/4-create-custom-github-copilot-agents#what-are-custom-github-copilot-agents)** | A named role with saved instructions and tool permissions, not a newly trained model. |
| **[Workspace agent](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/4-create-custom-github-copilot-agents#what-are-custom-github-copilot-agents)** | Stored in the repo's [agent folder](../.github/agents/); versioned and shared through Git. Here: API Specialist. |
| **[User / personal agent](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/4-create-custom-github-copilot-agents#what-are-custom-github-copilot-agents)** | Stored in your editor profile, not the repo. In Codespaces, that's your codespace profile. Here: My Reviewer. |
| **[Frontmatter](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/4-create-custom-github-copilot-agents#agent-frontmatter-fields)** | The `---` header declaring the name, description and tools. The Markdown body below it holds the instructions. |
| **[Tools / guardrails](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/4-create-custom-github-copilot-agents#define-agent-capabilities-with-tools)** | The capabilities available to the agent. A `read`/`search`-only list leaves out editing and terminal access. |

**The key distinction:** instructions guide behaviour; tool permissions limit available actions. Asking an agent not to edit is different from removing its edit tool.

Links are optional Microsoft Learn references; follow the exercises below for the workshop's UI.

## Before you start

Complete the [setup checks](01-setup.md). Keep the same model for the before/after comparison so you can focus on what the agent configuration changes.

---

## Part A: Rebuild the API Specialist (⏱ 25 min)

### A1. Baseline with the built-in Agent (⏱ 3 min)
 Start a new chat (this opens Agent mode by default) and send:
```
I need a Cart API: POST /cart creates a cart and returns a cartId, plus CRUD endpoints under /cart/:cartId.
Don't implement anything yet. Reply with your plan only: the files you would create or change, the endpoints, and how you would test them.
```
Keep this chat open: you'll compare against it in A4.

### A2. Create the agent file (⏱ 4 min)
1. In the Chat view: Click on **+**, click on **Agent...**, choose **Configure Custom Agent** and click on **+** next to **Workspace**.
   (Or open the Command Palette with **Ctrl+Shift+P** → **Chat: New Custom Agent** → **Workspace**.)
2. Name it `api-specialist`. VS Code creates `.github/agents/api-specialist.agent.md` with a template.

> Can't find the menu? Ask the agent: `Create an empty custom agent file at .github/agents/api-specialist.agent.md`

### A3. Let Copilot draft it (⏱ 7 min)
Start a new chat, choose **Agent** and send:
```
Fill in .github/agents/api-specialist.agent.md as a custom agent for this repository:
- Header: name "API Specialist" and a one-line description.
- Body: the agent's role, a step-by-step workflow for adding or changing API endpoints, and its rules.
- Base it on docs/architecture.md and .github/instructions/api.instructions.md.
- Max 50 lines. Only edit this one file.
```
1. **Keep** the change, then read the file. Would you trust this colleague?
2. Change one thing yourself, for example add `Always propose tests for every change.`

### A4. Test it (⏱ 5 min)
1. Start a new chat, click on **+**, click on **Agent...**, choose **API Specialist** (yours) in the agent picker.
2. Send the **A1 prompt** again.
3. Compare with the baseline. Did it mention:
   - the repository pattern?
   - migrations?
   - Swagger docs?
   - tests?
   - error handling?

### A5. Compare with the original (⏱ 6 min)
```bash
git add .github/agents/api-specialist.agent.md
git diff origin/main -- .github/agents/api-specialist.agent.md
```
Or open the file on branch `main` on GitHub.

The original is 140 lines, so yours is shorter, and that's fine. What does the original have that yours doesn't (for example "when to use", example scenarios, tips)? Copy one idea you like into your version.

**Catch up:** `git checkout origin/main -- .github/agents/api-specialist.agent.md`

✅ Same model, same prompt, but a different (better) plan. The difference is the agent file.

---

## Part B: My Reviewer, a personal read-only agent (⏱ 20 min)

### B1. Create it (⏱ 4 min)
1. In the Chat view: Click on **+**, click on **Agent...**, choose **Configure Custom Agent** and click on **+** next to **User**. (If **User** isn't available, use **Workspace**; the agent works the same.)
2. Name it `my-reviewer`.
3. Replace the template with:
```markdown
---
name: my-reviewer
description: Reviews files in my area and explains the findings. Read-only - never edits files or runs commands.
tools: ['read', 'search']
---
You are my personal code reviewer. I am a <ROLE> and I mostly work on <AREA>.

When I ask for a review:
1. Read the file I point to, plus related files if you need them.
2. Check for: <CHECK 1>, <CHECK 2>, and obvious bugs or security issues.
3. Report at most 5 findings in a table: severity, line, problem, suggested fix.
4. Never edit files or run commands. If I ask you to fix something, explain the fix instead.
```
- **Why User?** A personal agent belongs to you, not to the repo. In this codespace it lives in your codespace profile. Workspace agents (Part A) are shared with your team through git.
- **Why `tools: ['read', 'search']`?** The agent can only read and search, which is a guardrail. Without a `tools` line it would get the default tools, including editing.

### B2. Make it yours (⏱ 5 min)
Fill in `<ROLE>`, `<AREA>` and two checks. Some ideas:

| Role | Example checks |
|---|---|
| Backend developer | SQL is always parameterised; errors use the shared error classes |
| Frontend developer | accessibility (labels, alt text, keyboard); no hard-coded API URLs |
| Data engineer | every schema change is a new numbered migration (old ones are never edited); foreign keys have indexes |
| Platform / DevOps | no secrets in plain text; resource limits and health checks are set |

### B3. Use it (⏱ 6 min)
Open a file from your area:

| Role | File |
|---|---|
| Backend developer | [api/src/routes/order.ts](../api/src/routes/order.ts) |
| Frontend developer | [frontend/src/components/Login.tsx](../frontend/src/components/Login.tsx) or [Navigation.tsx](../frontend/src/components/Navigation.tsx) |
| Data engineer | any file in [api/database/migrations/](../api/database/migrations/) |
| Platform / DevOps | [infra/container-apps.bicep](../infra/container-apps.bicep), [api/Dockerfile](../api/Dockerfile) or [docker-compose.yml](../docker-compose.yml) |

Start a new chat → choose **my-reviewer** in the agent picker → send:
```
Review the file I have open.
```

### B4. Guardrail test (⏱ 3 min)
In the same chat, send:
```
Fix the first finding.
```
It should **explain** the fix instead of editing, because it has no edit tool. Check **Source Control**: no files should have changed.

### B5. Share (⏱ 2 min)
Show your pair your best finding.

✅ You now have two agents: **API Specialist** (team, in the repo) and **my-reviewer** (personal, read-only).

---

## Extensions (if you finish early)
- Build a second personal agent for something you do every week, for example "explain this pipeline failure" or "write a PR description".
- **Hand-offs:** agents can pass work to each other with a button. See the header of [.github/agents/tdd-planner.agent.md](../.github/agents/tdd-planner.agent.md) ("Write Red Tests" → tdd-red). This only works in the *Local* session type.
- Turn your reviewer into a team agent: create it under **Workspace** so that it lives in [.github/agents/](../.github/agents/).

## Think about this for the wrap-up
- Same model, same prompt: what made the API Specialist answer differently?
- What would *your team's* reviewer check?
- Which of your weekly tasks deserves its own agent?
