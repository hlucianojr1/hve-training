# Lab 4B — Adoption Plan

**Time:** ~60 min · **Self-runnable:** yes · **This is a document lab — no coding.**
**Goal:** write an adoption plan for your *real* team or client — not a hypothetical — that you could put in front of that team's lead next week. Module 2 is your source material; [`docs/customization/team-adoption.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/team-adoption.md) and the [Transparency Note](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md) are your references.

## Prerequisites

- Module 2 completed
- A real team or client in mind: you should know their environment (local VS Code? Codespaces? devcontainers?), rough headcount, role mix, and main tech stack. If you're between teams, use your most recent one — the point is grounding every choice in real constraints.

## The Deliverable

One markdown document, `adoption-plan-<team>.md`, with the six required sections below. Write it *for the team's lead*, not for your instructor: concrete names, real repos, honest risks. Expect 2–4 pages.

## Steps

### 1. Copy the skeleton

```markdown
# HVE Adoption Plan — <team/client name>

## 1. Starting Point
- Environment: <local VS Code / devcontainer / Codespaces / mixed>
- Team: <size, roles, solo-vs-team repos>
- Update preference: <auto / controlled> and why
- Install method (from the decision matrix): <method>
- First collection: <one collection>, pinned to release tag <tag>

## 2. Phase Sequence
### Phase 1 — Instructions
- Ships: <2–3 specific instruction files>
- Entry criteria: <what must be true to start>
- Exit criteria: <observable — what must be true to call it done>
### Phase 2 — Agents + Prompts
- Ships / Entry / Exit: ...
### Phase 3 — Skills + Collections
- Ships / Entry / Exit: ...

## 3. Governance Model
- Artifact ownership: <who owns what, by category>
- Review process: <PR review, CODEOWNERS routing, required checks>
- Conflict resolution: <the three-step priority order, localized>

## 4. Success Metrics
- Quantitative (pick 3): ...
- Qualitative (pick 2): ...
- Review cadence: <retro item / quarterly artifact review>

## 5. Risk Register
| Risk | Likelihood | Mitigation |
|------|-----------|------------|
| ... | ... | ... |

## 6. Role Adoption Paths
| Role | Step 1 (consume) | Step 2 (customize) | Step 3 (contribute) |
|------|------------------|--------------------|---------------------|
| ... | ... | ... | ... |
```

### 2. Starting Point (10 min)

Answer the decision matrix's three questions for your team — environment, solo/team, update preference — and commit to one install method, citing the [matrix](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/methods/comparison.md). Then pick **one** starting collection (the Transparency Note's adopt-one-at-a-time guidance) and a pin strategy: name the release tag, and who reviews upgrades. If your answer is "the extension, auto-updating," say what that trades away (no pinning, no customization — [ref #8](../../resources/limitations-and-workarounds.md)) and why that's acceptable for this team.

### 3. Phase Sequence (15 min)

For each of the three phases, write what ships, entry criteria, and exit criteria. Make exit criteria *observable*: not "team is comfortable" but "all three instruction files merged via PR review; every member has seen one fire in their own edits; zero conflicts reported for two weeks." Phase 1 must name the 2–3 actual instruction files you'd write first — pull them from the team's real conventions (their linter config, their PR template, their commit style).

### 4. Governance Model (10 min)

Assign ownership by name or role for each artifact category. Localize the review process (which repo, which CODEOWNERS entries, which npm checks run in their CI). Restate the conflict-resolution priority order in the team's own terms.

### 5. Success Metrics (10 min)

Pick exactly **3 quantitative + 2 qualitative** indicators from Module 2 §6 and, for each, write *how* you'd collect it for this team without new tooling. Then add one sentence stating what these metrics will never be used for — individual performance assessment — so it's in writing before anyone asks.

### 6. Risk Register (10 min)

Minimum five rows. At least two must be **Transparency-Note red lines** phrased as risks — e.g. "a manager requests per-engineer agent-usage reports" (mitigation: policy line, refuse with citation, offer team-level workflow metrics instead) and "review-agent verdicts become a merge-blocking gate with no human" (mitigation: agents stay a pre-review pass; human keeps the merge decision). Good candidates for the rest: instruction conflicts across squads, unpinned upgrades changing behavior mid-sprint, CLI-only users missing auto-applied standards, stale artifacts eroding trust.

### 7. Role Adoption Paths (5 min)

One row per role that actually exists on the team, using the consume → customize → contribute progression. Delete roles the team doesn't have; add none it doesn't.

## Verify Your Work

- [ ] All six sections present; every choice cites a real constraint of *this* team, not a generic best practice
- [ ] Install method is defensible from the decision matrix's three questions
- [ ] Each phase has observable entry *and* exit criteria
- [ ] Exactly 3 quantitative + 2 qualitative metrics, each with a collection mechanism
- [ ] Risk register has ≥5 rows including ≥2 Transparency-Note red lines with mitigations
- [ ] The document is something you would genuinely send — read it once as the team lead and note what they'd push back on

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Everything is Phase 1 / the plan is one big bang | You're listing artifacts, not sequencing adoption — force-rank by "value delivered per behavior change required"; instructions win precisely because they require zero behavior change |
| Exit criteria all read "team is comfortable with X" | Rewrite each as something a skeptic could verify: merged files, observed invocations, survey numbers, a date |
| Governance section names no humans | Ownership without a name is a wish; if you can't name an owner, that's a finding — put it in the risk register |
| Metrics section drifts toward measuring people | Re-read Module 2 §6's closing warning; metrics describe artifacts and workflows — reframe or replace the metric |
| It's 8 pages | You're writing documentation, not a plan; cut anything the team lead wouldn't act on in the first quarter |

## What You Learned

- An adoption plan is a sequence of small, observable promises — not a feature list.
- Governance and red lines are cheapest to establish *before* rollout, in writing, when nobody's in a dispute yet.
- The decision matrix, phased rollout, and Transparency Note aren't three documents — they're the environment, sequencing, and boundary sections of the same client conversation.

## Stretch Goal

Pressure-test the plan: give it to a colleague playing a skeptical client sponsor with three scripted objections — "why not everything at once?", "can we see who's using it most?" (red line — rehearse the refusal + alternative), and "what happens when Microsoft ships a breaking change?" (your pin/upgrade story). Revise the plan wherever you had to improvise.
