# Mona's Project Pulse agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate the work, with each
specialist assigned a clear scope and coordinated through the **Orchestrator**.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Coordinates the team, breaks the dashboard request into phases, delegates work, manages dependencies and file ownership, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository, documentation, dependencies, edge cases, and risks, then produces the ordered implementation plan and validation expectations. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements the dashboard logic and runnable-app support within the assigned files, keeping behavior explicit, deterministic, testable, and validated. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Shapes the Project Pulse user experience: information hierarchy, accessibility, interaction flow, responsive layout, visual clarity, project cards, status badges, priorities, and styling hooks. | `.github/agents/designer.agent.md` |

The Orchestrator will generally obtain the Planner's research first, then
delegate implementation and design tasks to the Coder and Designer in parallel
when their file scopes do not overlap, followed by integration and validation.
