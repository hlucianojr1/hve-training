# Level 2 — Instructor Guide

Companion to the [Level 2 README](README.md) agenda. General facilitation rules: [Instructor Handbook](../instructor-handbook.md).

## §1 Opening (0:00–0:10)

- Artifact check, not roll call: "Open your `.copilot-tracking/` from Lab 1C — thumbs up if you have research, plan, and changes." Anyone missing Lab 1C pairs with a neighbor and finishes it during breaks; don't hold the room.
- Frame the level in one line: *"Level 1 taught you the workflow. Today: why it breaks, how to see it breaking, and how to run it without training wheels — then we split by role."*
- Preview the split early so people sit with their track for Module 4: engineer / PM-TPM / tech lead.

## §2 Module 1 — Strict RPI Mastery (0:10–0:55)

**Talk track beats:**

1. Open with the loop diagram: "You learned three phases. The real cycle has four, and it *loops*." Review isn't an appendix — it's the phase that grades the other three.
2. The severity table sets commit policy: Critical/Major block, Minor accumulates. Say it as policy, not preference — teams will adopt whatever norm you state here.
3. Per-phase craft: spend the most time on **plans as contracts**. Have the room read a pre-baked plan and hunt for a task with no success criteria — there's always one; planted or not.
4. Stop controls as a *dial*: risk up → taskStop; routine → phaseStop. The anti-pattern to name: both false plus rubber-stamping equals "expensive autocomplete."
5. Handoff buttons: demo one (📋 Create Plan), then say plainly when *not* to use them — full reset needed, custom parameters, or the button doesn't match the review outcome. Typed commands are the fallback on VS Code <1.106 and the precision tool everywhere.

**Rework drill demo (10 min):** pre-bake a review log with one Major finding. Live: `/clear` → open review log → `/task-implement` → show the implementor targeting exactly the finding → `/task-review` → Complete. Narrate the invariant: "clear, open the artifact, invoke the phase — same move as R→P, just a different file."

**Anticipated questions:**

- *"Do I really review every cycle? Even a 2-file change?"* — For anything multi-file or heading to a PR, yes; the review log writes half your PR description. For genuinely trivial changes, the honest answer is judgment — but Lab 2A requires it so judgment has a baseline.
- *"The reviewer runs tests — so it replaces CI?"* — No. It runs validation commands as evidence-gathering. CI and human review gates stay; the reviewer is a pre-gate.
- *"Handoff buttons carry context — isn't that the contamination Module 2 warns about?"* — Sharp question, park it for Module 2 and call back: buttons are fine for single forward transitions; degradation risk grows with accumulated length, which is why manual `/clear` remains the reset tool.

### Module 1 answer key

1. Complete → commit, done; Needs Rework → Implement; Research Gap → Research; Plan Gap → Plan.
2. `/clear`, open the review log in the editor, invoke the target phase (`/task-implement`, `/task-research`, or `/task-plan`).
3. When risk is high or territory unfamiliar — you want to inspect after every task, not every phase. Default `phaseStop=true` suits routine work on a solid plan.
4. Any two of: you need a complete context reset; you want custom parameters for the next agent; the button doesn't match your intended path; VS Code is below 1.106.
5. Because it validates only against *documented* specifications. Anything research never covered or the plan never stated can't be validated — so it surfaces as visible gaps and undocumented deviations, feeding back into better research/plan prompts next cycle.

## §3 Module 2 — Context Engineering (0:55–1:45)

This is the session's centerpiece — protect the full 50 minutes.

**Demo: show degradation live (15 min).** This demo requires preparation:

- **Beforehand:** run a full `/rpi` cycle on your demo repo and *leave the session open* — you need 50K+ tokens of accumulated implementation output sitting in a chat. Do this the morning of the session; keep the VS Code window untouched.
- **Live:** in that prepared long session, issue a second `/rpi` for an unrelated small task. Let the room watch it skip research and jump to code. Run the four-symptom checklist against the output on screen, out loud, symptom by symptom.
- **Then the fix:** `/clear`, reopen nothing (new task), re-run the same request fresh — show the research phase engaging properly. Two or three minutes of real research is enough contrast; don't wait for completion.
- **Contingency:** degradation is probabilistic. If the second `/rpi` behaves, say exactly that — "this failure is probabilistic, which is why we clear preventively" — and show a pre-recorded capture or pre-baked transcript of a degraded run. Modeling honest expectations *is* teaching HVE. Have that capture ready regardless.

**Talk track beats:**

