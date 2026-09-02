# Module 1.3 — The Four Artifact Types & Collections

**Time:** 40 min · **Roles:** all · **Source docs:** [`docs/architecture/ai-artifacts.md`](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md), [`docs/getting-started/collections.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/collections.md)

## Objectives

- Distinguish the four artifact types and what question each answers.
- Trace the delegation chain: User → Prompt → Agent → Instructions + Skills.
- Understand collections as the distribution unit and maturity as the quality gate.

## 1. The Type System

Everything in HVE is one of four markdown-based artifact types:

| Type | File pattern | Activated by | Answers the question |
|------|-------------|--------------|---------------------|
| **Prompt** | `*.prompt.md` | You typing `/name` | "What does the user want to accomplish?" |
| **Agent** | `*.agent.md` | Agent picker, or a prompt's `agent:` frontmatter | "How should this task be executed?" |
| **Instruction** | `*.instructions.md` | **Automatically** — current file matches `applyTo:` glob | "What standards apply to this context?" |
| **Skill** | `SKILL.md` (+ scripts) | Description match or explicit name | "What executable utility does this need?" |

Two contrasts worth internalizing:

- **Instructions vs skills:** instructions are *passive reference* (standards injected into context); skills are *active execution* (real scripts, often `.sh` + `.ps1` pairs for cross-platform).
- **Prompts vs agents:** a prompt is an entry point that may delegate; an agent is the behavior itself, with declared `tools:`, subagent `agents:`, and `handoffs:`.

## 2. The Delegation Chain

```text
User request → Prompt → Agent → Instructions (auto-applied) + Skills (invoked)
```

**Instructor walkthrough (10 min) — trace one real chain on screen:**

1. Open [`.github/prompts/hve-core/task-research.prompt.md`](https://github.com/microsoft/hve-core/blob/main/.github/prompts/hve-core/task-research.prompt.md) — show the `agent:` frontmatter delegating to Task Researcher.
2. Open [`.github/agents/hve-core/task-researcher.agent.md`](https://github.com/microsoft/hve-core/blob/main/.github/agents/hve-core/task-researcher.agent.md) — show `tools:`, and `handoffs:` pointing onward to the planner.
3. Open [`.github/instructions/coding-standards/python-script.instructions.md`](https://github.com/microsoft/hve-core/blob/main/.github/instructions/coding-standards/python-script.instructions.md) (any language file works) — show `applyTo:` and explain it fires with **zero invocation** whenever a matching file is in play.
4. Open any skill, e.g. [`.github/skills/rpi/rpi-research/SKILL.md`](https://github.com/microsoft/hve-core/blob/main/.github/skills/rpi/rpi-research/SKILL.md) — show kebab-case `name` matching its directory and the "Use when…" description Copilot matches against.

**Follow-along (5 min):** open any file in your own repo in a language HVE covers (Python, C#, Terraform, Bash…), ask Copilot Chat for a small improvement, and observe standards-conscious output — that's an instruction firing invisibly.

## 3. Collections: How Artifacts Ship

Artifacts are bundled by **collection manifests** (`collections/*.collection.yml`) — the source of truth from which both the VS Code extensions and CLI plugins are built.

- `hve-core` (flagship): RPI workflow + git prompts, ~68 artifacts — what you installed
- `hve-core-all`: everything (~260 artifacts)
- Domain collections: `coding-standards`, `project-planning`, `security`, `design-thinking`, `ado`, `github`, `jira`, `data-science`, …

Each collection and each item carries a **maturity**: `experimental` → `preview` → `stable` (plus `deprecated`/`removed`). Stable channel ships only stable; the pre-release channel includes preview and experimental. That's why some agents mentioned in docs may not appear in your install — check the collection and channel before filing a bug.

> **PM/TL lens:** collections are the governance unit. "What does my team adopt?" is answered per-collection, not per-file — Level 4 builds on this.

## Use Case Spotlight

A team standardizing Terraform: they adopt just the `coding-standards` collection. From that moment every `.tf` file edit in Copilot follows the org conventions — no one invokes anything. That's the instructions tier doing systematic work that prompting can't: humans forget to ask; globs don't.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Instruction coverage is uneven (C#, Python, PowerShell, Rust, Bash, Bicep, Terraform strongest) | For other stacks, you author your own — Level 3 Module 3 teaches exactly this ([ref #16](../../resources/limitations-and-workarounds.md)) |
| Experimental/preview artifacts may be missing on the stable channel | Check maturity in the collection manifest before assuming breakage |
| Multiple instructions can stack on one file | Overlap is by design; specificity wins for conflicts — keep team instructions scoped |

## Knowledge Check

1. Which artifact type activates with no user action at all, and via what mechanism?
2. In one sentence each: what question do prompt, agent, instruction, and skill answer?
3. A colleague says "I installed HVE but can't find the design-thinking coach." What two things do you check?
4. Instructions and skills both extend agents — what's the fundamental difference?
5. What is the source of truth that both the extension and the CLI plugin are built from?

*(Answers: [instructor guide](../instructor-guide.md#module-3-answer-key))*

## Further Reading

- [AI Artifacts Architecture](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md) — the full spec including frontmatter contracts
- [Collections](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/collections.md) — every collection with counts and marketplace links
