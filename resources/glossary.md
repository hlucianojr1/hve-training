# HVE Glossary

Terms as used throughout this curriculum. Authoritative definitions live in [`docs/architecture/ai-artifacts.md`](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md) and [`docs/rpi/`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/README.md).

| Term | Definition |
|------|-----------|
| **HVE (Hypervelocity Engineering)** | Microsoft's workflow system for GitHub Copilot: specialized agents, reusable prompts, auto-applied instructions, and executable skills, distributed as collections. Not a model or runtime — a prompt/artifact library. |
| **RPI** | Research → Plan → Implement → Review. The core methodology: four phases, each a specialized agent, each producing a durable artifact, with context cleared between phases. |
| **Strict RPI** | Running the four RPI phases as separate sessions with manual `/clear` between each. Maximum quality and auditability. |
| **rpi-agent** | Single-session orchestrator that runs all phases in one conversation using subagent dispatch. Convenient; vulnerable to context degradation. |
| **Artifact (workflow)** | A durable file produced by a phase — research doc, plan, changes log, review — stored in `.copilot-tracking/`. Artifacts, not chat history, carry context between phases. |
| **Artifact (AI artifact)** | One of the four customization file types: prompt, agent, instruction, skill. |
| **Prompt** (`*.prompt.md`) | User-invoked slash-command entry point. Captures intent; may delegate to an agent via `agent:` frontmatter; supports `${input:...}` templating. |
| **Agent** (`*.agent.md`) | A behavior definition: persona, `tools:` access, `agents:` subagent delegation, `handoffs:` to other agents. Selected in the Copilot Chat agent picker or activated by a prompt. |
| **Instruction** (`*.instructions.md`) | Standards auto-applied when the current file matches the `applyTo:` glob. Passive guidance — never invoked directly. |
| **Skill** (`SKILL.md` + scripts) | Executable capability in `.github/skills/<domain>/<name>/`; kebab-case `name` matching its directory; discovered by description match or invoked by name. Active execution, unlike instructions. |
| **Delegation chain** | User → Prompt → Agent → Instructions + Skills. Who hands work to whom. |
| **Collection** | A YAML manifest (`collections/*.collection.yml`) bundling artifacts into an installable package (VS Code extension + CLI plugin from the same source of truth). |
| **Flagship collection** | `hve-core` — the RPI workflow + git prompts (~68 artifacts). `hve-core-all` bundles everything (~260 artifacts). |
| **Maturity** | Release-channel gate on collections and items: `experimental` → `preview` → `stable` (plus `deprecated`, `removed`). Set in collection manifests, not artifact frontmatter. |
| **`.copilot-tracking/`** | Gitignored directory where all workflow artifacts land: `research/`, `plans/`, `details/`, `changes/`, `reviews/`, plus planner-specific subdirs. |
| **`/clear`** | Copilot Chat command that wipes conversation history. The load-bearing habit of HVE: run it between phases. |
| **`/compact`** | Summarizes chat history to reduce tokens (lossy). Mid-phase only; removed from handoff buttons. |
| **`/checkpoint`** | Memory-agent prompt that persists session state to disk for cross-session recovery. |
| **Context degradation** | Quality loss as conversations grow: recency bias makes the model deprioritize its instructions. Four symptoms: phase skipping, ignored instructions, shallow output, echoing prior patterns. |
| **Recency bias** | LLM attention favors recent tokens; 3K tokens of instructions lose to 50K+ tokens of accumulated output. |
| **Handoff** | A button (VS Code 1.106+) defined in agent `handoffs:` frontmatter that moves work to the next agent/prompt. |
| **Memory agent** | Agent that stores persistent notes in `memories/`, readable by other agents across sessions. |
| **Lifecycle (9-stage)** | HVE's project arc: Setup → Discovery → Product Definition → Decomposition → Sprint Planning → Implementation → Review → Delivery → Operations. |
| **Planning chain** | The upstream flow BRD → PRD → ADR → backlog work items → RPI execution. |
| **BRD / PRD / ADR** | Business Requirements Doc / Product Requirements Doc / Architecture Decision Record — produced by `brd-builder`, `prd-builder`, `adr-creation` agents. |
| **Specialty planners** | Governance-aware planning agents: security (STRIDE), accessibility, RAI (Responsible AI), SSSC (secure supply chain), privacy. Confined to `.copilot-tracking/`; never modify source. |
| **`runSubagent`** | Host tool enabling an agent to dispatch subagents. Required by rpi-agent, code-review, and others; absence means fall back to strict RPI. |
| **MCP (Model Context Protocol)** | Optional server integrations (GitHub, ADO, Microsoft docs, Figma…) that extend agent capabilities. HVE works without them. |
| **Transparency Note** | `TRANSPARENCY-NOTE.md` — Microsoft's authoritative statement of intended uses, limitations, and when not to use HVE. |
| **Vally / evals** | The repo's evaluation suites (`evals/`) that grade skill and agent quality — HVE's own QA harness. |
