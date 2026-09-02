# Module 2.1 — Strict RPI Mastery

**Time:** 45 min · **Roles:** all · **Source docs:** [`docs/rpi/using-together.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md), [`docs/rpi/task-reviewer.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/task-reviewer.md), [`docs/rpi/why-rpi.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md)

## Objectives

- Run the full four-phase cycle with **Review as a first-class phase**, not a stretch goal.
- Sharpen the craft of each phase: scoping research, treating plans as contracts, using human approval gates deliberately, and letting review findings drive iteration.
- Know both transition surfaces — handoff buttons and typed commands — and when each is the right tool.

## 1. Review Joins the Cycle

Level 1 taught R → P → I. The real cycle is R → P → I → **Review**, and it loops:

```text
Research → Plan → Implement → Review ──→ Complete: commit
                     ↑    ↑                 │
                     └────┴── findings ─────┘
```

Task Reviewer validates the implementation against what was *documented* — research and plan — not against assumptions. It extracts a checklist from your artifacts, verifies each item with evidence from the codebase, runs validation commands (lint, build, test), and writes a review log to `.copilot-tracking/reviews/` with findings at three severities:

| Severity | Means | Before committing |
|----------|-------|-------------------|
| Critical | Incorrect or missing required functionality | Must fix |
| Major | Deviates from specifications or conventions | Must fix |
| Minor | Style, docs gaps, optimization opportunities | Optional; batch later |

Because the reviewer can only check what was documented, gaps in your research or plan become *visible* here. That feedback is the point: it improves your next cycle's research prompts and plan reviews.

## 2. Per-Phase Craft

You know the mechanics. Level 2 is about doing each phase *well*.

**Research — scope is your main lever.** Broad research runs 20–60 minutes and produces sprawl; scoped research runs minutes and produces answers. Good prompts list specific questions, name existing patterns to match, and state constraints. Bad prompts describe the feature and hope. (Full guidance: [Task Researcher — Tips](https://github.com/microsoft/hve-core/blob/main/docs/rpi/task-researcher.md).)

**Plan — read it as a contract.** The plan is what implementation will do *instead of* improvising. So review it like a contract: are phases in logical order? Does every task have success criteria you could verify? Does each detail trace to a research line reference (`Plan → Details (Lines X–Y) → Research (Lines A–B)`)? If the plan invents requirements you never stated, fix it *now* — it's cheaper than fixing code.

**Implement — approval gates are a dial, not a formality.** Task Implementor pauses via stop controls: `phaseStop=true` (default) pauses after each phase; `taskStop=true` after each task; both false runs to completion. Set the dial to the risk: unfamiliar territory → task stops; routine work following a solid plan → phase stops. At every stop: read the diff, run linters, then continue. Rubber-stamping here defeats the whole methodology.

**Review — findings drive iteration.** The review status tells you where to go next ([iteration paths](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md)):

| Review outcome | Action | Next phase |
|----------------|--------|------------|
| Complete | Commit | Done |
| Needs Rework | `/clear`, open the review log, `/task-implement` — the implementor uses findings to guide fixes, then re-review | Implement |
| Research Gap | `/clear`, open the review log, `/task-research` the specific gap | Research |
| Plan Gap | `/clear`, open the review log, `/task-plan` to add the missing scope | Plan |

The pattern is always the same: clear, open the review log in the editor, invoke the target phase. The review log is the handoff artifact for rework, exactly as the research doc was for planning.

## 3. Handoff Buttons vs Typed Commands

RPI agents show **handoff buttons** when they finish (requires VS Code 1.106+):

| From | Button | Goes to |
|------|--------|---------|
| Task Researcher | 📋 Create Plan | Task Planner |
| Task Planner | ⚡ Implement | Task Implementor |
| Task Implementor | ✅ Review | Task Reviewer |
| Task Reviewer | 🔬 Research More | Task Researcher |
| Task Reviewer | 📋 Revise Plan | Task Planner |

Buttons pre-fill the prompt and carry conversation context over. Use typed `/clear` + `/task-*` instead when: you want a *complete* context reset, you need custom parameters for the next agent, or the button doesn't match your intended path (e.g., review said Needs Rework, buttons offer research/plan). Typed commands always work, on any VS Code version — they're the fallback and the precision tool.

The RPI Agent also shows a **💾 Save** button that writes session state to `.copilot-tracking/memory/` via the memory agent — Module 2 covers when that matters.

## 4. Follow-Along: Rework Drill (10 min)

Instructor shows a pre-baked review log with one Major finding, then runs the rework loop live: `/clear` → open review log → `/task-implement` → watch the implementor target exactly the finding → `/task-review` again. Watch for: the implementor doesn't re-plan or wander — the review log scopes it.

## Use Case Spotlight

In the [Azure Blob Storage walkthrough](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md), the review phase returns Complete with 0 Critical, 0 Major, 2 Minor findings and 1 follow-up item ("add performance benchmarks — deferred from research"). Notice what that last item is: the reviewer caught something research *explicitly deferred* and turned it into tracked future work instead of a forgotten intention. That's the audit trail earning its keep.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Second request in one session skips phases | `/clear` between every phase and before rework loops ([ref #1](../../resources/limitations-and-workarounds.md)) |
| Handoff buttons require VS Code 1.106+ | Typed `/task-*` commands always work ([ref #10](../../resources/limitations-and-workarounds.md)) |
| Live research runs 20–60 min on broad questions | Scope research prompts to specific questions; run big research as a background task ([ref #5](../../resources/limitations-and-workarounds.md)) |

## Knowledge Check

1. What are the four possible review outcomes, and which phase does each send you to?
2. The rework loop always starts with the same three moves — what are they?
3. When should you set `taskStop=true` instead of the default `phaseStop=true`?
4. Handoff buttons carry conversation context forward. Name two situations where you should use `/clear` + typed commands instead.
5. Why does Task Reviewer make gaps in *research* visible, even though it runs after implementation?

*(Answers: [instructor guide](../instructor-guide.md#module-1-answer-key))*

## Further Reading

- [Using RPI Agents Together](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md) — the complete walkthrough including iteration loops and handoffs
- [Task Reviewer Guide](https://github.com/microsoft/hve-core/blob/main/docs/rpi/task-reviewer.md) — severity levels, scope parameters, next-step flows
