# Module 1.4 — Your First RPI Workflow

**Time:** 45 min · **Roles:** all (PMs follow the demo and read artifacts; the lab has a PM variant) · **Source docs:** [`docs/getting-started/first-workflow.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/first-workflow.md), [`docs/rpi/context-engineering.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md)

## Objectives

- Watch and begin a complete Research → Plan → Implement cycle.
- Practice the phase mechanics: agent switch → prompt → artifact → `/clear` → next phase.
- Internalize the one rule: **artifacts carry context, chat history does not.**

## 1. The Mechanics of a Phase

Every RPI phase has the same shape:

1. **Select the agent** (picker) or invoke its prompt (`/task-research` auto-switches for you).
2. **Prompt it** with the task — including anything the previous phase's artifact established.
3. **It produces an artifact** in `.copilot-tracking/` — this is the deliverable, not the chat text.
4. **Read the artifact.** Non-negotiable. It's where quality is visible and where you catch drift.
5. **`/clear`**, then carry the artifact (open it in the editor, or reference its path) into the next phase.

Two equivalent invocation surfaces (both on the cheat sheet): prompt entries `/task-research → /task-plan → /task-implement → /task-review` and skill entries `/rpi-research → /rpi-plan → /rpi-implement → /rpi-review`. This course uses `/task-*` while learning because the agent switch makes phases visible.

## 2. Live Demo: the Validation-Script Task

> **Instructor:** this is the pre-baked demo (see [Handbook §2](../../instructor-handbook.md)). Start the research live for authenticity, then walk pre-baked artifacts. The demo task mirrors the official tutorial: a script validating that every `docs/` subfolder has a `README.md`.

Watch for these beats:

- **Research prompt** includes *what to consider* (existing script patterns, conventions, output format) — steering research scope is your main skill lever.
- **The research doc** cites file:line evidence. Instructor will point at 2–3 citations and open one to prove it's real.
- **`/clear` happens on screen** before planning. Then the research doc is *reopened in the editor* — that's the context restore.
- **The plan** breaks work into sequenced steps with success criteria — notice it cites the research, not vibes.
- **Implementation** follows the plan mechanically and logs to a changes file. Each edit is approved by a human.

## 3. Follow-Along Start (last 15 min of session)

Students begin [Lab 1C](../workshops/lab-1c-guided-rpi-cycle.md) in-session: pick the small task, write the research prompt with the instructor circulating, and *launch* research. It runs a few minutes; finish the cycle as homework. This ordering exists because prompt-writing is where beginners stall — do it with help in the room.

## 4. Why `/clear` Is Not Optional

Preview of Level 2's deep dive, in one paragraph: model attention favors recent tokens. After one full cycle your conversation holds 50K+ tokens of implementation output competing against ~3K tokens of agent instructions — so a second request in the same session *pattern-matches to "keep implementing"* and skips research entirely. `/clear` resets the ratio; the artifact files survive because they live on disk, not in chat.

If your agent ever starts skipping phases or producing shallow output: that's not a bug to report, it's context degradation to clear.

## Use Case Spotlight

The demo task (a docs-validation script) is deliberately boring — a script, a convention to discover, an npm hook. That's the point: RPI's value shows even at small scale, and the same four beats scale up to "add Azure Blob Storage to this service" ([the full worked example](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md)) without changing shape.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Research runs long (2–5 min on a scoped question; 20–60 min on a big one) | Scope research questions tightly; treat long research as a background task ([ref #5](../../resources/limitations-and-workarounds.md)) |
| Second `/rpi` in one session skips phases | `/clear` before every new request — the course's one rule ([ref #1](../../resources/limitations-and-workarounds.md)) |
| First research docs feel verbose | Intentional — they're reference material for planning, not prose; scoping improves with practice |

## Knowledge Check

1. What are the five steps of any RPI phase, in order?
2. After `/clear`, the chat history is gone. How does the planner get the research findings?
3. What's the difference between `/task-research` and `/rpi-research`?
4. Why does a second `/rpi` request in the same session tend to skip straight to implementation?
5. What should you do *every time* a phase completes, before moving on?

*(Answers: [instructor guide](../instructor-guide.md#module-4-answer-key))*

## Further Reading

- [Your First Full Workflow](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/first-workflow.md) — the official 15-minute tutorial this module is built on
- [Using the Agents Together](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md) — the Azure Blob Storage end-to-end walkthrough with handoffs and iteration loops
