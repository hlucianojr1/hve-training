# Module 2.3 — rpi-agent & Mode Selection

**Time:** 35 min · **Roles:** all · **Source docs:** [`docs/rpi/why-rpi.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md), [`docs/rpi/context-engineering.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md)

## Objectives

- Understand how rpi-agent orchestrates the whole workflow in one session — and why that design is convenient *and* vulnerable.
- Match tool to task with the decision matrix, and defend the choice.
- Know the escalation path (start light, escalate to strict) and the `runSubagent` fallback.

## 1. What rpi-agent Is

**rpi-agent** (invoked as `/rpi`, or `/rpi-quick` as the skill entry) runs the full research-through-review flow in a **single session**, orchestrating work by dispatching specialized task agents through the `runSubagent` tool while keeping overall control. Subagent research lands in `.copilot-tracking/subagent/YYYY-MM-DD/`. One invocation, no manual phase transitions, fast feedback.

That's the trade: strict RPI buys quality with ceremony (`/clear`, artifact restore, four agent switches); rpi-agent buys speed by keeping everything in one conversation — which means tokens accumulate across all phases.

## 2. The Vulnerability: Phase Order Is Advisory

Strict RPI's phase separation is *structural*: the researcher literally cannot write code; the `/clear` between phases means the implementor never sees research-exploration tokens.

rpi-agent's phase ordering is **prose in a system prompt, not a programmatic constraint**. On a fresh session that prose dominates attention and the agent behaves. After a full cycle, 50K+ tokens of implementation output compete against ~3K tokens of advisory instructions — and Module 2's recency bias takes over. The advisory instruction loses its influence; the second request in the same session tends to skip phases.

Practical consequences:

- rpi-agent is fine *per invocation* on a fresh context; it's the *second* request in the same session that degrades.
- `/clear` (or at minimum `/compact`) before any second `/rpi` in a conversation.
- Watch for the four degradation symptoms during long rpi-agent runs — they apply mid-run too.

## 3. Matching Tool to Task

| Factor | Strict RPI | rpi-agent |
|--------|-----------|-----------|
| Research depth | Deep, verified, cited | Moderate, inline |
| Context contamination | Eliminated via `/clear` | Possible |
| Audit trail | Complete artifacts | Summary only |
| Review phase | Explicit with findings log | Integrated in iteration loop |
| Best for | Complex, unfamiliar, team work | Simple, familiar, solo work |

Choose **strict RPI** when: new frameworks, external APIs, or compliance requirements demand deep research; multi-file pattern discovery; artifacts must support team handoff; work you'll maintain long-term. Choose **rpi-agent** when: straightforward feature or bug fix with clear scope; codebase-only investigation; active development with fast iteration loops; exploratory or prototype work.

**The audit-trail row matters more than it looks.** Strict RPI leaves a complete, replayable record: research doc, plan + details, changes log, review log — anyone can reconstruct *why* every decision was made. rpi-agent leaves summaries. And multi-agent runs are not fully auditable or replayable in general — the artifacts *are* the audit trail. If auditability matters (compliance, handoff, anything you'll defend later), that alone decides for strict RPI.

## 4. The Escalation Path

You don't have to decide upfront. Start with rpi-agent for speed; if the task reveals hidden complexity, **escalate**: rpi-agent can hand off to Task Researcher when it hits something beyond its scope, and you can force the escalation yourself — `/clear`, then `/task-research` the specific gap, and continue through strict phases from there. The Review phase also escalates: findings that reveal research or plan gaps send you back to those phases regardless of which mode you started in.

Escalation is not failure. Downgrading a degraded strict cycle to "just let rpi-agent finish it" — *that's* the anti-pattern.

## 5. When runSubagent Isn't Available

rpi-agent **requires the `runSubagent` tool**. So do task-implementor's orchestration, code-review, and security-reviewer. Subagent dispatch is a host capability, not a guarantee — it varies by Copilot settings and environment.

Check first: if `/rpi` errors or stalls immediately, or the agent reports it can't dispatch subagents, don't fight it. **Fall back to strict RPI with manual phase transitions** — the four `/task-*` prompts need no subagent tool and always work. This is the standing fallback for every subagent-dependent workflow in HVE.

## Use Case Spotlight

Two real tasks, two right answers. *"Rename this config key and update its three usages"* — clear scope, familiar code, solo: rpi-agent, done in one pass. *"Add Azure Blob Storage to the pipeline"* — unfamiliar SDK, auth decisions, multi-file, teammates will maintain it: strict RPI, and the [walkthrough](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md) shows why — the research doc alone (managed identity vs connection string, streaming for >1GB files) is a decision record no summary would preserve.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| rpi-agent phase order is advisory prose, not a constraint | Strict RPI for high-stakes work; watch the four symptoms; `/clear` before a second `/rpi` ([ref #2](../../resources/limitations-and-workarounds.md)) |
| `runSubagent` required by rpi-agent, task-implementor orchestration, code-review, security-reviewer | Check availability first; fall back to strict RPI with manual transitions ([ref #4](../../resources/limitations-and-workarounds.md)) |
| Multi-agent runs are not fully auditable/replayable | Treat artifacts as the audit trail; strict RPI when auditability matters ([ref #14](../../resources/limitations-and-workarounds.md)) |
| Token accumulation across phases in one session | Module 2's toolbox applies inside rpi-agent runs too ([ref #1](../../resources/limitations-and-workarounds.md)) |

## Knowledge Check

1. Strict RPI enforces phase order structurally. How does rpi-agent "enforce" it, and why does that stop working?
2. A teammate must be able to reconstruct why every decision was made, six months from now. Which mode, and which matrix row decides it?
3. What is the escalation path, and who can trigger it?
4. `/rpi` fails because `runSubagent` isn't available. What's your fallback, and which three other HVE workflows share this dependency?
5. Using the matrix, justify a mode for: "prototype a CSV export for tomorrow's demo, files I wrote myself last week."

*(Answers: [instructor guide](../instructor-guide.md#module-3-answer-key))*

## Further Reading

- [Why the RPI Workflow Works — Choosing Your Workflow](https://github.com/microsoft/hve-core/blob/main/docs/rpi/why-rpi.md) — the full matrix and escalation guidance
- [Context Engineering — The rpi-agent Difference](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md) — the vulnerability, mechanistically
