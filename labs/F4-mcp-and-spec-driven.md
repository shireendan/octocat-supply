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

Complete the [setup checks](01-setup.md) in **your own template-based repo** on `workshop`, then follow the cleanup and chat instructions below.

Run terminal commands from the **repo root**. The Microsoft Learn HTTP MCP setup is the same in Codespaces and in VS Code on a Windows laptop or DevBox.

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
2. Look for a plan in `app/docs/tdd-plans/`; if it wasn't saved, use the fallback below. Open the saved file and read the **Acceptance Criteria** and **Test Specifications**.

> **No file in `app/docs/tdd-plans/`?** Choose **Agent** and send: `Save that plan as app/docs/tdd-plans/orders-by-branch.md. Don't change anything else.`

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
Implement the failing tests for the plan in app/docs/tdd-plans/<plan-file>.md. Use the file, because I edited it. Only create test files, then run them.
```
1. Review the new test file (for example `app/api/src/routes/order.test.ts`) and choose **Keep**.
2. The tests should **fail because the feature doesn't exist yet**, for example with a 404. A typo in the test is the wrong reason: tell tdd-red.

If the agent needs help running tests, use the [single-run API test command reference](troubleshooting.md#local-windows-equivalents-for-lab-commands) for your environment. A missing command or dependency is not the expected red-phase failure.

> **About the buttons:** the agents may show buttons such as **Write Red Tests** and **Write Green Implementation**. They switch agents and send the prompt for you, but only in the *Local* session type, and they send the plan from the *chat*, not your edited file. That's why we use the agent picker and name the file.

### 2.5 Green: make them pass (⏱ 4 min)
In the same chat, choose **tdd-green** and send, using the same filename:
```
Implement the code to make these tests pass, using app/docs/tdd-plans/<plan-file>.md as the spec.
Don't change the tests.
```
Review every change with **Keep** or **Undo**:
- Which files did it change? Did it add the route next to the other order routes?
- Did it touch the test file? It shouldn't have.

### 2.6 Verify (⏱ 2 min)
From the repo root, use the commands for your environment:

| Task | Codespaces | Windows PowerShell |
|---|---|---|
| API tests | `make -C app test-api` | `npm.cmd --prefix .\app\api test` |
| Start API | `make -C app dev-api` | `npm.cmd --prefix .\app\api run dev` |

1. Run the API tests. Check that your new and existing API tests pass, then press **q** to quit watch mode. For a single run instead, use the [single-run commands](troubleshooting.md#local-windows-equivalents-for-lab-commands).
2. Start the API, then open Swagger:
   - **Codespaces:** open **Ports** → **3000** and add `/api-docs` to the forwarded URL.
   - **Windows:** open http://localhost:3000/api-docs on the laptop or DevBox running the API.

   Find your new endpoint and use **Try it out**. **Codespaces only:** if Swagger targets `localhost`, use the same endpoint path on the forwarded port URL instead. Stop the server with **Ctrl+C**.
3. Go through the acceptance criteria in your plan: does each one have a passing test?

| Role | Your focus |
|---|---|
| Backend developer | Did tdd-green write only what the tests need? Would you approve this pull request? |
| Frontend developer | Read the tests: could you build a "My branch's orders" screen from them alone? |
| Data engineer | How does the query filter by branch? What happens with a branch that has thousands of orders? |
| Platform / DevOps | Which Learn suggestion would matter most in production: status codes or pagination? |

> **Agent missing from the picker, or it says it can't edit files or run tests?** Choose **Agent** and put the role in front of the same prompt, for example `Act as a TDD red-phase agent: only write tests, never the feature.` or `Act as a TDD green-phase agent: write the smallest code that passes the tests, and don't change the tests.`

> **No answer key on `reference_solution` this time.** Your spec and your tests *are* the answer key. That's the point of spec-driven development.

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

---

## For your own use case

Bringing your own repository? Connect Microsoft Learn as a source of technical guidance, then take a small feature from a specification you approve to tested, working code.

**Before you start:**
- Open your repository in VS Code, save your work and use a working branch where you are allowed to make changes.
- Keep any real repository instructions or skills you created in F3. Do not copy OctoCAT's fictional TAO rule into your project.
- Choose a small feature that is not already implemented, such as validating an input, filtering a result list or adding a command-line option. State who needs it, why, and what is out of scope.
- Check that the relevant existing tests run successfully before starting. Use your project's documented tools and shell; if there is no test setup, agree an approach with your team rather than installing a new framework without review.
- Replace every `<placeholder>` below. Your repository does not need OctoCAT's `tdd-planner`, `tdd-red` or `tdd-green` agents: the prompts below use the built-in **Plan** and **Agent** modes.

### Part 1: Connect Microsoft Learn MCP in your repository

**1.1 Add or reuse the server.**
1. Open the Command Palette with **Ctrl+Shift+P** and choose **MCP: Add Server...**.
2. Choose **HTTP** and enter `https://learn.microsoft.com/api/mcp`.
3. Name it `microsoft-learn` and choose **Workspace** so the configuration is associated with your repository. If that server is already configured, reuse it rather than adding a duplicate.
4. Open your repository's `.vscode/mcp.json` and start the Microsoft Learn entry. Preserve any other server entries and leave unrelated servers stopped for this exercise. Review any trust or tool-approval prompts before accepting.

