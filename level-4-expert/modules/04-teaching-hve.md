# Module 4.4 — Teaching HVE

**Time:** 40 min · **Roles:** all (anyone who will run a session, brown-bag, or client enablement) · **Source docs:** [Instructor Handbook](../../instructor-handbook.md) (esp. §2), this curriculum's per-level instructor guides

## Objectives

- Prepare a live demo using the pre-baked-artifact pattern, with fallback branches and an outage contingency.
- Facilitate mixed-role rooms so PMs and TPMs are participants, not spectators.
- Assess students on artifacts and process — and explain why identical outputs would be a red flag, not a rubric.
- Run the Feynman/dry-run preparation loop and adapt the curriculum to real client constraints.

This module turns you from student into instructor. Lab 4C is the assessment: you'll deliver Level 1 Module 1 yourself.

## 1. The Pre-Baked Demo Craft (Handbook §2 — read it twice)

The single biggest live-demo failure mode: **a real research phase runs 20–60 minutes autonomously.** Never start a cold `/task-research` live and wait. The pattern:

1. **The week before**, run the demo task end-to-end in a repo you'll project. Keep every artifact `.copilot-tracking/` produces — research doc, plan + details, changes log, review.
2. **Live-start-then-switch:** start the real research so the room sees the invocation, agent picker, and first tool calls — 2–3 minutes of genuine behavior is convincing — then switch: *"research runs ~20 minutes; here's one I ran earlier."* Walk the pre-baked document, pointing at file:line citations.
3. Plan/Implement demos take the pre-baked research doc as input; those phases are faster and can often run truly live.
4. Keep a **fallback branch** of the demo repo at the pre-demo state (`git checkout demo-start`) so you can reset between cohorts.

**Copilot outage contingency:** if Chat misbehaves live — rate limits, outage, model unavailability — narrate over your pre-baked artifacts entirely. The artifacts *are* the teaching content; the live typing is theater. And announce the switch honestly: modeling honest expectations about AI *is* teaching HVE. An instructor caught faking a "live" demo has lost the room on the course's core value, verifiability.

## 2. Mixed-Role Facilitation

The curriculum is designed so PMs/TPMs are never spectators — your job is to keep it that way:

- During engineer-leaning segments, assign PMs the **artifact-reader role**: "you'll be consuming these documents from your team — what would you ask about this plan?"
- Backlog and planning segments flip the dynamic: engineers review BRD/PRD outputs.
- Pair mixed roles in labs: engineer drives, PM reads artifacts aloud and challenges them — this mirrors real HVE usage where artifacts are the collaboration surface.
- When tracks split, run parallel breakouts and reconvene for a 10-minute cross-track show-and-tell.

Pacing rule from the handbook: concepts never run past **25 minutes without keyboards**. Every module has follow-along steps; use them.

## 3. Grading: Artifacts and Process, Never Identical Outputs

Models are non-deterministic and HVE pins nothing — outputs *will* vary between students, and that's expected, not a defect. So assessment looks at:

- **Artifacts produced:** does `.copilot-tracking/` contain a research doc with real citations, a plan traceable to findings, a changes log? Ask to see files, never screenshots of chat.
- **Process followed:** did they `/clear` between phases and restore from the artifact? Did they read the plan before implementing? Did they catch drift?

Knowledge checks run as 5-minute group discussions, not tests — wrong answers surface misconceptions worth addressing live. Two students with identical outputs should make you *more* suspicious (shared answers), not satisfied. This grading philosophy is itself course content: it's [ref #13](../../resources/limitations-and-workarounds.md) applied to pedagogy.

## 4. The Feynman / Dry-Run Prep Loop

Before teaching a level, the handbook's self-preparation checklist (§7) is your bar — for Level 1: explain the why-rpi thesis **in 2 minutes without notes**, name the four artifact types with an example of each, run the memory-agent first interaction cold. The loop:

