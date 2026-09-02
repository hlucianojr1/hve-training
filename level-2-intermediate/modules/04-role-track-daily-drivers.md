# Module 2.4 — Role-Track Daily Drivers

**Time:** 45 min (delivered as parallel breakouts; read your track, skim the others) · **Roles:** split — engineer / PM-TPM / tech lead · **Source docs:** [`docs/hve-guide/roles/engineer.md`](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/roles/engineer.md), [`docs/hve-guide/roles/tpm.md`](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/roles/tpm.md), [`docs/hve-guide/roles/tech-lead.md`](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/roles/tech-lead.md)

## Objectives

- Learn the agents and prompts your role reaches for daily, beyond the RPI core.
- Run your track's flow once in-session; Lab 2C makes it real.
- Understand what the *other* tracks produce, because their artifacts land on your desk.

> **Everyone reads this box.** The three tracks aren't silos — they're one pipeline. TPM planning artifacts (work items, sprint plans) become engineers' RPI research inputs. Engineers' PRs become tech leads' code-review targets. Tech leads' ADRs and curated instructions become constraints in everyone's research and review phases. The mixed-role notes at the end of each track say what your output means to the others.

---

## Track A — Engineer

Your daily loop wraps strict RPI with delivery prompts, and coding standards apply themselves.

**Coding standards auto-apply.** Instruction files activate automatically by file type — C# (`*.cs`), Python (`*.py`), Bash (`*.sh`), Bicep (`bicep/**`), Terraform (`*.tf`), GitHub Actions workflows (`*.yml`). You do nothing; the standards ride along in every chat that touches a matching file. Don't override them manually — if a standard is wrong for your team, that's a Level 3 authoring conversation, not an ad-hoc skip. (Caution: this auto-apply is a VS Code extension behavior; CLI plugins don't do it.)

**Delivery prompts.** After a cycle's review comes back Complete:

| Prompt | Does | Notes |
|--------|------|-------|
| `/git-commit` | Stages changes and writes a Conventional Commit | Read the message before confirming; it's a draft |
| `/pull-request` | Creates the PR with a structured description | Feed it your changes log — the RPI artifacts write most of the description for you |
| `/pr-review` | Review assistance on an existing PR | Assistive pre-review, not a replacement for human review |
| `/git-merge` | Merge/rebase workflows with conflict handling | Including `rebase --onto` cases |

