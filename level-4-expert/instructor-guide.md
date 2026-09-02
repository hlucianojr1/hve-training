# Level 4 — Instructor Guide

Companion to the [Level 4 README](README.md) agenda. General facilitation rules: [Instructor Handbook](../instructor-handbook.md). Note the recursion in this level: you are teaching them to teach — everything Handbook §2 says about your demos, you're also *demonstrating* as course content for Module 4.

## §1 Opening (0:00–0:10)

- Artifact review first: "open your Lab 3B set — instruction, prompt, agent, skill. Thumbs up when all four are on screen." These files are Lab 4A's raw material; anyone missing them pairs with a neighbor and uses the neighbor's set for the session (they build their own collection later).
- Frame the level in one line: *"Levels 1–3 made you good at HVE. Today is about everyone else: shipping it, governing it, and teaching it."*
- Set the capstone expectation now: "by end of session you'll pair up for Lab 4C teach-backs — you will each teach Level 1 Module 1 to a peer this week."

## §2 Module 1 — Collections & Distribution (0:10–0:55)

**Talk track beats:**

1. Open with the governance callback from Level 1 Module 3: "collections are the adoption unit — today you learn to *author* that unit."
2. The two-file package, then straight into the live manifest walkthrough (below) — the fields are best taught pointing at real YAML, not slides.
3. Land the two-level maturity story: item maturity gates artifacts within a channel; collection maturity gates the whole package. "New things start `experimental` and graduate by editing one field" — that's the Lab 4A default.
4. Dependencies: the one that bites — subagents are **not** auto-resolved by plugin generation. Tell the war story of the parent agent that shipped without its validator subagent and "worked" until someone used that path.
5. Install methods: don't read the matrix — run scenarios. Call out three ("solo, local, wants to hack on artifacts", "12-person team, Codespaces, compliance wants pinning", "terminal die-hard") and have the room shout the method, then show the row.
6. Enterprise: one minute each on forking (sync obligation is the headline) and the artifact hub env vars (air-gapped clients exist; the answer is `HVE_*` variables, not "copy files around").

**Demo (10 min) — trace one item from manifest to plugin:**

