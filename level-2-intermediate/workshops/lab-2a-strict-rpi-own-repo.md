# Lab 2A — Strict RPI on Your Own Repo, Unaided

**Time:** 75 min · **Self-runnable:** yes
**Goal:** complete a full Research → Plan → Implement → **Review** cycle on your own repo without prompt templates — you write every prompt yourself — producing all four artifacts and running at least one review-driven iteration decision.

This is Lab 1C without training wheels: a bigger task, no fill-in-the-blank prompts, and Review is mandatory, not a stretch goal. This lab is Level 2's primary exit criterion.

## Prerequisites

- Level 2 Modules 1–2 completed
- Your experiment repo open, working tree clean (`git status`)
- Your Lab 1C artifacts available — you'll compare your prompt-writing against them at the end

## Pick Your Task

A **medium, real, self-contained improvement** — one notch up from Lab 1C:

- Touches **3–5 files** (Lab 1C was 1–3); implementable by hand in under an hour
- Has **at least two genuine unknowns** — a convention to discover *and* an integration point or behavior question
- Crosses at least one boundary: e.g., a utility *plus* its wiring into build/config, or a feature *plus* its test
- Still zero-risk: not on a critical path, revertible with `git checkout .`

Good shapes: extend an existing validation script to a second rule and wire both into CI; add a small feature to an internal tool including its test; consolidate duplicated config handling into one module. **PM/TPM variant:** a multi-file documentation task — e.g., a docs section spanning three pages plus an index update, where research must discover the actual current practice.

**No template this time.** You write the research, plan, and review prompts from scratch. Guardrails, not scripts: a good research prompt states the task, lists 3–5 specific questions or things to consider, and names constraints. A good plan prompt feeds findings *from the research doc*, not from memory.

## Steps

### Phase 1 — Research

1. Select **Task Researcher** (or `/task-research <topic>`). Write your own scoped prompt. Before sending, self-check: could a stranger tell what questions research should answer? If not, revise.
2. When it finishes, **open and read** `.copilot-tracking/research/<...>-research.md`. Verify at least two citations by opening the cited files. Extract the findings you'll feed to planning.

   **Expected output:** a research doc with file:line evidence, alternatives weighed, and ONE recommended approach per question.

### Phase 2 — Plan

3. `/clear`. Open the research doc in the editor.
4. Select **Task Planner** (or `/task-plan`). Prompt with requirements drawn from your research findings.
5. **Open and read both files**: the plan in `.copilot-tracking/plans/` and the details in `.copilot-tracking/details/`. Contract check: every task has success criteria; every detail traces to research line references; nothing invented that you didn't state.

   **Expected output:** plan file with checkbox phases/tasks + details file with per-task specifications and line references.

### Phase 3 — Implement

6. `/clear`. Open the plan file in the editor.
7. Select **Task Implementor** (or `/task-implement`). Since this task is bigger than 1C, keep `phaseStop=true` (the default) and consider `taskStop=true` for any phase touching unfamiliar code.
8. At every stop point: read the diffs, run linters/tests, then approve. When done, **read the changes log** in `.copilot-tracking/changes/`.

   **Expected output:** working code across 3–5 files + a changes log with Added/Modified/Removed sections matching what you approved.

### Phase 4 — Review (mandatory)

9. `/clear`. Open the changes log in the editor (keep plan and research handy).
10. Run **`/task-review`** (Task Reviewer). Let it locate artifacts, extract the checklist, validate, and run lint/build/test.
11. **Open and read the review log** in `.copilot-tracking/reviews/`. Note the overall status and findings by severity.
12. **Act on the outcome** — this step is the point of the lab:
    - *Complete:* note any Minor findings and follow-up items; you're done.
    - *Needs Rework:* `/clear`, open the review log, `/task-implement` to fix Critical/Major findings, then re-review.
    - *Research/Plan Gap:* `/clear`, open the review log, `/task-research` or `/task-plan` the gap. Even a small gap — run the loop once; the muscle memory is the deliverable.

## Verify Your Work

- [ ] **All four artifacts exist and you read each:** research doc, plan (+ details), changes log, review log
- [ ] Every prompt was your own — no template copied from Lab 1C or the docs
- [ ] You ran `/clear` at every transition and restored context by opening the correct artifact (per the transition table)
- [ ] The review log shows a real validation pass (checklist items with evidence, validation commands run)
- [ ] You executed the review outcome — committed on Complete, or ran one iteration loop otherwise
- [ ] The change works, including at least one failure-path check

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Research came back broad and shallow | Your prompt lacked specific questions — `/clear`, re-run with 3–5 concrete questions and named constraints |
| Planner invented requirements you never stated | Plans drift too — tell it what to remove, or re-prompt citing the research sections to honor |
| Implementor wandered off-plan mid-phase | `/clear`, reopen the plan file, re-prompt: "Follow the plan at [path] exactly" — and check for degradation symptoms |
| Reviewer says "no artifacts found" | Ensure the changes log exists and is open in the editor; give explicit paths to research/plan/changes in the prompt |
| Reviewer's findings feel wrong | Findings are evidence-based — open the cited evidence. If it's genuinely wrong, that's a documentation gap in *your* plan; note it |
| Review found nothing at all on a 3–5 file change | Suspicious — check that the plan had verifiable success criteria; a reviewer can only validate what was documented |

## What You Learned

- Prompt-writing is the skill; the agents are the easy part.
- Review is where documented intent meets actual code — and where your research/plan quality gets graded.
- The iteration loop (clear → open review log → target phase) is the same move every time.

## Reflection Questions

Write short answers in your notes — the Level 2 close-out discussion uses them:

1. Which of your prompts was weakest, and how would you rewrite it?
2. What did Review catch (or confirm) that you would have missed by eyeballing the diff?
3. Compare your Lab 1C and 2A research prompts: what did you scope better this time?
4. Where did you feel the pull to skip a `/clear`? What would it have cost?

## Stretch Goal

Re-run the *same task* as a single `/rpi` (rpi-agent) invocation on a throwaway branch, then diff the experience: research depth, artifact completeness, total time. Keep both artifact sets — Module 3's matrix stops being abstract when it's your own task on both sides.
