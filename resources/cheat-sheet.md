# HVE Cheat Sheet

One page. Print it, pin it. Full docs: [`docs/rpi/`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/README.md).

## The RPI Loop

```text
Uncertainty → Knowledge → Strategy → Working Code → Validated Code
   (you)      Research      Plan       Implement       Review
```

| Phase | Agent | Prompt entry | Skill entry | Artifact produced |
|-------|-------|-------------|-------------|-------------------|
| Research | Task Researcher | `/task-research` | `/rpi-research` | `.copilot-tracking/research/<topic>-research.md` |
| Plan | Task Planner | `/task-plan` | `/rpi-plan` | `.copilot-tracking/plans/<topic>-plan.instructions.md` (+ details) |
| Implement | Task Implementor | `/task-implement` | `/rpi-implement` | `.copilot-tracking/changes/<topic>-changes.md` |
| Review | Task Reviewer | `/task-review` | `/rpi-review` | `.copilot-tracking/reviews/<topic>-review.md` |
| Full flow (one session) | rpi-agent | `/rpi` | `/rpi-quick` | all of the above |

**Prompt entries (`/task-*`) auto-switch the agent. Skill entries (`/rpi-*`) run without an agent switch.**

## The One Rule

> **`/clear` between phases. Artifacts carry context — chat history does not.**

After `/clear`, restore context by **opening the artifact file in the editor** (or referencing its path in your prompt):

| Transition | Open this file |
|------------|---------------|
| Research → Plan | `.copilot-tracking/research/<topic>-research.md` |
| Plan → Implement | `.copilot-tracking/plans/<topic>-plan.instructions.md` |
| Implement → Review | `.copilot-tracking/changes/<topic>-changes.md` |
| Review → rework | `.copilot-tracking/reviews/<topic>-review.md` |

## Context Commands

| Command | Effect | Use when |
|---------|--------|----------|
| `/clear` | Wipes conversation history | Between phases; switching tasks; agent behaving badly |
| `/compact` | Summarizes history (lossy) | Mid-phase, conversation growing long |
| `/checkpoint` | Persists state to disk via memory agent | Between sessions |
| New chat | Fresh session | Starting unrelated work |

**Degradation symptoms** (any one → `/clear`): ① skips phases ② ignores its own instructions ③ shallow output, missed edge cases ④ echoes the previous task's structure.

## Strict RPI vs rpi-agent

| Factor | Strict RPI (4 agents + `/clear`) | rpi-agent (one session) |
|--------|----------------------------------|-------------------------|
| Research depth | Deep, cited | Moderate, inline |
| Context contamination | Eliminated | Possible |
| Audit trail | Complete artifacts | Summary only |
| Best for | Complex, unfamiliar, team work | Simple, familiar, solo work |

Start with rpi-agent for small clear-scope work; **escalate to strict RPI** when hidden complexity appears. rpi-agent requires the `runSubagent` tool — if unavailable, use strict RPI.

## The Four Artifact Types

| Type | File | Activates | Answers |
|------|------|-----------|---------|
| **Prompt** | `*.prompt.md` | You type `/name` | "What does the user want?" |
| **Agent** | `*.agent.md` | Picker or prompt delegation | "How should this execute?" |
| **Instruction** | `*.instructions.md` | Automatic via `applyTo:` glob | "What standards apply here?" |
| **Skill** | `SKILL.md` + scripts | Description match or by name | "What utility does this need?" |

Delegation chain: **User → Prompt → Agent → Instructions + Skills**

## Daily-Driver Prompts

`/git-commit` · `/pull-request` · `/pr-review` · `/checkpoint` — plus backlog families: `/github-*`, `/ado-*`, `/jira-*`

## Setup Essentials

- Install: VS Code Marketplace → **HVE Core** (`ise-hve-essentials.hve-core`); everything: **HVE Core All**
- Verify: Copilot Chat → type `@` → see `task-researcher` etc.
- **Always**: add `.copilot-tracking/` to `.gitignore`
- CLI: `copilot plugin install hve-core@hve-core` (caution: instructions not auto-applied on CLI)
