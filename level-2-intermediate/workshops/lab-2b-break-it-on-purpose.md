# Lab 2B — Break It on Purpose

**Time:** 45 min · **Self-runnable:** yes
**Goal:** deliberately trigger the two most common HVE failure modes — skipping research, and context degradation — observe the built-in defenses and the symptoms, then practice the recovery drill. You're building diagnostic instincts: after this lab, degraded output should *look wrong* to you before it costs you anything.

Reading about failure modes doesn't build recognition; seeing them does. Everything in this lab is a controlled burn.

## Safety First

> **Do this entire lab on a throwaway branch.** Part B intentionally produces degraded implementation output that you do not want anywhere near real work.

```bash
git checkout -b lab-2b-throwaway
```

Verify a clean tree first (`git status`). When the lab is done you'll delete the branch entirely.

## Prerequisites

- Lab 2A completed (you need to know what *healthy* output looks like to recognize sick output)
- Modules 2 and 3 read
- Two small candidate tasks from your repo, Lab 1C-sized (1–3 files each) — call them Task X and Task Y. They should be unrelated to each other.

## Part A — Skip Research, Meet the Gate (10 min)

Try to plan with no research and watch the planner defend itself.

1. Fresh chat (or `/clear`). Confirm there is **no** research doc for Task X in `.copilot-tracking/research/` (move any old ones aside).
2. Go straight to **`/task-plan`** with a normal planning prompt for Task X: `Create an implementation plan to [Task X].`
3. **Observe — don't help it.** Expected output, one of two behaviors (both are the gate working):
   - The planner **stops and asks for research** — validating that research exists is its mandatory first step; or
   - The planner **automatically invokes task-researcher first** to fill the gap, and only then plans.
4. Note which behavior you got. Either way: the plan is never built from vibes. Cancel/abandon the session once you've seen the gate — you don't need the output.

**What this teaches:** the research-validation gate is a structural defense. Strict RPI doesn't rely on your discipline alone — the planner is constrained. (Contrast with Part B, where the defense is only advisory.)

## Part B — Two /rpi Requests, One Session (20 min)

Now trigger the failure the docs warn about: recency bias causing phase-skipping.

1. Fresh chat. Run **`/rpi`** for Task X, normally. Let it run to completion — you *want* the session full of implementation tokens (50K+). While it runs, reread the four degradation symptoms in [Module 2](../modules/02-context-engineering.md).
2. **Without clearing**, in the same session, run a second **`/rpi`** for Task Y (the unrelated task).
3. **Watch for the symptoms** — score what you see against the checklist:
   - [ ] Skips phases (jumps straight to code, no research/plan for Task Y)
   - [ ] Ignores its own instructions (phase ordering, formats, conventions missing)
   - [ ] Shallow output (thin analysis, edge cases unaddressed)
   - [ ] Echoes Task X (Task Y's output mirrors Task X's structure or approach)

   **Expected output:** typically symptom 1 plus at least one other. The severity varies by session length and model — that variance is itself the lesson: degradation is probabilistic, which is why you clear *preventively*, not reactively.
4. Save the evidence: copy the degraded response (or screenshot) into a scratch note. You'll want it for the debrief — and for convincing skeptical teammates later.

> If Task Y comes out *fine*: your first session was probably short (small Task X, fast run). Ask for one more change to Task X first ("also add [small thing]") to grow the session, then retry step 2. Do not conclude the failure mode is mythical — see [ref #1](../../resources/limitations-and-workarounds.md).

## Part C — Recovery Drill (10 min)

You've recognized degradation mid-task. Now recover Task Y properly.

1. **Recognize:** name aloud (or in your notes) which symptoms you saw — recognition is step zero of the drill.
2. **`/clear`.**
3. **Restore from artifacts:** Task X's artifacts are safely on disk in `.copilot-tracking/` — they survived. Task Y has no artifacts yet (that's *why* its output was shallow — nothing carried context). So restoration here means: start Task Y's cycle properly with `/task-research` (or a fresh `/rpi`), on clean context.
4. Let research run for a few minutes — enough to confirm the contrast: same session-lineage, same task, night-and-day depth.
5. Abandon the run, then clean up:

```bash
git checkout . && git checkout - && git branch -D lab-2b-throwaway
```

Also delete the throwaway artifacts this lab created under `.copilot-tracking/` so they don't pollute future auto-discovery.

## Verify Your Work

- [ ] You saw the planner's research gate fire (Part A) and can say which of the two behaviors you got
- [ ] You produced and *recognized* at least two degradation symptoms by name (Part B)
- [ ] You saved evidence of the degraded output
- [ ] You ran the recovery drill: recognize → `/clear` → restore/restart from artifacts (Part C)
- [ ] Throwaway branch deleted; no lab artifacts left in `.copilot-tracking/`

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Part A: planner happily planned with no research | Check the agent picker — you were likely in a generic mode, not Task Planner. `/clear` and use `/task-plan` explicitly |
| Part B: second `/rpi` behaves perfectly | Session too short — grow it with another Task X change and retry; degradation needs token mass |
| Part B: first `/rpi` fails immediately | `runSubagent` likely unavailable ([ref #4](../../resources/limitations-and-workarounds.md)) — run Part B with two sequential `/task-research`→`/task-implement` flows in one session instead; same recency mechanics apply |
| Accidentally ran Part B on real work | That's what the branch was for — `git checkout .`, and treat it as an unplanned rehearsal of Part C |
| Can't tell if output is "shallow" | Compare against your Lab 2A research doc for structure and citation density — that's your baseline for healthy |

## What You Learned

- Strict RPI's gates are structural (the planner *can't* plan without research); rpi-agent's phase order is advisory (it *can* skip, and under recency bias it does).
- Degradation has a face: four nameable symptoms you can now spot mid-session.
- Recovery is cheap and mechanical — recognize, `/clear`, restore from artifacts — *if* you catch it early. The artifacts on disk are what make recovery possible at all.

## Stretch Goal

Repeat Part B but insert `/compact` (instead of `/clear`) before the second `/rpi`. Compare against both the degraded run and a `/clear` run. You'll usually see partial protection — better than nothing, worse than clean — which is exactly why `/compact` is the mid-phase tool, not the between-phases tool.
