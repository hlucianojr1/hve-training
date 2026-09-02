# HVE Limitations & Workarounds — Consolidated Reference

Honest constraints, taught in-context throughout the curriculum and consolidated here. Authoritative sources: [`TRANSPARENCY-NOTE.md`](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md), [`docs/rpi/context-engineering.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md), [`docs/getting-started/troubleshooting.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/troubleshooting.md), [`docs/getting-started/methods/`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/README.md).

## Context & Workflow Limitations

| # | Limitation | Why it happens | Workaround | Taught in |
|---|-----------|----------------|------------|-----------|
| 1 | **Context degradation / phase skipping** — a second `/rpi` in the same session jumps straight to implementation | LLM recency bias: ~3K tokens of instructions vs 50K+ tokens of accumulated output; recent tokens win attention | `/clear` between phases and before every new `/rpi`; artifacts in `.copilot-tracking/` carry knowledge; reopen the artifact file after clearing | L1 M4, L2 M2 |
| 2 | **rpi-agent phase order is advisory** — it's prose in a system prompt, not a programmatic constraint | Single-session design accumulates tokens across all phases | Strict RPI for high-stakes/complex work; watch for the four degradation symptoms; escalate mid-task if needed | L2 M3 |
| 3 | **`/compact` is lossy** and was removed from agent handoff buttons (Autopilot compaction loops) | Model-summarized history drops nuance unpredictably | Prefer `/clear` + artifact restore between phases; `/checkpoint` (memory agent) for cross-session state | L2 M2 |
| 4 | **`runSubagent` tool required** by rpi-agent, task-implementor orchestration, code-review, security-reviewer | Subagent dispatch is a host capability, not guaranteed | Check availability first; fall back to strict RPI with manual phase transitions | L2 M3 |
| 5 | **Live research runs 20–60 min autonomously** — kills live demos and surprises new users | Deep multi-file investigation is genuinely slow | Set expectations; run research as a background task; instructors pre-bake artifacts (see [Instructor Handbook §2](../instructor-handbook.md)) | Handbook, L4 M4 |

## Installation & Environment Limitations

| # | Limitation | Workaround | Taught in |
|---|-----------|------------|-----------|
| 6 | **CLI plugins do not auto-apply instructions** — the plugin spec has no `instructions` component; `applyTo` matching never fires | Prefer the VS Code extension when coding-standards enforcement matters; on CLI, reference instruction files explicitly in prompts | L1 M2, L4 M1 |
| 7 | **Mounted-directory install does not work in GitHub Codespaces**; requires container rebuild | Use the extension or submodule method for Codespaces; decision matrix in [`methods/comparison.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/comparison.md) | L4 M1 |
| 8 | **Marketplace extension can't be modified**, no git control, per-member install | Clone-based methods (peer clone, submodule) when you need to customize artifacts | L4 M1 |
| 9 | **Collection conflicts** between HVE Core All and HVE Installer when both installed | Pick one path; troubleshooting steps in [`troubleshooting.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/troubleshooting.md) | L1 M2 |
| 10 | **Handoff buttons require VS Code 1.106+** | Update VS Code, or use typed slash commands (always work) | L1 M2 |

## Model & Platform Limitations

| # | Limitation | Workaround | Taught in |
|---|-----------|------------|-----------|
| 11 | **`model:` frontmatter is a preference hint, not a guarantee** — VS Code falls back through the list, then to the session model | Don't build workflows that assume a specific model; validate outputs regardless | L4 M3 |
| 12 | **Subagent model tier is capped by the parent picker** — pick Sonnet-class and subagents can't use Opus-class | Choose the session model deliberately for orchestrating agents | L4 M3 |
| 13 | **Behavior varies** by Copilot Chat version, model choice, and other installed extensions — nothing is pinned | Pin to release tags for teams; grade process and artifacts, not identical outputs | L4 M2 |
| 14 | **Multi-agent runs are not fully auditable/replayable** | Treat artifacts as the audit trail; strict RPI when auditability matters | L2 M3, L4 M2 |

## Scope & Responsibility Limitations (from the Transparency Note)

| # | Limitation | Response | Taught in |
|---|-----------|----------|-----------|
| 15 | **HVE adds no safety layer of its own** — inherits the host model's failures and biases, no content filtering | Human review gates stay mandatory; treat decision-shaping outputs as drafts | L1 M1, L4 M2 |
| 16 | **Uneven coverage** — coding standards favor C#, Python, PowerShell, Rust, Bash, Bicep, Terraform; output mainly English | Author your own instructions for other stacks (Level 3 teaches exactly this) | L3 M3 |
| 17 | **When NOT to use HVE** — automated decisions in regulated areas, inferring protected characteristics, **assessing developer performance from HVE telemetry/review verdicts**, sole basis for high-stakes calls, unsupported clients | These are policy lines, not technical gaps — no workaround; covered in governance | L1 M1, L4 M2 |
| 18 | **Supported clients only** — current Copilot Chat in VS Code + Copilot CLI; other IDEs/older versions/third-party model hosts are uncharacterized | Standardize teams on supported clients; flag anything else as unsupported experimentation | L1 M2 |
| 19 | **Specialty reviewers are assistive, not authoritative** — security/code-review agents are pattern-matching reviewers: no code execution, no tests run, not a substitute for SAST/DAST/pen-test or human review | Keep them as a *pre*-review pass; never replace existing security/quality gates | L3 M2 |
| 20 | **Saved memory is host-controlled**; check `memories/` before sharing a workspace | Review/clean memory files when pairing or handing off machines | L1 M2 |
