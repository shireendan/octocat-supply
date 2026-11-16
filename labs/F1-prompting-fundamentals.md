# F1 · Prompting Fundamentals: lab

[Labs index](README.md) · [Previous](01-setup.md) · [Next](F2-build-your-personal-agent.md)

⏱ **30 minutes** · Part 1: 8 · Part 2: 5 · Part 3: 10 · Part 4: 7

## What will you do?

Build a small feature in an unfamiliar application: count the orders belonging to a branch and expose that count through the API. Then plan a separate cart feature without building it.

**Why it matters:** onboarding and feature work mix questions, small edits and wider changes. This lab helps you choose the right Copilot tool, give it useful context and review the result instead of delegating blindly.

**Your story:** you have just joined the OctoCAT team. Your first ticket is *"Show how many orders each branch has."* You have never seen this repo before.

**You'll practise:** Ask (understand) → code completion and inline chat (write) → Agent (build) → Plan (think first).
**Missing on your branch:** nothing. F1 uses the app as it is.
**Done when:** 
- Swagger shows your new "count orders by branch" endpoint.
- You have read a Plan for a cart.

> **Role tip:** not a TypeScript person? You won't write code by hand. Copilot writes it; you describe, review and verify.

## New words
                                                                        
**Ask**, **Plan** and **Agent** are different ways of working, not interchangeable labels:

| Choice | What it's for in this lab |
|---|---|
| **Ask** | Understand the repo and locate relevant files without edits (Part 1). |
| **Plan** | Research and propose steps before implementation; here, only plan the cart (Part 4). |
| **Agent** | Make coordinated file changes; here, add the endpoint and Swagger docs within your prompt's boundaries (Part 3). |


- **Code completion**: Predict the next code as you type; accept suggestions for the counting method (Part 2).
- **Inline chat**: Request a focused change to selected code, such as a short comment (Part 2).
- **Prompt**: is the request you give Copilot. A vague request leaves it guessing. The improved prompt adds **context** (your role), **intent** (your goal), **clarity** (a direct task) and **specificity** (scope and an output limit). Judge the difference by the usefulness of the answer, not the prompt's length.

## Before you start

Complete the [setup checks](01-setup.md). Follow Parts 1–4 in order, run commands only when the sheet asks, and review changes before choosing **Keep**.

Run terminal commands from the **repo root**. Use the Codespaces commands or the Windows PowerShell alternatives for your laptop or DevBox.

---

## Part 1: Ask, to understand the repo (⏱ 8 min)

**1.1 A vague prompt.** Start a new chat, click on **+**, click on **Agent...**, choose **Ask**, and send:
```
explain this project
```

**1.2 A better prompt.** Start a new chat, choose **Ask** again, fill in the `<…>` parts and send:
```
I'm a <your role, e.g. data engineer> and new to this repo.
Explain what this project does, which languages and frameworks it uses, how I run it and how it is tested.
Focus on the parts that matter for my area: <API / website / database / build and deployment>.
Answer in at most 10 bullets.
```
Compare 1.1 and 1.2. Which of the four pillars made the difference: **context**, **intent**, **clarity** or **specificity**?

**1.3 Scope the ticket.** In the same chat (still **Ask**):
```
I need to add "count orders by branch" to the API.
Which files would change, and in which order? Don't write code.
```

> **Finished early?** Drag [.github/prompts/refine-prompt.prompt.md](../.github/prompts/refine-prompt.prompt.md) into the chat, paste your 1.2 prompt and ask: `Use the attached instructions to score and improve my prompt.`

---

## Part 2: Code completion and inline chat (⏱ 5 min)

**2.1** Open these three files and keep them open, because open files give Copilot context:
- [app/api/src/models/order.ts](../app/api/src/models/order.ts)
- [app/api/src/repositories/ordersRepo.ts](../app/api/src/repositories/ordersRepo.ts)
- [app/api/src/routes/order.ts](../app/api/src/routes/order.ts)

**2.2** In [ordersRepo.ts](../app/api/src/repositories/ordersRepo.ts), click on an empty line **between two existing `async` methods**. Don't use the very top or bottom of the class. Type:
```
// Count orders by branch
```
Then press **Enter**.

**2.3** Grey "ghost text" appears. Press **Tab** to accept it, and press Tab again if more lines are suggested.
No suggestion? Wait a second, or start typing `async countBy` and watch.

