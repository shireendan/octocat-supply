# PGGM Innovation Day: Foundation Track labs

Welcome! This is your participant guide for the hands-on part of PGGM Innovation Day. We'll follow a familiar engineering journey: join an unfamiliar project, understand what its users need, learn the team's way of working and deliver a feature with GitHub Copilot.

You don't need to know this application or be a TypeScript specialist. Whatever your role, your job is to **describe, review and verify**: give the agent a clear task, judge its suggestions and check that the result actually works.

[Back to the repo README](../README.md) for environments and app commands.

**Jump to:** [The application](#meet-octocat-supply) | [Setup options](#before-you-begin) | [Schedule](#schedule) | [How the labs work](#how-the-labs-work) | [Conventions](#conventions-in-the-lab-sheets) | [Getting help](#stuck)

## Meet OctoCAT Supply

**OctoCAT Supply is a fictional business that sells smart cat products through a network of branches.** Think automatic feeders, smart collars and cat cameras. The sample application combines a customer-facing product catalog with the business records needed to organise products, branch orders and supplier deliveries.

Here's what those terms mean in everyday use:

| Part of the business | What it represents |
|---|---|
| **Products** | The items the business offers, with names, descriptions, prices and a supplier. Customers browse and search the catalog; product administrators maintain its entries. |
| **Suppliers** | The businesses providing those products. Product and delivery records identify the relevant supplier. |
| **Headquarters and branches** | The organisation's locations. A branch belongs to a headquarters and places its own orders. |
| **Orders** | A branch's requests for products, with an order date and status. A manager needs to distinguish their branch's orders from everyone else's. |
| **Order lines** | The individual products in an order: which item, how many units and at what unit price. One order can contain several lines. |
| **Deliveries** | Shipments from suppliers. Delivery records can be linked to order lines to record which quantities have been fulfilled. |

### A business problem behind your first tickets

Imagine you manage a branch that has ordered ten **SmartFeeder One** units. You don't want to search through every other branch's orders just to find your own work.

- **"How many orders does my branch have?"** gives you a quick view of its workload. In [F1](F1-prompting-fundamentals.md), you add an API operation to answer that question.
- **"Show me the orders for my branch."** gives you the actual list to work through. In [F4](F4-mcp-and-spec-driven.md), you specify, test and implement that feature.

These are deliberately small, understandable requests. The challenge is learning how to find the right part of an existing application, describe the change precisely and prove that it works.

**A note on scope:** this is a training sample, not a PGGM system or a complete commerce platform. The website supports browsing products and product administration, but not every business operation has a finished website screen. We'll explore some operations through **Swagger**, the interactive API documentation where you can try requests in your browser. The cart exercise in F1 is a planning exercise, not a request to build a complete checkout.

## Before you begin

**Codespaces is recommended, not required.** Choose an environment and complete its checks before the training:

| Environment | What you need | Setup |
|---|---|---|
| **GitHub Codespaces (recommended)** | A GitHub account with repo access, a Copilot license and Codespaces access | [Codespaces setup](01-setup.md#option-a-github-codespaces-recommended) |
| **Native Windows Node.js and npm (laptop or DevBox)** | VS Code, Git, the latest Node.js 24.x release (npm included), repo access and a Copilot license; no Docker or `make` required | [Windows setup](01-setup.md#option-b-local-nodejs-and-npm-windows) |

Both options cover the same labs. Codespaces prepares the tools automatically; Windows users install dependencies and build with npm, then run the API and website in two PowerShell terminals on their laptop or DevBox. A Windows DevBox uses this same native Node/npm route, not a separate container setup. Open the local app URLs in a browser on the machine running the app.

**Use the shared baseline:** Node.js **24.x** and the committed lockfiles.

On managed Windows machines, check the [enterprise certificate-trust guidance](01-setup.md#enterprise-certificate-trust) **before npm installation** if company certificates are required or certificate errors appear. The Windows trust-store setting preserves HTTPS verification and is terminal-scoped; it is not a requirement for every participant or for Codespaces.

Follow your option's setup and app checks before the training. If installation permissions, enterprise network restrictions or time prevent a working setup, contact IT or ask about pairing with someone whose environment is ready.

1. Follow [01-setup.md](01-setup.md) before F1. It covers your environment, the `workshop` branch, getting the reference files, checking the app and Copilot, and troubleshooting.
2. Keep the current lab sheet open next to your workspace. On GitHub, use a second browser tab so you can copy prompts and commands. In VS Code, right-click the lab file and choose **Open Preview** for the formatted view.
3. Read the opening story and completion checks before starting each lab. They explain what you're trying to achieve; the numbered steps then guide you through it.

## Schedule

Each session includes an introduction, hands-on work and a wrap-up. The shorter timings inside a lab sheet are for the exercises, not the entire session.

| Time | Session | Why this session matters | Lab sheet |
|---|---|---|---|
| Before 09:45 | Set up your workspace | Make sure the app and Copilot work before the exercises begin. | [01-setup.md](01-setup.md) |
| 09:45–10:45 | F1. Prompting Fundamentals | Learn the repo, improve a prompt and choose the right way to work with Copilot. | [F1-prompting-fundamentals.md](F1-prompting-fundamentals.md) |
| 11:00–12:15 | F2. Build Your Personal Agent | Give an agent a focused role and boundaries, including a reviewer that cannot edit files. | [F2-build-your-personal-agent.md](F2-build-your-personal-agent.md) |
| 13:00–14:30 | F3. Context Engineering in Practice | Make the team's knowledge and working practices reusable through instructions and a skill. | [F3-context-engineering.md](F3-context-engineering.md) |
| 14:45–15:30 | F4. First Steps in Agentic Development | Use external guidance, own the specification and let agents build against tests. | [F4-mcp-and-spec-driven.md](F4-mcp-and-spec-driven.md) |

## How the labs work

### A repeatable rhythm

1. **Understand the idea.** A short introduction explains the technique and why you would use it in everyday engineering.
2. **Try it on a real task.** Follow the lab's numbered steps. Some deliberately compare a baseline with an improved prompt or better context, so don't skip straight to the "better" version.
3. **Inspect the evidence.** Review the changed files, try the operation or run the checks the lab asks for. A confident answer from Copilot is not a substitute for a working result.
4. **Compare and reflect.** Discuss what changed and why. The wrap-up is about what you could reuse in your own work, not whose agent generated the most code.

Read each step's boundaries carefully. **"Don't implement anything"**, **"only edit this file"** and **"don't change the tests"** are part of the exercise: they help you practise controlling an agent's scope.

### One application and one evolving workspace

You use OctoCAT Supply throughout the day rather than starting a different demo each session. Keep your workspace between sessions and follow any **Before you leave** or **Before you start** instructions in the lab sheets.

- **`workshop` is your working branch.** Selected Copilot configuration was deliberately removed or left incomplete for F2 and F3. Rebuilding it lets you see what changes when an agent has a role, team instructions or a reusable skill. A **Missing on your branch** note describes that starting point; it doesn't mean your setup failed.
- **`main` provides reference material, not an answer to every task.** Some **Compare** steps use the original configuration to give you ideas. F1 and F4 do not have a finished feature solution on `main`; their checks, and in F4 your specification and tests, determine whether you're done.
- **Compare does not mean copy everything.** Your wording and implementation can differ. Look for useful differences in behaviour, scope and completeness. Work on `workshop`; don't switch to or commit to `main` to complete a lab.
- **Carry forward the right context, not every experiment.** For example, F4 asks you to remove the fictional TAO team rule introduced in F3 before agents write real code. Follow that cleanup step rather than carrying an intentionally made-up dependency into the next feature.

### Work as an engineer, not a spectator

Work in mixed pairs where possible, such as a backend developer with a platform engineer. One person can drive while the other checks the plan and changes; swap roles along the way.

Copilot is **probabilistic**: the same request can produce different wording, filenames or approaches. That's expected. Compare whether the result meets the requirement, respects the boundaries and passes the relevant checks, not whether it matches your neighbour's output line for line.

## Conventions in the lab sheets

"Conventions" means the reading shortcuts used consistently across the sheets. They tell you where to act, what to replace and when to pause.

### Where does copied text go?

Not every grey code block belongs in the same place. Read the sentence above it:

- **"Send" or "ask Copilot":** paste the text into **Chat** as a prompt.
- **"Run in the terminal":** use your Codespaces terminal, or local PowerShell with the [command equivalents](01-setup.md#local-windows-equivalents-for-lab-commands) instead of `make`. Unless a step says otherwise, start from the repository root.
- **"Create", "replace" or "paste into a file":** edit the named file. A Markdown or JSON example is file content, not a terminal command.

The labs use **API-only tests**: `make test-api` in Codespaces or `npm.cmd --prefix .\api test` in Windows PowerShell. Press **q** to leave watch mode, or use the [single-run commands](01-setup.md#local-windows-equivalents-for-lab-commands) when an agent runs tests.

The code-block copy button saves typing; it doesn't decide where the text should go.

When a lab says **Ports** then **3000**, local users open http://localhost:3000/api-docs for Swagger instead. The local website is at http://localhost:5137; you do not need a public Codespaces URL.

### Labels and controls you'll see

| Convention | What it means, and why |
|---|---|
| **Your story / The story** | The user's situation and the reason for the task. Use it to judge whether the proposed solution solves the right problem. |
| **You'll practise / Done when / completion checks** | The learning goal and the observable finish line. Check the result yourself before moving on. |
| **`<angle brackets>`** | Replace the placeholder, including the brackets, with your own value. For example, `<ROLE>` could become `data engineer`; `<plan-file>.md` must become the actual plan filename. |
| **Ask / Agent / Plan** | Choices in the agent picker, not text to type into a prompt. Use Ask to understand, Plan to work out an approach and Agent to carry out changes. Select the choice requested by the step. |
| **New chat** | Use the **+** at the top of Chat to start a fresh conversation, so an earlier answer doesn't influence the next experiment. It does **not** reset your files. If a lab says to stay in the same chat, keep its conversation context; F4 relies on this. |
| **Attach a file / Add Context** | Give Copilot the specific material the task depends on, for example by dragging a file into Chat or using **Add Context**. Don't assume it knows which design, rule or edited plan you mean. |
| **Keep / Undo** | Review the proposed file changes, then decide whether to retain or reject them. Don't click Keep just because the agent says it has finished. |
| **Compare** | Inspect differences from the reference or discuss them with your pair. This is a learning step, not a requirement to make your files identical. |
| **Catch up** | An optional way to restore a specific reference file so you can continue. It can overwrite your work; read the guidance below first. |
| **⏱ / Extensions / Finished early?** | Timings are suggested timeboxes, not a speed test. Extensions are optional extra practice, not prerequisites for the next session. |

## Stuck?

**Being stuck means you can't confidently take the next step.** That might be a command failing, Copilot not responding, a missing menu, an unexpected result or simply not understanding what you're being asked to do. It is not a failure, and you don't need to spend the whole lab debugging alone.

A different answer from your neighbour is not automatically a problem. First check whether yours meets the lab's completion criteria.

### What to do next

1. **Pause and locate the problem.** Note the lab and step, what you expected and what actually happened. If the agent is making unwanted changes, stop it and inspect the changes before accepting anything.
2. **Check the immediate context.** Are you in the requested chat mode, on `workshop`, using the right file and replacing the placeholders? For a failed command, keep the actual error message rather than only the agent's summary.
3. **Ask for help early.** Raise your hand or ask your pair. Show the facilitator the step and the relevant screen, error or changed file. "F2, A4: my agent isn't in the picker" is much easier to help with than "it doesn't work".
4. **Choose a recovery that fits the problem.** Use the setup troubleshooting for environment issues, the lab's fallback for a missing control, or its specific Catch up step when you need a reference file.

| Situation | Useful next step |
|---|---|
| The codespace or local workspace, Copilot sign-in or app won't start | Open the [setup troubleshooting](01-setup.md#troubleshooting) and ask a facilitator if it persists. |
| Codespaces is unavailable or local software installation is restricted | Check the [environment options](01-setup.md#1-choose-your-environment) with a facilitator. If none can be ready in time, ask about pairing with someone who has a working setup. |
| A menu or agent isn't available as described | Check that lab's fallback instructions. UI labels and available controls can vary; don't spend the session hunting for a button. |
| The agent edited more than you requested | Stop it, review the affected changes and ask for help if you're unsure what to undo. Don't accept unrelated edits to keep moving. |
| The output looks different but seems reasonable | Check the stated requirements and evidence. Discuss the differences instead of trying to reproduce someone else's exact output. |
| You're running out of time | Ask which checkpoint matters for the next session. Skip optional extensions and use a documented Catch up step where appropriate. |

### What "Catch up" does

A Catch up command takes the reference version of **the file named in that command** from `main` and puts it into your working branch. It lets you continue learning without having to finish recreating that file first.

- **It can replace your local edits to that file.** Save a separate copy of anything you want to keep, and ask for help if you're unsure.
- **Use the exact step in the current lab.** Don't reset the whole workspace or restore the entire branch. Some labs require another action afterwards; for example, F3 asks you to reapply its team rule after restoring the instructions.
- **It isn't a universal solution.** F1 and F4 have no finished feature answer key to restore. Use the requirements, tests and facilitator support instead.

The [setup guide](01-setup.md) explains how to view, compare and restore reference files. You can compare first without replacing your work.

## Before moving on

Check the lab's completion criteria, review what changed and follow its handover or cleanup instructions. Bring one observation to the wrap-up: what context helped, what suggestion you rejected, or what evidence made you trust the result.

The aim is to leave with a way of working you can apply at PGGM: **understand the problem, give the agent the right context, keep control of the decisions and verify the outcome.**