1. **Feynman pass:** explain each module's core idea out loud, from memory, as if to a smart colleague from another field. Wherever you reach for jargon or hand-wave, you've found a gap.
2. **Remediate as a student:** the corresponding lab is your fix — do it yourself. (Shaky on collections? Lab 4A. Shaky on the demo? Run it end-to-end; that's step 1 of pre-baking anyway.)
3. **Dry-run with a timer:** deliver the module to an empty room or a colleague, at full pace, with your actual demo repo. Timing discipline is learned here, not live — you cannot recover 15 lost minutes in a 45-minute module.
4. Repeat the Feynman pass on whatever the dry run exposed.

Lab 4C runs exactly this loop with a rubric attached.

## 5. Adapting to Client Contexts

The curriculum's default shape — four half-day sessions, one per week, labs between — is the *best* shape, because lab time between sessions is where habits form. When a client's constraints won't allow it:

- **Compressing to a bootcamp (2–3 consecutive days):** keep every lab but run them in-room as facilitated work blocks; the thing you lose is between-session practice on real work, so assign a "first real task" follow-up with a scheduled 1-hour check-in a week later. Pre-bake *all* demos — back-to-back days leave no overnight recovery slack.
- **Splitting into 2-hour sessions:** cut at module boundaries, never mid-module; open each session with a 10-minute artifact review of the prior lab (the Level 2+ pattern) so continuity survives the gaps.
- **Role-skewed rooms:** an all-PM cohort takes Levels 1–2 plus the planning tracks; an all-engineer cohort can compress Level 1 Module 1 but never skip it — the *why* is what prevents the "this is just ceremony" backslide.
- **What never gets cut:** Lab 1C (the first full cycle), the `/clear`-and-restore moment on screen, and the honest-limitations beats. A cohort that never touched keyboards learned a slideshow, not a workflow.

## Use Case Spotlight

A champion runs their first internal brown-bag: 50 minutes, mixed roles, projector. They pre-bake a research doc on the team's own repo the week before, live-start research for 90 seconds, switch honestly, and let a *real* citation — "12 existing modules use `resource_prefix`, see `variables.tf#L47`" — land the plausible-vs-verified argument on code the room maintains. Two PMs get the artifact-reader role and find a plan gap live. The session produces three volunteers for a pilot — not because the demo was flashy, but because it was verifiable and about *their* code. That's the craft: preparation invisible, honesty visible.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Live research runs 20–60 min autonomously — lethal to demos | Pre-bake artifacts; live-start-then-switch; announce the switch honestly ([ref #5](../../resources/limitations-and-workarounds.md)) |
| Copilot can be flaky live: rate limits, outages, model unavailability | Narrate over pre-baked artifacts entirely; keep a `demo-start` fallback branch |
| Outputs vary between students — no two labs look alike | Grade on artifacts produced and process followed, never identical output ([ref #13](../../resources/limitations-and-workarounds.md)) |
| Some student environments lack `runSubagent` or auto-applied instructions (CLI) | Know the documented fallbacks cold (strict RPI; explicit instruction references) and turn hiccups into teaching moments |

## Knowledge Check

1. Why must you never start a cold research phase live, and what are the four steps of the pre-baked demo pattern?
2. Copilot Chat starts rate-limiting ten minutes into your Module 4 demo. What do you do, and why does it still work pedagogically?
3. Two students finish Lab 1C with very different implementations. One instructor deducts points for inconsistency — what's wrong with that, and what should be graded instead?
4. What role do you give PMs during an engineer-leaning segment, and what real-world dynamic does it rehearse?
5. A client offers one 2-day bootcamp instead of four weekly sessions. Name two things you change and two things you refuse to cut.

*(Answers: [instructor guide](../instructor-guide.md#module-4-answer-key))*

## Further Reading

- [Instructor Handbook](../../instructor-handbook.md) — logistics, demo prep (§2), mixed-role facilitation, common situations, what to send when
- [Level 1 Instructor Guide](../../level-1-foundational/instructor-guide.md) — the talk tracks and answer keys you'll deliver from in Lab 4C
- [Lab 4C — Capstone Teach-Back](../workshops/lab-4c-capstone-teachback.md) — the rubric you'll be graded against, worth reading before the session ends