This is a remote HTTP server: the setup is the same in Codespaces and local VS Code, with local MCP package to install.

**1.2 Confirm the tools are available.** Open Chat's tool configuration using the controls from Part 1 of the main lab. Look for `microsoft_docs_search` and `microsoft_docs_fetch`, and make sure they are available to the chat.

**1.3 Find guidance relevant to your feature.** Start a new chat in **Agent** mode and send:
```
Use the Microsoft Learn MCP tools to find guidance on <TECHNICAL TOPIC RELEVANT TO MY FEATURE>.
Summarise the guidance in at most 6 bullets with source links.
Distinguish general design advice from requirements specific to a Microsoft product or version.
Don't edit files or run terminal commands.
```

Choose a topic that genuinely helps your decision, such as API validation, error responses, pagination, accessibility or testing guidance for technology you use. Search with general technical terms, not private source code or credentials. Do not introduce a Microsoft service or framework just because a search result uses it.

Expand the tool call and open a cited source. Did the tools actually run, and does the guidance apply to your technology and version?

**Done when:** a Microsoft Learn tool call returns useful guidance with sources you have checked. Keep this chat open for the specification work.

### Part 2: Specify, test and implement your feature

Use the same chat for the following steps, switching between **Plan** and **Agent** as directed. The saved specification, not an earlier chat draft, will be the source of truth.

**2.1 Draft the specification.** Choose **Plan** and send:
```
Specify <SMALL FEATURE> for this repository so that <USER> can <BENEFIT>.
Inspect the existing code and use the relevant Microsoft Learn guidance already discussed.
Include:
- The goal, current behaviour and proposed behaviour.
- In-scope and out-of-scope work.
- Numbered acceptance criteria covering the normal case and relevant edge/error cases.
- A Given/When/Then test case for each criterion, linked to its number.
- Likely files to change and existing patterns to reuse.
- Existing test/build commands, their working directories and a manual verification scenario.
Call out unanswered questions and assumptions instead of choosing business rules for me.
Return the specification only. Don't edit files, implement code or run terminal commands.
```
Read it as a product and engineering decision, not just a list of files. Are the expected results precise enough that you could judge a test as passing or failing?

**2.2 Review, decide and save.** Switch to **Agent** and send:
```
Review this draft against the Microsoft Learn sources and this repository's conventions.
Suggest at most three improvements to the acceptance criteria and their test cases.
Don't edit files or implement anything.
```

Accept or reject each suggestion yourself. Resolve open questions, remove unnecessary scope and update each affected test case along with its criterion.

Save the approved specification as a Markdown file in your repository's usual location. If there is no convention, choose a clear location such as `docs/feature-specs/<feature-name>.md`. You can create it yourself or ask **Agent**:
```
Save the approved specification as <SPEC_FILE>.
Only create or update that specification file. Don't implement the feature or write tests yet.
```
Open the saved file, make any final corrections by hand and save it. Use this exact path in the next prompts.

**2.3 Red: write tests before implementation.** Stay in **Agent** and send:
```
Act as a TDD red-phase agent.
Read <SPEC_FILE> from disk; I may have edited it since the chat draft.
Write tests for its acceptance criteria using this repository's existing test framework and conventions.
Only change the required test files and test fixtures. Don't change production code, dependencies or the specification.
Run the relevant tests with the existing single-run command and report which criteria each test covers.
The new tests must fail because the specified feature is missing, not because of a broken setup.
Don't deploy, commit or push.
```

Review the tests and the actual failure output. Do the assertions express your agreed behaviour? A missing tool, dependency or invalid test command is a setup problem, not the expected red result. Resolve that before continuing, without weakening the feature's tests.

**2.4 Green: implement only the agreed behaviour.** Stay in **Agent** and send:
```
Act as a TDD green-phase agent.
Implement the smallest change that satisfies <SPEC_FILE> and the tests just written.
Read the saved specification and tests first. Reuse this repository's existing patterns and helpers.
Don't change the specification or tests, add unrelated features, or update dependencies.
If the specification is contradictory or cannot be implemented as written, stop and explain the issue.
Run the relevant new and existing tests with the project's single-run commands.
Update directly affected documentation where needed, and summarise the changed files and verification results.
Don't deploy, commit or push.
```

Review every change before choosing **Keep**. Check that the agent did not make tests pass by deleting assertions, changing expected results or broadening the scope.

**2.5 Verify the feature yourself.**
1. Run the relevant tests yourself using the commands verified for your project. Run any additional affected checks or build required by your team's workflow.
2. Try the manual scenario in the specification through your project's actual interface: a screen, API request, script, command or data-processing result.
3. Check each acceptance criterion against its test and observed result. Include the relevant invalid-input, empty-result or other edge cases you agreed.
4. Review the diff for unexpected files and confirm that unrelated behaviour is preserved. If the requirements need to change, explicitly revise the specification and corresponding tests before continuing implementation.

Use your own repository's commands and interface, not OctoCAT's (unless your project happens to use them).

**Done when:** the small feature works, its tests and relevant existing checks pass, and every acceptance criterion has evidence. A plan alone or an agent's success message is not completion.

**Reflect:** what did the external guidance improve? Which suggestion did you reject? Could another developer implement the feature from the saved specification without reading your chat? You have now practised the whole loop on your own codebase: find guidance, own the specification, observe failing tests, implement and verify.
