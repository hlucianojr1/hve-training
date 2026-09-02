# Lab 3C — Specialty Review

**Time:** 45 min · **Self-runnable:** yes
**Goal:** run one specialty workflow end to end on a real target, produce its artifacts, and practice stating — accurately — what the output is and is not.

Pick **one** track. All three end with the same verification discipline: every artifact you produce gets an honest scope statement.

## Prerequisites

- Module 2 completed
- **Track A:** the `security` collection installed (experimental — pre-release channel). A target app with known issues: [OWASP Juice Shop](https://github.com/juice-shop/juice-shop) cloned locally, or your own **non-production** app. Never run this lab against a repo you can't freely discuss findings about
- **Track B:** the accessibility-planner available (ships in the `project-planning` collection), plus a web project of yours with real UI surfaces
- **Track C:** the `design-thinking` collection installed (preview channel), plus a fuzzy problem from your actual work — one where you're not sure what to build

## Track A — Security (pick A1 or A2)

### A1: Automated review with `/security-review` (fits the timebox)

1. Open the target repo. Fresh chat, then run `/security-review` (the security-reviewer agent's entry point). Default mode is `audit` — a full scan against the applicable OWASP skills. On a large repo, scope it: check out a branch with a known-vulnerable area changed and use `diff` mode instead.
2. Let the orchestrator run its pipeline: profile codebase → assess applicable skills → verify findings → generate report. This takes a while (subagent-driven verification); read the interim output while it works.
3. **Expected output:** a report at `.copilot-tracking/security/YYYY-MM-DD/security-report-NNN.md` with severity-ranked findings. On Juice Shop, expect real hits (injection, broken auth patterns) — it's built to be vulnerable.
4. Now the important part: pick **one** finding and manually verify it — open the file, read the code, confirm or refute. Then pick one *known* Juice Shop vulnerability class the report missed, if any, and note it.

### A2: STRIDE pass with security-planner (deeper, partial in 45 min)

1. Fresh chat. Run `/security-capture` (or from Lab 3A's PRD: `/security-plan-from-prd`). Give a project slug.
2. Work Phases 1–2 fully: scoping (tech stack, data classification, compliance), then bucket classification across the seven operational buckets. Answer 3–5 questions per turn; confirm each phase gate.
3. At the timebox, stop and note your resume point — state lives in `.copilot-tracking/security-plans/{slug}/state.json`; the STRIDE phase (4) and backlog handoff (5–6) make a good second session.
4. **Expected output:** `state.json` plus the growing security plan file, buckets classified, threats appearing as `T-{BUCKET}-{NNN}` once Phase 4 starts.

## Track B — Accessibility

1. Fresh chat. Select **Accessibility Planner** from the agent picker, in your web project's workspace. It creates `state.json` under `.copilot-tracking/accessibility/{project-slug}/` and starts Phase 1 (Discovery) — capture mode if you have no upstream PRD, seeded mode if you point it at one.
2. Phase 2: framework selection. Keep the defaults (`wcag-22@AA`, `section-508`) unless you have a reason; record a reason for anything you disable — the planner asks.
3. Phase 3: let it map your in-scope surfaces against success criteria. Timebox: getting through Phase 3 with a partial criteria map is a successful lab; Phases 4–6 (risk, evidence register, dual-format backlog) resume from state later.
4. **Expected output:** `state.json`, a surface inventory, framework selections with conformance levels, and success-criteria mappings with evidence pointers.

## Track C — Design Thinking → RPI Handoff

1. Fresh chat. Run `/dt-start-project` and give the dt-coach your fuzzy problem. Work the **problem space** (Methods 1–3: scope conversations, design research, input synthesis) in abbreviated form — tell the coach you're timeboxed; it adapts.
2. Aim for a problem statement with confidence markers (`validated` / `assumed` / `unknown` / `conflicting`) on its findings. The markers are the point: they're what downstream agents calibrate against.
3. Run `/dt-handoff-problem-space` to package Exit 1. **Expected output:** session state in `.copilot-tracking/design-thinking-sessions/{project-slug}/` and a handoff artifact in `docs/design-thinking/{project-slug}/` carrying `exit_point`, `artifacts`, `constraints`, and `assumptions` with confidence markers.
4. `/clear`, then hand to **Task Researcher** (`/task-research`) referencing the handoff artifact. Watch the researcher treat `assumed` items as verification targets and `unknown` items as primary research targets — read the first section of its research doc and find one example of each.

## Verify Your Work

- [ ] The track's artifacts exist under `.copilot-tracking/` (and `docs/design-thinking/` for Track C) and you read them
- [ ] Everything the workflow wrote stayed inside its tracking directory — confirm with `git status`: **no source files were modified** (Track C's handoff doc in `docs/` is the designed exception)
- [ ] You manually verified at least one finding/mapping/assumption rather than accepting it (A: confirmed a vulnerability in code; B: checked one criterion mapping against the actual UI; C: challenged one `assumed` constraint)
- [ ] **The scope statement, written at the top of your artifact or notes, in your own words:** what this pass is (assistive pre-review / planning input) and what still stands between it and a decision (for A: SAST/DAST, pen test, security team review; for B: qualified accessibility professional review, real assistive-technology testing; for C: stakeholder validation of the problem statement)

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Agent or prompt not found | Check the collection and channel — security and design-thinking artifacts aren't on the stable flagship install (Module 2's maturity rule). Install the specific collection extension |
| `/security-review` audit runs far too long | Use `diff` mode against a scoped branch, or point it at a subdirectory. Same expectation-setting as long research runs |
| Security reviewer reports zero findings on Juice Shop | Suspicious, not reassuring — check it actually profiled the codebase (read the report's profile section). Rerun scoped to `routes/` or another known-hot area |
| Planner asks questions you can't answer (compliance regime, data classification) | Answer "unknown" honestly — planners record gaps as gaps. Inventing answers poisons the plan; the unknown *is* a finding |
| Phase gate refuses to advance | By design: summarize-and-confirm is the contract. Answer the open ❓ items or explicitly accept the gaps |
| dt-coach goes deep when you need abbreviated | Restate the timebox and ask to drive toward Exit 1; the coach supports early exits — that's what the three exit points are for |
| Findings feel generic / template-ish | You gave generic inputs. Feed concrete surfaces, real file paths, actual user groups — specificity in, specificity out |

## What You Learned

- Specialty workflows share one operating contract: interview → state file → phased gates → artifacts confined to `.copilot-tracking/` → backlog-or-handoff output.
- Verification of an assistive tool means sampling its output against ground truth, not reading its confidence.
- The scope statement is a skill: you can now say precisely what an AI security/accessibility/discovery pass buys — and what it never replaces.

## Stretch Goal

Chain two planners: from your Track A security plan, launch **rai-planner** in `from-security-plan` mode (if your target has AI components) or run the accessibility planner seeded from the same project — and find the reference field (`securityPlanRef`) in the second planner's `state.json` linking back to the first. That's the cross-planner evidence flow from Module 2, observed in the wild.
