# Lab 1B — First Research on Your Own Repo

**Time:** ~30 min · **Self-runnable:** yes
**Goal:** run the Research phase alone, on a question you actually care about, and learn to read a research artifact critically.

## Prerequisites

- Lab 1A completed (working install, gitignored `.copilot-tracking/`)
- Your experiment repo open in VS Code

## Pick Your Research Question

Choose something you **genuinely don't fully know** about your own codebase. Good shapes:

- "How is error handling structured across the services in this repo?"
- "What patterns does this codebase use for configuration and secrets?"
- "What would be affected if we upgraded [dependency X]?"

Selection criteria (keeps the lab bounded):

- Answerable from this repo (not "compare five frameworks" — that's a bigger research mode)
- Specific enough to finish in ~5 minutes of agent time — name the module/area, don't say "the whole system"
- The answer would actually be useful to you this month

Contrived questions teach the mechanics; real questions teach the mechanics *and* produce something you'll reuse.

## Steps

1. Open Copilot Chat, select **Task Researcher** (or invoke `/task-research`).
2. Send your question. Add 2–4 bullets of scope, mirroring the official tutorial's shape:

   ```text
   Research how error handling is structured in this repository.

   Consider:
   * The main service entry points and middleware
   * Any shared error/exception utilities
   * How errors reach logs and the user
   * Inconsistencies between modules
   ```

3. Wait (2–5 minutes for a well-scoped question). Watch the tool calls scroll by — it's searching and reading, not generating.
4. Open the artifact: `.copilot-tracking/research/<date>-<topic>-research.md`.
5. **Read it critically** with this checklist:
   - Pick any three factual claims. Does each cite a file (ideally file:line)? Open one citation and confirm it says what the doc claims.
   - Find the section on open/remaining questions — what did it admit it doesn't know?
   - Find one thing *you* know about your codebase that the research missed or got wrong. (There usually is one — this is the point.)

## Verify Your Work

- [ ] A research document exists in `.copilot-tracking/research/`
- [ ] You verified at least one citation by opening the cited file
- [ ] You identified at least one gap or error in the research
- [ ] You can answer: "would I hand this doc to a teammate as a starting point?" with a reasoned yes/no

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Research wanders for 15+ min | Your question was too broad — stop it, narrow to one module/area, retry |
| Output is a chat answer, no file created | Confirm you're on the Task Researcher agent / used `/task-research`; confirm a workspace folder is open |
| Findings are shallow ("this repo uses functions") | Add the "Consider:" scope bullets — steering scope is the skill this lab trains |
| Citations reference files that don't exist | Note it — models can still hallucinate paths; this is exactly why you verify. Re-prompt: "verify every file path you cited exists" |

## What You Learned

- Research is a *constrained investigation* that produces a citable artifact, not a chat answer.
- The artifact is only as good as your scoping — the "Consider:" bullets are your steering wheel.
- **You are still the reviewer.** Verified-looking is not verified; you spot-check citations. This habit is what separates engineering with AI from trusting AI.

## Stretch Goal

Run a second research with a deliberately vague version of your question ("research error handling") and diff the two artifacts. The quality gap you see is your scoping skill, made visible.