**2.4** Select your new method → press **Ctrl+I** (Mac: **Cmd+I**) → type `Add a short JSDoc comment` → **Accept**.

**2.5** (Optional) Right-click the method → **Copilot** → **Explain**.

✅ [ordersRepo.ts](../app/api/src/repositories/ordersRepo.ts) now has a method that counts orders for a branch.

---

## Part 3: Agent, to build the feature end to end (⏱ 10 min)

**3.1** Start a new chat (this opens Agent mode by default) and send:
```
Expose the countByBranch method I just added to ordersRepo.ts as a GET endpoint in the orders route,
and add Swagger documentation for it in the same style as the other endpoints.
Don't run, deploy or test anything yet.
```
(If your method has a slightly different name, the agent will find it.)

**3.2** Watch the agent work, then **review every change**. The edited files are listed in the chat; choose **Keep** or **Undo**.
- You can stop the agent at any time with the stop button.
- Each request has a checkpoint you can restore.

**3.3** Try it:
1. Start the API from the repo root:
   - **Codespaces:** `make -C app dev-api`
   - **Windows PowerShell:** `npm.cmd --prefix .\app\api run dev`
2. Open Swagger:
   - **Codespaces:** open **Ports** → **3000** → add `/api-docs` to the forwarded URL.
   - **Windows:** open http://localhost:3000/api-docs in a browser on the laptop or DevBox running the API.
3. Open **Orders** and find your new endpoint (something like `GET /api/orders/branch/{branchId}/count`).
4. Choose **Try it out**, set `branchId` = `1`, and choose **Execute**. **Codespaces only:** if Swagger targets `localhost`, use the same endpoint path on the forwarded port URL instead.

**3.4** Stop the server with **Ctrl+C**.

✅ Swagger shows your endpoint and it returns a number.

---

## Part 4: Plan, to think before you build (⏱ 7 min)

**4.1** Start a new chat, click on the message icon at the bottom, and choose **Plan**.
**4.2** Attach the design: drag [app/docs/design/cart.png](../app/docs/design/cart.png) from the Explorer into the chat (or use **Add Context** → **Files**).
**4.3** Send:
```
Using the attached design, plan a simple cart for the website: a cart icon in the navigation bar
that shows how many items are in the cart, and a cart page that lists them.
No discounts and no tests for now. Keep the phases small. Don't implement anything.
```
**4.4** Read the plan:
- How many phases does it have?
- Which files would it touch?
- Did it ask you any questions?

**Don't build the cart.** This part is only about planning.

✅ You have a plan, and no files were changed.

---

## Compare
F1 has no answer key, because every endpoint will look a little different. Compare with your neighbour:
- Same endpoint path?
- Same Swagger documentation?
- Who reviewed more carefully before clicking **Keep**?

## Before you leave
Nothing later today depends on F1. Keep your changes, or discard them with **Source Control** → **…** → **Discard All Changes**.

