# Level 3 — Advanced

**Theme:** The full lifecycle + authoring your own artifacts.
**Audience:** Engineers and tech leads primarily. PMs/TPMs take Modules 1–2 plus Lab 3A (their capstone).
**Format:** 3.5-hour instructor session + ~3 hours self-paced labs.
**Prerequisite:** Level 2 exit criteria met — you run strict RPI unaided and can explain context degradation without notes.

## Objectives

By the end of Level 3, students can:

1. Place all agent families on the 9-stage AI-assisted project lifecycle and explain where RPI sits (stages 6–7) versus the upstream planning chain (stages 2–5).
2. Run the full planning chain: BRD → PRD → ADR → work-item decomposition → one RPI cycle, with clean artifact handoffs between every link.
3. Use the specialty planners (security, accessibility, RAI, SSSC) for assistive pre-review — and state precisely why they never replace SAST/DAST, pen tests, conformance audits, or human experts.
4. Author all four artifact types — an instruction, a prompt, a custom agent, and a skill — that load and work in Copilot Chat in their own repo.
5. Whiteboard the delegation chain (User → Prompt → Agent → Instructions + Skills) from memory, including the frontmatter fields that wire each link.

## Session Agenda (3.5 h)

| Time | Segment | Material |
|------|---------|----------|
| 0:00–0:10 | Welcome, Level 2 lab review (show your review logs) | [Instructor guide §1](instructor-guide.md) |
| 0:10–1:00 | **Module 1 — The Lifecycle & the Planning Chain** (ADR specimen walkthrough) | [modules/01-lifecycle-and-planning-chain.md](modules/01-lifecycle-and-planning-chain.md) |
| 1:00–1:40 | **Module 2 — Specialty Planners** | [modules/02-specialty-planners.md](modules/02-specialty-planners.md) |
| 1:40–1:50 | Break — PMs/TPMs may leave or stay for the authoring half | |
| 1:50–2:35 | **Module 3 — Authoring Instructions & Prompts** (live-author demo) | [modules/03-authoring-instructions-prompts.md](modules/03-authoring-instructions-prompts.md) |
| 2:35–2:40 | Break | |
| 2:40–3:25 | **Module 4 — Authoring Agents & Skills** | [modules/04-authoring-agents-skills.md](modules/04-authoring-agents-skills.md) |
| 3:25–3:30 | Knowledge-check review, lab briefing, Q&A | [Instructor guide §6](instructor-guide.md) |

## Self-Paced Labs (complete before Level 4)

| Lab | Time | Produces |
|-----|------|----------|
| [3A — The Planning Chain](workshops/lab-3a-planning-chain.md) (PM/TPM capstone) | 75 min | BRD, PRD, one ADR, planned work items, and one work item executed via RPI |
| [3B — Author an Artifact Set](workshops/lab-3b-author-artifact-set.md) | 75 min | One instruction, one prompt, one agent, one skill — all verified working in your repo. **Keep these: Level 4's collection lab packages them** |
| [3C — Specialty Review](workshops/lab-3c-specialty-review.md) | 45 min | One planner/reviewer pass (security, accessibility, or design thinking) with artifacts and an honest scope statement |

## Exit Criteria

You're ready for Level 4 when you can show:

- [ ] The **planning-chain artifact trail** from Lab 3A: a BRD and PRD in `docs/project-planning/`, one ADR, planned work items (planning files only — nothing posted without approval), and the RPI artifacts for one executed item
- [ ] Your **authored artifact set** from Lab 3B loads and works in Copilot Chat: the instruction fires on a matching file, the prompt appears under `/`, the agent appears in the agent picker, the skill activates on a description match
- [ ] At a whiteboard, unaided: draw the delegation chain with the frontmatter fields that wire it (`agent:`, `tools:`, `handoffs:`, `applyTo:`, skill `name`/`description`), and place the agent families you know on the 9-stage lifecycle

## Sources

[`docs/hve-guide/lifecycle/`](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/lifecycle/README.md) · [`docs/agents/README.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/README.md) · [`docs/agents/project-planning/brd-prd-builders.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/project-planning/brd-prd-builders.md) · [`docs/agents/security/`](https://github.com/microsoft/hve-core/blob/main/docs/agents/security/README.md) · [`docs/getting-started/cross-planner-integration.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/cross-planner-integration.md) · [`docs/customization/`](https://github.com/microsoft/hve-core/blob/main/docs/customization/README.md) · [`docs/architecture/ai-artifacts.md`](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md)
