# Module 4.3 — Quality Engineering

**Time:** 40 min · **Roles:** all (metrics and economics matter to leads/PMs as much as authors) · **Source docs:** [`docs/contributing/ai-artifacts-common.md`](https://github.com/microsoft/hve-core/blob/main/docs/contributing/ai-artifacts-common.md), [`evals/README.md`](https://github.com/microsoft/hve-core/blob/main/evals/README.md), [`docs/getting-started/mcp-configuration.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/mcp-configuration.md)

## Objectives

- Use HVE's own meta-tooling — Prompt Builder and prompt analysis — to improve the artifacts you author.
- Know what the Vally eval suites test, and what a QA harness for prompt files even means.
- Reason about model economics: cost tiers, the `model:` hint, and the subagent tier cap.
- Place MCP servers correctly: curated, optional, per-agent — never a prerequisite.

## 1. Use HVE to Improve HVE

The framework eats its own dog food: artifact authoring has an authoring workflow. The loop you met in Level 3 becomes your team's standard quality pass:

1. Create the artifact file with minimal frontmatter
2. **`/prompt-build`** — the Prompt Builder agent generates or revises the body, following the patterns of reference files you point it at ("make it like *this* existing instruction")
3. **`/prompt-analyze`** — analyzes the file against quality criteria and reports gaps
4. Iterate with `/prompt-build` until the analysis comes back clean
5. `npm run lint:all`, then PR

The insight to internalize: **your artifacts are code, so they get code's quality tooling.** A prompt nobody reviewed, analyzed, or linted is exactly as trustworthy as a script nobody reviewed. When a teammate's agent "works weirdly," the first diagnostic is `/prompt-analyze` on its file — not vibes.

**Follow-along (5 min):** run `/prompt-analyze` against one artifact from your Lab 3B set. Note the top finding — you'll fix it before Lab 4A packages the file.

## 2. The Eval Harness: Vally Suites

`evals/` contains [Vally](https://github.com/microsoft/hve-core/blob/main/evals/README.md) evaluation specs — the closest thing prompt files have to a test suite:

| Suite | Executor | What it tests |
|-------|----------|---------------|
| `skill-quality` | `copilot-sdk` | Skills provide accurate guidance via real agent conversation |
| `agent-behavior` | `copilot-sdk` | Agents respond correctly to domain prompts |
| `script-validation` | `copilot-sdk` | Agent reasoning about validation rules |
| `baseline-equivalence` | `copilot-sdk` | Customization preserves baseline model behavior beyond documented divergences |
| `behavior-conformance` | `copilot-sdk` | Tier 3 advisory conformance for prompts, instructions, and skill behavior (doesn't fail PR builds) |
| `skill-hygiene` | `vally lint` | Structural checks on every `SKILL.md` — the only suite that lints instead of executing |

Run them with `npx vally eval` (or `--suite skill-quality` for one), lint specs with `npm run eval:lint:vally`. Because model output is non-deterministic, executing suites use multiple runs (`runs: 3`+) with per-stimulus graders — never a single run with an `output-contains` string match. CI enforces coverage: change an AI artifact and the `eval-presence` gate fails the PR until an eval spec backlinks it (`stimuli[].tags.<kind>: <slug>`).

For your team the takeaway is the *pattern*, not the tooling: an artifact worth distributing is worth a repeatable behavioral check, graded on multiple runs, that a PR pipeline can execute.

## 3. Model Economics

The `model:` frontmatter property is **optional** and, when present, a **preference hint with fallback** — never a guarantee. VS Code never fails an invocation over model availability: it falls through the array in order, then to the session model in the picker.

| Tier | Multiplier | Use when | Examples |
|------|-----------|----------|----------|
| Fast | 0.25x–0.33x | Read-only research, mechanical file ops, classification | Claude Haiku 4.5, GPT-5.4 mini |
| Standard | 1x | Code generation, architecture, complex synthesis | Claude Sonnet 4.6, GPT-5.4 |
| Premium | 3x–15x | Vision-capable tasks, complex architectural decisions | Claude Opus 4.6, GPT-5.5 |

Two rules with real cost consequences:

- **The subagent cap:** VS Code enforces that subagent models cannot *exceed* the parent's cost tier. Session model Sonnet-class (Standard) → subagents may use Haiku, never Opus. So for orchestrating agents (rpi-agent, code-review), the *user's picker choice* silently sets the ceiling for every subagent — choose it deliberately.
- **Format and catalog:** references use the VS Code display name with the required `(copilot)` suffix — e.g. `Claude Haiku 4.5 (copilot)` — and must exist in `scripts/linting/model-catalog.json` (`npm run lint:models` validates; a weekly workflow catches catalog drift and retiring models).

The economic play for artifact authors: route the mechanical 80% of a workflow to Fast-tier subagents and reserve Standard/Premium for synthesis. That's a 3–4x cost difference for identical outcomes on the mechanical steps — the kind of number that makes an adoption plan's budget slide write itself.

## 4. MCP Servers: Optional Enhancement, Never Prerequisite

HVE Core curates **five** MCP servers — `context7`, `microsoft-docs`, `ado`, `github`, `figma` — and its stance is deliberately conservative: MCP configuration is *optional*; agents that depend on an MCP tool say so when the server is unavailable, and agents without MCP dependencies work with none configured.

Dependencies are per-agent, not framework-wide: `task-researcher`/`task-planner` use context7 and microsoft-docs for documentation lookup (optional), `github-backlog-manager` needs the github server, `ado-prd-to-wit` needs ado, `dt-figma-export` needs figma. Configure only what the workflow uses — typically `github` *or* `ado`, not both — via `.vscode/mcp.json` in the workspace root (the installer skill can generate this). When rolling out to a team, MCP is a Phase 2+ concern: don't let a five-server config review stall Phase 1 instructions that need zero configuration.

## Use Case Spotlight

A tech lead notices the team's custom code-review agent burns Premium-tier quota on every PR — someone set `model: Claude Opus 4.6 (copilot)` "to be safe." The fix is quality engineering end-to-end: `/prompt-analyze` flags the missing fallback array; the frontmatter becomes a Fast-tier preference for the mechanical diff-summarization subagent with Standard for synthesis; a small `agent-behavior`-style eval (3 runs, per-stimulus graders) proves review quality holds. Cost drops roughly 10x, verdicts are unchanged, and there's now a regression harness for the next edit.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| `model:` is a preference hint — VS Code falls back through the array, then to the session model | Never build workflows that assume a specific model; validate outputs regardless ([ref #11](../../resources/limitations-and-workarounds.md)) |
| Subagent model tier is capped by the parent picker selection | Choose the session model deliberately when running orchestrating agents ([ref #12](../../resources/limitations-and-workarounds.md)) |
| Eval executors are non-deterministic — a passing run proves little on its own | `runs: 3`+, per-stimulus graders, no `output-contains` as sole grader, no pinned models in specs |
| Eval execution needs Copilot credentials (`COPILOT_GITHUB_TOKEN`); fork PRs can't run them | Lint suites (`eval:lint:*`) and `skill-hygiene` run credential-free; execution happens on trusted branches |
| MCP-dependent features fail quietly confusing users ("tool unavailable") | Treat MCP as optional enhancement; check `.vscode/mcp.json` and the per-agent dependency table before filing bugs |

## Knowledge Check

1. Describe the meta-tooling loop for raising an artifact's quality, naming both slash commands and what each contributes.
2. Which eval suite is the only one that uses `vally lint` instead of `vally eval`, and what does it check?
3. A user's picker is set to a Standard-tier model and an agent's subagent requests `Claude Opus 4.6 (copilot)`. What actually runs, and why?
4. Why does `model:` in frontmatter never cause an invocation to fail — and what does that imply for workflows that "need" a specific model?
5. An agent reports an MCP tool is unavailable. Is HVE broken? What two things do you check?

*(Answers: [instructor guide](../instructor-guide.md#module-3-answer-key))*

## Further Reading

- [AI Artifacts Common Standards](https://github.com/microsoft/hve-core/blob/main/docs/contributing/ai-artifacts-common.md) — model catalog, cost tiers, validation commands, quality gates
- [Evaluations README](https://github.com/microsoft/hve-core/blob/main/evals/README.md) — suite architecture, executors, anti-patterns
- [Evals in CI](https://github.com/microsoft/hve-core/blob/main/docs/contributing/evals-ci.md) — the presence gate, auth contract, and adding a spec
- [MCP Server Configuration](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/mcp-configuration.md) — the five curated servers and the full config template
