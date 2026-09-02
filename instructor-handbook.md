# Instructor Handbook

Cross-level facilitation guide for delivering the HVE training curriculum. Read this before your first session; per-level agendas, talk tracks, and answer keys live in each level's `instructor-guide.md`.

## 1. Session Logistics

**Format per level:** one 3–4 hour instructor-led session (with two 10-minute breaks) followed by self-paced labs students complete before the next level.

**Room requirements:**

- Every student on their own laptop with VS Code + Copilot license verified *before* the session (send Lab 1A as pre-work for Level 1; verify with a show-of-hands "type `@` in Copilot Chat — do you see `task-researcher`?" in the first 10 minutes).
- Screen sharing/projector for live demos; a second screen or split view helps (chat panel + artifact file side by side — the artifact is the star, keep it visible).
- A shared channel (Teams/Slack) for pasting prompts so students don't retype them.

**Cohort size:** 6–16 works well. Above 16, add a co-facilitator for lab-time floor support.

**Pacing rule:** module concepts should never run past 25 minutes without students touching their keyboards. Every module has follow-along steps — use them.

## 2. Demo Preparation (read this twice)

The single biggest live-demo failure mode: **a real research phase runs 20–60 minutes autonomously.** Never start a cold `/task-research` live and wait.

The pre-baked artifact pattern:

1. The week before, run the demo task end-to-end yourself in a repo you'll project. Keep every artifact `.copilot-tracking/` produced (research doc, plan + details, changes log, review).
2. In the live session, *start* the real research so students see the invocation, the agent picker, and the first tool calls (2–3 minutes of live behavior is convincing).
3. Then switch to the pre-baked artifact: "here's what this produces when it finishes" — walk the document, point at citations with file:line references.
4. For Plan/Implement demos, the pre-baked research doc is your input; those phases are faster and can often run truly live.

Keep a **fallback branch** of the demo repo at the pre-demo state so you can reset between cohorts (`git checkout demo-start`).

**Copilot flakiness contingency:** if Copilot Chat misbehaves live (rate limits, outage, model unavailability), narrate over your pre-baked artifacts entirely. The artifacts *are* the teaching content; the live typing is theater.

## 3. Mixed-Role Facilitation

Levels 1–2 are designed so PMs/TPMs are never spectators:

- During engineer-leaning segments (e.g., reading a plan's implementation details), give PMs the **artifact-reader role**: "you'll be consuming these documents from your team — what would you ask about this plan?"
- Backlog and planning segments (Level 2 Module 4 PM track, Level 3 Module 1) flip the dynamic; engineers become reviewers of BRD/PRD outputs.
- Pair mixed roles in labs when possible: engineer drives, PM reads artifacts aloud and challenges them. This mirrors real HVE team usage where artifacts are the collaboration surface.
- When a role track splits (Lab 2C), run tracks in parallel breakouts and reconvene for a 10-minute cross-track show-and-tell.

## 4. Handling Common Situations

| Situation | Response |
|-----------|----------|
| Student's agent skips phases mid-lab | Teaching moment, not a failure — point them to the four degradation symptoms (Level 2 Module 2) and have them `/clear` and retry. Narrate it to the room. |
| Student lacks `runSubagent` tool (rpi-agent won't orchestrate) | Expected on some Copilot configurations. Fall back to strict RPI — that's the documented workaround and reinforces the methodology. |
| "Why not just prompt ChatGPT/Copilot directly?" | Don't argue — demo. Show the why-rpi Terraform failure framing, then a research doc with file:line citations. Plausible vs verified is the whole course. |
| Student on CLI instead of VS Code | Works for prompts/agents/skills, but **instructions are not auto-applied from CLI plugins**. Flag it, let them proceed, revisit in the install-methods segment. |
| "Is this task too small for RPI?" | Yes is a valid answer — teach the decision matrix (strict RPI vs rpi-agent vs plain chat). HVE is not a hammer for every nail. |
| Outputs vary between students | Expected — models are non-deterministic and HVE pins nothing. Grade labs on artifacts produced and process followed, never on identical output. |

## 5. Assessment Mechanics

- **Knowledge checks** end every module: 3–5 questions, student-facing without answers. Answer keys are in each level's `instructor-guide.md`. Run them as 5-minute group discussions, not tests — wrong answers surface misconceptions worth addressing live.
- **Exit criteria** per level are practical demonstrations (in each level README). A student passes a level by showing the artifacts their labs produced. Ask to see `.copilot-tracking/` contents, not screenshots of chat.
- **Capstone rubric** for Level 4 teach-back is in [Lab 4C](level-4-expert/workshops/lab-4c-capstone-teachback.md).

## 6. What to Send When

| When | Send |
|------|------|
| 1 week before Level 1 | [Lab 1A](level-1-foundational/workshops/lab-1a-install-verify.md) as pre-work + license check instructions |
| After each session | That level's `workshops/` links + the [cheat sheet](resources/cheat-sheet.md) |
| Before Level 2 | Ask students to bring "one small real task in your repo" (Lab 2A selection criteria) |
| Before Level 3 | Ask students to bring "one real feature idea" for the planning-chain lab |
| Before Level 4 | Students review their Level 3 artifacts — Lab 4A packages them into a collection |

## 7. Instructor Self-Preparation Checklist

Before teaching each level, you should be able to, without notes:

- **Level 1:** explain the why-rpi thesis in 2 minutes; name the four artifact types and one example of each; run the memory-agent first-interaction cold.
- **Level 2:** run a full strict RPI cycle; name all four degradation symptoms and the `/clear` / `/compact` / `/checkpoint` decision; argue both sides of strict-vs-rpi-agent.
- **Level 3:** whiteboard the delegation chain (User → Prompt → Agent → Instructions/Skills) and the 9-stage lifecycle; have authored at least one artifact of each type yourself.
- **Level 4:** have built and validated one collection; know the install-method decision matrix; have read `TRANSPARENCY-NOTE.md` end to end.

If any of these feel shaky, the corresponding lab is your remediation — do it as a student first.
