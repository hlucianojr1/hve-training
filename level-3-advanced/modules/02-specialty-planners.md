# Module 3.2 — Specialty Planners

**Time:** 40 min · **Roles:** all (PMs/TPMs: this is your risk-planning toolkit) · **Source docs:** [`docs/agents/security/README.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/security/README.md), [`docs/agents/security/agent-overview.md`](https://github.com/microsoft/hve-core/blob/main/docs/agents/security/agent-overview.md), [`docs/getting-started/cross-planner-integration.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/cross-planner-integration.md)

## Objectives

- Know the four specialty planners, what each assesses, and where each writes.
- Recognize the shared design: six phases, state files, confined to `.copilot-tracking/`, never touching source.
- Internalize the hard boundary: assistive pre-review, never a substitute for professional security/accessibility/compliance work.

## 1. The Four Planners

Each planner is a phase-based conversational agent that interviews you, maps your project against real standards, and lands prioritized work items in your backlog:

| Planner | Assesses against | Writes to | Entry modes |
|---------|-----------------|-----------|-------------|
| **security-planner** | OWASP Top 10, NIST 800-53, CIS Benchmarks; STRIDE threat modeling | `.copilot-tracking/security-plans/{project-slug}/` | capture, from-PRD |
| **accessibility-planner** | WCAG 2.2, ARIA APG, COGA, Section 508, EN 301 549 | `.copilot-tracking/accessibility/{project-slug}/` | capture, or seeded from PRD/BRD/RAI/security plan |
| **rai-planner** | Microsoft Responsible AI Impact Assessment Guide, NIST AI RMF 1.0 | `.copilot-tracking/rai-plans/{project-slug}/` | capture, from-PRD, from-security-plan |
| **sssc-planner** | OpenSSF Scorecard, SLSA, Sigstore, SBOM standards (supply chain) | `.copilot-tracking/sssc-plans/{project-slug}/` | capture, from-PRD, from-BRD, from-security-plan |

All four ship at **experimental maturity** — the security collection is listed as Experimental in the [collections table](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/collections.md), so these agents appear on the pre-release channel, not the stable flagship install (the maturity/channel rule from Level 1 Module 3).

## 2. The Shared Design

Learn one planner and you've learned the operating model for all four:

- **Six sequential phases** — scoping → analysis → standards mapping → model/gap analysis → backlog generation → review & handoff — with explicit confirmation before each phase advance, 3–5 questions per turn, emoji checklists.
- **State on disk** — `state.json` in the planner's tracking directory tracks phases, gates, and answers. Sessions resume across conversations; the agent re-reads state every turn (READ → VALIDATE → DETERMINE → EXECUTE → UPDATE → WRITE).
- **Operationally confined** — every generated file lands under the planner's `.copilot-tracking/<planner>/` directory. **The planners never modify source code or any file outside their tracking directory.** They plan; they don't fix.
- **Backlog handoff, dual-platform** — Phase 5–6 output is work items in ADO format (e.g., `WI-SEC-{NNN}`) or GitHub format (`{{SEC-TEMP-N}}`), each linked to the threats/gaps/standards that motivated it and tagged with an autonomy tier (Full / Partial / Manual) controlling how much human review it gets.
- **Evidence registers** — findings and evidence records conform to a shared `evidence-register.schema.json` with stable URIs, which is what lets the planners cite each other's work instead of redoing it.

**The security planner in slightly more depth** (it anchors the family): Phase 2 classifies your system into seven operational buckets (infrastructure, DevOps/platform-ops, build, messaging, data, web/UI/reporting, identity/auth). Phase 4 runs **STRIDE threat modeling per bucket**, generating threats in `T-{BUCKET}-{NNN}` format with likelihood-impact severity. Phase 3 dispatches a Researcher Subagent for standards lookups (WAF, CAF, MCSB, PCI-DSS, SOC 2, HIPAA, FedRAMP when in scope). If Phase 1 detects AI/ML components, Phase 6 recommends handing off to the RAI Planner in `from-security-plan` mode, pointed at the security `state.json`.

