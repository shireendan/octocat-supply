# F4 · First Steps in Agentic Development: lab

[Labs index](README.md) · [Previous: F3](F3-context-engineering.md) · Next: none

⏱ **27 minutes** · Part 1, MCP setup: 8 · Part 2, spec-driven development: 19

## What will you do?

**The story:** a branch manager wants to see all orders for their branch. OctoCAT's API can list *all* orders, or one order by ID, but not the orders for one branch. You'll:
1. connect Copilot to **Microsoft Learn** with MCP, and pull in REST API guidance;
2. let OctoCAT's **tdd-planner** agent write the **spec**: acceptance criteria, plus a test for each;
3. review that spec against the Learn guidance, and **decide yourself** what changes;
4. let **tdd-red** write failing tests, then **tdd-green** write just enough code to pass them;
5. **verify** by running the tests yourself.

**Why it matters:** agents can execute a plan; you still own its decisions and the evidence that it works.

**Your story:** you're shipping the branch manager's request. You need to choose sensible edge-case behaviour and prove the delivered endpoint matches those decisions, not just accept an agent's "done" message.

**Missing on your branch:** nothing. You *add* a knowledge source and a feature.
**Done when:** the tests pass, every acceptance criterion in your plan has a test, and the new endpoint shows up in Swagger.

> **Why spec and tests first?** In F1 you saw that Copilot is probabilistic. A spec you own says *what* to build; tests turn it into a finish line the agent can check itself. You get proof instead of a feeling.

## New words

