# Module 4.1 — Collections & Distribution

**Time:** 45 min · **Roles:** all · **Source docs:** [`docs/customization/collections.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/collections.md), [`docs/architecture/ai-artifacts.md`](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md), [`docs/getting-started/methods/comparison.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/comparison.md)

## Objectives

- Author a collection manifest from scratch: the two-file package, every field, and the rules the validator enforces.
- Explain the maturity ladder and how it gates what ships on each release channel.
- Trace how one manifest builds two distributables — a VS Code extension and a CLI plugin.
- Pick the right install method for any environment × team × update-cadence combination.
- Know the enterprise options when the public repo isn't enough: forking and artifact mirrors.

## 1. The Two-File Package

A collection is two files in `collections/`:

- `{id}.collection.yml` — the **manifest**: the machine-readable source of truth listing every artifact
- `{id}.collection.md` — the **description**: human-readable purpose, key artifacts, when to install it

Top-level manifest fields: `id` (lowercase kebab-case, must match `^[a-z0-9-]+$`), `name`, `description`, `items` (all required); `maturity`, `tags`, and `display` (`ordering: alpha` or `manual`) are optional. Each `items[]` entry:

| Field | Required | Notes |
|-------|----------|-------|
| `path` | Yes | Repo-relative. Skills reference the skill **directory**, not `SKILL.md` |
| `kind` | Yes | `agent`, `prompt`, `instruction`, `skill`, or `hook` |
| `maturity` | No | Item-level override; defaults to `stable` when omitted |
| `usage` | No | Optional usage guidance |

