# F3 · Context Engineering in Practice: lab

[Labs index](README.md) · [Previous](F2-build-your-personal-agent.md) · [Next](F4-mcp-and-spec-driven.md)

Two parts:
- **Part A (⏱ 25 min):** teach Copilot your team's rules with `.github/copilot-instructions.md`.
- **Part B (⏱ 27 min):** repair a **skill**, then let it build a whole feature.

**Missing on your branch:**
1. `.github/copilot-instructions.md` was deleted.
2. [.github/skills/api-endpoint/SKILL.md](../.github/skills/api-endpoint/SKILL.md) still has its body (the **how**) but lost its header (the **when**).

---

## Part A: Teach Copilot your team's rules (⏱ 25 min)
*The steps add up to 22 minutes; the other 3 minutes are buffer.*

> **The story.** Every PGGM team has internal standards that no AI model has ever seen. OctoCAT has one too: **TAO**, a made-up internal library for logging and metrics.
> - It exists only as a document: [docs/tao.md](../docs/tao.md). Its examples use the package name `@tao/core` and functions such as `initTAO`, `observe()`, decorators like `@Measure`, and `MetricRegistry`.
> - It is **not** a real npm package. That's why in this lab we only *ask* about TAO in **Ask** mode: no code gets written, so nothing breaks.

### A1. Before (⏱ 3 min)
Start a new chat, click on **+**, click on **Agent...**, choose **Ask**, and send:
```
How should I add logging and metrics to the supplier route? 5 bullets, no code.
```
Note what it suggests. Probably it suggests well-known packages such as winston, pino, prom-client or OpenTelemetry.

> **Did it already mention TAO?** Then Copilot found [docs/tao.md](../docs/tao.md) by searching, which was lucky. Carry on: the file you're about to write makes it happen *every time, for everyone*.

### A2. Draft the instructions file (⏱ 7 min)
Start a new chat (opens Agent mode by default) and send:
```
Create .github/copilot-instructions.md for this repository: what the app is, how to install, run and test it
(check the Makefile), the architecture (link to docs/architecture.md) and the main code conventions.
Max 40 lines. Don't change any other file.
Describe the app only; ignore the labs/ folder.
```
1. **Keep** the change, then open the file.
2. Check that the commands it lists (for example `make install`, `make dev`, `make test`) really exist in the [Makefile](../Makefile). Tests keep watching for changes; press **q** to quit.
3. Fix anything that's made up, because wrong instructions mislead every future chat.

> In the *Local* session type, `/init` does the same job. Typing the request works everywhere.

### A3. Add the team rule (⏱ 3 min)
<!-- 1. Open [demo/walkthroughs/copilot/copilot-in-ide.md](../demo/walkthroughs/copilot/copilot-in-ide.md).
2. Find **"Demo: Custom Instructions and Repository Configuration"** → **Option 2: Update the custom instructions file by hand, adding these additional guidelines**.
3. Copy the **"Additional Guidelines for REST APIs"** block. -->
1. Paste this at the **end** of `.github/copilot-instructions.md` and save.

```markdown
## Team rules for the API
- Observability: use our internal TAO library for all logging, metrics and tracing. See [TAO guide](../docs/tao.md).
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

### B5. Use it (⏱ 10 min)
Start a new chat (opens Agent mode by default) and send:
```
Add a new API endpoint for a new Entity called 'DeliveryVehicle', with full CRUD endpoints. Each vehicle belongs to a branch.
```
Watch for the skill being loaded: it shows up in the references or tool calls of the response. Then tick off the skill's own checklist:
- [ ] model in [api/src/models/](../api/src/models/)
- [ ] repository in [api/src/repositories/](../api/src/repositories/)
- [ ] route in [api/src/routes/](../api/src/routes/), registered in [api/src/index.ts](../api/src/index.ts)
- [ ] migration and seed data. *Where did they land?*
- [ ] unit tests

**Verify:**
1. Run `make test-api`. It keeps watching for changes; press **q** to quit.
2. Run `make dev-api` → **Ports** → **3000** → `/api-docs` and look for the DeliveryVehicle endpoints.

| Role | Your focus |
|---|---|
| Backend developer | Does the route follow the existing pattern? Do the tests pass? |
| Frontend developer | Are the Swagger docs and model types complete? |
| Data engineer | Compare the migration and seed with [references/database-conventions.md](../.github/skills/api-endpoint/references/database-conventions.md). Note that the skill text says `api/sql/…` while this repo uses [api/database/…](../api/database/). Did the agent notice? |
| Platform / DevOps | Does `make test-api` pass? It keeps watching for changes; press **q** to quit. |

### B6. Compare (⏱ 2 min)
```bash
git diff origin/main -- .github/skills/api-endpoint/SKILL.md
```
Only the header differs. Compare your description with OctoCAT's.

**Catch up:** `git checkout origin/main -- .github/skills/api-endpoint/SKILL.md`

✅ `api-endpoint` is listed, and the DeliveryVehicle endpoints show in Swagger.

---

## Extensions (if you finish early)
- **Skills need maintenance too.** Fix the `api/sql/…` paths in the skill so that they say [api/database/…](../api/database/).
- **Write a tiny skill of your own.** Choose **Agent** and send: `Create a skill in .github/skills/tao-metrics/ that explains how to add a TAO metric to a route, based on docs/tao.md. SKILL.md max 40 lines, with a name and description header.` (In the *Local* session type, `/create-skill` helps.)

## Think about this for the wrap-up
Where does each of these belong: **instructions**, a **skill** or an **agent**?
- "We use TAO for logging"
- "How we add an endpoint"
- "A reviewer that can't edit"
