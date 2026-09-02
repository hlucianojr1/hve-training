# Module 3.1 — The Lifecycle & the Planning Chain

**Time:** 50 min · **Roles:** all (this is where PM and engineer workflows connect) · **Source docs:** [`docs/hve-guide/lifecycle/README.md`](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/lifecycle/README.md), [`docs/agents/project-planning/brd-prd-builders.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/project-planning/brd-prd-builders.md), [`docs/agents/README.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/README.md)

## Objectives

- Name the nine lifecycle stages and place the agent families on them.
- Locate RPI precisely: it's the engine of stages 6–7, not the whole framework.
- Trace the planning chain — BRD → PRD → ADR → work items → RPI — including where each handoff artifact lives on disk.

## 1. The 9-Stage Lifecycle

Everything you've done in Levels 1–2 lives in two of nine stages. Here's the whole map:

| Stage | Name | Key tools | Who drives |
|-------|------|-----------|------------|
| 1 | Setup | hve-core-installer (skill), memory | Everyone, once |
| 2 | Discovery | task-researcher, **brd-builder**, security-planner, dt-coach, sssc-planner, rai-planner | PM + tech lead |
| 3 | Product Definition | **prd-builder**, product-manager-advisor, **adr-creation**, architecture-diagrams skill | PM + architect |
| 4 | Decomposition | ado-prd-to-wit, **github-backlog-manager** | TPM |
| 5 | Sprint Planning | github-backlog-manager, agile-coach | TPM |
| 6 | Implementation | task-researcher, task-planner, task-implementor, rpi-agent, prompt-builder, coding-standards | Engineers |
| 7 | Review | task-reviewer, code-review | Engineers + leads |
| 8 | Delivery | pull-request, git-commit, git-merge, ado-get-build-info | Engineers |
| 9 | Operations | documentation, incident-response | SRE / leads |

Three loops matter: Review → Implementation (rework), Delivery → Implementation (next sprint), and Operations → Discovery (next iteration). The lifecycle is a cycle, not a waterfall — same argument you learned about RPI phases in Level 1, one level up.

**RPI is stages 6–7.** The four-phase workflow you've mastered is the *implementation engine*. Stages 2–5 are the upstream planning chain that decides what's worth implementing. If your team's complaint is "the AI built the wrong thing well," the fix is upstream, not in RPI.

## 2. The Planning Chain

The chain is a sequence of agents, each producing an artifact the next one consumes — the same artifact-carries-context principle you know from RPI, applied to planning:

```text
brd-builder → BRD → prd-builder → PRD → adr-creation → ADR(s)
    → github-backlog-manager / ado-prd-to-wit → work items → RPI cycle per item
```

| Link | Agent | Produces | Where |
|------|-------|----------|-------|
| Business case | **brd-builder** | BRD (business objectives, stakeholder scope) | `docs/project-planning/<name>-brd.md`, session state in `.copilot-tracking/brd-sessions/` |
| Product spec | **prd-builder** | PRD (measurable requirements, acceptance criteria) | `docs/project-planning/<name>.md`, session state in `.copilot-tracking/prd-sessions/` |
| Key decisions | **adr-creation** | ADRs via Socratic coaching | draft in `.copilot-tracking/adrs/`, final in `docs/decisions/` |
| Decomposition | **github-backlog-manager** (or **ado-prd-to-wit** / **jira-prd-to-wit**) | Work-item hierarchy (Epics → Features → Stories) | planning files under `.copilot-tracking/` |
| Execution | RPI agents | Working code + the artifacts you know | `.copilot-tracking/{research,plans,changes,reviews}/` |

Details worth knowing before Lab 3A:

