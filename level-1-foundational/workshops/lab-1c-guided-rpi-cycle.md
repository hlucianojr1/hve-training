# Lab 1C — Guided RPI Cycle

**Time:** 60–75 min · **Self-runnable:** yes (started in-session during Module 4)
**Goal:** complete your first full Research → Plan → Implement cycle on a small real task in your own repo, with `/clear` between phases, and read every artifact produced.

## Prerequisites

- Labs 1A and 1B completed
- Your experiment repo open, working tree clean (`git status`) so you can see and revert everything the cycle changes

## Pick Your Task

A **small, real, self-contained improvement**. The official tutorial builds a validation script — match that scale:

- A small utility or validation script (check that every X has a Y)
- A linter/formatter rule wired into your build
- A doc generator or README consistency checker
- A config validator

Selection criteria:

- Touches 1–3 files; implementable in under 15 minutes by hand
- Has at least one genuine unknown (a convention to discover, an integration point) — otherwise research has nothing to do
- Zero risk: not on a critical path, easy to `git checkout .` away

**PM/TPM variant:** if you don't write code, run the same cycle on a documentation task — e.g., "create a CONTRIBUTING section describing how this repo's issues are triaged." Research discovers the actual practice; the plan structures the doc; implementation writes it. The methodology is identical.

## Steps

### Phase 1 — Research

1. Select **Task Researcher** (or `/task-research`). Send your task with scope bullets:

   ```text
   Research what's needed to [your task] in this repository.

   Consider:
   * Existing patterns for [similar things] in this repo
   * Naming and structural conventions I should follow
   * How this should hook into [build/docs/config]
   * Expected output/behavior
   ```

2. When it finishes, **open and read** `.copilot-tracking/research/<...>-research.md`. Extract 3–5 key findings (conventions, file locations, patterns) — jot them down; you'll feed them to the planner.

### Phase 2 — Plan

3. Type **`/clear`**.
4. Open the research document in the editor (this is the context restore — leave it visible).
5. Select **Task Planner** (or `/task-plan`). Prompt with requirements *drawn from your research findings*:

   ```text
   Create an implementation plan to [your task].

   Requirements from research:
   * [finding 1 — e.g., script location convention]
   * [finding 2 — e.g., naming convention]
   * [finding 3 — e.g., integration point]
   * [success criteria — e.g., exit codes, expected output]
   ```

6. **Open and read** the plan in `.copilot-tracking/plans/`. Check: does each step trace to a research finding? Are there success criteria you could verify? If it invented requirements you never stated, note that — plans drift too.

### Phase 3 — Implement

7. Type **`/clear`**. Open the plan file in the editor.
8. Select **Task Implementor** (or `/task-implement`). Prompt: `Implement this plan.` and paste or reference the plan.
9. **Approve each file change deliberately** — read the diffs, don't rubber-stamp. This human gate is part of the methodology, not friction.
10. When done, **open and read** the changes log in `.copilot-tracking/changes/`.

### Verify

11. Run the thing you built (script, rule, generator). Test both the success path and a failure path (e.g., temporarily break the condition it checks, confirm it catches it, restore).

## Verify Your Work

- [ ] Three artifacts exist: research doc, plan, changes log — and you read all three
- [ ] You ran `/clear` twice (between R→P and P→I) and restored context via the artifact file
- [ ] The implementation follows a convention research discovered (name it)
- [ ] The change works, including the failure case
- [ ] Bonus reflection: what would "just ask Copilot to write it" have gotten wrong? (Usually: the convention.)

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Planner asks for research / complains none exists | Good — that's the built-in gate. Make sure the research doc is open in the editor or referenced by path |
| Agent skips ahead (plans during research, implements during planning) | Context degradation or wrong agent selected — `/clear`, reselect, retry |
| Implementation ignores the plan | Reopen the plan file first, then re-prompt: "Follow the plan at [path] exactly; do not improvise" |
| The built thing doesn't work | Normal — iterate: small fixes with the implementor, or back to planning if the approach was wrong. Iteration *is* the workflow |

## What You Learned

- The full phase mechanics: agent → prompt → artifact → read → `/clear` → restore → next.
- Plans act as contracts; research findings are what make them non-fictional.
- Reading artifacts is where you catch drift — every phase, every time.

## Stretch Goal

Run **`/task-review`** (Task Reviewer) as a fourth phase after `/clear`: it validates the implementation against your research and plan and writes a findings log to `.copilot-tracking/reviews/`. Level 2 makes Review a first-class habit; you'll be a step ahead.
