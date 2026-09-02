# Lab 3B — Author an Artifact Set

**Time:** 75 min · **Self-runnable:** yes
**Goal:** author all four artifact types in **your own repo** — one instruction, one prompt, one agent, one skill — and verify each one actually loads and works in Copilot Chat.

> **Keep everything you build here.** Level 4's collection lab packages this exact artifact set into a distributable collection. Commit these files on a branch when done.

## Prerequisites

- Modules 3–4 completed
- Your work repo open (not hve-core) — these artifacts encode *your team's* reality
- You know one real team standard, one recurring team task, one review-shaped workflow, and one small check worth scripting. Spend 5 minutes listing these before touching files — content quality is the lab; file mechanics are easy

Suggested timeboxes: instruction 15 min, prompt 15 min, agent 20 min, skill 20 min, buffer 5 min.

## Steps

### Part 1 — Instruction: Encode a Real Team Standard (15 min)

1. Create `.github/instructions/<your-team>/<standard>.instructions.md`:

   ```markdown
   ---
   description: "<one line: what standard, for whom>"
   applyTo: '<narrowest glob that covers it, e.g. **/*.test.ts>'
   ---

   # <Standard> Instructions

   * <3–8 rules a senior reviewer actually flags in PRs>

   ## Anti-Patterns

   ❌ <the thing people do> — <why it's a problem>
   ✅ <the correct alternative>
   ```

2. Register the directory in `.vscode/settings.json`:

   ```json
   { "chat.instructionsFilesLocations": { ".github/instructions/<your-team>": true } }
   ```

3. **Verify:** open a file matching your glob, ask Copilot Chat for a small change in that file, and read the output for your standard being applied — uninvoked. Then open a *non-matching* file and confirm the standard does not leak in.
   **Expected result:** the standard shapes output only where the glob matches.

### Part 2 — Prompt: A Recurring Team Task (15 min)

4. Pick a task someone on your team does at least weekly (standup summary, release notes, PR description, test-gap check). Create `.github/prompts/<your-team>/<task>.prompt.md`:

   ```markdown
   ---
   description: "<one line shown in the / picker>"
   argument-hint: "<what to pass, e.g. sprintNumber=...>"
   ---

   # <Task Name>

   ## Inputs

   * ${input:requiredThing}: (Required) <what it is>.
   * ${input:optionalThing:sensible-default}: (Optional) <what it is>.

   ## Requirements

   1. <numbered, checkable requirements — not vibes>
   ```

5. Register `.github/prompts/<your-team>` under `chat.promptFilesLocations` the same way.
6. **Verify:** type `/` in Copilot Chat — your prompt appears by filename with its description. Run it with a real input.
   **Expected result:** it appears in the `/` autocomplete and produces output honoring your numbered requirements.

### Part 3 — Agent: A Persona With a Handoff (20 min)

