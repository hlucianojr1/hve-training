# Module 1.2 — Setup & First Contact

**Time:** 35 min · **Roles:** all · **Source docs:** [`docs/getting-started/install.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/install.md), [`docs/getting-started/first-interaction.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/first-interaction.md)

## Objectives

- Have a verified, working HVE Core installation.
- Understand the install options well enough to know which one you're on and why.
- Complete your first agent interaction (memory agent) and see the artifact pattern in miniature.

## 1. What "Installing HVE" Means

HVE artifacts are markdown files that GitHub Copilot discovers. Installation = getting those files where Copilot can see them. Three main paths:

| Path | What you get | When |
|------|-------------|------|
| **HVE Core** extension (Marketplace, `ise-hve-essentials.hve-core`) | Flagship collection, ~68 artifacts, auto-updates | **Default — this course assumes it** |
| **HVE Core All** extension | Everything (~260 artifacts) | Exploring all domains |
| **HVE Installer** extension | Pick collections selectively | Teams wanting specific domains |
| CLI plugin: `copilot plugin install hve-core@hve-core` | Same artifacts on Copilot CLI | Terminal-first users — **with a catch (below)** |
| Clone-based methods (peer clone, submodule, …) | Modifiable source | Level 4 territory |

**Follow-along (everyone, 5 min):** verify your install — open Copilot Chat (`Ctrl+Alt+I`), type `@`, confirm you see `task-researcher`, `task-planner`, `task-implementor`. If not, raise a hand (troubleshooting: [`docs/getting-started/troubleshooting.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/troubleshooting.md)).

**The one mandatory post-install step:** add `.copilot-tracking/` to your project's `.gitignore`. Every workflow artifact lands there; none of it belongs in your repo history.

## 2. First Contact: the Memory Agent

**Follow-along (everyone, 10 min)** — from [`first-interaction.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/first-interaction.md):

1. In Copilot Chat, select the **Memory** agent from the agent picker.
2. Prompt: `Remember that I am a [your role] and I'm learning HVE Core for the first time.`
3. Open the file it creates under `memories/` — your role, stored as a persistent note.
4. Open a **new chat thread** and ask: `Explain what this repository does and how it helps someone in my role.`
5. Notice: the response references your role without you repeating it.

You just proved four things: HVE is installed, agents respond, the memory system creates real files, and other agents read those files.

> This is the whole HVE pattern in miniature: **agents produce artifacts, and artifacts carry context** — you'll see the same move at full scale in Module 4.

## 3. Knowing What Exists

You don't memorize 68 artifacts; you learn where the catalogs are:

- [`.github/CUSTOM-AGENTS.md`](https://github.com/microsoft/hve-core/blob/main/.github/CUSTOM-AGENTS.md) — every agent in 7 families, each with purpose and **key constraint**
- [`.github/prompts/README.md`](https://github.com/microsoft/hve-core/blob/main/.github/prompts/README.md) — every prompt in 7 families, plus the prompts-vs-instructions-vs-agents distinction

**Follow-along (5 min):** find the agent you'd use to (a) create an ADR, (b) review a PR, (c) plan a sprint from GitHub issues. Race — first to paste all three names in the shared channel wins.

## Use Case Spotlight

A consultant landing in a new client codebase runs the memory agent first ("I'm a senior engineer on the payments team, focused on the billing service") — every subsequent agent tailors its output to that context for the rest of the engagement, across sessions.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| CLI plugins do **not** auto-apply instructions (`applyTo` never fires) | Prefer VS Code extension when coding standards matter; on CLI, reference instruction files explicitly ([ref #6](../../resources/limitations-and-workarounds.md)) |
| HVE Core All + HVE Installer together cause collection conflicts | Pick one; see [troubleshooting](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/troubleshooting.md) ([ref #9](../../resources/limitations-and-workarounds.md)) |
| Handoff buttons need VS Code 1.106+ | Typed slash commands always work ([ref #10](../../resources/limitations-and-workarounds.md)) |
| Only current VS Code Copilot Chat + Copilot CLI are supported clients | Other IDEs/hosts = uncharacterized territory ([ref #18](../../resources/limitations-and-workarounds.md)) |
| Memory files are plain files in your workspace | Check `memories/` before sharing a workspace or screen ([ref #20](../../resources/limitations-and-workarounds.md)) |

## Knowledge Check

1. What's the one mandatory `.gitignore` addition after any install method, and why?
2. Your teammate uses Copilot CLI exclusively. Which artifact type silently won't apply for them?
3. Where do you look to find out which agent handles a task you've never done before?
4. What did the memory-agent exercise demonstrate about how HVE carries context?

*(Answers: [instructor guide](../instructor-guide.md#module-2-answer-key))*

## Further Reading

- [Installing HVE Core](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/install.md) — full decision matrix
- [Comparing Setup Methods](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/comparison.md) — the eight methods in depth (revisited in Level 4)
