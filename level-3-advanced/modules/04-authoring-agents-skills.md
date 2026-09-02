# Module 3.4 — Authoring Agents & Skills

**Time:** 45 min · **Roles:** engineers, tech leads · **Source docs:** [`docs/customization/custom-agents.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/custom-agents.md), [`docs/customization/skills.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/skills.md), [`docs/contributing/ai-artifacts-common.md`](https://github.com/microsoft/hve-core/blob/main/docs/contributing/ai-artifacts-common.md)

## Objectives

- Author a custom agent: persona, `tools:`, `handoffs:`, subagent `agents:` — and know when an agent is warranted at all.
- Author a skill: `SKILL.md`, activation-driving description, cross-platform script pairs, `references/` siblings.
- Use prompt-builder as the meta-tool that generates, grades, and refines all of the above.

## 1. Agents: When and How

**First question: do you actually need one?** A prompt with `agent:` delegation covers most "reusable workflow" needs. Author a custom agent only when you need behavior a prompt can't carry:

| Need | Artifact |
|------|----------|
| One-shot task, existing behavior fits | Prompt (maybe with `agent:` delegation) |
| Multi-turn persona with its own protocol, phases, or interview style | Agent |
| Tool restrictions (e.g., a reviewer that must be read-only) | Agent (`tools:`) |
| Structured transitions to other agents mid-workflow | Agent (`handoffs:`) |
| Delegating subtasks with isolated context | Agent (`agents:` subagents) |

Also check the quality bar in [`ai-artifacts-common.md`](https://github.com/microsoft/hve-core/blob/main/docs/contributing/ai-artifacts-common.md) before building: duplicate research/indexing/planning/implementation agents are explicitly not accepted upstream — if task-researcher plus an instruction file covers it, don't fork the wheel. The same test applies inside your own repo.

**The anatomy.** An `.agent.md` is frontmatter plus a protocol body. Minimum viable frontmatter is `name` and `description` (with an attribution suffix by convention):

```yaml
---
name: Contoso Code Reviewer
description: "Reviews changes for Contoso's TypeScript API standards - Brought to you by contoso/engineering"
tools:
  - read
  - search
handoffs:
  - label: "📋 Create Plan"
    agent: Task Planner
    prompt: /task-plan
    send: true
---
```

- **`tools:`** — omit for full access; list to restrict. Values accept individual tools (`read_file`, `grep_search`), categories (`read`, `search`, `edit`, `web`, `agent`), category-specific (`execute/runInTerminal`), and wildcards (`github/*`). Read-only agents should omit edit/terminal tools entirely — that's an enforced constraint, unlike prose saying "please don't edit."
- **`handoffs:`** — structured transitions: a `label` the user sees as a button, the target `agent`, an optional `prompt`, and `send: true` to fire it automatically. Open [`task-researcher.agent.md`](https://github.com/microsoft/hve-core/blob/main/.github/agents/hve-core/task-researcher.agent.md) — its "📋 Create Plan" handoff is exactly this field; you've been clicking authored `handoffs:` entries since Level 1.
- **`agents:`** — subagent dependencies by human-readable name. Subagents get `user-invocable: false` so they stay out of the picker; pure orchestrators set `disable-model-invocation: true`. Subagents cannot spawn their own subagents — one level of delegation only.
- **The body is the persona and protocol:** purpose, phases (conversational) or numbered steps (autonomous), and response format. Everything you've observed in HVE agents — the researcher's "research-only" discipline, the planners' 3–5-questions-per-turn — is prose in this body plus `tools:` enforcement.

## 2. Skills: Packaged Knowledge Plus Executables

A skill is a directory under `.github/skills/{collection}/{skill-name}/` whose `SKILL.md` carries a `name` that is **kebab-case and matches the directory name exactly**, plus a description written to be matched against:

```yaml
---
name: api-review
description: >-
  Reviews API designs against Contoso's REST conventions, validates
  OpenAPI specifications, and checks for breaking changes. Use when
  reviewing or designing HTTP APIs.
---
```

- **The description is the activation trigger.** Copilot reads it to decide whether the skill applies to the current request — write it in the vocabulary users actually use, and follow the "Use when…" convention you saw in [`rpi-research/SKILL.md`](https://github.com/microsoft/hve-core/blob/main/.github/skills/rpi/rpi-research/SKILL.md). A perfect skill with a vague description never loads.
- **Progressive disclosure** keeps context lean: metadata (~100 tokens) is read first; the SKILL.md body (<5000 tokens) loads only if relevant; files in `references/` and `scripts/` load only when the body points at them. Put the protocol in SKILL.md; push schemas, checklists, and long reference material into `references/` siblings (split anything over ~2000 tokens).
- **Scripts ship in cross-platform pairs** — `scripts/check.sh` and `scripts/check.ps1` with the same behavior — because your teammates run different operating systems and the skill must work for all of them. This is the instructions-vs-skills line from Level 1 made concrete: instructions describe, skills *execute*.

## 3. Prompt-Builder: The Meta-Tool

You now author four artifact types; prompt-builder is the agent that authors, grades, and refactors all of them:

| Command | Use |
|---------|-----|
| `/prompt-build files=<references> promptFiles=<targets>` | Create or improve artifacts — `files` are style/convention references, `promptFiles` are what gets written |
| `/prompt-analyze promptFiles=<targets>` | Structured quality report: purpose, issues by severity, overall assessment |
| `/prompt-refactor promptFiles=<glob> requirements="..."` | Consolidate or restructure related artifacts |

It runs a dual-persona loop — drafting, then auto-testing with a Prompt Tester persona in a `.copilot-tracking/sandbox/` environment, up to three iterations. The high-leverage habit: `/prompt-analyze` anything you authored by hand in Lab 3B, then `/prompt-build` to apply the fixes. Analyze-then-build beats regenerate-and-hope.

## Use Case Spotlight

A platform team is drowning in inconsistent Terraform module reviews. They build one read-only agent (`tools: [read, search]`) whose protocol checks module structure against their ADRs, plus one skill, `tf-module-validate`, whose `.sh`/`.ps1` pair runs their structural lint and whose `references/` folder holds the module contract. The agent's `handoffs:` entry hands violations to Task Planner for remediation planning. Review time drops because the agent can't "helpfully fix" anything — the tool restriction makes it structurally incapable of the failure mode a prose instruction could only discourage.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Skills execute real scripts on real machines — HVE adds no sandbox or safety layer around them | Review skill scripts like any code you'd run; human review gates stay mandatory ([ref #15](../../resources/limitations-and-workarounds.md)) |
| An authored agent for an uncovered stack is only as good as the standards you encode | Pair the agent with an instruction file for the stack — coverage gaps are yours to close ([ref #16](../../resources/limitations-and-workarounds.md)) |
| Custom reviewer agents inherit the assistive-only boundary — pattern-matchers, not gates | Position authored reviewers as pre-review passes, never as replacements for CI checks or human review ([ref #19](../../resources/limitations-and-workarounds.md)) |
| Subagent delegation requires a subagent tool (`runSubagent`/`task`) in the host | Check availability before designing an orchestrator; provide an inline fallback path ([ref #4](../../resources/limitations-and-workarounds.md)) |

## Knowledge Check

1. Give two concrete signals that a task needs a custom agent rather than a prompt with `agent:` delegation.
2. What are the four fields in a `handoffs:` entry, and what does `send: true` do?
3. Why should a review-only agent's `tools:` list omit edit and terminal tools, when its body already says "never modify code"?
4. State the two naming/matching rules for a skill's frontmatter `name`, and explain what the `description` field controls.
5. Why do skill scripts ship as `.sh`/`.ps1` pairs, and what belongs in `references/` instead of the SKILL.md body?

*(Answers: [instructor guide](../instructor-guide.md#module-4-answer-key))*

## Further Reading

- [Creating Custom Agents](https://github.com/microsoft/hve-core/blob/main/docs/customization/custom-agents.md) — full frontmatter reference, subagent patterns, mode-based workflows
- [Authoring Custom Skills](https://github.com/microsoft/hve-core/blob/main/docs/customization/skills.md) — progressive disclosure, reference organization
- [AI Artifacts Common Standards](https://github.com/microsoft/hve-core/blob/main/docs/contributing/ai-artifacts-common.md) — the quality bar and what's not accepted
- [AI Artifacts Architecture](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md) — the frontmatter contracts across all four types