The full loop: RPI cycle → review Complete → `/git-commit` → `/pull-request` → teammate (or tech lead's code-review agent) reviews.

**Mixed-role note:** your changes log and review log are what make your PR reviewable-by-artifact. A tech lead running the code-review agent, or a TPM checking sprint status, reads *those* — keep them intact until the PR merges.

---

## Track B — PM / TPM

Your daily driver is the backlog pipeline: **Discovery → Triage → Sprint Planning → Execution**, run through the **github-backlog-manager** agent (GitHub) or the ADO prompt family (Azure DevOps). Same discipline as RPI: each stage produces a planning file, the next stage consumes it, and you `/clear` between stages — stale context from a previous workflow interferes with the current one's classification logic.

**GitHub pipeline** (prompts: `/github-discover-issues`, `/github-triage-issues`, `/github-sprint-plan`, `/github-execute-backlog`; plus `/github-add-issue` for one-offs):

| Stage | Input | Output file |
|-------|-------|-------------|
| Discovery | Repository scope | `.copilot-tracking/github-issues/discovery/<scope>/issue-analysis.md` |
| Triage | Discovery output | `triage/<YYYY-MM-DD>/triage-plan.md` |
| Sprint Planning | Triage output | `sprint/<milestone-kebab>/handoff.md` |
| Execution | Handoff files | `handoff-logs.md` (checkbox audit trail) |

**The human gate:** nothing touches GitHub until execution, and execution only applies *checked* operations in a handoff file you reviewed. Discovery/triage/planning are pure planning artifacts — review the triage plan, fix mislabeled items, uncheck what you don't want, *then* execute. Contributor-facing comments follow `community-interaction.instructions.md`, which loads automatically.

**ADO equivalents:** `/ado-get-my-work-items` → `/ado-process-my-work-items-for-task-planning` (the two-step work-item flow), plus `/ado-discover-work-items`, `/ado-triage-work-items`, `/ado-sprint-plan`, `/ado-update-wit-items`, and `/ado-create-pull-request`. For requirements upstream of the backlog: **brd-builder** → **prd-builder** → **ado-prd-to-wit** (planning-only; it never creates work items itself), with the **agile-coach** agent for story refinement and priority guidance.

**Mixed-role note:** your sprint handoff files and work items are the entry point of engineers' RPI cycles — a well-triaged issue with acceptance criteria becomes a research prompt almost verbatim. Write them like inputs, not just records.

---

## Track C — Tech Lead

You operate two quality levers — decisions and reviews — plus curation of the team's prompt artifacts.

**ADRs with adr-creation.** The **adr-creation** agent is a Socratic coach: Discovery → Research → Analysis → Documentation. It drafts in `.copilot-tracking/adrs/<topic>-draft.md` and finalizes to `docs/decisions/YYYY-MM-DD-<topic>.md`. It asks you questions rather than writing your opinions for you — bring a real pending decision, not a settled one, or the coaching has nothing to do. For broader design reviews (trade-offs across a system), **system-architecture-reviewer** complements it and delegates ADR authoring back to adr-creation.

**Code review with the code-review agent.** A **human-gated orchestrator**: it computes the diff, then *stops and confirms scope with you*; you choose any combination of five perspectives — `functional`, `standards`, `accessibility`, `security`, `pr` (or `full` for all five) — plus a depth tier (`basic`, `standard`, `comprehensive`). It dispatches one subagent per perspective and merges everything into `.copilot-tracking/reviews/code-reviews/<branch>/review.md`. Two constraints to respect: it **requires `runSubagent`** (fallback: `/pr-review` prompt or task-reviewer for plan-compliance checks), and it's review-only and *assistive* — it never modifies code and never replaces your human review or existing security gates.

**Curation with prompt-builder.** Your team's prompts, instructions, and agents are engineering artifacts, and you're their editor. The **prompt-builder** agent drafts and validates them with a built-in tester persona (up to 3 iterate cycles, sandboxed test logs); `/prompt-analyze` audits an existing artifact's quality before you touch it; `/prompt-refactor` restructures it. Level 3 teaches authoring from scratch — at Level 2, your job is knowing these exist so team standards live in versioned instruction files instead of tribal knowledge.

**Mixed-role note:** your ADRs become citable evidence in engineers' research phases, and your curated instruction files silently shape every review and implementation on the team. When engineers' review logs keep flagging the same convention issue, that's your signal to encode it as an instruction file.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| code-review (and other orchestrators) require `runSubagent` | Check availability; fall back to `/pr-review` or manual review ([ref #4](../../resources/limitations-and-workarounds.md)) |
| Mixing backlog workflow stages in one session gives unreliable results | `/clear` between pipeline stages — same recency-bias mechanics as RPI ([ref #1](../../resources/limitations-and-workarounds.md)) |
| Coding standards don't auto-apply on CLI plugins | Use the VS Code extension when standards enforcement matters ([ref #6](../../resources/limitations-and-workarounds.md)) |
| Review agents are assistive pattern-matchers — no code execution, no tests run | Keep them as a pre-review pass; human review gates stay mandatory ([ref #19](../../resources/limitations-and-workarounds.md)) |

## Knowledge Check

1. (Engineer) What triggers a coding-standards instruction file to apply, and what should you do if a standard is wrong for your team?
2. (PM/TPM) At which pipeline stage does anything actually change on GitHub, and what two things must happen first?
3. (Tech lead) The code-review agent is "human-gated" at two points — where, and what do you decide at each?
4. (All) Name one artifact your track produces and which other role consumes it, and how.
5. (All) The backlog pipeline and RPI both demand `/clear` between stages. What's the shared underlying reason?

*(Answers: [instructor guide](../instructor-guide.md#module-4-answer-key))*

## Further Reading

- [Engineer Guide](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/roles/engineer.md) · [TPM Guide](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/roles/tpm.md) · [Tech Lead Guide](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/roles/tech-lead.md)
- [GitHub Backlog — Using Workflows Together](https://github.com/microsoft/hve-core/blob/main/docs/agents/github-backlog/using-together.md) — the full pipeline walkthrough Lab 2C's PM track is built on
