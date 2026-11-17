# PGGM Innovation Day: Foundation Track labs

Practise GitHub Copilot with [OctoCAT Supply](../app/README.md), a fictional smart-cat-products application. Your role is to **describe, review and verify**.

**Before F1:** follow [01-setup.md](01-setup.md).

[Back to the repo README](../README.md)

**Jump to:** [Schedule](#schedule) | [How the labs work](#how-the-labs-work) | [Conventions](#conventions-in-the-lab-sheets) | [Getting help](#stuck)

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

- **Follow the lab sheet.** Read the story and completion checks, then follow the numbered steps and boundaries, including plan-only tasks.
- **Keep one workspace on `workshop` all day.** Missing F2/F3 configuration is intentional. Follow handover and cleanup steps, including removing the fictional TAO rule before generating real code.
- **Review and verify.** Inspect changes and run the required checks. Judge results against the requirements, not your neighbour's exact output.
- **Use `main` for comparison, not as your working branch.** It contains configuration references, not finished F1/F4 features.
- **Pair where possible.** Swap driving and reviewing, and bring one takeaway to the wrap-up. Extensions are optional.

Use the lab sheets for current commands and paths; the [slides](./slides/) still show the older layout in places.

## Conventions in the lab sheets

"Conventions" means the reading shortcuts used consistently across the sheets. They tell you where to act, what to replace and when to pause.

### Where does copied text go?

Not every grey code block belongs in the same place. Read the sentence above it:

- **"Send" or "ask Copilot":** paste the text into **Chat** as a prompt.
- **"Run in the terminal":** use Windows PowerShell with the [command equivalents](troubleshooting.md#local-windows-equivalents-for-lab-commands) instead of `make`, or your Codespaces terminal. Unless a step says otherwise, start from the repository root.
- **"Create", "replace" or "paste into a file":** edit the named file. A Markdown or JSON example is file content, not a terminal command.

The labs use **API-only tests**: `npm.cmd --prefix .\app\api test` in Windows PowerShell or `make -C app test-api` in Codespaces. Press **q** to leave watch mode, or use the [single-run commands](troubleshooting.md#local-windows-equivalents-for-lab-commands) when an agent runs tests.

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

Check the lab's completion criteria, chat mode, branch, files and placeholders. **Ask your pair or a facilitator early**; show the lab/step, expected result and actual error or changed file.

| Situation | Useful next step |
|---|---|
| The codespace or local workspace, Copilot sign-in or app won't start | Open [troubleshooting](troubleshooting.md#common-problems) and ask a facilitator if it persists. |
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

The [reference-file guide](troubleshooting.md#get-the-reference-files) explains how to view, compare and restore files. You can compare first without replacing your work.