- **Both builders are guided Q&A agents**, driven by the `requirements-author` skill and its canonical templates. They build sections iteratively as your answers accumulate and track progress in a session state file — you can pause mid-BRD today and resume next week; the agent detects the session file and picks up where you stopped. Don't dump all requirements up front; the questioning *is* the method.
- **The PRD Builder runs a seven-phase lifecycle** (Assess → Discover → Create → Build → Integrate → Validate → Finalize). During Validate it dispatches a **PRD Quality Reviewer** subagent that emits structured findings and a quality report which gates Validate exit — a built-in quality gate you don't invoke yourself.
- **Output modes** (`summary`, `section [name]`, `full`, `diff`) let you review a long document mid-session without scrolling chat.
- **TPMs can skip the PRD** when the BRD is sufficient (lifecycle transition rule: Stage 2 → Stage 4 directly). Skipping the BRD and starting at PRD also works for pure product work.
- **Decomposition agents are planning-only by design**: `ado-prd-to-wit` and `jira-prd-to-wit` never create real work items — they produce a handoff file. The github-backlog-manager *can* post to real trackers, governed by three autonomy tiers (Full / Partial / Manual); Lab 3A pins it to planning mode.

**Instructor walkthrough (10 min) — a real ADR as specimen:** open one ADR from [`docs/planning/adrs/`](https://github.com/microsoft/hve-core/blob/main/docs/planning/adrs) (e.g., `0003-generalize-requirements-author-skill-for-brd-and-prd.md` — an ADR *about* the planning chain itself). Trace: context → decision → consequences, and note it records a decision the PRD couldn't carry — that's the division of labor: PRD says *what*, ADR says *why this way*.

## 3. Why the Chain Beats a Mega-Prompt

You could paste a feature idea into chat and ask for "requirements, architecture, and tasks." You'd get plausible text. The chain gets you:

- **Separation of concerns with gates** — the BRD is deliberately solution-agnostic; the PRD forces measurable acceptance criteria; the ADR forces alternatives-considered. Each gate catches a different failure mode.
- **Cross-referencing against your codebase** — builders integrate references and flag conflicts between requirements and what actually exists (the same verified-truth argument from Level 1).
- **Resumable, reviewable artifacts** — stakeholders review a document in `docs/project-planning/`, not a chat scrollback.

> **Role framing:** this module is the seam between roles. PMs own links 1–2, architects link 3, TPMs link 4, engineers link 5. The handoff artifact is the contract at every seam — which is why Lab 3A makes each of you run the *whole* chain once, whatever your role.

## Use Case Spotlight

A tech lead inherits a "build a notification service" ask with no requirements. One brd-builder session (45 min of Q&A with the sponsor in the room) surfaces that the actual business driver is SLA-breach alerting for three enterprise accounts — not a general notification platform. The PRD scopes to one channel; the ADR records why polling beat webhooks for v1; decomposition yields six stories; the first RPI cycle ships the walking skeleton that week. The chain's value wasn't speed of writing — it was killing the wrong project scope before code existed.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Planning sessions run long — a real BRD or PRD is hours of Q&A, not minutes | Sessions are resumable via state files in `.copilot-tracking/{brd,prd}-sessions/`; timebox per sitting and resume. Same expectation-setting as long research runs ([ref #5](../../resources/limitations-and-workarounds.md)) |
| Requirements documents are decision-shaping output with no safety layer | Stakeholder and human review gates stay mandatory — treat generated BRDs/PRDs as drafts ([ref #15](../../resources/limitations-and-workarounds.md)) |
| Combining BRD and PRD creation in one session corrupts both | Separate conversations per document; `/clear` between links of the chain — documented pitfall in [`brd-prd-builders.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/project-planning/brd-prd-builders.md) |
| Generic-feeling template sections | Feed domain specifics during the build phase; let the Integrate phase cross-reference your codebase |

## Knowledge Check

1. Name the nine lifecycle stages in order, and say which two RPI occupies.
2. Which agent produces each link of the planning chain, and where does each handoff artifact land on disk?
3. A TPM has a solid BRD and no time for a PRD. Is skipping to decomposition legitimate? Why?
4. What is the PRD Quality Reviewer, and who invokes it?
5. Why must BRD and PRD creation run in separate chat sessions, and what carries the context between them?

*(Answers: [instructor guide](../instructor-guide.md#module-1-answer-key))*

## Further Reading

- [AI-Assisted Project Lifecycle](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/lifecycle/README.md) — all nine stage guides with per-stage tooling
- [BRD & PRD Builders](https://github.com/microsoft/hve-core/blob/main/docs/agents/project-planning/brd-prd-builders.md) — lifecycles, output modes, pitfalls
- [Role Guides](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/README.md) — which stages matter most for your role
