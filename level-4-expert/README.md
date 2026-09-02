# Level 4 — Expert

**Theme:** Distribution, adoption, and teaching HVE.
**Audience:** Champions, tech leads, and future instructors — any role that will drive HVE adoption on a team or at a client.
**Format:** 3.5-hour instructor session + ~3 hours self-paced labs.
**Prerequisite:** Level 3 exit criteria met — bring the custom artifact set you authored in [Lab 3B](../level-3-advanced/workshops/lab-3b-author-artifact-set.md) (instruction, prompt, agent, skill). Lab 4A packages exactly those files into a collection.

## Objectives

By the end of Level 4, students can:

1. Author a collection manifest that passes the repo's validation tooling, and explain how one manifest produces both a VS Code extension and a Copilot CLI plugin.
2. Choose an install method for any environment using the decision matrix, and name the trade-offs of each (including the CLI instructions gap).
3. Design a phased adoption plan — Instructions → Agents/Prompts → Skills/Collections — with governance, success metrics, and the Transparency Note's red lines built in.
4. Use HVE's meta-tooling (Prompt Builder, prompt analysis, Vally evals, model cost tiers) to engineer the quality of their own artifacts.
5. Deliver a Level 1 module to a real audience using the pre-baked-demo craft, and grade a teach-back against the capstone rubric.

## Session Agenda (3.5 h)

| Time | Segment | Material |
|------|---------|----------|
| 0:00–0:10 | Welcome, Level 3 artifact review (show your Lab 3B set) | [Instructor guide §1](instructor-guide.md) |
| 0:10–0:55 | **Module 1 — Collections & Distribution** | [modules/01-collections-and-distribution.md](modules/01-collections-and-distribution.md) |
| 0:55–1:45 | **Module 2 — Team Adoption & Governance** | [modules/02-team-adoption-and-governance.md](modules/02-team-adoption-and-governance.md) |
| 1:45–1:55 | Break | |
| 1:55–2:35 | **Module 3 — Quality Engineering** | [modules/03-quality-engineering.md](modules/03-quality-engineering.md) |
| 2:35–2:40 | Break | |
| 2:40–3:20 | **Module 4 — Teaching HVE** | [modules/04-teaching-hve.md](modules/04-teaching-hve.md) |
| 3:20–3:30 | Knowledge-check review, lab briefing (incl. teach-back pairing), Q&A | [Instructor guide §6](instructor-guide.md) |

## Self-Paced Labs (complete to finish the program)

| Lab | Time | Produces |
|-----|------|----------|
| [4A — Build a Collection](workshops/lab-4a-build-a-collection.md) | 60 min | `my-team.collection.yml` + `.collection.md` bundling your Lab 3B artifacts, validated |
| [4B — Adoption Plan](workshops/lab-4b-adoption-plan.md) | 60 min | A written adoption plan for your real team or client (document lab — no coding) |
| [4C — Capstone Teach-Back](workshops/lab-4c-capstone-teachback.md) | 60–90 min | Level 1 Module 1 delivered to a peer or small group, graded against the rubric |

## Exit Criteria

You've completed the program when you can show:

- [ ] A collection manifest for your own artifact set that **passes validation** (`npm run plugin:validate`, or the structural checklist in Lab 4A if you're on an extension-only install), plus its markdown description (Lab 4A)
- [ ] A **written adoption plan** for your real team or client with phase sequence, governance model, success metrics, and a risk register that includes the Transparency Note red lines (Lab 4B)
- [ ] A **teach-back delivered** — Level 1 Module 1 to a live audience or recorded solo — scoring at or above the passing bar on the [Lab 4C rubric](workshops/lab-4c-capstone-teachback.md)

## Sources

[`docs/customization/collections.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/collections.md) · [`docs/architecture/ai-artifacts.md`](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md) · [`docs/getting-started/methods/comparison.md`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/comparison.md) · [`docs/customization/team-adoption.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/team-adoption.md) · [`docs/contributing/ai-artifacts-common.md`](https://github.com/microsoft/hve-core/blob/main/docs/contributing/ai-artifacts-common.md) · [`evals/README.md`](https://github.com/microsoft/hve-core/blob/main/evals/README.md) · [`TRANSPARENCY-NOTE.md`](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md) · [Instructor Handbook](../instructor-handbook.md)