**Instructor walkthrough (10 min):** open [`collections/rpi.collection.yml`](https://github.com/microsoft/hve-core/blob/main/collections/rpi.collection.yml) — the compact template you'll mirror in Lab 4A. Trace one skill item and one agent item to their source files, then to the built plugin output under `plugins/`. Note what's *absent*: no dependency fields, no version numbers. Manifests select and gate; they don't resolve.

Two placement rules the validator enforces:

- **Root-level artifacts are repo-specific.** Files directly under `.github/{type}/` (no subdirectory) govern this repo's own CI and conventions — they MUST NOT appear in any manifest. Distributable artifacts live in subdirectories like `.github/agents/{collection-id}/`.
- **Kind must match suffix.** `agent` → `*.agent.md`, `prompt` → `*.prompt.md`, `instruction` → `*.instructions.md`, `skill` → a directory containing `SKILL.md`.

## 2. The Maturity Ladder

Maturity lives in the collection manifest — **never** in artifact frontmatter. The ladder: `experimental` → `preview` → `stable`, plus two retirement states, `deprecated` and `removed`. Omitted means `stable`.

Maturity gates at two levels, and the channel rules differ:

| Maturity | Item-level: stable channel | Item-level: pre-release | Collection-level: stable channel | Collection-level: pre-release |
|----------|---------------------------|-------------------------|----------------------------------|-------------------------------|
| `stable` | Included | Included | Included | Included |
| `preview` | Excluded | Included | Included | Included |
| `experimental` | Excluded | Included | Excluded | Included |
| `deprecated` | Excluded | Excluded | Excluded | Excluded |
| `removed` | Excluded | Excluded | Excluded | Excluded |

New collections (and new items you're unsure about) start `experimental` and graduate by changing a single field — that's your Lab 4A setting. `removed` is the manifest-only way to withdraw an artifact from every distribution while its source file stays put for history; the alternative, moving files to `.github/deprecated/{type}/`, is path-based archival the build excludes automatically. Module 2 covers when to use which as a change-management decision.

## 3. One Manifest, Two Distributables

The same YAML drives two build pipelines:

- **VS Code extension** (`.vsix`): `Prepare-Extension.ps1` + `Package-Extension.ps1`, parameterized by collection — this is how `ise-hve-essentials.hve-core` and the other marketplace extensions are produced, one per collection, on stable and pre-release channels.
- **Copilot CLI plugin**: `npm run plugin:generate` creates symlink-based plugin structures under `plugins/{collection-id}/` with a generated README and `plugin.json`. Consumers install via `copilot plugin marketplace add microsoft/hve-core` then `copilot plugin install {id}@hve-core`.

Rules of the road: files under `plugins/` are **generated output — never edit them directly**; regenerate after any manifest or frontmatter change, and commit source + manifest + generated output together (CI blocks merge on a mismatch).

## 4. Dependencies: Mostly Manual

Manifests do not declare dependencies, so completeness is on you:

- **Subagents must be listed explicitly.** If your agent's `agents:` frontmatter names subagents, every subagent file needs its own `items[]` entry — the plugin generation pipeline does not resolve transitive agent dependencies. Omit one and the installed parent silently loses that capability.
- **Handoffs are resolved for extensions only.** Extension packaging (`Resolve-HandoffDependencies`) walks `handoffs:` frontmatter breadth-first and pulls in every reachable agent so UI buttons work.
- Agents can declare `requires` on other artifacts, and the resolver completes those graphs at install — but don't lean on it in your own manifests: list what you ship.

## 5. The Eight Install Methods

The [decision matrix](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/comparison.md) answers three questions — environment, solo vs team, auto vs controlled updates:

| Environment | Team | Updates | Method |
|-------------|------|---------|--------|
| Any (simplest) | Any | Auto | **VS Code Extension** ⭐ |
| Local, no container | Solo | Manual | Peer Directory Clone |
| Local (any) or Codespaces | Team | Controlled | **Submodule** |
| Local devcontainer | Solo | Auto | Git-Ignored Folder |
| Codespaces only | Solo | Auto | GitHub Codespaces |
| Both local + Codespaces | Any | Any | Multi-Root Workspace |
| Advanced shared install | Solo | Auto | Mounted Directory |
| Any (CLI preferred) | Any | Manual | CLI Plugins |

The pattern worth memorizing: **extension for simplicity, clone-based methods for customization, submodule whenever a team needs version control**. Two traps: CLI plugins never auto-apply instructions (the plugin spec has no `instructions` component — `applyTo` matching only fires from the project's own `.github/instructions/`), and the mounted-directory method does not work in Codespaces.

## 6. Enterprise Distribution

When a client can't consume the public repo as-is:

- **In-place customization first** — add your own artifacts and collections alongside HVE's; this is what Lab 4A practices and it carries no merge burden.
- **Fork** ([`docs/customization/forking.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/forking.md)) when you must replace core agents, modify the build pipeline, or run a private distribution channel. The cost is a permanent upstream-sync obligation — the doc's sync-vs-skip table is your merge playbook.
- **Artifact hub / air-gapped networks** ([`docs/customization/enterprise-artifact-hub.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/enterprise-artifact-hub.md)): `HVE_*` environment variables (`HVE_GITHUB_RELEASES_URL`, `HVE_PSGALLERY_REPOSITORY`, `HVE_DEVCONTAINER_IMAGE`, …) redirect tool downloads and module installs to internal mirrors; unset means public defaults, so nothing to configure on open networks.

## 7. Validation Tooling

Two commands, in this order, before any collection change merges:

```bash
npm run plugin:validate   # alias of lint:collections-metadata → scripts/collections/Validate-Collections.ps1
npm run plugin:generate   # regenerate plugins/ from the manifests
```

The validator checks YAML syntax, required fields, that every `path` exists, valid `kind` and `maturity` values, kind-suffix consistency, duplicate paths, and the root-level exclusion rule. Note its scope honestly: it validates **all manifests in the repo's `collections/` directory** — there is no flag to point it at an arbitrary external file, which shapes how you validate your own collection in Lab 4A.

## Use Case Spotlight

Contoso's platform architect bundles API-design instructions, a service-mesh skill, and an architecture-review agent into a `microservices-standards` collection. New teams onboarding to the platform install **one thing** and receive the whole governance surface — no wiki page of files to copy, no drift between teams. When the review agent matures, one `maturity` field change promotes it from pre-release adopters to everyone. The collection is the unit of adoption, promotion, and rollback.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| CLI plugins do not auto-apply instructions — no `instructions` component in the plugin spec | Prefer the extension where coding-standards enforcement matters; on CLI, reference instruction files explicitly ([ref #6](../../resources/limitations-and-workarounds.md)) |
| Mounted-directory install does not work in GitHub Codespaces | Use extension or submodule for Codespaces; consult the decision matrix ([ref #7](../../resources/limitations-and-workarounds.md)) |
| Marketplace extension can't be modified, no git control, per-member install | Clone-based methods (peer clone, submodule) when customization or pinning matters ([ref #8](../../resources/limitations-and-workarounds.md)) |
| Plugin generation does not resolve transitive subagent dependencies | List every subagent explicitly in `items[]`; test the installed collection in isolation |
| `Validate-Collections.ps1` only scans the repo's `collections/` directory | Work in a clone and add your manifest there, or validate structurally (Lab 4A covers both paths) |

## Knowledge Check

1. What two files make up a collection package, and which one is the source of truth for the build pipelines?
2. An `items[]` entry has no `maturity` field. Which release channels include it, and why?
3. Your parent agent declares two subagents in its `agents:` frontmatter. What must the manifest contain, and what breaks if you forget?
4. A client team works exclusively in GitHub Codespaces and wants version-controlled, explicit updates. Which install method does the matrix recommend?
5. A CLI-plugin user complains that their Terraform edits ignore the team's coding standards, but the same standards work for extension users. What's the cause and the workaround?

*(Answers: [instructor guide](../instructor-guide.md#module-1-answer-key))*

## Further Reading

- [Managing Collections](https://github.com/microsoft/hve-core/blob/main/docs/customization/collections.md) — the full manifest format and creation steps
- [AI Artifacts Architecture](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md) — maturity gating, deprecation mechanisms, extension integration
- [Comparing Setup Methods](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/comparison.md) — decision matrix and per-method docs
- [Copilot CLI Plugins](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/cli-plugins.md) — install commands and the instructions limitation in detail