1. The numbers first: 3K instructions vs 50K–200K output. "The model doesn't forget — it deprioritizes." 30% of a 10K conversation vs 1.5% of 200K.
2. The three-step failure sequence, verbatim from the doc. Tell the room they'll reproduce it themselves in Lab 2B — this demo is the preview.
3. Drill the four symptoms until they're recitable — they're an exit criterion. Quick call-and-response works: you name a behavior, room names the symptom.
4. The command table: emphasize it's a *decision*, not a ritual. `/compact` gets the honest treatment: lossy by design, removed from handoff buttons because Autopilot compaction loops degraded context unpredictably — automated summarization compounds its own losses.
5. Cross-session: 💾 Save / memory agent / `/checkpoint continue`. One sentence to land: "checkpoints supplement artifacts, never replace them."

**Anticipated questions:**

- *"Why not just make context windows bigger?"* — Bigger windows raise the ceiling, not the ratio; instruction share still collapses as output accumulates, and performance degrades well before the limit.
- *"Can't the tool auto-clear between phases?"* — Handoff buttons partially do (pre-filled transitions), but carried context is sometimes what you want; the judgment stays with you. Also see the `/compact` removal story — automating context management has already backfired once.
- *"Is `/compact` ever actually right?"* — Yes: mid-phase, long conversation, current task must continue. It's the only tool that keeps *some* awareness without a restart. Just never at phase boundaries.

### Module 2 answer key

1. Recency bias: recent tokens receive disproportionate attention weight. At 200K tokens the 3K system prompt is ~1.5% of the conversation — deprioritized to background noise, not forgotten.
2. (1) First `/rpi` executes all phases correctly; (2) conversation grows to 50K+ tokens of implementation output; (3) second `/rpi` skips directly to implementation, producing shallow output that misses edge cases.
3. Skips phases; ignores its own instructions; shallow output / missed edge cases; echoes the previous task's structure.
4. `/compact` — you need to keep awareness of prior decisions in the current task, so `/clear` would destroy needed context. Accept the caveat that it's lossy and the model chooses what survives.
5. Autopilot mode could trigger compaction loops that degraded context unpredictably. For cross-session persistence use the memory agent — 💾 Save or `/checkpoint`, restored via `/checkpoint continue <description>` — deterministic disk state instead of summarization.

## §4 Module 3 — rpi-agent & Mode Selection (1:55–2:30)

**Demo: same task, both modes (10 min).** Prepare both runs beforehand (research time makes live runs impossible):

- Run one small task twice on your demo repo: once via strict RPI, once via a single `/rpi`. Keep both artifact trees.
- Live, show the two side by side: strict's `research/`, `plans/`, `details/`, `changes/`, `reviews/` vs rpi-agent's summary plus `subagent/` notes. Open the strict research doc next to the rpi-agent equivalent — let citation density do the arguing.
- Close on the matrix's audit-trail row: "If you'll ever have to defend this work, that row alone decides."

**Talk track beats:**

