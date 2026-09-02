# Lab 2C — Role-Track Lab

**Time:** 60 min · **Self-runnable:** yes
**Goal:** run your role's daily-driver workflow from [Module 4](../modules/04-role-track-daily-drivers.md) on real work, producing your track's deliverable: a PR-ready change (engineer), a sprint-plan draft (PM/TPM), or an ADR plus a code review (tech lead).

Do **your** track. If time allows, skim another track's section — Module 4's point stands: their outputs land on your desk.

## Prerequisites (all tracks)

- Lab 2A completed; Module 4 read
- Your experiment repo open, clean tree — **work on a branch** for all tracks:

```bash
git checkout -b lab-2c-<yourtrack>
```

---

## Track A — Engineer: RPI to PR

Take a real small code change all the way to a pull request.

### Steps

1. **Pick a task** — Lab 1C-sized (1–3 files) is fine; this lab is about the delivery tail, not the cycle.
2. **Run strict RPI** through Review, exactly as in Lab 2A (write your own prompts, `/clear` between phases). Get the review to **Complete** — fix Critical/Major findings via the rework loop if needed.
3. **`/clear`**, then run **`/git-commit`**. Read the generated Conventional Commit message *before* confirming — check the type (`feat`/`fix`/etc.), scope, and that the description matches what actually changed. Edit if it doesn't.

   **Expected output:** a staged-and-committed change with a message like `feat(validation): add config schema check`.
