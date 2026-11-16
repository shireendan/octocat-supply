# Welcome to PGGM Innovation Day

**From an unfamiliar codebase to a working feature, with GitHub Copilot.**

New team. New repository. Your first feature request. It's a familiar engineering challenge: before you can build anything, you need to understand the application, find the right files and learn the team's conventions. Today, we'll work through that journey together, with an AI teammate.

This repo is your hands-on playground for the **Foundation Track**. We'll move from asking Copilot questions to working with agents that can plan, edit files and run tools. Along the way, you'll learn how to give them useful context and clear boundaries, while keeping the engineering decisions in your hands.

Whether you work on frontends, APIs, data or platforms, you're welcome here. You don't need to know this codebase or be a TypeScript expert: your job is to **describe, review and verify**. Where possible, pair up with someone from a different role and compare perspectives.

Our shared application is **OctoCAT Supply**, a sample supply-chain app, not a PGGM production system. [labs/](./labs/) contains the guided exercises; everything else is the application we'll explore and extend throughout the day.

## Start here

1. Sign in with the GitHub account that has your Copilot license and access to this repo.
2. Use **native Windows Node.js/npm on your DevBox or laptop** as the primary option. GitHub Codespaces is the alternative if you cannot prepare that environment.
3. Follow the [setup guide](./labs/01-setup.md) to install tools, prepare your workspace, run the app and check Copilot before F1.
4. Open the [labs README](./labs/README.md) for the schedule and context on how the exercises work.

## Environment options

- **Option 1 - Windows DevBox/local (recommended):** run Node.js/npm directly on a Windows DevBox or laptop, using VS Code, Git and Node.js **24.x** (npm included). Follow the [Windows setup](./labs/01-setup.md#option-a-local-nodejs-and-npm-windows), including [Node.js and Python installation](./labs/01-setup.md#install-nodejs-and-python). No Docker or `make` is required.
- **Option 2 - GitHub Codespaces:** follow the [Codespaces setup](./labs/01-setup.md#option-b-github-codespaces) for a cloud workspace with tools, dependencies and builds prepared automatically.

  [![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/shireendan/octocat-supply/tree/workshop?quickstart=1)

Both options use the same app and labs, with **3000** for the API and **5137** for the website. Codespaces uses forwarded URLs; local users open `localhost`. The local instructions use Windows PowerShell and npm commands instead of the labs' `make` shortcuts.

Open the **repository root** in VS Code: [app/](./app/) contains the application and [labs/](./labs/) contains the exercises. Repository-wide configuration stays at the root so GitHub, Codespaces and Copilot can discover it.

**Version baseline:** use the latest **Node.js 24.x** release and the committed dependency lockfiles. Node 26 is not the workshop baseline.

Complete the app and Copilot checks before the training. Both options require repo access and a working Copilot license; local setup also needs permission to install tools and download packages. If installation or enterprise network restrictions prevent a working setup, contact IT or ask a facilitator about pairing with someone whose environment is ready.

## Branches

- **`workshop`:** where you work during the labs.
- **`main`:** the complete app and reference solution you'll compare with. Never commit to `main`.

## What we'll do together

Start with the overview and setup guide, then follow one application through four steps from getting oriented to agentic development:

| Guide or session | Purpose | What it covers |
|---|---|---|
| [Labs README](./labs/README.md) | Get the workshop context before you begin. | The schedule, how the labs work, lab conventions and where to get help. |
| [Setup guide](./labs/01-setup.md) | Complete setup before F1. | Install tools, prepare a Windows DevBox/local workspace or Codespace, run the app and check Copilot. |
| [Troubleshooting and reference](./labs/troubleshooting.md) | Use only if you need help or extra commands. | Certificate and PATH fixes, native-build requirements, test commands, branch recovery and reference-file comparisons. |
| [F1. Prompting Fundamentals](./labs/F1-prompting-fundamentals.md) | Understand an unfamiliar repo and tackle your first ticket: count orders by branch. | Clear prompts, code completion, and choosing between Ask, Agent and Plan. |
| [F2. Build Your Personal Agent](./labs/F2-build-your-personal-agent.md) | Give an AI teammate a focused role and clear boundaries. | A shared API Specialist and your own read-only review agent. |
| [F3. Context Engineering in Practice](./labs/F3-context-engineering.md) | Help Copilot follow the team's conventions instead of guessing. | Reusable repository instructions, project context and skills. |
| [F4. First Steps in Agentic Development](./labs/F4-mcp-and-spec-driven.md) | Take an orders-by-branch API feature from a specification to a working endpoint. | External documentation through Model Context Protocol (MCP), acceptance criteria, tests first and implementation. |

The goal isn't just to generate code. It's to practise a workflow you can take back to your PGGM team: **understand, plan, provide context, build, review and verify**. You decide what success looks like, review the agent's changes and check that the tests and running application back up its claims.

Follow the [labs index](./labs/README.md) for the schedule and step-by-step exercises. Different Copilot outputs are expected; bring your questions, compare approaches and check the results together.

## What's where

| Folder | Contents |
|---|---|
| [app/](./app/) | Application code, build commands, documentation, infrastructure and legacy demos |
| [labs/](./labs/) | Hands-on workshop exercises |
| [app/api/](./app/api/) | Express REST API and SQLite database |
| [app/frontend/](./app/frontend/) | React website |
| [app/docs/](./app/docs/) | Architecture, database, build and deployment guides |
| [app/infra/](./app/infra/) | Infrastructure and deployment scripts |
| [app/demo/](./app/demo/) | walkthroughs from the original sample |
| [.github/](.github/) | Copilot and GitHub configuration |
| [.devcontainer/](.devcontainer/) | Codespaces environment setup |
| [.vscode/](.vscode/) | VS Code tasks, debug and workspace configuration |
