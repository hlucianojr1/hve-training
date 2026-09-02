# Lab 3A — The Planning Chain

**Time:** 75 min · **Self-runnable:** yes · **PM/TPM capstone:** this lab is your Level 3 deliverable
**Goal:** run the full chain — BRD → PRD → ADR → work items → one RPI cycle — on a real feature idea of yours, with a clean artifact handoff at every link.

> **Hard safety rule for this entire lab: planning mode only.** Nothing gets posted to a real issue tracker. Use the planning-only agents (`ado-prd-to-wit` / `jira-prd-to-wit`) or pin `github-backlog-manager` to **Manual autonomy** so every mutating operation gates on approval — and then don't approve any. Work items exist as planning files under `.copilot-tracking/` until a human deliberately promotes them after the lab.

## Prerequisites

- Level 3 Modules 1–2 read; Level 2 exit criteria met (engineers) or Level 2 Labs 2A–2B done with the PM variant (PMs/TPMs)
- The `project-planning` collection installed (brd-builder, prd-builder, adr-creation are in it); verify by typing the agent picker open and finding **brd-builder**
- Your experiment repo open, clean working tree, `.copilot-tracking/` gitignored

## Pick Your Feature

Bring a **real feature idea from your actual work** — something you'd genuinely propose. Criteria:

- Big enough to justify requirements (a real business driver, at least two stakeholder groups, one non-obvious technical decision)
- Small enough to decompose into ~4–8 stories
- Contains at least one work item implementable in your repo in under 20 minutes (that's your RPI target in Step 5)

The 75 minutes assume timeboxes, not completeness: **BRD 15 min, PRD 20 min, ADR 10 min, decomposition 10 min, RPI 20 min.** Real planning sessions run much longer — both builders persist session state precisely so you can resume later. Today you're learning the chain, not shipping the documents.

## Steps

### Step 1 — BRD (15 min)

1. Fresh chat. Select **brd-builder** and open with scope up front (this skips the obvious scoping questions):

   ```text
   Create a BRD for [your feature].
   Scope:
   * Business driver: [why this matters — cost, risk, revenue, compliance]
   * Stakeholders: [the 2+ groups affected]
   * Success metrics: [how the business would measure this]
   ```

2. Answer its questions honestly for the timebox — the iterative Q&A is the method, not friction. At 15 minutes, ask for `summary` (an output mode) and stop.
3. **Expected output:** a BRD taking shape at `docs/project-planning/<name>-brd.md` and session state in `.copilot-tracking/brd-sessions/`. Open the BRD and read it. Note what it does *not* contain: no solution, no tech stack. That's correct — BRDs are solution-agnostic.

### Step 2 — PRD (20 min)

4. **`/clear`.** Never combine BRD and PRD in one session — documented pitfall.
5. Select **prd-builder**. Point it at your BRD:

   ```text
   Create a PRD for [your feature], building on the BRD at
   docs/project-planning/<name>-brd.md.
   Define requirements with measurable acceptance criteria for:
   * [core capability 1]
   * [core capability 2]
   ```

6. Work the Q&A through the timebox. Push for **measurable** requirements — when it accepts "fast" or "easy to use" from you, that's your drift signal: give numbers.
7. **Expected output:** PRD at `docs/project-planning/<name>.md`, state in `.copilot-tracking/prd-sessions/`. Read it; every requirement should trace to a BRD objective.

### Step 3 — ADR (10 min)

8. **`/clear`.** Select **adr-creation**. Pick the one genuinely contested technical decision your feature implies (build-vs-buy, sync-vs-async, storage choice) and ask for an ADR for it, referencing the PRD.
9. It coaches Socratically — it asks what alternatives you considered rather than dictating an answer. Play along; that's the method. **Expected output:** a draft in `.copilot-tracking/adrs/`, finalized to `docs/decisions/YYYY-MM-DD-<topic>.md`.

### Step 4 — Decompose to Work Items, Planning Mode Only (10 min)

10. **`/clear`.** Choose your tracker's planner:
    - **Azure DevOps:** select **ado-prd-to-wit** — planning-only by design; it cannot create work items.
    - **Jira:** select **jira-prd-to-wit** — same planning-only design.
    - **GitHub:** select **github-backlog-manager** and open with: `Manual autonomy. Plan issues from the PRD at docs/project-planning/<name>.md — do not create anything.` Do not approve any create operation it proposes.
11. Feed it the PRD path and let it build the Epic → Feature → Story hierarchy. **Expected output:** planning files under `.copilot-tracking/` (e.g., `workitems/prds/<name>/work-items.md` for ADO) with a handoff file — and **zero new items in any real tracker**. Verify that last part now: check your tracker.

### Step 5 — Execute One Item via RPI (20 min)

12. Pick the smallest implementable story from your hierarchy. **`/clear`**, then run the cycle you know: `/task-research` scoped to that story → read the research → `/clear` → `/task-plan` → read the plan → `/clear` → `/task-implement`. The story's acceptance criteria (which trace to the PRD, which traces to the BRD) are your plan's success criteria — notice the chain doing its job.

## Verify Your Work

- [ ] Five artifact groups exist and you read each: BRD, PRD, one ADR, work-item planning files, and the RPI trio (research/plan/changes) for one story
- [ ] Every link consumed the previous link's artifact — the PRD cites the BRD, the ADR references the PRD, stories trace to PRD requirements, the plan traces to a story
- [ ] You ran `/clear` at every seam (four times minimum)
- [ ] Your real issue tracker contains **nothing new** from this lab
- [ ] You can say which link surfaced something you hadn't thought of (if none did, your feature was too small — note it and pick bigger next time)

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Builder asks endless questions and the timebox dies | Front-load scope in the opening prompt; at the timebox, request `summary` output mode and move on — resume the session later via its state file |
| PRD session doesn't see the BRD | Reference the BRD by explicit path in your prompt; the builders cross-reference files you point at, not files you imagine they'll find |
| Session not detected on resume | Check state files exist in `.copilot-tracking/brd-sessions/` or `prd-sessions/` and you're in the same workspace |
| Backlog manager proposes creating real issues | Working as designed — that's the gate. Decline, restate Manual autonomy / planning-only. If anything slipped through, delete it from the tracker now and note the miss |
| adr-creation lectures instead of coaching, or vice versa | Tell it your experience level; it adapts coaching style. If it drifts into designing the whole system, rescope: one decision per ADR |
| planner (Step 5) invents requirements not in the story | Same drift you learned in Lab 1C — re-prompt with the story's acceptance criteria pasted verbatim |

## What You Learned

- The planning chain is RPI's discipline applied upstream: each agent produces an artifact, the next agent consumes it, `/clear` guards every seam.
- Solution-agnostic BRD → measurable PRD → alternatives-considered ADR are three different quality gates, not three formats for the same document.
- Decomposition can and should run in planning mode: work items are reviewable files before they're tracker entries.

## Stretch Goal

Run **security-planner** in from-PRD mode (`/security-plan-from-prd`) against your PRD for one phase — watch it seed Phase 1 scoping from the artifact you already built. That's the cross-planner integration from Module 2, and a preview of Lab 3C's security track.
