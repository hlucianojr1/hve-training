# Level 2 — Intermediate

**Theme:** Operating HVE well, every day.
**Audience:** All roles. Role tracks (engineer / PM-TPM / tech lead) begin in Module 4 and continue in Lab 2C.
**Format:** 3.5-hour instructor session + ~3 hours self-paced labs.
**Prerequisite:** Level 1 exit criteria met — bring your `.copilot-tracking/` folder from Lab 1C.

## Objectives

By the end of Level 2, students can:

1. Run a full Research → Plan → Implement → **Review** cycle unaided, with review findings driving iteration back to earlier phases.
2. Explain *why* context degrades (recency bias mechanics), recognize its four symptoms, and apply the right fix (`/clear`, `/compact`, `/checkpoint`, or new chat) for each situation.
3. Choose deliberately between strict RPI and rpi-agent using the decision matrix, and escalate mid-task when hidden complexity appears.
4. Use their role's daily-driver agents and prompts: delivery prompts (engineers), backlog pipelines (PMs/TPMs), or ADR and code-review workflows (tech leads).

## Session Agenda (3.5 h)

| Time | Segment | Material |
|------|---------|----------|
| 0:00–0:10 | Welcome, Level 1 lab review (show your artifacts) | [Instructor guide §1](instructor-guide.md) |
| 0:10–0:55 | **Module 1 — Strict RPI Mastery** (Review phase becomes first-class) | [modules/01-strict-rpi-mastery.md](modules/01-strict-rpi-mastery.md) |
| 0:55–1:45 | **Module 2 — Context Engineering** (live degradation demo) | [modules/02-context-engineering.md](modules/02-context-engineering.md) |
| 1:45–1:55 | Break | |
| 1:55–2:30 | **Module 3 — rpi-agent & Mode Selection** | [modules/03-rpi-agent-and-mode-selection.md](modules/03-rpi-agent-and-mode-selection.md) |
| 2:30–2:35 | Break, split into role-track groups | |
| 2:35–3:20 | **Module 4 — Role-Track Daily Drivers** (breakouts) | [modules/04-role-track-daily-drivers.md](modules/04-role-track-daily-drivers.md) |
| 3:20–3:30 | Knowledge-check review, lab briefing, Q&A | [Instructor guide §6](instructor-guide.md) |

## Self-Paced Labs (complete before Level 3)

| Lab | Time | Produces |
|-----|------|----------|
| [2A — Strict RPI on Your Own Repo](workshops/lab-2a-strict-rpi-own-repo.md) | 75 min | Unaided full cycle: research, plan, changes, and review artifacts on a 3–5 file task |
| [2B — Break It on Purpose](workshops/lab-2b-break-it-on-purpose.md) | 45 min | Firsthand experience of the planner's research gate, context degradation, and recovery |
| [2C — Role-Track Lab](workshops/lab-2c-role-track-lab.md) | 60 min | Your role's real deliverable: PR-ready change / sprint-plan draft / ADR + code review |

## Exit Criteria

You're ready for Level 3 when you can show:

- [ ] A complete **unaided** strict RPI cycle — including the Review phase — on your own repo, with all four artifacts in `.copilot-tracking/`: research doc, plan (+ details), changes log, and review log (Lab 2A)
- [ ] You can name the **four context-degradation symptoms** and the fix for each, without notes
- [ ] Given a task description, you can justify choosing strict RPI or rpi-agent using the [decision matrix](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md) — and say what would make you escalate

## Sources

[`docs/rpi/context-engineering.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md) · [`docs/rpi/why-rpi.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md) · [`docs/rpi/using-together.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md) · [`docs/rpi/task-reviewer.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/task-reviewer.md) · [`docs/hve-guide/roles/`](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/roles/engineer.md) · [`docs/agents/github-backlog/using-together.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/github-backlog/using-together.md)