1. Open [`collections/rpi.collection.yml`](https://github.com/microsoft/hve-core/blob/main/collections/rpi.collection.yml). Point at one skill item and one subagent item; note `kind`, the skill path being a directory, and the absence of any dependency fields.
2. Open the corresponding source under `.github/skills/rpi/` — "the manifest points here; this is the single source of truth."
3. Open the built output under `plugins/` for a generated collection — show the symlinked artifact, the generated README, and `plugin.json`. One sentence: "same YAML also parameterizes `Prepare-Extension.ps1` for the `.vsix` — one manifest, two distributables."
4. Run `npm run plugin:validate` live (it's fast and deterministic — one of the few things in this course that is). Show the pass output, then break it for effect: change a `kind: skill` to point at the `SKILL.md` file instead of the directory, re-run, show the kind-suffix error, revert.

**Anticipated questions:**

- *"How do we pin versions for a client?"* — Clone-based methods (submodule pins by SHA; peer clone by tag) plus the Transparency Note's pin-to-release-tag guidance; the marketplace extension auto-updates and can't pin — that's [ref #8](../resources/limitations-and-workarounds.md) and exactly why the matrix sends version-controlled teams to submodule.
- *"What if the client uses JetBrains / Cursor / Visual Studio?"* — Unsupported client. Per the Transparency Note, supported hosts are current Copilot Chat in VS Code and Copilot CLI; behavior elsewhere is uncharacterized. Don't oversell portability — flag it as experimentation the client owns.
- *"Why symlinks in the plugins directory?"* — Zero-copy: one source of truth in `.github/`, so an edit isn't silently forked into the plugin tree. Also why you never hand-edit `plugins/`.
- *"Can one artifact live in several collections?"* — Yes, one `items[]` entry per manifest; that's how RPI artifacts appear in both `hve-core` and `hve-core-all`.

### Module 1 answer key

1. The YAML manifest (`{id}.collection.yml`) and the markdown description (`{id}.collection.md`); the **manifest** is the source of truth both build pipelines consume.
2. Both channels — omitted maturity defaults to `stable`, and stable items ship on stable and pre-release alike.
3. An `items[]` entry for each subagent file, because plugin generation does not resolve transitive agent dependencies; forgetting one means the installed parent silently loses that capability.
4. Submodule — the matrix's answer for Codespaces + team + controlled updates.
5. CLI plugins have no `instructions` component, so `applyTo` matching never fires from plugin directories; workaround: copy instruction files into the project's own `.github/instructions/` (or reference them explicitly in prompts), or use the VS Code extension where standards enforcement matters.

## §3 Module 2 — Team Adoption & Governance (0:55–1:45)

**Talk track beats:**

1. Frame it as the client-conversation toolkit: "every section is an answer to a question a sponsor will actually ask you."
2. Phased rollout: the argument is *value per behavior change required*. Instructions demand zero behavior change — that's why they're first, not because they're simplest to write.
3. Governance: artifacts are production code. If the room includes tech leads, let them design the CODEOWNERS routing live for a hypothetical two-squad repo — 5 minutes, whiteboard.
4. The Transparency floor deserves gravity, not speed. Read the developer-performance line verbatim from the [Transparency Note](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md) ("Do not repurpose HVE Core telemetry, code-review verdicts, or agent activity logs to rate, rank, or evaluate the people doing the work"). Then rehearse the refusal: pick a student, play the client asking for per-engineer usage reports, have them say no out loud with the citation. Awkward is the point — the first real refusal shouldn't be in front of a paying client.
5. Onboarding: the first-customization walkthrough is the highest-leverage hour of any rollout — say so and connect it forward to Module 3's meta-tooling.
6. Metrics: end on the distinction — everything measures artifacts and workflows; the moment a name is attached for evaluation, you've left intended use.

**Discussion prompt (10 min):** "Where is your real team on the phase ladder, and what's the honest blocker?" Collect 3–4; they become Lab 4B raw material — tell students to write theirs down.

**Anticipated questions:**

- *"Can we track who uses HVE?"* — The red-line discussion. Adoption measurement at team/workflow level (invocation frequency, artifact counts) is intended use; individual-level tracking for evaluation is explicitly not. Offer the reframe: measure whether the *workflow* is being adopted, not whether Maria is. If pressed ("but leadership wants it"), the answer is that it's a stated misuse of the framework in Microsoft's own Transparency Note — put it in the risk register and move on.
- *"What if two teams want conflicting instructions in one monorepo?"* — Specificity wins by `applyTo` pattern; same-specificity conflicts go to the artifact owner via PR. Practically: scope team instructions to team paths.
- *"How fast can we get through the phases?"* — Exit criteria, not calendars. A five-person team can clear Phase 1 in two weeks; a 40-person org might take a quarter. Moving on before exit criteria is how adoption programs quietly die.

### Module 2 answer key

1. Lowest effort and immediate value with **zero invocation** — they shape every conversation automatically, so users benefit without changing behavior; trust is built before anything asks the user to adopt a new habit.
2. (1) `copilot-instructions.md` highest-priority rules override everything; (2) more specific `applyTo` patterns beat broader ones; (3) same-specificity conflicts are resolved by the artifact owner through a pull request.
3. Refuse, citing the Transparency Note's when-not-to-use list: HVE telemetry, review verdicts, and activity logs describe artifacts and workflows, not people, and must not be repurposed to rate, rank, or evaluate developers. Offer team-level workflow metrics as the legitimate alternative.
4. Any three quantitative of: artifact count over time, invocation frequency, error reduction (before/after), onboarding velocity; any two qualitative of: team confidence (survey), consistency of outputs with conventions, feedback quality (fewer manual corrections).
5. Pin to a release tag (reproducible), and adopt one collection at a time (reviewable scope); accept "read an agent's description before loading it" as a defensible third.

## §4 Module 3 — Quality Engineering (1:55–2:35)

**Talk track beats:**

1. Open with the thesis: "your artifacts are code, so they get code's quality tooling" — then run the follow-along (`/prompt-analyze` on a student's Lab 3B artifact) *early*; the findings write the module's motivation for you.
2. Evals: the point for this audience is the pattern (repeatable behavioral checks, multiple runs, per-stimulus graders) more than the toolchain. The anti-patterns list is gold — read "don't use `output-contains` as the sole grader" and ask why (plausible-vs-verified again, applied to QA).
3. Model economics: draw the tier cap on the whiteboard — picker choice at top, subagents capped below it. The "someone set Opus to be safe" story lands with every audience that has a Copilot bill.
4. MCP: keep it short and deflationary — five curated servers, optional, per-agent. The teaching goal is that students stop treating MCP config as an install prerequisite.

**Demo (10 min) — walk an eval suite:**

1. Open [`evals/README.md`](https://github.com/microsoft/hve-core/blob/main/evals/README.md) — the suite table. Point at `skill-hygiene` as the odd one out (`vally lint`, structural, deterministic).
2. Open one `eval.yaml` under `evals/agent-behavior/` or `evals/skill-quality/` — show a stimulus, its `tags` backlink to an artifact slug, the grader, and `runs: 3`. Narrate: "one stimulus per test case, graded per-stimulus, multiple runs because output varies."
3. If you have `COPILOT_GITHUB_TOKEN` locally and time to spare, run `npx vally eval --suite skill-quality` as a background task at the *start* of the module and read results now — otherwise walk a captured results JSON from your prep run (pre-baking applies to eval demos too; say so, it reinforces Module 4).
4. Close on CI: change an agent without a spec backlink and the `eval-presence` gate fails the PR. "Coverage is enforced, not aspirational."

**Anticipated questions:**

- *"Can we run evals against our own private artifacts?"* — The Vally CLI is public npm (`@microsoft/vally-cli`) and the spec format is documented in `evals/README.md`; the hve-core CI wrappers are repo-internal. Teams adopt the pattern with their own specs and a Copilot credential.
- *"Why not pin models in eval specs for reproducibility?"* — Explicit anti-pattern: evals should verify behavior across the models users will actually get; pinning creates false confidence — nothing users run is pinned either ([ref #11](../resources/limitations-and-workarounds.md)).
- *"Is `model:` worth setting at all if it's just a hint?"* — Yes, for cost: Fast-tier hints on mechanical subagents cut real spend, and the fallback makes it safe. Just never build *correctness* assumptions on it.

### Module 3 answer key

1. `/prompt-build` (Prompt Builder) generates or revises the artifact body from reference files; `/prompt-analyze` checks it against quality criteria and reports gaps; iterate build→analyze until clean, then lint and PR.
2. `skill-hygiene` — `vally lint` structural checks over every `SKILL.md` under `.github/skills/`; authoritative, no executor calls, no model involved.
3. A Standard-or-lower model runs. VS Code enforces that subagents cannot exceed the parent picker's cost tier, so the Opus request falls back through the model array, then to the session model.
4. It's a preference hint — VS Code never fails an invocation over model availability; it falls back through the array then to the session model. Implication: workflows must validate outputs regardless and never assume a specific model ran.
5. Not a bug — MCP is optional enhancement, and agents report unavailability by design. Check (a) whether `.vscode/mcp.json` exists in the workspace root with the needed server, and (b) the per-agent dependency table to confirm which server that agent actually uses.

## §5 Module 4 — Teaching HVE (2:40–3:20)

**Talk track beats:**

1. Own the recursion: "everything I've done today — the pre-baked eval results, the honest 'here's one I ran earlier' — was Handbook §2. Now I'll show you the seams." Replay your own Module 3 demo prep as the worked example of pre-baking.
2. The pattern's four steps, then the contingency ladder: live-start-then-switch → narrate-over-artifacts → (never) wait on a cold research run.
3. Grading philosophy: run the thought experiment — "two students hand in identical Lab 1C artifacts; good sign or bad?" Let the room argue for a minute before landing it (bad — non-determinism means identical outputs suggest copying; grade artifacts + process).
4. The Feynman loop: do a live 2-minute why-rpi thesis yourself, timed, then have everyone do it simultaneously in pairs (2 min each way). This is Lab 4C prep happening in-room.
5. Adaptation: present the bootcamp-vs-weekly trade honestly — the labs-between-sessions rhythm is the thing you're giving up, and the mitigations (in-room lab blocks, scheduled follow-up) are damage control, not equivalence.

**Demo (5 min):** show your actual prep artifacts for *this* session — the fallback branch, the pre-baked eval results JSON, your timing sheet. Demystifying your own preparation is the module.

**Anticipated questions:**

- *"What if I'm asked to teach this to 40 people at once?"* — Above 16, add a co-facilitator for lab-time floor support (Handbook §1); for 40, split cohorts. Lecture scales; lab support doesn't, and labs are where learning happens.
- *"Do I have to be the best engineer in the room to teach this?"* — No — you have to be the most honest. The demos are pre-baked, the answer keys exist, and "I don't know, let's check the docs" is a passing move on the rubric. Facilitation and honesty, not brilliance.
- *"Can I reuse your demo repo?"* — Build your own on a codebase you know cold; audience questions go off-script the moment a real repo is on screen, and that's when pre-baked-someone-else's-demo collapses.

### Module 4 answer key

1. Because a real research phase runs 20–60 minutes autonomously. Pattern: (1) run the task end-to-end the week before and keep all `.copilot-tracking/` artifacts; (2) live-start the real research for 2–3 minutes; (3) switch honestly to the pre-baked artifact and walk it; (4) keep a fallback branch (`demo-start`) to reset between cohorts.
2. Switch to narrating over the pre-baked artifacts entirely — the artifacts are the teaching content, live typing is theater. It works because the course's claims are about the artifacts (citations, structure, traceability), which are still on screen.
3. Models are non-deterministic and HVE pins nothing, so output variance is expected — identical outputs would suggest copying. Grade the artifacts produced (research doc with citations, plan, changes log in `.copilot-tracking/`) and the process followed (`/clear` between phases, artifact restore, drift caught).
4. The artifact-reader role — "you'll be consuming these documents from your team; what would you ask about this plan?" It rehearses real HVE team usage, where artifacts are the collaboration surface between roles.
5. Change (any two): run labs as facilitated in-room blocks; pre-bake all demos since there's no overnight slack; add a scheduled real-task follow-up a week later; cut at module boundaries only. Refuse to cut (any two): Lab 1C / hands-on lab time, the on-screen `/clear`-and-restore moment, the honest-limitations beats, Module 1's *why*.

## §6 Close (3:20–3:30)

- Rapid knowledge-check sweep: one question per module, group-discussion style.
- **Lab briefing with logistics** — this level's labs need coordination the earlier ones didn't:
  - 4A this week; students on extension-only installs should decide now whether to make a scratch clone for Option A (recommend it — running the real validator is worth the setup).
  - 4B is the deliverable most students will actually reuse — set a date for an optional 30-minute plan-review clinic.
  - 4C: **pair students before they leave** (see §7). Set the teach-back window (within two weeks, while material is fresh).
- Close the program deliberately: exit criteria are the three artifacts (validated collection, adoption plan, passed teach-back). "When you can show all three, you're not a student of this curriculum anymore — you're its next instructor. The [Instructor Handbook](../instructor-handbook.md) is now yours."

## §7 Lab 4C Facilitation — Running Teach-Backs in a Cohort

**Pairing and formats:**

- **Pairs (default):** A teaches B, then B teaches A, ideally on different days so the second student doesn't just mirror the first delivery. Both experiences count — teaching and being a graded audience.
- **Rotating triads (cohorts of 6+):** one teaches, one plays naive student (asks the questions, brings the war story), one grades silently against the rubric. Rotate all three roles across three sessions. This is the best format: the dedicated grader produces far better rubric evidence than a participating audience.
- **Mixed-role bonus:** pair engineers with PMs where possible — a PM audience forces the teacher off jargon and onto the actual thesis, which is precisely the Feynman test.
- **Solo/remote students:** the recorded variant in the lab is legitimate; offer to be their second grader on the recording.

**Grading with the rubric:**

- Insist on the mechanics: graders score independently *before* discussing, and every score needs a timestamp or quote as evidence. "All 3s, no notes" gets sent back.
- Calibrate the cohort once: grade the *instructor's own* Module 4 delivery (or a 5-minute deliberately-flawed sample you perform) together against the rubric before any student teach-back. Ten minutes of calibration prevents grade inflation better than any rule.
- Hold the bar: ≥11/15 and no dimension at 1. A single 1 fails regardless of total — explain why each 1-descriptor is a session-killer in the field (a stalled demo, a bluffed answer, a skipped red line).
- Retakes are normal and should be framed that way: name the weak dimension, prescribe the matching remediation (dimension 1–2 → source docs + Feynman pass; dimension 2 → re-pre-bake; dimension 5 → timed dry-run), redeliver within a week. Track completion — the teach-back is the program's exit gate, and letting it quietly slide is how cohorts end with zero new instructors.
- Collect the graded rubrics: they're your co-instructor shortlist. A student who scores 13+ with strong evidence notes is ready to take Level 1 Module 2 at your next cohort with you in the room — say so on their rubric.