7. Create `.github/agents/<your-team>/<name>.agent.md`. Give it a real reason to be an agent (Module 4's table): tool restrictions, a multi-turn protocol, or a handoff. A read-only reviewer is the safest first agent:

   ```markdown
   ---
   name: <Your Team> <Role>
   description: "<what it does> - Brought to you by <org>/<team>"
   tools:
     - read
     - search
   handoffs:
     - label: "📋 Plan the Fixes"
       agent: Task Planner
       prompt: /task-plan
       send: true
   ---

   # <Name>

   <Purpose: one paragraph. What it reviews, against what standard.>

   ## Required Steps

   ### Step 1: <analyze inputs>
   ### Step 2: <apply your standard — reference your Part 1 instruction file>
   ### Step 3: <report findings by severity; never modify files>
   ```

8. Register `.github/agents/<your-team>` under `chat.agentFilesLocations`.
9. **Verify:** open the **agent picker** in Copilot Chat — your agent is listed by its `name`. Select it, run it against a real file, and confirm: (a) it follows your steps, (b) it cannot edit (it has no edit tools — ask it to fix something and watch it decline or produce a suggestion instead of a diff), (c) the handoff button appears when the workflow reaches it.
   **Expected result:** picker presence, protocol adherence, structural read-only behavior.

### Part 4 — Skill: Knowledge Plus a Cross-Platform Script (20 min)

10. Pick a small deterministic check (naming lint, required-file check, config validator). Create the directory — **folder name and frontmatter `name` must match, kebab-case**:

    ```text
    .github/skills/<your-team>/<skill-name>/
    ├── SKILL.md
    └── scripts/
        ├── check.sh
        └── check.ps1
    ```

    `SKILL.md`:

    ```markdown
    ---
    name: <skill-name>
    description: >-
      <What it does, in the vocabulary users would use.> Use when
      <the situations that should trigger it>.
    ---

    # <Skill Name>

    ## Protocol

    1. Run the check script for the current platform:
       * Bash: `./scripts/check.sh <args>`
       * PowerShell: `./scripts/check.ps1 <args>`
    2. <how to interpret and report results>
    ```

11. Write both scripts with identical behavior (shebang line, usage comment, non-zero exit on failure). Test each in a terminal first — the skill wraps a working script; it doesn't debug a broken one.
12. Register `.github/skills/<your-team>` under `chat.agentSkillsLocations`.
13. **Verify description-match activation:** in a fresh chat, describe the *task* without naming the skill ("check that every X in this repo has a Y") and confirm the skill activates. If it only works when named explicitly, your description doesn't match user vocabulary — rewrite the description, not the protocol.
    **Expected result:** activation by description match, script executes, result reported.

### Part 5 — Grade Your Work (buffer)

14. Run `/prompt-analyze promptFiles=<each file you authored>` on at least two of the four. Read the severity-ranked findings; fix anything critical with `/prompt-build`.

## Verify Your Work

- [ ] Four artifacts exist in your repo under `.github/{instructions,prompts,agents,skills}/<your-team>/`, and all four directories are registered in `.vscode/settings.json`
- [ ] Instruction: fires on matching files, silent on non-matching files
- [ ] Prompt: appears in `/` autocomplete; output honors its numbered requirements
- [ ] Agent: appears in the agent picker; respects tool restrictions; handoff works
- [ ] Skill: `name` matches its directory; activates on description match; both scripts run
- [ ] Everything committed to a branch — Level 4 needs it

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Artifact doesn't appear at all | The two usual suspects: directory not registered in the matching `chat.*FilesLocations` setting, or filename missing the exact suffix (`.instructions.md`, `.prompt.md`, `.agent.md`, or `SKILL.md`). Reload the VS Code window after settings changes |
| Instruction never fires | Test your glob: does the file you're editing actually match `applyTo`? Quote the glob (`'**/*.py'`) and check for typos; try a broad glob to isolate, then narrow |
| Instruction fires but gets ignored | Standards conflict — check what else stacks on that file (specificity wins); or your rules are vague. Rules a linter could check work; "write good code" doesn't |
| Prompt appears but inputs don't fill | Syntax is `${input:name}` / `${input:name:default}` exactly — no spaces inside the braces |
| Agent can still edit files | You listed an edit-capable tool or category (e.g., `edit`) in `tools:`, or omitted `tools:` entirely (omission grants everything) |
| Skill never activates by description | Rewrite the `description` in task vocabulary with a "Use when…" clause; the description drives activation, the body doesn't |
| Script works in `.sh` but not `.ps1` (or vice versa) | Behavior parity is on you — same args, same exit codes. Test both; teammates on the other OS will find it otherwise |

## What You Learned

- The four frontmatter contracts, from the producing side: `applyTo:` globs, `${input:}` variables, `tools:`/`handoffs:`/`agents:`, and skill `name`/`description`.
- Verification has a distinct shape per type: match-fire (instruction), picker-and-requirements (prompt), picker-and-restriction (agent), description-activation (skill).
- Tool restrictions and descriptions are load-bearing engineering surfaces, not metadata.

## Stretch Goal

Wire your set together: give your prompt `agent:` frontmatter delegating to your agent, and have the agent's protocol invoke your skill and cite your instruction file. Run the whole chain from a single `/` command — that's the Level 1 delegation-chain diagram, except now every link is yours.