## 3. Running Planners Together

When several planners apply, run them so upstream evidence feeds downstream gates:

```text
1. security-planner   → surface inventory + AI/ML flags
2. rai-planner        → risk classification + AI-generated-UI flags
3. accessibility-planner → reuses both, maps success criteria, produces evidence
4. sssc-planner       → consumes accessibility evidence for procurement gates
```

The order is a recommendation, not a requirement — integration is opportunistic. Each planner detects available upstream artifacts (via reference fields like `securityPlanRef` and `raiPlanRef` in `state.json`) and adapts; each also runs perfectly well standalone.

## 4. The Hard Boundary: Assistive, Not Authoritative

Say this back to anyone who suggests otherwise: **these are pattern-matching reviewers.** They execute no code, run no tests, exercise no exploits, and hold no compliance authority. The security collection ships with a CAUTION notice stating exactly this, and the [Transparency Note](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md) carries per-agent appendices for the decision-shaping planners (RAI, security, and peers).

What that means in practice:

- Planner output is a **pre-review pass**: it makes sure the right questions get asked and the resulting work items land with context. It does not replace SAST, DAST, SCA, penetration testing, conformance audits, or qualified human reviewers.
- The accessibility planner produces **planning artifacts, not a conformance certification** — findings require review by a qualified accessibility professional.
- Never remove an existing security or quality gate because a planner "already covered it." Add the planner *in front of* your gates, not in place of them.

## Use Case Spotlight

A team three sprints from shipping an internal LLM-powered support tool runs security-planner in from-PRD mode. Phase 1 flags the AI components; Phase 4 STRIDE surfaces `T-DATA-003` (prompt logs containing customer PII flowing to an unclassified store) — a threat nobody had written down. Phase 6 hands off to rai-planner, which adds human-review controls for generated responses. Total cost: two structured sessions. The pen test still happens before launch — but it starts from a threat model instead of a blank page, and the backlog already contains the remediation items with acceptance criteria.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Specialty reviewers are assistive pattern-matchers — no execution, no tests, no exploit verification | Keep them as a *pre*-review pass; never replace SAST/DAST/pen-test/human review ([ref #19](../../resources/limitations-and-workarounds.md)) |
| No safety layer of its own; inherits host-model failures into security/compliance-shaped output | Qualified human review is mandatory before any planner artifact drives a decision ([ref #15](../../resources/limitations-and-workarounds.md)) |
| Experimental maturity — planners are absent from the stable channel and may change between releases | Install the security collection / pre-release channel deliberately; pin versions for team use ([ref #13](../../resources/limitations-and-workarounds.md)) |
| Six-phase sessions are long and stateful | Resume via `state.json`; start each new plan in a fresh session (`/clear`), one project slug per plan |

## Knowledge Check

1. Name the four specialty planners and one standard/framework each maps against.
2. What two operational constraints do all four planners share regarding the filesystem?
3. What is the recommended run order when multiple planners apply to one project, and what mechanism lets a downstream planner reuse upstream findings?
4. In the security planner, what do STRIDE, the seven buckets, and `T-{BUCKET}-{NNN}` each refer to?
5. A director asks: "Can we drop the annual pen test now that the security planner runs on every project?" Give the two-sentence correct answer.

*(Answers: [instructor guide](../instructor-guide.md#module-2-answer-key))*

## Further Reading

- [Security Planning overview](https://github.com/microsoft/hve-core/blob/main/docs/agents/security/README.md) and [Agent Overview](https://github.com/microsoft/hve-core/blob/main/docs/agents/security/agent-overview.md) — phases, state schema, operational constraints
- [Cross-Planner Integration](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/cross-planner-integration.md) — the evidence-register schema and integration matrix
- [Accessibility Planner Quickstart](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/accessibility-planner.md) — five-minute first session
- [Transparency Note](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md) — per-agent appendices for the decision-shaping planners
