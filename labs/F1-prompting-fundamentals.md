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
- [api/src/models/order.ts](../api/src/models/order.ts)
- [api/src/repositories/ordersRepo.ts](../api/src/repositories/ordersRepo.ts)
- [api/src/routes/order.ts](../api/src/routes/order.ts)

**2.2** In [ordersRepo.ts](../api/src/repositories/ordersRepo.ts), click on an empty line **between two existing `async` methods**. Don't use the very top or bottom of the class. Type:
```
// Count orders by branch
```
Then press **Enter**.

**2.3** Grey "ghost text" appears. Press **Tab** to accept it, and press Tab again if more lines are suggested.
No suggestion? Wait a second, or start typing `async countBy` and watch.

**2.4** Select your new method → press **Ctrl+I** (Mac: **Cmd+I**) → type `Add a short JSDoc comment` → **Accept**.

**2.5** (Optional) Right-click the method → **Copilot** → **Explain**.

✅ [ordersRepo.ts](../api/src/repositories/ordersRepo.ts) now has a method that counts orders for a branch.

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
1. In the terminal, run `make dev-api`.
2. Open **Ports** → **3000** → add `/api-docs`.
3. Open **Orders** and find your new endpoint (something like `GET /api/orders/branch/{branchId}/count`).
4. Choose **Try it out**, set `branchId` = `1`, and choose **Execute**. If Swagger targets `localhost`, use the same endpoint path on the forwarded port URL instead.

**3.4** Stop the server with **Ctrl+C**.

✅ Swagger shows your endpoint and it returns a number.

---

## Part 4: Plan, to think before you build (⏱ 7 min)

**4.1** Start a new chat, click on the message icon at the bottom, and choose **Plan**.
**4.2** Attach the design: drag [docs/design/cart.png](../docs/design/cart.png) from the Explorer into the chat (or use **Add Context** → **Files**).
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
- Agent: `Write unit tests for countByBranch following the style of the existing repository tests.` Then run `make test-api`. It keeps watching for changes; press **q** to quit.
- Send the 1.2 prompt again with a different model from the model picker. What changes?

## Think about this for the wrap-up
- Which pillar made your second prompt better?
- When would you start with **Plan** instead of **Agent**?
- What did you check before you clicked **Keep**?
