# F3 · Context Engineering in Practice: lab

[Labs index](README.md) · [Previous](F2-build-your-personal-agent.md) · [Next](F4-mcp-and-spec-driven.md)

## What will you do?

Teach Copilot how this team works: restore repository instructions for the fictional TAO standard, repair the API skill's discovery header, then use that skill to add branch-owned delivery vehicles, including API operations and tests.

**Why it matters:** knowing a framework isn't the same as knowing your team's standards. Context engineering puts the right project knowledge in reach instead of repeating it in every prompt. [Microsoft Learn: why customize Copilot?](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/2-examine-benefits-customize-github-copilot)

**Your story:** a branch manager asks you to track delivery vehicles in OctoCAT. Your team already has TAO logging guidance and an API checklist. You want Copilot to find and follow them, not invent its own approach.

Two parts:
- **Part A (⏱ 25 min):** teach Copilot your team's rules with `.github/copilot-instructions.md`.
- **Part B (⏱ 27 min):** repair a **skill**, then let it build a whole feature.

**Missing on your branch:**
1. `.github/copilot-instructions.md` was deleted.
2. [.github/skills/api-endpoint/SKILL.md](../.github/skills/api-endpoint/SKILL.md) still has its body (the **how**) but lost its header (the **when**).

## New words

| Term | What it means in this lab |
|---|---|
| **[Repository instructions](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/3-customize-copilot-responses-instruction-files)** | Standing team rules for chats in this repo; for example, which logging convention to follow. |
| **[Skill](https://code.visualstudio.com/docs/agent-customization/agent-skills)** | A reusable task workflow with instructions and supporting files, loaded when relevant. Here: how to add an API entity correctly. |
| **Frontmatter** | The `---` header with `name` and `description`: metadata for discovering when a skill applies. Its body explains how. |
| **[Custom agent](https://learn.microsoft.com/en-us/training/modules/configure-customize-github-copilot-visual-studio-code/4-create-custom-github-copilot-agents#what-are-custom-github-copilot-agents)** | A role and tool permissions: who does the work, rather than the workflow they can load. |
| **CRUD** | Create, read, update and delete: the operations you'll add for delivery vehicles. |
| **[TAO](../app/docs/tao.md)** | A fictional logging/metrics standard for the before/after comparison, not a package to install. |

**The key distinction:** instructions set rules, a skill packages a repeatable process, and an agent supplies the role and tools.

Links are optional official references; follow the exercises below for the workshop's UI.

## Before you start

Complete the [setup checks](01-setup.md) and stay on `workshop`. Use fresh chats where requested so you compare instructions, not old conversation history.

Run terminal commands from the **repo root**. Use the Codespaces commands or the Windows PowerShell alternatives for your laptop or DevBox.

---

## Part A: Teach Copilot your team's rules (⏱ 25 min)
*The steps add up to 22 minutes; the other 3 minutes are buffer.*

> **The story.** Every PGGM team has internal standards that no AI model has ever seen. OctoCAT has one too: **TAO**, a made-up internal library for logging and metrics.
> - It exists only as a document: [app/docs/tao.md](../app/docs/tao.md). Its examples use the package name `@tao/core` and functions such as `initTAO`, `observe()`, decorators like `@Measure`, and `MetricRegistry`.
> - It is **not** a real npm package. That's why in this lab we only *ask* about TAO in **Ask** mode: no code gets written, so nothing breaks.

### A1. Before (⏱ 3 min)
Start a new chat, click on **+**, click on **Agent...**, choose **Ask**, and send:
```
How should I add logging and metrics to the supplier route? 5 bullets, no code.
```
Note what it suggests. Probably it suggests well-known packages such as winston, pino, prom-client or OpenTelemetry.

> **Did it already mention TAO?** Then Copilot found [app/docs/tao.md](../app/docs/tao.md) by searching, which was lucky. Carry on: the file you're about to write makes it happen *every time, for everyone*.

### A2. Draft the instructions file (⏱ 7 min)
Start a new chat (opens Agent mode by default) and send:
```
Create .github/copilot-instructions.md for this repository: what the app is, how to install, run and test it,
the architecture (link to app/docs/architecture.md) and the main code conventions.
Check the app/Makefile, app/api/package.json and app/frontend/package.json. Include Codespaces make commands and
Windows PowerShell npm.cmd equivalents, all run from the repo root. Use API-only test commands.
Max 40 lines. Don't change any other file.
Describe the app only; ignore the labs/ folder.
```
1. **Keep** the change, then open the file.
2. Check its commands against the [app/Makefile](../app/Makefile), [API scripts](../app/api/package.json), [frontend scripts](../app/frontend/package.json) and [setup command table](01-setup.md#local-windows-equivalents-for-lab-commands). It should cover both environments. For API tests, use **Codespaces:** `make -C app test-api`; **Windows PowerShell:** `npm.cmd --prefix .\app\api test`. Tests keep watching for changes; press **q** to quit.
3. Fix anything that's made up, because wrong instructions mislead every future chat.

> In the *Local* session type, `/init` does the same job. Typing the request works everywhere.

### A3. Add the team rule (⏱ 3 min)
<!-- 1. Open [app/demo/walkthroughs/copilot/copilot-in-ide.md](../app/demo/walkthroughs/copilot/copilot-in-ide.md).
2. Find **"Demo: Custom Instructions and Repository Configuration"** → **Option 2: Update the custom instructions file by hand, adding these additional guidelines**.
3. Copy the **"Additional Guidelines for REST APIs"** block. -->
1. Paste this at the **end** of `.github/copilot-instructions.md` and save.

```markdown
## Team rules for the API
- Observability: use our internal TAO library for all logging, metrics and tracing. See [TAO guide](../app/docs/tao.md).
- TAO is provided by our platform: never install it or add it to package.json.
- Don't introduce other logging or metrics libraries.
```


### A4. After (⏱ 5 min)
1. Start a new chat, click on **+**, click on **Agent...**, choose **Ask**, and send the **A1 prompt** again.
   - You should now see TAO: `initTAO`, `observe()`, `MetricRegistry` or the decorators.
2. Above the answer, expand the **References** list (for example "Used 3 references"). `copilot-instructions.md` should be in it.

**Why a new chat?** So that the earlier answer can't influence this one.

### A5. Compare (⏱ 4 min)
Open `.github/copilot-instructions.md` on branch **`main`** on GitHub, or run:
```bash
git add .github/copilot-instructions.md
git diff origin/main -- .github/copilot-instructions.md
```
- OctoCAT's original (50 lines) has review guidance, an escalation order and tone rules. What did it include that you didn't?
- The original has **no** TAO block, because we add it live in our demo. TAO lines that appear only in your version are expected.

**Catch up:** `git checkout origin/main -- .github/copilot-instructions.md`, then repeat A3.

✅ The "after" answer uses TAO, and **References** lists your instructions file.

> **Good to know:** instructions files steer chat and agents. They don't steer the grey-text code completions.

---

## Part B: Repair the skill (⏱ 27 min)
*The steps add up to 25 minutes; the other 2 minutes are buffer. Step B5 is the longest because the agent creates several files.*

> **A skill is a folder with a [SKILL.md](../.github/skills/api-endpoint/SKILL.md) file.**
> - The **header** (`name` and `description`) tells Copilot **when** to load it.
> - The **body** tells it **how** to do the task.
> - Extra material lives in the folder (here: [references/](../.github/skills/api-endpoint/references/)).
>
> On your branch, `api-endpoint` has 470-odd lines of "how", but no "when". Copilot never knows to use it.

### B1. Spot the gap (⏱ 2 min)
1. In the Chat view, click on **+**, click on **Agent...**, choose **Configure Custom Agents**, and open **Skills**, or type `/` in the chat box.
   - `frontend-testing` is listed; `api-endpoint` isn't.
2. Open [.github/skills/api-endpoint/SKILL.md](../.github/skills/api-endpoint/SKILL.md). It starts with `# API Endpoint Development`, with no `---` header.

### B2. Read the "how" (⏱ 5 min)
Skim these sections:
- **When to Use This Skill**
- **Workflow**: Steps 1–7 (model, repository, route, register the route, migration, seed data, unit tests)
- **Quick Reference: Complete Checklist**

Then peek into [references/](../.github/skills/api-endpoint/references/): [database conventions](../.github/skills/api-endpoint/references/database-conventions.md), [error handling](../.github/skills/api-endpoint/references/error-handling.md), [Swagger patterns](../.github/skills/api-endpoint/references/swagger-patterns.md) and [testing patterns](../.github/skills/api-endpoint/references/testing-patterns.md).

> **Use this repo's test commands:** the testing reference contains `--workspace=api` examples, but this repo has no root npm workspace. Use the API commands in B5 or the [setup command table](01-setup.md#local-windows-equivalents-for-lab-commands) instead. To filter tests by name, Vitest uses `-t` / `--testNamePattern`, not the reference's `--grep`. Check the agent's commands too; a setup error is not a failing feature test.

### B3. Write the "when" (⏱ 5 min)
Add this at the **very top** of [SKILL.md](../.github/skills/api-endpoint/SKILL.md), then write your description:
```markdown
---
name: api-endpoint
description: <What this skill does and when to use it, in 1-3 sentences>
---
```
- **`name`** must match the folder name exactly (lowercase, hyphens). Otherwise the skill is silently ignored.
- **`description`** is what Copilot matches your request against. Say what the skill does *and* when to use it, for example new entities, CRUD endpoints, routes, repositories and Swagger docs.
- For style, look at the header of [.github/skills/frontend-testing/SKILL.md](../.github/skills/frontend-testing/SKILL.md).

> Stuck? Go to **Ask** mode, attach [SKILL.md](../.github/skills/api-endpoint/SKILL.md) and send: `Suggest a 2-sentence description for this skill's header: what it does and when to use it.`

### B4. Check (⏱ 1 min)
`api-endpoint` now appears in the Skills list and in the `/` menu.

**Required before B5:** in [SKILL.md](../.github/skills/api-endpoint/SKILL.md) and [database-conventions.md](../.github/skills/api-endpoint/references/database-conventions.md), replace every `app/api/sql/` path with `app/api/database/`. Migrations belong in [app/api/database/migrations/](../app/api/database/migrations/) and seed data in [app/api/database/seed/](../app/api/database/seed/). Correct the guidance only; do not move existing database files.

### B5. Use it (⏱ 10 min)
Start a new chat (opens Agent mode by default) and send:
```
Add a new API endpoint for a new Entity called 'DeliveryVehicle', with full CRUD endpoints. Each vehicle belongs to a branch.
```
Watch for the skill being loaded: it shows up in the references or tool calls of the response. Then tick off the skill's own checklist:
- [ ] model in [app/api/src/models/](../app/api/src/models/)
- [ ] repository in [app/api/src/repositories/](../app/api/src/repositories/)
- [ ] route in [app/api/src/routes/](../app/api/src/routes/), registered in [app/api/src/index.ts](../app/api/src/index.ts)
- [ ] migration in [app/api/database/migrations/](../app/api/database/migrations/) and seed data in [app/api/database/seed/](../app/api/database/seed/)
- [ ] unit tests

**Verify:**
1. Run the API tests from the repo root using your environment's command. Tests keep watching for changes; press **q** to quit before starting the API.
2. Start the API using the matching command:

   | Task | Codespaces | Windows PowerShell |
   |---|---|---|
   | API tests | `make -C app test-api` | `npm.cmd --prefix .\app\api test` |
   | Start API | `make -C app dev-api` | `npm.cmd --prefix .\app\api run dev` |

3. Open Swagger and look for the DeliveryVehicle endpoints:
   - **Codespaces:** open **Ports** → **3000** → add `/api-docs` to the forwarded URL.
   - **Windows:** open http://localhost:3000/api-docs on the laptop or DevBox running the API.
4. Try an endpoint with **Try it out**. **Codespaces only:** if Swagger targets `localhost`, use the same endpoint path on the forwarded port URL instead. Stop the API with **Ctrl+C** when finished.

For automated agent test runs, use the [single-run commands](01-setup.md#local-windows-equivalents-for-lab-commands) rather than leaving a watcher running.

| Role | Your focus |
|---|---|
| Backend developer | Does the route follow the existing pattern? Do the tests pass? |
| Frontend developer | Are the Swagger docs and model types complete? |
| Data engineer | Check that the migration and seed are under [app/api/database/](../app/api/database/) and follow the corrected [database conventions](../.github/skills/api-endpoint/references/database-conventions.md). |
| Platform / DevOps | Do the API tests pass using your environment's command above? If watching for changes, press **q** to quit. |

### B6. Compare (⏱ 2 min)
```bash
git diff origin/main -- .github/skills/api-endpoint/SKILL.md
```
Compare your header with OctoCAT's and confirm that the database paths match the reference's `app/api/database/` paths. Your description can differ; check any other differences against the exercise requirements.

**Catch up:** `git checkout origin/main -- .github/skills/api-endpoint/SKILL.md`, then repeat the required database-path correction above before using the skill again.

✅ `api-endpoint` is listed, and the DeliveryVehicle endpoints show in Swagger.

---

## Extensions (if you finish early)
- **Write a tiny skill of your own.** Choose **Agent** and send: `Create a skill in .github/skills/tao-metrics/ that explains how to add a TAO metric to a route, based on app/docs/tao.md. SKILL.md max 40 lines, with a name and description header.` (In the *Local* session type, `/create-skill` helps.)

## Think about this for the wrap-up
Where does each of these belong: **instructions**, a **skill** or an **agent**?
- "We use TAO for logging"
- "How we add an endpoint"
- "A reviewer that can't edit"

---

## For your own use case

Bringing your own repository? Create two pieces of reusable context for your team:
- **Repository instructions:** the project facts and rules Copilot should consider across tasks.
- **A workflow skill:** the steps Copilot should load when a particular kind of task comes up.

**Before you start:**
- Open your repository in VS Code, save your work and use a working branch where you can experiment.
- Read any existing Copilot instructions, other agent guidance and skills. Preserve existing rules; do not overwrite or create conflicting guidance. If your team already maintains an instructions file, agree a small improvement to it instead of replacing it.
- Choose a real team convention and a small, repeatable workflow. Use your own project's standards, paths and commands, not OctoCAT's fictional TAO rule.
- Replace every `<placeholder>` below. Keep the same model for before/after comparisons.

### Part A: Create your repository's Copilot instructions

**A1. Get a baseline answer.** Pick a small task where a team convention matters, such as handling an error, adding a data transformation or changing a shared component. Start a new chat, choose **Ask**, and send:
```
How should I <SMALL TASK> in <AREA OF THIS REPOSITORY>?
Point to the relevant files and suggest an approach in at most 5 bullets.
Don't write code, edit files or run commands.
```
Keep the answer. Does it follow your team's conventions, or offer generic advice?

**A2. Draft the instructions.** In a new chat, choose **Agent**, attach any existing team guidance, and send:
```
Create .github/copilot-instructions.md for this repository.
If it already exists, preserve its useful rules and make only the additions needed below.
Inspect the existing documentation, project configuration and representative code.
Include:
- A short project overview and the main folders or architectural boundaries.
- Verified install, build and test commands, including the directory and shell they require.
- Important project-specific conventions, with links to existing documentation or examples.
- How to verify a change and when to ask for clarification.
Use only facts you can verify or that I supplied. Flag missing information instead of inventing rules.
Keep it concise, around 40 lines; link to detailed documentation rather than copying it.
Only edit .github/copilot-instructions.md. Don't run commands or change application code.
```

Review the file before choosing **Keep**. Check a few paths and commands yourself. Remove guesses, irrelevant boilerplate and duplicated documentation. These instructions should describe what is true for your repository, not a generic project template.

**A3. Add one real team rule.** Write a short, actionable rule in the instructions file. For example:
- Use an existing shared error handler rather than creating another one.
- Reuse a particular component or validation helper.
- Put new schema changes in new migration files rather than editing old migrations.

Name the actual helper, pattern or location in your project and link to an existing example. Only include a rule that your team really follows and that applies broadly; detailed steps for one workflow belong in the skill below.

**A4. Compare the behaviour.** Start a **new chat**, choose **Ask**, and send the **exact A1 prompt** again without repeating the new rule.

Check:
- Does the answer now use your team's convention and point to real code?
- Is `.github/copilot-instructions.md` shown in the response's references or context, where the UI exposes them?
- Did it still respect the request not to edit or run anything?

If the rule was missed, check the file's location and wording and try a fresh chat. Treat the response as evidence to inspect, not a guarantee that instructions are always followed. Make one small wording improvement if needed.

**Done when:** your repository instructions are concise, grounded in your project and demonstrably useful for the chosen task.

### Part B: Create a new skill for a repeatable workflow

**B1. Choose a workflow, not another role.** Think of a task with several repeatable steps that someone on your team might otherwise explain each time.

| Your area | Example workflow | Possible output |
|---|---|---|
| Application or service | Add input validation using existing patterns | A scoped change, tests and a verification summary |
| Data | Check a proposed transformation against a data contract | Validation findings, edge cases and test cases |
| Frontend | Review a component change for accessibility | Evidence-based findings and a manual-check checklist |
| Platform or automation | Assess a configuration change before rollout | Impact, risks, checks and rollback considerations |

Write down:
1. **When to use it:** the kinds of requests that should trigger the workflow.
2. **Inputs:** the files, requirements or examples it needs.
3. **Steps:** three to six actions in the order you actually follow.
4. **Output and completion checks:** what it produces and how to tell it is correct.
5. **Boundaries:** what it must not change or run, and when it should stop to ask for missing information.

Keep the first version small. Choose a safe example you can inspect yourself; do not start with deployment, production data or a workflow requiring new credentials.

**B2. Create the skill file.** Choose an unused lowercase, hyphenated name, such as `change-checklist`. In a new chat, choose **Agent**, attach your workflow notes and relevant examples, and send:
```
Create .github/skills/<skill-name>/SKILL.md for <WORKFLOW> in this repository.
Use my attached workflow notes and the repository's existing conventions.
- Add YAML frontmatter with name matching the folder and a description explaining what the skill does and when to use it.
- Include concrete trigger phrases, required inputs, ordered steps, the expected output and completion checks.
- State when to ask for missing information and what actions are outside scope.
- Use actual project paths and existing verification commands where relevant.
- Link to existing guidance instead of copying long documents.
Keep the skill under 80 lines. Only create this SKILL.md; don't implement the workflow yet.
Don't create scripts, install tools, change application code, run commands, commit or push.
```

Review before choosing **Keep**. The first lines should have this shape, with the name and description adapted to your chosen workflow:

```yaml
---
name: change-checklist
description: "Create a scoped change and verification checklist. Use when planning a small code change or assessing its impact in this repository."
---
```

Check that:
- The file is called `SKILL.md` and sits in `.github/skills/<skill-name>/`.
- The frontmatter starts at the top, with `---` on both sides, and `name` matches the folder.
- The description contains the words someone would naturally use to request this task.
- The body explains the workflow, not just the role of a helpful assistant.
- Its paths, commands and output checks are real and consistent with your repository instructions.

**B3. Check discovery.** Open the Skills list using the same controls as the main lab, or type `/` in Chat and look for the new skill. If it is missing, check the file location and frontmatter before continuing.

**B4. Try the workflow on your own example.** Start a new chat in **Agent** mode and make a natural request matching your description. Do not attach the skill at first, so you can check whether Copilot discovers it:
```
I need to <WORKFLOW TASK> for <SMALL EXAMPLE>.
Use <RELEVANT FILES OR REQUIREMENTS> as the input.
For this first trial, walk through the workflow and return <EXPECTED OUTPUT>.
Don't edit files, run commands, deploy, commit or push.
```

Look for the skill being loaded in references or tool calls. If it is not selected, refine the trigger description and retry in a fresh chat. You can also explicitly select `/<skill-name>` if available, or attach `SKILL.md` and ask Copilot to follow it; that tests the instructions, but not automatic discovery.

Compare the result with your workflow notes. Did it follow the steps, use the right inputs, ask about missing information and produce the expected output? Refine one weak step and repeat.

**B5. Verify the result.** A convincing response is not enough: inspect the cited files and check the output against your completion criteria. This first walkthrough does not prove that a code-changing or command-running workflow executes correctly. If you later test execution, agree a small scope on your working branch, review each change and use your project's existing checks.

**Done when:** the skill is discoverable, its workflow can be followed on a bounded example, and you have checked the result rather than just accepting its summary.

**Reflect:** which guidance belongs in the short, project-wide instructions file, and which belongs in the on-demand skill? You now have both in your own repository; no application implementation, deployment, commit or push is required for this exercise.
