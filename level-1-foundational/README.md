# Level 1 — Foundational

**Theme:** Why HVE exists, and your first real workflow.
**Audience:** All roles (engineers, tech leads, PMs/TPMs, designers). No HVE experience assumed.
**Format:** 3.5-hour instructor session + ~2 hours self-paced labs.

## Objectives

By the end of Level 1, students can:

1. Explain the core thesis: AI can't distinguish investigating from implementing, and constraining it to research-only makes it optimize for verified truth instead of plausible code.
2. Install and verify HVE Core, and know where every artifact type lives.
3. Name the four artifact types (prompt, agent, instruction, skill) and describe the delegation chain.
4. Complete a full guided Research → Plan → Implement cycle with `/clear` between phases.
5. State the one non-negotiable habit: artifacts carry context, chat history does not.

## Session Agenda (3.5 h)

| Time | Segment | Material |
|------|---------|----------|
| 0:00–0:15 | Welcome, setup verification, course map | [Instructor guide §1](instructor-guide.md) |
| 0:15–1:00 | **Module 1 — Why HVE** | [modules/01-why-hve.md](modules/01-why-hve.md) |
| 1:00–1:35 | **Module 2 — Setup & First Contact** (follow-along: memory agent) | [modules/02-setup-and-first-contact.md](modules/02-setup-and-first-contact.md) |
| 1:35–1:45 | Break | |
| 1:45–2:25 | **Module 3 — Artifact Types & Collections** | [modules/03-artifact-types-and-collections.md](modules/03-artifact-types-and-collections.md) |
| 2:25–2:35 | Break | |
| 2:35–3:20 | **Module 4 — Your First RPI Workflow** (live demo + follow-along start) | [modules/04-first-rpi-workflow.md](modules/04-first-rpi-workflow.md) |
| 3:20–3:30 | Knowledge-check review, lab briefing, Q&A | [Instructor guide §6](instructor-guide.md) |

## Self-Paced Labs (complete before Level 2)

| Lab | Time | Produces |
|-----|------|----------|
| [1A — Install & Verify](workshops/lab-1a-install-verify.md) (also sent as pre-work) | 20 min | Working HVE install, gitignore entry |
| [1B — First Research on Your Repo](workshops/lab-1b-first-research.md) | 30 min | A research document about *your* codebase |
| [1C — Guided RPI Cycle](workshops/lab-1c-guided-rpi-cycle.md) | 60–75 min | Research + plan + implemented change + all artifacts |

## Exit Criteria

You're ready for Level 2 when you can show:

- [ ] `.copilot-tracking/` in your repo containing at least one research doc, one plan, and one changes log from a completed cycle (Lab 1C)
- [ ] You can explain, in under 2 minutes and without notes, why the research agent is forbidden from writing code
- [ ] Given any task ("review this PR", "create an ADR"), you can find the relevant agent or prompt in [`.github/CUSTOM-AGENTS.md`](https://github.com/microsoft/hve-core/blob/main/.github/CUSTOM-AGENTS.md) or [`.github/prompts/README.md`](https://github.com/microsoft/hve-core/blob/main/.github/prompts/README.md)

## Sources

[`docs/rpi/why-rpi.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md) · [`docs/getting-started/`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/README.md) · [`docs/architecture/ai-artifacts.md`](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md) · [`TRANSPARENCY-NOTE.md`](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md)
