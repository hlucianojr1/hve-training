# Module 4.2 — Team Adoption & Governance

**Time:** 50 min · **Roles:** all · **Source docs:** [`docs/customization/team-adoption.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/team-adoption.md), [`TRANSPARENCY-NOTE.md`](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md)

## Objectives

- Sequence a team's adoption through the three phases and justify the order.
- Design a governance model: ownership, review gates, and conflict resolution for artifacts.
- Onboard a new team member and manage artifact change/deprecation without workflow whiplash.
- Pick success metrics that measure adoption without crossing the Transparency Note's red lines.

This module is your **client-conversation toolkit**: every section maps to a question a team lead or client sponsor will actually ask you, and Lab 4B turns it into a written plan for your real team.

## 1. The Phased Rollout

Adopt incrementally — confidence with simple artifacts before complex ones:

| Phase | What ships | Why this order |
|-------|-----------|----------------|
| **1 — Instructions** | 2–3 instruction files: coding standards, commit format, PR conventions | Lowest effort, immediate value, zero invocation — they shape every conversation automatically. Nobody has to change how they work to benefit |
| **2 — Agents + Prompts** | Custom agents for repeatable workflows (reviews, research), prompts for one-shot operations | Requires users to *invoke* things — that's a habit change, so it comes after trust is built |
| **3 — Skills + Collections** | Domain knowledge packaged as skills; everything bundled into collections for cross-team reuse | Highest authoring effort; collections make sense only once there's something worth distributing |

The starting point question comes first: recommend the **HVE Core extension** for the flagship RPI workflow, use the **Installer skill** when the team is ready to choose a clone-based method (it evaluates the environment and configures MCP + agent bundles), and move to direct clone setup only when artifact modification demands it. That's the Module 1 decision matrix applied.

**Discussion (5 min):** where is *your* team on this ladder right now, and what's blocking the next phase? (Common honest answer: Phase 1 never formally happened — standards live in people's heads.)

## 2. Governance

Treat Copilot customization files with the same rigor as production code:

- **Ownership:** a designated maintainer or team owns each collection; cross-domain instruction files can have separate owners; the root `copilot-instructions.md` is cross-cutting and requires broader review.
- **Review and approval:** PR review required for changes to instructions, agents, and skills; CODEOWNERS routes reviews to artifact owners; `npm run lint:all` before merge; `npm run plugin:generate` after manifest changes.
- **Conflicting instructions** resolve in priority order:
  1. `copilot-instructions.md` highest-priority rules override everything
  2. More specific `applyTo` patterns beat broader ones
  3. Same-specificity conflicts go to the artifact owner, resolved through a PR

> **Consultant lens:** governance is where client conversations get real. "Who approves a change to the agent that reviews our PRs?" is an org-design question wearing a YAML costume. Have an answer per artifact category before rollout, not after the first dispute.

## 3. The Transparency-Note Governance Floor

Whatever governance you design sits on top of non-negotiables from the [Transparency Note](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md). Three you must be able to state cold:

- **Never assess developer performance from HVE signals.** Telemetry, code-review verdicts, and agent activity logs describe *artifacts and workflows*, not people. A client asking to rank engineers by agent usage gets a "no" with this citation — it's a policy line, not a technical gap.
- **Pin to a release tag.** Main is a moving target; for anything production-relevant, pin the file version and review changes before upgrading. Behavior also varies by Copilot version and model — nothing is pinned by HVE itself, so reproducibility is *your* configuration discipline.
- **Adopt one collection at a time.** Start with the collection closest to the team's work and grow. This is also the practical antidote to "install hve-core-all and see what sticks" — which produces confusion, not adoption.

Round out the floor with the rest of the "when not to use" list (no automated decisions in regulated areas, no inferring protected characteristics, never the sole basis for high-stakes calls, supported clients only) and the draft doctrine: decision-shaping output — plans, review verdicts, handoffs — is a draft until a qualified human approves it.

## 4. Onboarding New Members

The documented sequence: (1) Getting Started guide for install, (2) first interaction with an existing agent — the RPI workflow is the recommended opener, (3) edit an instructions file *together* so they see behavior change, (4) introduce the team's custom agents and when to use each, (5) share naming conventions and governance expectations.

Then the **first-customization walkthrough** — the new member authors a simple style instruction using the meta-tooling loop (`/prompt-build` with a reference file, `/prompt-analyze` for quality gaps, iterate), tests it in chat, and submits it through the team's PR process. Pair them with someone experienced. This one exercise teaches the artifact model, the tooling, and the governance gate simultaneously — it's the highest-leverage hour in any rollout.

## 5. Change Management & Deprecation

New artifacts follow a structured path: feature branch → `/prompt-build` body generation → `/prompt-analyze` iteration → `npm run lint:all` → update collection manifests → `npm run plugin:generate` → PR with a clear "what and why."

Communicate every workflow-affecting change: new agents get a name + purpose + invocation example; modified instructions get a what-changed-and-why; deprecations get migration steps and a timeline. The maturity ladder from Module 1 is your signaling vocabulary — `experimental` sets expectations for early adopters, `deprecated` starts the migration clock, `removed` (or a move to `.github/deprecated/`) ends distribution. Track rarely-used artifacts quarterly and deprecate deliberately; a graveyard of stale agents erodes trust in the live ones.

## 6. Measuring Success

From the source doc — pick a handful, don't dashboard everything:

- **Quantitative:** artifact count over time · invocation frequency of custom agents and prompts · error reduction (before/after rates for the mistakes the customizations target) · onboarding velocity (time-to-productivity with vs without HVE Core)
- **Qualitative:** team confidence (survey) · consistency of generated outputs against team conventions · feedback quality (do suggestions need fewer manual corrections?)

Feedback loops: customization effectiveness as a retrospective item, a shared channel for reporting gaps, quarterly artifact review. And repeat the red line, because measurement is where it gets crossed: every metric above measures the *artifacts and the workflow* — the moment "invocation frequency" gets a person's name attached for evaluation purposes, you've left intended use.

## 7. Role-Based Adoption Paths

Adoption isn't one ladder — the doc gives nine role paths, each a three-step progression. The pattern: **every role starts by consuming, then customizes, then contributes.** Engineers go instructions → review agent → skill; tech leads go ADR conventions → standards-enforcing review agent → a team collection; TPMs go RPI-for-status-research → report prompts → dependency-tracking agent; business PMs go story-draft prompts → requirements agent → Design Thinking workflow. In Lab 4B you'll assign a path per role that actually exists on your team.

## Use Case Spotlight

A consulting team lands at a client with 40 engineers, three squads, and "we tried Copilot, results were inconsistent." The pitch that works isn't a demo of 260 artifacts — it's Phase 1: three instruction files encoding the client's *existing* standards, shipped in week one, visible in every Copilot answer by week two. Governance rides in quietly (the instruction files went through PR review with a named owner), and the Phase 2 conversation starts from demonstrated value instead of promised value. The phasing *is* the change management.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Behavior varies by Copilot version, model, and installed extensions — nothing is pinned | Pin teams to release tags; grade adoption on process and artifacts, never on identical outputs ([ref #13](../../resources/limitations-and-workarounds.md)) |
| Hard "when not to use" lines — especially developer-performance assessment from telemetry/review verdicts | Policy lines, not technical gaps: build them into the adoption plan's risk register as non-negotiables ([ref #17](../../resources/limitations-and-workarounds.md)) |
| HVE adds no safety layer; decision-shaping outputs are drafts | Keep human review gates in the governance model; agents never commit, file, or send without operator confirmation ([ref #15](../../resources/limitations-and-workarounds.md)) |
| Copied files lose their history, lint coverage, and publisher verification | Record the source version, keep copyright/SPDX headers, apply your own governance to copies |

## Knowledge Check

1. Why do instructions come first in the phased rollout — what two properties make them the lowest-risk entry point?
2. Two instruction files give contradictory guidance for the same file. Walk the three-step resolution order.
3. A client sponsor asks you to use HVE invocation telemetry to identify "low-performing engineers." What do you say, and what's your citation?
4. Name three quantitative and two qualitative adoption indicators you'd actually track for a 10-person team.
5. Which two "getting the best results" practices from the Transparency Note make a client rollout reproducible and reviewable?

*(Answers: [instructor guide](../instructor-guide.md#module-2-answer-key))*

## Further Reading

- [Team Adoption and Governance](https://github.com/microsoft/hve-core/blob/main/docs/customization/team-adoption.md) — the full source: naming conventions, all nine role paths, feedback cadences
- [Transparency Note](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md) — intended uses, when-not-to-use, limitations, and getting-the-best-results; read it end-to-end before your first client conversation
- [Forking and Extending HVE Core](https://github.com/microsoft/hve-core/blob/main/docs/customization/forking.md) — when governance requirements outgrow in-place customization