## Extensions (if you finish early)
- Agent: `Write unit tests for countByBranch following the style of the existing repository tests.` Then run the API tests from the repo root: **Codespaces:** `make -C app test-api`; **Windows PowerShell:** `npm.cmd --prefix .\app\api test`. Tests keep watching for changes; press **q** to quit. For a single run, use the [setup guide's test commands](01-setup.md#local-windows-equivalents-for-lab-commands).
- Send the 1.2 prompt again with a different model from the model picker. What changes?

## Think about this for the wrap-up
- Which pillar made your second prompt better?
- When would you start with **Plan** instead of **Agent**?
- What did you check before you clicked **Keep**?

---

## For your own use case

Bringing your own repository? Follow the same four parts below using your codebase instead of OctoCAT. The language, file names, commands and feature should come from your project.

**Before you start:**
- Open your repository in VS Code and check that Copilot responds.
- Save your existing work and use a working branch where you are allowed to experiment. Only undo changes made for this exercise; do not discard unrelated work.
- Choose a small feature you can describe in one sentence, such as filtering a list, adding validation or adding a command-line option. Decide what observable result would show that it works.
- Replace every `<placeholder>` below with your own information.

### Part 1: Ask, to understand your codebase

**1.1 A vague prompt.** Start a new chat, choose **Ask**, and send:
```
explain this project
```

**1.2 A refined prompt.** Start a new chat, choose **Ask** again, and send:
```
I'm a <ROLE> working in this repository.
Explain what this project does, its main languages and frameworks, and how it is run and tested.
Focus on <AREA, e.g. data processing / user interface / API / automation>.
Point to the files or configuration that support your explanation.
Answer in at most 10 bullets. Don't edit files or run commands.
```
Compare the answers. Did adding your role, area and output expectations make the second answer more useful? Check a few of its claims against the actual files.

**1.3 Find the starting point.** In the same chat, still in **Ask**, send:
```
I want to add <SMALL FEATURE> so that <USER> can <BENEFIT>.
Which existing files and functions are relevant? Is there similar behaviour I can reuse?
Suggest the smallest useful change. Don't implement anything.
```

**Done when:** you can explain the relevant part of your codebase and identify where your small change belongs.

### Part 2: Code completion and inline chat, to make a small change

1. Open the relevant implementation file, a related file and an existing example of similar behaviour. Keep them open to give Copilot context.
2. At a suitable place in the implementation file, type a short comment describing **one small piece** of your feature, using your language's comment syntax. For example: "Return only items matching the selected category." Press **Enter**.
3. Inspect the suggested code before accepting it with **Tab**. Does it use the project's types, naming and helpers? Accept only a useful suggestion; reject unrelated changes. If nothing appears, start typing a meaningful function name or signature.
4. Select your new code, open inline chat with **Ctrl+I** (Mac: **Cmd+I**), and send:
   ```
   Add a short comment explaining this code, following this repository's style.
   Don't change its behaviour.
   ```
5. Review the edit before accepting. If you are unsure what the code does, ask Copilot to explain the selection.

**Done when:** you have a small, reviewed change that fits your project. It does not need to complete the whole feature yet.

### Part 3: Agent, to complete and verify the feature

**3.1 Give the agent a bounded task.** Start a new chat, choose **Agent**, attach the relevant file or files, and send:
```
Complete <SMALL FEATURE>, building on the change I made in <FILE>.
Expected behaviour: <OBSERVABLE RESULT>.
Follow this repository's patterns and reuse existing helpers.
Only change files needed for this feature; avoid unrelated refactoring or dependency updates.
Update directly affected tests and documentation where appropriate.
Don't run commands, deploy, commit or push anything yet.
```

**3.2 Review before keeping.** Check every changed file. Does the change meet your expected behaviour? Did the agent stay within scope? Use **Keep** or **Undo** after reviewing, not just because it reports success.

**3.3 Verify using your project's tools.** In **Ask**, request:
```
Which existing commands should I run to verify this change?
Check this repository's documentation, scripts and test configuration.
Give the smallest relevant test/build commands and a manual check of the feature.
Don't run anything or edit files.
```
Check those commands before running them yourself. Use **your repository's** setup and tools, not OctoCAT's commands unless they also apply. Stop at the first error, and check that any failing test is about the feature rather than a missing tool or dependency.

**Done when:** the relevant checks pass and you can demonstrate the feature's expected result. Do not deploy, commit or push as part of this exercise.

### Part 4: Plan, to think through your next feature

1. Think of a second feature or a broader improvement that you would normally discuss before coding.
2. Write down who needs it, what they should be able to do and what is out of scope. Attach an issue description, design or your notes if you have them.
3. Start a new chat, choose **Plan** using the same control as Part 4 above, and send:
   ```
   Plan <NEXT FEATURE> for this repository so that <USER> can <BENEFIT>.
   Use the attached requirements and the existing code as context.
   Keep <OUT-OF-SCOPE ITEMS> out of the plan.
   Propose small phases, likely files to change, important edge cases and how to verify each phase.
   Call out assumptions and questions rather than inventing requirements.
   Reply with the plan only. Don't implement anything, edit files or run commands.
   ```
4. Review the plan. Are the phases small enough? Does it reuse existing code? Are the suggested files and checks real? Answer its questions and refine the scope before anyone implements it.

**Done when:** you have a plan you can discuss with your team, with no implementation changes from this part.

**Reflect:** which context improved the answer, which suggestion did you reject, and what evidence made you trust the result? The same rhythm applies in your own work: understand with **Ask**, make a focused edit with **code completion/inline chat**, complete a bounded task with **Agent**, and think ahead with **Plan**.
