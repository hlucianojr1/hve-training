# Lab 4A — Build a Collection

**Time:** ~60 min · **Self-runnable:** yes
**Goal:** bundle the artifact set you authored in Level 3 [Lab 3B](../../level-3-advanced/workshops/lab-3b-author-artifact-set.md) (instruction + prompt + agent + skill) into a `my-team.collection.yml` + `my-team.collection.md` package, validate it, and test-load the artifacts in VS Code.

## Prerequisites

- Your Lab 3B artifacts, working, in your experiment repo
- [`collections/rpi.collection.yml`](https://github.com/microsoft/hve-core/blob/main/collections/rpi.collection.yml) and [`collections/rpi.collection.md`](https://github.com/microsoft/hve-core/blob/main/collections/rpi.collection.md) open as your template — a compact real collection with skills, agents, and an instruction
- For the full-validation path (Step 4, Option A): a local clone of `hve-core` with `npm install` done, PowerShell 7.4+, and the PowerShell-Yaml module

## Steps

### 1. Stage your artifacts under a collection ID

Pick a collection ID: lowercase, hyphens only (`^[a-z0-9-]+$`) — this lab uses `my-team`; use your real team's name. Verify each Lab 3B artifact sits in a **subdirectory** named for it:

```text
.github/
  agents/my-team/{name}.agent.md
  prompts/my-team/{name}.prompt.md
  instructions/my-team/{name}.instructions.md
  skills/my-team/{skill-name}/SKILL.md
```

If any artifact sits at the root of `.github/{type}/` (no subdirectory), move it now — root-level artifacts are repo-specific by convention and the collection validator **rejects** them in manifests.

### 2. Write the manifest

Create `collections/my-team.collection.yml` (in your repo, mirroring the hve-core layout), following `rpi.collection.yml`'s shape:

```yaml
id: my-team
name: My Team Workflow
description: <one honest sentence — what workflow does this bundle serve?>
maturity: experimental
tags:
  - <domain tag>
  - <technology tag>
items:
  - path: .github/agents/my-team/<name>.agent.md
    kind: agent
    maturity: experimental
  - path: .github/prompts/my-team/<name>.prompt.md
    kind: prompt
    maturity: experimental
  - path: .github/instructions/my-team/<name>.instructions.md
    kind: instruction
    maturity: experimental
  - path: .github/skills/my-team/<skill-name>
    kind: skill
    maturity: experimental
```

Get the details right — these are the errors the validator actually catches:

- `path` is repo-root-relative; the **skill entry points at the directory**, not `SKILL.md`
- `kind` must match the file suffix (`agent` → `.agent.md`, etc.)
- `maturity: experimental` on the collection *and* each item — new collections start experimental and graduate by editing one field
- **If your agent declares subagents in its `agents:` frontmatter, add an `items[]` entry for every subagent** — the plugin pipeline does not resolve transitive dependencies

### 3. Write the description

Create `collections/my-team.collection.md`: 5–15 lines covering what the collection is for, the key artifacts (name each with its slash command or invocation), and when a team would install it. Look at `rpi.collection.md` for tone — note that it also documents its own local-testing settings, which you'll use in Step 5.

### 4. Validate

**Option A — run the real validator (clone-based installs).** The validator, [`scripts/collections/Validate-Collections.ps1`](https://github.com/microsoft/hve-core/blob/main/scripts/collections/Validate-Collections.ps1), scans the `collections/` directory of the hve-core repo it lives in — it has no flag to target an external file, and it checks that every `path` exists. So validate the way a contributor would: in your hve-core clone (use a scratch branch), copy your four artifacts into the matching `.github/{type}/my-team/` locations and your manifest + description into `collections/`, then:

```bash
npm run plugin:validate
```

Expected output: a pass across all manifests including `my-team`, with results written to `logs/collection-validation-results.json`. To see the distribution story end-to-end, also run `npm run plugin:generate` and inspect the generated `plugins/my-team/` directory — symlinked artifacts, generated README, `plugin.json`. Don't commit any of this to main; it's a scratch branch.

**Option B — structural validation (extension-only installs).** The tooling is repo-internal; without a clone you validate against the schema by inspection — an honest limitation, so be systematic. Check every box:

- [ ] `id` matches `^[a-z0-9-]+$` and matches the filename `{id}.collection.yml`
- [ ] `id`, `name`, `description`, `items` all present
- [ ] Every `path` exists in your repo (open each one)
- [ ] Every `kind` is one of `agent`/`prompt`/`instruction`/`skill`/`hook` and matches the path suffix; skill paths are directories containing `SKILL.md`
- [ ] Every `maturity` is one of `stable`/`preview`/`experimental`/`deprecated`/`removed`
- [ ] No duplicate `path` entries; no root-level `.github/{type}/` paths
- [ ] All subagents referenced by your agent's frontmatter appear as items

### 5. Test-load the artifacts locally

In your own repo, artifacts in the conventional `.github/` locations are discovered by Copilot as you saw in Level 3. To test loading from **non-standard folders** — which is how collection packaging gets verified before release — use the workspace settings documented in `rpi.collection.md` (`.vscode/settings.json`):

```json
{
  "chat.agentSkillsLocations": {
    ".github/skills/my-team": true
  },
  "chat.agentFilesLocations": {
    ".github/agents/my-team": true
  }
}
```

Then verify in Copilot Chat: your agent appears in the agent picker, your prompt appears as a slash command, and asking for a task in the skill's domain triggers it. One documented host constraint to know: `chat.promptFilesLocations` supports **whole-directory toggles only** — you cannot disable a single conflicting prompt file, only its directory. If your prompt name collides with an installed one, rename yours or toggle the conflicting directory.

## Verify Your Work

- [ ] `my-team.collection.yml` and `my-team.collection.md` exist and mirror the `rpi` template's structure
- [ ] All four (or more, with subagents) items carry correct `path`, `kind`, and `maturity: experimental`
- [ ] Validation passed — Option A's `npm run plugin:validate`, or every Option B checkbox (and you noted which path you took)
- [ ] Your agent, prompt, and skill load and respond in Copilot Chat
- [ ] You can say in one sentence why the instruction item needs no "loading" at all (it auto-applies by `applyTo` glob from your repo's `.github/instructions/`)

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Validator: "kind 'skill' expects a directory containing SKILL.md" (or similar suffix errors) | Skill `path` must be the directory; check every kind-suffix pairing from Step 2 |
| Validator flags a path as missing | Paths are repo-root-relative — in Option A that's the hve-core clone root, so the artifact must be copied in at exactly that path |
| Validator rejects a root-level artifact path | Move the artifact into a `{collection-id}/` subdirectory and update the manifest |
| Agent doesn't appear in the picker after Step 5 | Check the settings key is `chat.agentFilesLocations`, the path is workspace-relative, and reload the VS Code window |
| Your prompt's slash command runs the wrong (installed) prompt | Directory-scope conflict — rename your prompt or disable the conflicting prompts directory; per-file disabling isn't supported |
| `plugin:generate` output missing your subagent | It was absent from `items[]` — transitive dependencies are never auto-resolved; add it and regenerate |

## What You Learned

- A collection is a two-file contract: the manifest selects and gates; the description sells and explains.
- Validation is mechanical and strict — path existence, kind-suffix consistency, maturity vocabulary — which is exactly why manifests are trustworthy as a source of truth.
- Dependency completeness is the author's job; the pipeline won't save you from a forgotten subagent.
- `experimental` is the honest starting maturity: distribution-ready is a promotion decision, not a default.

## Stretch Goal

Take the distribution story one step further in your hve-core scratch branch: after `npm run plugin:generate`, install your own plugin into Copilot CLI from the local marketplace and invoke your agent from the terminal. Then note what *didn't* come along — your instruction file's auto-application ([ref #6](../../resources/limitations-and-workarounds.md)) — and write the one-line workaround you'd give a CLI-only teammate.
