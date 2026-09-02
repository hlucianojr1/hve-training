# Level 1 — Instructor Guide

Companion to the [Level 1 README](README.md) agenda. General facilitation rules: [Instructor Handbook](../instructor-handbook.md).

## §1 Opening (0:00–0:15)

- Setup verification first, teaching second: "Open Copilot Chat, type `@` — thumbs up if you see `task-researcher`." Anyone stuck pairs with a neighbor on [Lab 1A](workshops/lab-1a-install-verify.md) during Module 1; don't hold the room.
- Frame the course in one line: *"This is a course about making AI coding verifiable instead of plausible. Four levels; today is why + first workflow."*
- Set the artifact norm early: "Every exercise in this course produces files. When I ask to see your work, I'm asking to see files, not chat."

## §2 Module 1 — Why HVE (0:15–1:00)

**Talk track beats:**

1. Open with the war-story discussion prompt (5 min) *before* any slides — the room supplies the motivation. Collect 2–3 stories; you'll callback to them all day ("remember Maria's phantom API? Research phase catches exactly that").
2. The Terraform framing from `why-rpi.md` — read it verbatim, it's well-written.
3. The one sentence to land hard, twice: **"When AI knows it cannot implement, it stops optimizing for plausible code and starts optimizing for verified truth."**
4. The `prefix` vs `resource_prefix` example — smallest possible illustration of plausible-vs-verified.
5. Honest notes: first workflow feels slower (say it before they feel it); HVE is not a safety layer; the Transparency Note's "never use for developer performance assessment" line matters in a consulting context — clients will ask.

**Demo (10 min):** side-by-side of a plain-chat answer vs a research doc for the same task. Prepare both beforehand from your demo repo. Let the citations do the arguing.

**Anticipated questions:**

- *"Isn't this just waterfall for AI?"* — No: cycles run in minutes-to-hours, iteration between phases is expected and cheap; it's phase *separation*, not phase *rigidity*.
- *"Our team already reviews AI code carefully — why the ceremony?"* — Review catches bad output after generation; RPI prevents bad generation by fixing inputs (verified research). Cheaper failure point.
- *"Does this work with [Cursor/JetBrains/other]?"* — Supported clients are current VS Code Copilot Chat and Copilot CLI; anything else is uncharacterized per the Transparency Note. Don't oversell portability.

### Module 1 answer key

1. It generates plausible output because it can't distinguish investigating from implementing — asked for code, it writes code without verifying conventions, APIs, or context.
2. Knowing it can never write the code; it starts optimizing for verified truth (citations, existing patterns) rather than plausible code.
3. Research → research doc; Plan → plan (+details); Implement → changes log; Review → review/findings log. All under `.copilot-tracking/`.
4. Any two of: not a model; not a runtime; not a safety layer / no content filtering; not a substitute for human review.
5. Any of: automated decisions in regulated areas; inferring protected characteristics; assessing developer performance from telemetry/review verdicts; synthetic media of real people; sole basis for high-stakes decisions.

## §3 Module 2 — Setup & First Contact (1:00–1:35)

- The memory-agent follow-along is the session's first hands-on: budget the full 10 minutes, circulate. Common snag: no workspace folder open → no `memories/` file.
- The catalog race (find the ADR / PR-review / sprint-planning agents) creates useful competitive energy — answers: `adr-creation`, `code-review` (or `/pr-review` prompt), `github-backlog-manager` (or ADO/Jira equivalents). Accept any defensible answer; the skill is *finding*, not memorizing.
- Emphasize the gitignore step now and check it again during Lab 1C review — it's the most-skipped step and the most annoying to discover late.

### Module 2 answer key

1. `.copilot-tracking/` — it fills with ephemeral workflow artifacts that must not enter repo history.
2. Instructions — the CLI plugin spec has no instructions component, so `applyTo` matching never fires.
3. `.github/CUSTOM-AGENTS.md` for agents; `.github/prompts/README.md` for prompts.
4. Agents produce artifacts (files), and other agents read them — context persists on disk, not in conversation.

## §4 Module 3 — Artifact Types & Collections (1:45–2:25)

- The four-file walkthrough is the core demo — have all four files bookmarked. Keep it brisk: frontmatter is the story, not file bodies.
- The invisible-instructions follow-along lands the "systematic vs on-demand" distinction. If a student's stack isn't covered (e.g., Go), turn it into the Level 3 teaser: "you'll author that instruction file yourself in two weeks."
- For PMs: frame collections as the procurement/governance unit — "your adoption decision will be per-collection."

### Module 3 answer key

1. Instructions — automatic activation when the current file matches the `applyTo:` glob.
2. Prompt: "what does the user want?" Agent: "how should this execute?" Instruction: "what standards apply here?" Skill: "what utility does this need?"
3. (a) Which collection it's in (`design-thinking`) and whether it's installed; (b) its maturity vs the channel (design-thinking is Preview — not on the stable-channel flagship extension).
4. Instructions are passive reference auto-injected by glob; skills are active execution — real scripts invoked for a task.
5. The collection manifests: `collections/*.collection.yml`.

## §5 Module 4 — First RPI Workflow (2:35–3:20)

- **Pre-bake per Handbook §2.** Start research live for 2–3 minutes of real behavior, then switch to your pre-baked artifacts. Announce the switch honestly — "research runs ~20 minutes; here's one I ran earlier" — modeling honest expectations *is* teaching HVE.
- The single most valuable on-screen moment: typing `/clear`, then **reopening the research doc** before planning. Narrate it: "history is gone; the artifact isn't."
- Reserve the last 15 minutes for the Lab 1C start. The stall point is always task selection — push students toward "smaller than you think" and enforce the 1–3 file criterion.

### Module 4 answer key

1. Select agent → prompt it → it produces an artifact → read the artifact → `/clear` and restore into the next phase.
2. From the artifact on disk: the research doc is reopened in the editor (or its path referenced in the prompt).
3. `/task-research` is a prompt entry that auto-switches to the Task Researcher agent; `/rpi-research` is a skill entry providing the same workflow without an agent switch.
4. Recency bias — accumulated implementation tokens (50K+) dominate attention over the ~3K tokens of phase instructions, so the model pattern-matches to "keep implementing."
5. Read the artifact it produced.

## §6 Close (3:20–3:30)

- Run the four knowledge checks as a rapid group quiz (pick 1–2 questions per module).
- Lab briefing: 1B and 1C due before Level 2; bring their `.copilot-tracking/` folders to show. Hand out (or link) the [cheat sheet](../resources/cheat-sheet.md).
- Preview Level 2: "Today you followed the workflow. Next time you'll learn why it breaks, how to see it breaking, and how to run it without training wheels — and we split into role tracks."