1. Don't teach rpi-agent as the lesser mode — it's the *right* mode for a real class of tasks, and the escalation path removes the pressure to choose perfectly upfront.
2. "Advisory prose vs structural constraint" is the sentence to land twice. Callback to Lab 2B Part A (planner's gate = structural) vs Part B (rpi-agent ordering = advisory).
3. Escalation is not failure; the named anti-pattern is downgrading a degraded strict cycle to "let rpi-agent finish."
4. `runSubagent`: have students check their own environment now — one minute, saves lab pain. The fallback (strict, manual transitions) applies to code-review and security-reviewer too, which tees up Module 4's tech-lead track.

**Anticipated questions:**

- *"If strict is safer, why does rpi-agent exist?"* — Cost. Ceremony has a price; paying it on a rename is waste. Matching tool to task *is* the skill.
- *"Can I get full artifacts out of rpi-agent?"* — You get summaries and subagent research notes, not the full four-artifact trail. If you're asking, your task probably wants strict.
- *"How do I know mid-task that I should escalate?"* — Hidden complexity signals: research questions you can't answer inline, a second subsystem appearing, or any degradation symptom. Escalating costs one `/clear` plus a research run — cheap versus rework.

### Module 3 answer key

1. As advisory prose in its system prompt, not a programmatic constraint. It stops working under recency bias: after 50K+ tokens of implementation output, the ~3K of ordering instructions lose attention weight and the model pattern-matches to recent behavior.
2. Strict RPI; the audit-trail row (complete artifacts vs summary only). Bonus: multi-agent runs aren't fully auditable/replayable in general (ref #14), so artifacts are the only durable audit trail.
3. Start with rpi-agent; when hidden complexity appears, escalate to strict — rpi-agent can hand off to Task Researcher itself, or the user forces it (`/clear` + `/task-research` the gap). Review findings also escalate back to research or planning.
4. Strict RPI with manual phase transitions (`/task-*` prompts need no subagent tool). Shared dependency: task-implementor orchestration, code-review, security-reviewer.
5. rpi-agent — clear scope, familiar solo code, prototype/fast-iteration work; no team handoff, no audit need. A defensible strict answer must invoke a matrix factor (e.g., "it'll be maintained long-term") — grade the justification, not the pick.

## §5 Module 4 — Role-Track Breakouts (2:35–3:20)

**Breakout logistics:**

- Split during the 2:30 break, not at 2:35 — post the three groups (engineer / PM-TPM / tech lead) where everyone can see. People wearing two hats pick the hat they'll use most next quarter.
- Room shape: one facilitator can run all three by staging starts — engineers are most self-sufficient (start them first from the module file), then PM/TPM, then sit with tech leads (adr-creation's Socratic style confuses people who expect a form-filler: it asks questions *at* you, that's the design).
- Each track runs its Module 4 follow-along against the instructor's prepared repo/backlog; Lab 2C repeats it on their own real work.
- **Prepare per track:** engineers — a repo with an uncommitted RPI-completed change (so `/git-commit` and `/pull-request` have something real); PM/TPM — a demo repo with 15+ messy open issues (or use a public repo read-only); tech leads — one open PR of moderate size and a genuinely undecided design question.
- **Reconvene at 3:10 for cross-track show-and-tell (10 min):** one artifact per track, one minute each — engineer shows a generated PR description, TPM shows a triage plan, tech lead shows a merged review or ADR draft. Then land Module 4's pipeline point: handoff → research prompt; changes log → review target; ADR → research evidence. This reconvene is the mixed-role payoff; don't skip it to buy lab time.

**Anticipated questions:**

- *"(PM) The agent mislabeled half my issues."* — Working as designed: triage plans are drafts for your review; your corrections are the human gate. If it were reliably right, the gate wouldn't need you.
- *"(Engineer) Why not let `/git-commit` just commit without me reading it?"* — Same reason as every other gate: draft generators are wrong confidently. Reading a one-line message is the cheapest review you'll ever do.
- *"(Tech lead) code-review vs task-reviewer?"* — task-reviewer validates *your implementation against your plan* (inside RPI); code-review reviews *a diff/PR from five perspectives* (team quality gate). Different inputs, different questions.

### Module 4 answer key

1. The current file matching the instruction's `applyTo:` glob / file type (e.g., `*.py`, `*.tf`). If a standard is wrong for the team: revise or author the instruction file (prompt-builder / Level 3) — don't skip it ad hoc.
2. Only at the Execution stage. First: a sprint/triage handoff file must exist, and a human must have reviewed it — execution applies only the *checked* operations. (Credit mentioning `/clear` before the stage.)
3. Gate 1: scope confirmation after it computes the diff. Gate 2: perspective selection (any of functional/standards/accessibility/security/pr, or full) plus depth tier (basic/standard/comprehensive).
4. Accept any defensible pairing, e.g.: TPM's sprint handoff → engineer's research prompt; engineer's changes log → tech lead's review target or TPM's status update; tech lead's ADR → citable evidence in engineers' research; curated instruction files → everyone's implement/review phases.
5. Same recency-bias mechanics: each stage loads specific instructions and planning artifacts, and stale context from the previous stage interferes with the current stage's logic (misapplied labels there, skipped phases here). Context discipline is workflow-agnostic.

## §6 Close (3:20–3:30)

- Rapid group quiz: the four degradation symptoms (call-and-response), one review-outcome routing question, one mode-selection scenario.
- Lab briefing: 2A is the exit-criterion lab — every prompt their own, Review mandatory. 2B **must be on a throwaway branch**; say it twice. 2C on their own real work, per track; PM/TPM track posts *nothing*.
- Collect Lab 2A reflection answers at Level 3's opening — they seed that session's warm-up discussion.
- Preview Level 3: "You now operate HVE well. Next: the full lifecycle, and authoring your own artifacts — the instruction file your stack is missing, you'll write it."

## Lab 2B Debrief Guidance (run at Level 3 opening, or async in team channel)

Failure stories are the best teaching material this curriculum produces — harvest them deliberately:

- Ask for *evidence*, not summaries: have 2–3 students show their saved degraded output next to their clean Lab 2A research doc. The visual gap teaches better than any slide.
- Tally which Part A behavior each student got (planner asked for research vs auto-invoked researcher) — the split itself demonstrates nondeterminism, worth naming.
- Expect and welcome "mine didn't degrade" reports: probabilistic failure is the lesson, and it's exactly why the discipline is *preventive* clearing. Ask how long their first session was — short sessions usually explain it.
- Watch for the wrong takeaway ("the tools are flaky") and reframe: the *defenses* differ — structural gates held (Part A), advisory ordering failed under load (Part B). That asymmetry is the whole Module 3 argument, now experienced firsthand.
- Best debrief closer: ask who has since caught a degradation symptom in *real* work. By Level 3 someone always has — let them tell it.
