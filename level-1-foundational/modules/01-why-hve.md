# Module 1.1 — Why HVE

**Time:** 45 min · **Roles:** all · **Source docs:** [`docs/rpi/why-rpi.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md), [`TRANSPARENCY-NOTE.md`](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md)

## Objectives

- Describe the failure mode of unstructured AI coding, from experience.
- Explain the counterintuitive insight: the fix is *constraining* AI, not making it smarter.
- Name the four RPI phases and what each is forbidden from doing.
- Know what HVE is (a prompt/agent library for GitHub Copilot) and is not (a model, a runtime, a safety layer).

## 1. The Problem You Already Have

AI coding assistants are brilliant at small tasks and dangerous at large ones. Ask for a string-reversal function: perfect code in seconds. Ask for a feature touching twelve files across three services: you get something that *looks* right, *compiles* cleanly, and breaks everything it touches.

The canonical example from the HVE docs:

> **You:** "Build me a Terraform module for Azure IoT"
> **AI:** *immediately generates 2000 lines of code*
> **Reality:** missing dependencies, wrong variable names, outdated patterns, breaks existing infrastructure.

**Discussion (5 min):** share one time an AI assistant produced confident, wrong code for you. What did it get wrong — a convention, an API, an assumption?

The root cause: **AI can't tell the difference between investigating and implementing.** When you ask for code, it writes code. It never stops to verify that its variable naming matches your existing modules, or that the API it's calling still exists. It generates *plausible* output — and plausible is not correct.

## 2. The Counterintuitive Insight

The solution isn't a smarter model. It's **preventing AI from doing certain things at certain times**.

> When AI knows it *cannot* implement, it stops optimizing for "plausible code" and starts optimizing for "verified truth." The constraint changes the goal.

Concretely — without RPI, AI thinks: *"This looks like a reasonable variable name. I'll use `prefix`."* With RPI, the researcher finds: *"12 existing modules use `resource_prefix`, not `prefix`. See `variables.tf#L47`."*

## 3. The RPI Answer

HVE's core methodology splits work into four phases, each a specialized agent with hard constraints:

| Phase | Agent | Can | Cannot |
|-------|-------|-----|--------|
| **Research** | Task Researcher | Search, read, cite file:line evidence | Write any code |
| **Plan** | Task Planner | Sequence tasks, define success criteria | Implement anything |
| **Implement** | Task Implementor | Execute the plan using researched patterns | Invent new approaches |
| **Review** | Task Reviewer | Validate against research + plan, run checks | Assume — it verifies |

Each phase produces a **durable artifact** (a file in `.copilot-tracking/`), and context is deliberately cleared between phases. The next phase reads the artifact, not the chat history. The pipeline transforms: **Uncertainty → Knowledge → Strategy → Working Code → Validated Code**.

> **Instructor demo (10 min):** show a real research document side-by-side with a "just write it" chat response for the same task. Point at the citations. Ask the room: "which one would you let touch production?"

## 4. What HVE Actually Is

**HVE (Hypervelocity Engineering) Core** is Microsoft's open-source library of GitHub Copilot customizations: ~26 agents, prompts, auto-applied coding instructions, and executable skills in the flagship collection (~260 artifacts total), distributed as VS Code extensions and CLI plugins.

Equally important — what it is **not**:

- **Not a model.** It shapes how Copilot's models behave; it brings no intelligence of its own.
- **Not a safety layer.** It inherits the host model's failures and biases and adds no content filtering. Human review stays mandatory.
- **Not for everything.** The Transparency Note draws hard lines: never use it for automated decisions in regulated areas, for inferring protected characteristics, or for **assessing developer performance** from its telemetry or review verdicts.

## 5. The Honest Trade

Your first RPI workflow **will feel slower** — the docs say so themselves. You're learning a process and building the `/clear` habit. By your third feature it feels natural, and the value compounds: research documents become institutional memory, decisions become traceable, and new team members can read *how* past choices were made instead of asking around.

## Use Case Spotlight

The IaC scenario is HVE's own flagship example for a reason: infrastructure code punishes plausible-but-wrong output hardest (a wrong Terraform default doesn't fail a unit test — it changes production). Research-first catches the existing module conventions, provider versions, and naming standards *before* any HCL is written.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| HVE adds no safety layer; outputs are drafts | Human review gates stay; treat decision-shaping output as a draft ([ref #15](../../resources/limitations-and-workarounds.md)) |
| Hard "when not to use" lines exist | Regulated decisions, protected characteristics, developer-performance assessment — policy lines, not technical gaps ([ref #17](../../resources/limitations-and-workarounds.md)) |
| First workflow feels slower | Expected learning curve; natural by the third feature |

## Knowledge Check

1. Why does an AI assistant produce "plausible but wrong" code when asked to build a feature directly?
2. What single constraint transforms the researcher's behavior, and what does it start optimizing for?
3. Name the four RPI phases and the artifact each produces.
4. Give two things HVE explicitly is *not*.
5. According to the Transparency Note, name one use HVE should never be put to.

*(Answers: [instructor guide](../instructor-guide.md#module-1-answer-key))*

## Further Reading

- [Why the RPI Workflow Works](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md) — the full argument, including the quality-difference table
- [Transparency Note](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md) — intended uses and limitations, worth reading end-to-end once
