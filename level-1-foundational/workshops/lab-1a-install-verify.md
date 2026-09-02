# Lab 1A — Install & Verify

**Time:** ~20 min · **Self-runnable:** yes (also sent as pre-work before the Level 1 session)
**Goal:** a verified HVE Core installation in a repo you can safely experiment in.

## Prerequisites

- VS Code (1.106+ recommended — check **Help → About**) with **GitHub Copilot** and **GitHub Copilot Chat** extensions signed in and licensed (the Copilot icon in the status bar should not show an error)
- Git installed; a work repository you can experiment in without risk (a side project or a scratch clone of a work repo is fine)

## Steps

1. **Install the extension.** In VS Code: Extensions view (`Ctrl+Shift+X`) → search **HVE Core** → install the one published by `ise-hve-essentials` → reload when prompted.
   - *Expected:* the extension shows as installed; no error notifications.

2. **Verify agents are discoverable.** Open Copilot Chat (`Ctrl+Alt+I`) → type `@` in the message box.
   - *Expected:* the agent list includes `task-researcher`, `task-planner`, `task-implementor`, `task-reviewer`, and `memory`.

3. **Verify prompts.** Type `/` in the chat box and start typing `task-`.
   - *Expected:* `/task-research`, `/task-plan`, `/task-implement`, `/task-review` appear. Also check `/rpi-research` exists.

4. **Gitignore the tracking directory.** Open your experiment repo, add this line to `.gitignore` (create the file if needed):

   ```text
   .copilot-tracking/
   ```

   Commit that change.
   - *Expected:* `git status` no longer shows `.copilot-tracking/` even after later labs create it.

5. **First contact.** Select the **Memory** agent in the picker and send:

   > Remember that I am a [your role] and I'm learning HVE Core for the first time.

   - *Expected:* the agent confirms and creates a file under `memories/` in your workspace. Open it and read it.

6. **Prove persistence.** Open a **new** chat thread (not `/clear` — an actually new thread) and ask:

   > Explain what this repository does and how it helps someone in my role.

   - *Expected:* the answer references your role without you restating it.

## Verify Your Work

- [ ] `@` in Copilot Chat lists the four `task-*` agents and `memory`
- [ ] `/task-research` and `/rpi-research` both autocomplete
- [ ] `.copilot-tracking/` is in your repo's `.gitignore`, committed
- [ ] A file exists under `memories/` containing your role
- [ ] A fresh thread knew your role without being told

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| No HVE agents in the `@` list | Reload VS Code; confirm the publisher is `ise-hve-essentials`; see [troubleshooting](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/troubleshooting.md) |
| Copilot Chat itself won't respond | License/sign-in problem, not HVE — check the Copilot status bar icon first |
| Both HVE Core All *and* HVE Installer installed | Known conflict — uninstall one ([troubleshooting guide](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/troubleshooting.md)) |
| Memory agent responded but no `memories/` file | Make sure a workspace folder is open (not a single loose file) and retry |

## What You Learned

HVE is a set of markdown artifacts Copilot discovers; installing it is just making them visible. Agents produce real files (you've now seen `memories/`), and those files — not chat history — are how context persists. That pattern scales up to everything else in this course.

## Stretch Goal

Skim the two catalogs you'll use all course: [`.github/CUSTOM-AGENTS.md`](https://github.com/microsoft/hve-core/blob/main/.github/CUSTOM-AGENTS.md) and [`.github/prompts/README.md`](https://github.com/microsoft/hve-core/blob/main/.github/prompts/README.md). Don't memorize — just learn their shape.