| Term | What it means in this lab |
|---|---|
| **[MCP](https://learn.microsoft.com/en-us/training/support/mcp)** | Model Context Protocol: a standard way for an agent to use external tools and data. Here: Microsoft Learn documentation. |
| **[Tool call / approval](https://learn.microsoft.com/en-us/training/support/mcp-get-started)** | A requested action, such as a documentation search. You decide whether to allow it. |
| **Specification / spec** | Written behaviour and constraints, agreed before implementation. |
| **Acceptance criteria** | Observable conditions defining success; in this lab, each must have a test. |
| **[TDD: red / green](https://learn.microsoft.com/en-us/visualstudio/test/quick-start-test-driven-development-with-test-explorer?view=visualstudio)** | Test-driven development: write a failing test, then just enough code to make it pass. |
| **[Pagination](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)** | Return results in pages instead of one unbounded list; a design choice to discuss before building. |

Links are optional background; some examples use other languages or IDEs. Follow this lab's steps.

## Before you start

Complete the [setup checks](01-setup.md), then follow the cleanup and chat instructions below.

> **Before you start:** if `.github/copilot-instructions.md` still has the TAO team rule from F3, delete those lines. TAO is made up, so code that uses it can't run, and in this lab the agents write real code.

> **Do the whole lab in one chat.** Each step builds on what the previous one found.

---

## Part 1: MCP setup (⏱ 8 min)

### 1.1 Add the Microsoft Learn MCP server
1. Open the Command Palette (**Ctrl+Shift+P**) → **MCP: Add Server…** → **HTTP**.
2. Paste `https://learn.microsoft.com/api/mcp` → name it `microsoft-learn` → **Workspace**.
3. VS Code adds it to [.vscode/mcp.json](../.vscode/mcp.json). Start **only this server**. Leave OctoCAT's other entries alone: you don't need them for this lab.

> **Prefer editing the file?** Inside `"servers": { … }` in [.vscode/mcp.json](../.vscode/mcp.json), add the entry below (mind the comma between entries), save, then click **Start** above it.
> ```json
> "microsoft-learn": {
>   "type": "http",
>   "url": "https://learn.microsoft.com/api/mcp"
> }
> ```

### 1.2 Check it
click on **+**, click on **Agent...**, choose **Configure Custom Agents**, and open **Tools**. Find `microsoft_docs_search` and `microsoft_docs_fetch` in the list.

### 1.3 Use it
Start a new chat (opens Agent mode by default) and send:
```
Using Microsoft Learn, summarise REST API guidance on status codes and pagination, with links.
Max 6 bullets. Don't edit any files.
```
1. When Copilot asks to run the tool, choose **Allow**. Notice the options: allow **once**, for this **session**, or **always**. Which would you pick at PGGM, and for which servers?
2. Expand the tool call to see what it searched for and fetched.

**Why this matters:** the model's training stops at a cut-off date. MCP lets it read *today's* documentation, and your team's other systems too: GitHub, Azure DevOps, Jira and more.

> **MCP blocked by policy?** Send this instead: `#fetch https://learn.microsoft.com/azure/architecture/best-practices/api-design Summarise the guidance on status codes and pagination in 6 bullets.`

✅ The answer cites Learn pages, and you can see the tool call. **Keep this chat open.**

---

## Part 2: Spec-driven development (⏱ 19 min)

OctoCAT comes with three custom agents that work as a team:

| Agent | Does | Never does |
|---|---|---|
| [**tdd-planner**](../.github/agents/tdd-planner.agent.md) | Turns your request into a plan: acceptance criteria, plus a test for each (Given / When / Then) | Write code or tests |
| [**tdd-red**](../.github/agents/tdd-red.agent.md) | Writes the tests from the plan and runs them. They must **fail**, because the feature doesn't exist yet | Write the feature |
| [**tdd-green**](../.github/agents/tdd-green.agent.md) | Writes the **smallest** code that makes the tests pass, and runs them | Add anything the tests don't ask for |

### 2.1 Spec (⏱ 3 min)
In the same chat, choose **tdd-planner** in the agent picker and send:
```
Add an endpoint that returns all orders for a given branch ID.
```
1. The planner researches the code first, so give it a minute.
2. Look for a plan in `docs/tdd-plans/`; if it wasn't saved, use the fallback below. Open the saved file and read the **Acceptance Criteria** and **Test Specifications**.

> **No file in `docs/tdd-plans/`?** Choose **Agent** and send: `Save that plan as docs/tdd-plans/orders-by-branch.md. Don't change anything else.`

### 2.2 Review against Learn (⏱ 3 min)
In the same chat, choose **Agent** and send:
```
Review that plan against the Learn guidance. Suggest at most three changes to the acceptance criteria. Don't write code.
```
Typical suggestions: what to return for a branch that doesn't exist (404, or 200 with an empty list?), what to do with an invalid ID (400), and whether to add pagination.

### 2.3 Human check (⏱ 3 min)
Open the plan file and **edit it by hand**: accept or reject each suggestion. Change the acceptance criteria and their tests together, and save.

**This is the core lesson of spec-driven development:** you change the *spec*, not the code. The agents build whatever the spec says, so the spec is where your judgement goes.

### 2.4 Red: tests first (⏱ 4 min)
In the same chat, choose **tdd-red** and send (replace `<plan-file>.md` with your saved plan's filename):
```
Implement the failing tests for the plan in docs/tdd-plans/<plan-file>.md. Use the file, because I edited it. Only create test files, then run them.
```
1. Review the new test file (for example `api/src/routes/order.test.ts`) and choose **Keep**.
2. The tests should **fail because the feature doesn't exist yet**, for example with a 404. A typo in the test is the wrong reason: tell tdd-red.

> **About the buttons:** the agents may show buttons such as **Write Red Tests** and **Write Green Implementation**. They switch agents and send the prompt for you, but only in the *Local* session type, and they send the plan from the *chat*, not your edited file. That's why we use the agent picker and name the file.

### 2.5 Green: make them pass (⏱ 4 min)
In the same chat, choose **tdd-green** and send, using the same filename:
```
Implement the code to make these tests pass, using docs/tdd-plans/<plan-file>.md as the spec.
Don't change the tests.
```
Review every change with **Keep** or **Undo**:
- Which files did it change? Did it add the route next to the other order routes?
- Did it touch the test file? It shouldn't have.

### 2.6 Verify (⏱ 2 min)
1. In the terminal, run:
   ```bash
   make test
   ```
   It keeps watching for changes: check that your new and existing API tests pass, then press **q** to quit. Make then attempts the frontend tests, but this repo has no frontend `test` script. A `Missing script: "test"` message at that point is a known limitation, not an API-test failure.
2. Run `make dev-api`, open **Ports** → **3000** in the browser and add `/api-docs` to the end of the URL. Find your new endpoint in Swagger, and try it with **Try it out**. If it targets `localhost`, use the same endpoint path on the forwarded port URL instead. Stop the server with **Ctrl+C**.
3. Go through the acceptance criteria in your plan: does each one have a passing test?

| Role | Your focus |
|---|---|
| Backend developer | Did tdd-green write only what the tests need? Would you approve this pull request? |
| Frontend developer | Read the tests: could you build a "My branch's orders" screen from them alone? |
| Data engineer | How does the query filter by branch? What happens with a branch that has thousands of orders? |
| Platform / DevOps | Which Learn suggestion would matter most in production: status codes or pagination? |

> **Agent missing from the picker, or it says it can't edit files or run tests?** Choose **Agent** and put the role in front of the same prompt, for example `Act as a TDD red-phase agent: only write tests, never the feature.` or `Act as a TDD green-phase agent: write the smallest code that passes the tests, and don't change the tests.`

> **No answer key on `main` this time.** Your spec and your tests *are* the answer key. That's the point of spec-driven development.

✅ The tests pass, every acceptance criterion has a test, and the endpoint is in Swagger.

---

## Extensions (if you finish early)
- **Run a second cycle.** Choose **tdd-planner** and send: `Add an optional status filter to the orders-by-branch endpoint, for example ?status=pending. Update the plan.` Edit the plan, then run **tdd-red** and **tdd-green** again.
- **Tidy up while green.** Choose **Agent** and send: `Refactor the orders-by-branch code to match the style of the other order routes. Don't change its behaviour, and run the tests before and after.`
- **Tell every agent about Learn.** Add this line to `.github/copilot-instructions.md`: `For Azure and Microsoft technology questions, use the Microsoft Learn MCP tools.` This connects F3 and F4.

## Think about this for the wrap-up
- What did MCP add that the model didn't know?
- Which suggestion did you reject in the human check, and why?
- Did the tests change how much you trust the agent's code?
- Which building block will you use first at PGGM: prompts, agents, instructions, skills, MCP, or specs and tests?

[Labs index](README.md) · [Previous: F3](F3-context-engineering.md) · Next: none