4. **Run `/pull-request`** with your changes log open in the editor — the structured description should draw on your artifacts. Review the description: does it explain *why* (from research) and *what* (from changes log)?

   **Expected output:** a PR (or PR-ready description, if you'd rather not open one on a shared repo — creating it needs your explicit go-ahead anyway).
5. Optional: run **`/pr-review`** against your own PR and compare its findings to your Task Reviewer log.

### Verify Your Work

- [ ] Review log shows Complete before any commit
- [ ] Commit message is Conventional-Commit compliant and truthful — and you actually read it
- [ ] PR description traces to your artifacts (not regenerated from vibes)
- [ ] All work on the lab branch, main untouched

---

## Track B — PM/TPM: Backlog Pipeline to Sprint Draft

Run Discovery → Triage → Sprint Planning on a **real backlog** — planning artifacts only.

> **Hard rule for this lab: nothing gets posted.** You will produce and review planning files; you will **not** run the execution stage. No labels applied, no comments, no milestone changes without explicit team approval. That's not a lab constraint — it's the workflow's actual human gate, exercised.

### Steps

1. **Pick a real repo with a real backlog** you have read access to — your team's repo, or any active repo whose issues you understand. Select the **github-backlog-manager** agent. (ADO shop? Run the equivalent flow with `/ado-get-my-work-items` → `/ado-triage-work-items` → `/ado-sprint-plan` and adapt the steps below.)
2. **Discovery.** Prompt with a bounded scope, e.g.: `Discover all open issues in <org>/<repo> that are unassigned and have no milestone.`

   **Expected output:** analysis files under `.copilot-tracking/github-issues/discovery/<scope>/` including `issue-analysis.md`. Read it; confirm the scope caught what you meant.
3. **`/clear`.** Then **triage**: `Triage the issues from my latest discovery session for <org>/<repo>. Flag duplicates with confidence scores and suggest labels.` (Or invoke `/github-triage-issues`.)

   **Expected output:** `triage/<YYYY-MM-DD>/triage-plan.md`. **Review it critically** — you know this backlog better than the agent: fix wrong labels, challenge low-confidence duplicate flags. Your edits are the human gate working.
4. **`/clear`.** Then **sprint planning**: `Plan sprint assignments for <org>/<repo> using the triage results. Assign high-priority bugs to <next milestone>.` (Or `/github-sprint-plan`.)

   **Expected output:** `sprint/<milestone-kebab>/handoff.md` with checkbox operations. Read every operation; uncheck anything you wouldn't stand behind in sprint review.
5. **Stop.** Do not execute. If your team later approves the handoff, execution (`/github-execute-backlog`) applies exactly the checked operations and logs them — but that's a team decision, not a lab step.

### Verify Your Work

- [ ] Three planning artifacts exist and you read all three: issue-analysis, triage-plan, handoff
- [ ] You `/clear`ed between every stage
- [ ] You corrected at least one agent suggestion in the triage plan (if you found zero, look harder)
- [ ] Nothing was posted to GitHub/ADO — zero writes

---

## Track C — Tech Lead: ADR + Code Review

Produce one real ADR, then run the code-review agent on a real PR.

### Part 1 — ADR (30 min)

1. **Bring a genuinely open decision** — one your team hasn't settled (a library choice, an architectural boundary, a standard to adopt). Settled decisions give the Socratic coaching nothing to work with.
2. Select the **adr-creation** agent and state the decision, drivers, and alternatives you're aware of. Answer its questions honestly — it coaches through Discovery → Research → Analysis → Documentation; expect it to poke at alternatives you dismissed.

   **Expected output:** a working draft at `.copilot-tracking/adrs/<topic>-draft.md`, finalized to `docs/decisions/YYYY-MM-DD-<topic>.md` (on your lab branch).
3. Read the final ADR as its future audience: would an engineer citing this in a research phase two years from now understand *why*, including the rejected options?

### Part 2 — Code Review (30 min)

4. **Pick a real PR or branch diff** — one of your team's open PRs, or your own Lab 2C-A branch if you did both tracks.
5. `/clear`, then select the **code-review** agent from the picker. Note the `runSubagent` requirement up front: if dispatch fails, the fallback is the `/pr-review` prompt (single-perspective, no subagents) — do that instead and note the difference in depth.
6. Work the two human gates: **confirm the scope** it computed, then **choose perspectives and depth** — for a first run, `standards` + `functional` at `standard` depth is a sensible pair; `full` on a large PR is a long run.

   **Expected output:** a merged review at `.copilot-tracking/reviews/code-reviews/<branch>/review.md` plus `metadata.json`.
7. Read the merged review next to your own human judgment of the same PR. It's assistive: what did it catch that you missed? What did you know that it couldn't (context, intent, roadmap)? That gap is the argument for keeping the human gate.

### Verify Your Work

- [ ] ADR finalized under `docs/decisions/` with drivers, alternatives, and consequences — on the lab branch
- [ ] Code review ran human-gated: you confirmed scope and chose perspectives/depth deliberately
- [ ] You know the `runSubagent` fallback and whether your environment needs it
- [ ] Merged review read and compared against your own judgment; agent never modified code

---

## If Something Went Wrong (all tracks)

| Symptom | Fix |
|---------|-----|
| `/git-commit` message misdescribes the change | Edit before confirming — it's a draft generator, not an authority |
| `/pull-request` description is generic | Open the changes log (and research doc) in the editor first, then re-run |
| Backlog discovery finds nothing / wrong scope | Tighten the scope phrase (repo, filters, labels) and re-run — discovery is cheap |
| Triage suggestions are confidently wrong | Expected sometimes — the triage plan is a *draft for your review*; correct it, that's the workflow |
| adr-creation keeps asking questions instead of writing | That's the Socratic design — answer them; if you truly have all answers, say so and ask it to proceed to documentation |
| code-review stalls or errors on dispatch | `runSubagent` unavailable ([ref #4](../../resources/limitations-and-workarounds.md)) — fall back to `/pr-review` |
| Any agent behaving oddly after several stages | Degradation symptoms apply outside RPI too — `/clear` and restore from the stage's planning file |

## What You Learned

- Your role's daily drivers ride the same discipline as RPI: artifacts between stages, `/clear` between contexts, human gates before anything irreversible.
- Engineer: the delivery prompts are draft generators — RPI artifacts make their drafts good, and you remain the editor.
- PM/TPM: the pipeline's power is that everything is reviewable *before* it touches the backlog.
- Tech lead: ADRs and merged reviews are institutional memory — assistive agents draft it, your judgment signs it.

## Stretch Goal

Swap deliverables with someone from another track (in-team or a course peer): engineer reads a sprint handoff and drafts a research prompt from one issue; TPM reads an engineer's changes log and writes the status update it implies; tech lead runs code-review on the engineer's lab PR. The mixed-role notes in Module 4 stop being theory the first time you consume another track's artifact cold.
