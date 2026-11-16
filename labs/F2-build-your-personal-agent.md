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
| **[User / personal agent](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/4-create-custom-github-copilot-agents#what-are-custom-github-copilot-agents)** | Stored in the active VS Code user profile, not the repo: the profile on your Windows laptop/DevBox or in your codespace. Here: My Reviewer. |
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
- Base it on app/docs/architecture.md and .github/instructions/api.instructions.md.
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
- **Why User?** A personal agent belongs to your active VS Code user profile, not to the repo. That can be the profile on your Windows laptop/DevBox or in your codespace. Workspace agents (Part A) are shared with your team through git.
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
| Backend developer | [app/api/src/routes/order.ts](../app/api/src/routes/order.ts) |
| Frontend developer | [app/frontend/src/components/Login.tsx](../app/frontend/src/components/Login.tsx) or [Navigation.tsx](../app/frontend/src/components/Navigation.tsx) |
| Data engineer | any file in [app/api/database/migrations/](../app/api/database/migrations/) |
| Platform / DevOps | [app/infra/container-apps.bicep](../app/infra/container-apps.bicep), [app/api/Dockerfile](../app/api/Dockerfile) or [docker-compose.yml](../app/docker-compose.yml) |

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

---

## For your own use case

Bringing your own repository? Build the same two kinds of agent for your work: a **repository agent** that knows your team's conventions, and a **personal agent** that reflects your own review priorities.

**Before you start:**
- Open your repository in VS Code and check that Copilot responds.
- Save your existing work and use a working branch where you are allowed to experiment. Do not overwrite existing agents or discard unrelated changes.
- Pick a small, realistic task to discuss, such as adding validation, changing a report or extending an automation script. This exercise compares plans; you will not implement the feature.
- Choose unused agent names. The examples below use `project-specialist` and `my-reviewer`; change the names and paths if they already exist.
- Replace every `<placeholder>` with your own information. Keep the same model when comparing answers.

### Part A: A repository agent for your team

**A1. Choose its job.** Think of a responsibility that should be handled consistently by anyone working in this repository. Keep the role focused, rather than creating an agent that does everything.

| Your area | Example repository agent | Knowledge it should use |
|---|---|---|
| Application or service | Service Specialist | Existing modules, error handling, interfaces and tests |
| Data | Pipeline Specialist | Data contracts, transformations, validation and retry conventions |
| Frontend | UI Specialist | Shared components, accessibility and interaction patterns |
| Platform or automation | Automation Specialist | Existing scripts, configuration, safe defaults and verification steps |

**A2. Get a baseline plan.** Start a new chat, choose the built-in **Agent**, and send:
```
I want to add <SMALL FEATURE> so that <USER> can <BENEFIT>.
Reply with a plan only: the relevant files, existing patterns to reuse, important risks and how to verify the change.
Don't implement anything, edit files or run commands.
```
Keep the answer so you can compare it later.

**A3. Create the workspace agent.** Use **Chat: New Custom Agent** in the Command Palette, choose **Workspace**, and name it `project-specialist`. The file belongs in `.github/agents/project-specialist.agent.md` in your repository.

In a new chat with the built-in **Agent**, attach any existing team guidance or relevant code examples, then send:
```
Fill in .github/agents/project-specialist.agent.md as a custom agent for this repository.
- Header: name "<TEAM ROLE>", a description saying when to use it, and tools: ['read', 'search', 'edit'].
- Role: help the team with <FOCUSED RESPONSIBILITY>.
- Read the existing project documentation and relevant code to identify real conventions.
- Include a short workflow: understand the request, inspect related code, reuse patterns, make scoped changes when requested, and propose relevant verification.
- Include these team requirements: <TWO OR THREE REAL RULES>.
- Use only facts supported by this repository or the guidance I attached. Flag missing information instead of inventing rules.
- Never deploy, commit or push. No terminal access is needed for this exercise.
Max 50 lines. Only edit this agent file, not application code.
```

Review the file before choosing **Keep**. Are its rules useful to your team? Are the paths and conventions real? Change one rule yourself to make the role more precise. The tool list allows file changes when requested but leaves out terminal execution; instructions and tool permissions are different controls.

**A4. Compare with the baseline.** Start a new chat, select your agent by its display name, and send the **exact A2 prompt** again using the same model.

Compare the two plans:
- Does the specialist identify the right files and reuse your existing patterns?
- Does it apply the team requirements you gave it?
- Are its verification steps appropriate for your project?
- Does it ask about missing requirements rather than guessing?
- Did it respect the request to plan without editing files?

There is no OctoCAT answer key for your repository. Judge the result against your baseline, your team's standards and the actual code. Adjust the agent and repeat if its guidance is too vague.

**Done when:** your repository agent appears in the picker and gives a focused plan grounded in your project. Its configuration lives in the repository so it can be reviewed and shared through your team's normal process.

### Part B: A personal agent for your own work

**B1. Choose your review priorities.** Think of something you repeatedly check or explain in your role. For this exercise, make a **read-only reviewer** with two concrete checks, such as clear error messages, accessible controls, data validation or reliable retry behaviour.

Choose checks that matter to you across projects. Keep repository-specific rules in the team agent rather than hard-coding one project's paths into your personal agent.

**B2. Create it in your user profile.** Use **Chat: New Custom Agent**, choose **User**, and name it `my-reviewer`. This keeps it in the active VS Code user profile on your laptop, DevBox or codespace, not in the repository. If **User** is unavailable, you can practise with **Workspace**, but that fallback is repository-scoped, not personal.

Replace the template with this, then fill in your role, area and checks:

```markdown
---
name: my-reviewer
description: "Use when I ask for a review in my area. Read-only advice; never edits files or runs commands."
tools: ['read', 'search']
---
I am a <ROLE> and I mostly work on <AREA>. You are my personal reviewer.

When I ask for a review:
1. Read the file or selection I provide and related files needed to understand it.
2. Focus on <CHECK 1>, <CHECK 2> and obvious correctness problems.
3. Use the current repository's conventions. State uncertainties instead of inventing requirements.
4. Report at most 5 findings with the file, line, problem and suggested fix. Do not invent findings to fill the list.
5. Never edit files or run commands. If I ask for a fix, explain the proposed change instead.
```

**B3. Try it on your code.** Open a file you understand, start a new chat, select **my-reviewer**, attach the file or selection, and send:
```
Review this code using my two checks.
Explain the most important findings and point to the evidence.
Don't edit anything or run commands.
```
Check the findings yourself. Are they specific, correct and useful? Refine one of your checks if the feedback is too generic.

**B4. Test the boundary.** In the same chat, send:
```
Fix the first finding.
```
If there were no findings, ask it to rewrite the selected code for readability instead. It should **explain a proposed change without applying it**, because it only has read and search tools. Check the tool calls and **Source Control**: no review-related file edits or terminal commands should have occurred.

**Done when:** your personal reviewer is available in the picker, applies your priorities and stays read-only even when asked to make a change.

**Reflect:** which rules belong to the team and should travel with the repository? Which priorities belong to you and should travel with your user profile? Keep the two scopes distinct. Do not implement the planned feature, commit or push as part of this exercise.
