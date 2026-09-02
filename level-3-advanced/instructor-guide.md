# Level 3 — Instructor Guide

Companion to the [Level 3 README](README.md) agenda. General facilitation rules: [Instructor Handbook](../instructor-handbook.md).

## §1 Opening (0:00–0:10)

- Lab review first: "Show me a review log from Lab 2A." Anyone without Level 2 artifacts pairs up during Module 1; the authoring half (Modules 3–4) genuinely requires the Level 2 foundation — flag stragglers early.
- Frame the level in one line: *"Levels 1–2 made you a good operator of two lifecycle stages. Today: the other seven stages, and then you stop consuming artifacts and start writing them."*
- Tell PMs/TPMs the plan up front: Modules 1–2 and Lab 3A are theirs; the authoring half is optional for them. Nobody should feel walked out at the break.

## §2 Module 1 — Lifecycle & Planning Chain (0:10–1:00)

**Talk track beats:**

1. Open with the map: draw the 9 stages on the whiteboard and ask the room to place the agents *they already know* on it. They'll cluster everything into stages 6–7 — that's the point. "You've mastered two-ninths of this diagram."
2. The chain-as-RPI-writ-large framing carries the module: artifact → `/clear` → next agent consumes artifact. They already have this habit; you're widening its domain, not teaching a new one.
3. Land the seam sentence: **"The handoff artifact is the contract at every role seam — PM to architect to TPM to engineer."**
4. Honest note before Lab 3A: real BRD/PRD sessions run hours. The lab timeboxes aggressively and leans on session resume — say so now, or students will feel they failed the timebox.

**Demo (10 min) — walk a real ADR as specimen:** open [`docs/planning/adrs/0003-generalize-requirements-author-skill-for-brd-and-prd.md`](https://github.com/microsoft/hve-core/blob/main/docs/planning/adrs) (nice recursion: an ADR about the planning chain's own tooling — students find that memorable). Trace context → decision → consequences on screen. Ask: "What here could a PRD not have carried?" (Answer you're fishing for: the *alternatives considered and rejected* — the why-this-way.) Then flick through the directory listing so they see ADRs are small, numerous, and dated.

**Anticipated questions:**

- *"Do we really need all four links for every feature?"* — No. The lifecycle's own transition rules allow skipping (TPMs go BRD → decomposition when the BRD suffices). The skill is knowing what each gate catches so you skip deliberately, not by default.
- *"Our PM writes PRDs in Word/Confluence — is this compatible?"* — The builders integrate external references during their Integrate phases; and a hand-written PRD can still seed decomposition and the specialty planners' from-PRD modes. The chain cares about artifacts existing, not about which tool authored them.
- *"Isn't adr-creation slower than just writing the ADR?"* — For an experienced architect writing a routine ADR, yes. The Socratic coaching pays off for contested decisions and for engineers writing their first ADRs — it forces the alternatives section that humans skip.

### Module 1 answer key

1. Setup → Discovery → Product Definition → Decomposition → Sprint Planning → Implementation → Review → Delivery → Operations. RPI occupies stages 6 (Implementation) and 7 (Review).
2. brd-builder → BRD at `docs/project-planning/<name>-brd.md`; prd-builder → PRD at `docs/project-planning/<name>.md` (session state for both under `.copilot-tracking/{brd,prd}-sessions/`); adr-creation → draft in `.copilot-tracking/adrs/`, final in `docs/decisions/`; github-backlog-manager / ado-prd-to-wit / jira-prd-to-wit → planning files under `.copilot-tracking/`; RPI → the familiar research/plans/changes/reviews artifacts.
3. Yes — it's an explicit lifecycle transition rule (Stage 2 → Stage 4 when the BRD is sufficient). The gates are tools for catching specific failure modes, not mandatory ceremony; skipping is legitimate when done deliberately.
4. A read-only subagent the prd-builder dispatches itself during the Validate phase; it emits structured findings plus a quality report whose gate decision authorizes Validate exit. Users never invoke it directly.
5. It's a documented pitfall — the guided Q&A sessions and their state files interfere when combined. Context crosses the `/clear` boundary the same way it does in RPI: via the artifact on disk (the BRD file, referenced by explicit path in the PRD prompt).

## §3 Module 2 — Specialty Planners (1:00–1:40)

**Talk track beats:**

1. Teach the family through one member: put the security planner's six phases on screen and generalize. "Different standards bolted onto the same chassis" saves 20 minutes over touring all four.
2. The two operational constraints deserve the board: **confined to `.copilot-tracking/<planner>/`** and **never modifies source**. Connect back to Level 1: same reason the researcher can't write code — constraint produces honesty.
3. Deliver the assistive-only boundary as non-negotiable and quotable: *"Pattern-matching reviewers. No execution, no tests, no exploits, no compliance authority."* Read the security collection's CAUTION notice verbatim — it's well-lawyered and lands harder than paraphrase.
4. Maturity honesty: these are experimental. If someone's install lacks them, that's the channel doing its job, not a bug (callback to Level 1 Module 3).

**Demo (8 min):** open a pre-baked security-planner session (run Phases 1–2 on a demo repo beforehand — a live one won't fit). Show `state.json` (the phase gates, the 21 fields — don't read them all, just show that state is inspectable), the bucket classification, and one `T-{BUCKET}-{NNN}` threat with its severity. Then `git status`: clean. "It planned a threat model and touched nothing."

**Anticipated questions:**

- *"Can the planners see our private repos?"* — They see exactly what your Copilot session sees: the open workspace, under your existing Copilot license and data terms. No extra service, no exfiltration channel — artifacts land in your local `.copilot-tracking/` (which is why it's gitignored). The real data-handling question is what *you* do with generated plans containing threat inventories: treat them as sensitive documents.
- *"Our security team will hate this."* — Invert it: the output is a better-prepared *input* to the security team — threat model drafted, standards pre-mapped, backlog items with context. Position it as making their review cheaper, never as replacing it. Same for accessibility professionals.
- *"Why is the run order Security → RAI → Accessibility → SSSC?"* — Downstream planners consume upstream evidence: security produces the surface inventory and AI flags, RAI classifies risk, accessibility reuses both, SSSC consumes accessibility evidence for procurement gates. It's a recommendation — each planner runs standalone and adapts.

### Module 2 answer key

1. security-planner — OWASP Top 10 / NIST 800-53 / CIS (STRIDE modeling); accessibility-planner — WCAG 2.2 / ARIA APG / COGA / Section 508 / EN 301 549; rai-planner — Microsoft RAI Impact Assessment Guide / NIST AI RMF 1.0; sssc-planner — OpenSSF Scorecard / SLSA / Sigstore / SBOM standards. (Any one framework per planner suffices.)
2. All generated files stay under the planner's own `.copilot-tracking/<planner>/{project-slug}/` directory, and the planner never modifies source code or any file outside that directory.
3. Security → RAI → Accessibility → SSSC. Reuse works through the shared `evidence-register.schema.json` (evidence records with stable URIs) and reference fields in `state.json` such as `securityPlanRef` and `raiPlanRef`.
4. STRIDE is the threat-modeling methodology Phase 4 applies per bucket; the seven operational buckets (infrastructure, DevOps/platform-ops, build, messaging, data, web/UI/reporting, identity/auth) are Phase 2's classification of the system; `T-{BUCKET}-{NNN}` is the ID format each generated threat gets, with a likelihood-impact severity.
5. "No — the planner is an assistive pre-review pass: a pattern-matching reviewer that executes no code and verifies no exploits, so it can't do what a pen test does. Keep the pen test; the planner's job is to make sure it starts from a documented threat model instead of a blank page."

## §4 Module 3 — Authoring Instructions & Prompts (1:50–2:35)

**Talk track beats:**

1. This module lives or dies on the live-author demo — it's the Level 1 "invisible instructions" moment from the producing side. Budget the full 10 minutes.
2. The content bar matters more than the mechanics: "Encode what your senior reviewer flags in PRs. If your team doesn't enforce it, don't encode it." Weak instruction files are how teams conclude the tier doesn't work.
3. The division-of-labor heuristic is the take-home: **"If you're writing 'always' in a prompt, it belongs in an instruction."**

**Demo (10 min) — live-author an instruction and show it firing:** create a small instruction file in a demo repo on screen (pick a rule the room's stack makes visible — e.g., `'**/*.py'` requiring type hints on public functions, or a Conventional Commits rule at `'**'`). Save, register the directory in `chat.instructionsFilesLocations`, reload the window (narrate this — it's the #1 lab stumble), open a matching file, ask Copilot for a small change, and point at the standard appearing uninvoked. Then edit a non-matching file to show silence. If time allows, run `/prompt-analyze` on your hand-written file and let the room watch it get graded.

**Anticipated questions:**

- *"How many rules per instruction file?"* — Small and scoped beats sprawling: a handful of enforceable rules per concern, more files rather than bigger ones. Stacking is additive by design.
- *"Can instructions leak into unrelated answers?"* — They activate on file match, not globally (that's `copilot-instructions.md`'s job). If guidance shows up where it shouldn't, the glob is too broad — narrow it.
- *"Prompt vs. just saving text in a snippets file?"* — Variables, `#file:` injection, agent delegation, and team distribution via the repo. A snippet is personal; a prompt file is infrastructure.

### Module 3 answer key

1. `applyTo:` — the glob evaluated against the current file. Omit it and the file never fires automatically (it's just inert reference text with a description).
2. The Python rule wins while editing `.py` files: stacking loads all matches (global baseline first, broad globs, then specific), and the more specific pattern takes precedence on conflict.
3. `${input:sprintNumber}` and `${input:stylePath:docs/style.md}`.
4. Execution is handed to the Task Planner agent, whose full protocol (phases, gates, tool restrictions) governs the run. The prompt body should supply scope and context — requirements, file references — and must not duplicate or re-specify the agent's workflow.
5. The directory isn't registered under `chat.promptFilesLocations` (workspace settings), or the filename lacks the `.prompt.md` suffix. (Accept "didn't reload the VS Code window" as the third.)

## §5 Module 4 — Authoring Agents & Skills (2:40–3:25)

**Talk track beats:**

1. Open with the restraint question, not the syntax: "When is a custom agent overkill?" Let the room argue, then land the table — most needs are a prompt with `agent:` delegation. The upstream quality bar (duplicate research/planning/implementation agents not accepted) is a useful external authority here.
2. Structural enforcement is the module's big idea: `tools:` omission grants everything; a read-only reviewer is read-only because it *can't* edit, not because it promised. Contrast prose ("never modify code") with the frontmatter that makes the promise unnecessary.
3. For skills: the description is a *matching surface*. Most first skills fail activation, not execution — say it before the lab so students debug the right thing.
4. Close by connecting prompt-builder to everything since the break: analyze-then-build as the standing habit.

**Demo (8 min):** open [`task-researcher.agent.md`](https://github.com/microsoft/hve-core/blob/main/.github/agents/hve-core/task-researcher.agent.md) frontmatter — students have clicked its "📋 Create Plan" button for two levels; show them it's four lines of `handoffs:` YAML. Then open [`rpi-research/SKILL.md`](https://github.com/microsoft/hve-core/blob/main/.github/skills/rpi/rpi-research/SKILL.md) and read just the `description` — point at the "Use when…" clause and ask "what user sentences does this catch?"

**Anticipated questions:**

- *"When is a custom agent overkill?"* — When there's no persona, no tool restriction, no handoff, and no subagent — i.e., when the value is entirely in the words, it's a prompt. Also when an existing agent plus an instruction file covers it: don't fork the researcher to add your naming rules.
- *"Can my agent call another team's agent?"* — Via `handoffs:` (user-mediated transition) or `agents:` subagent delegation (requires the subagent tool — limitation #4). One level deep only: subagents can't spawn subagents.
- *"Do skills run automatically? That sounds dangerous."* — Skills load on description match, but scripts execute through the same tool-approval flow as any terminal command; nothing runs silently. Still: review skill scripts like code, because they are (ref #15 — no added safety layer).
- *"Which model should my agent pin?"* — Default: don't. `model:` is a preference hint with fallback, not a guarantee (limitation #11); pin only with a cost or capability reason, and validate against the model catalog.

### Module 4 answer key

1. Any two of: needs a multi-turn persona/protocol of its own; needs enforced tool restrictions (e.g., read-only); needs `handoffs:` transitions mid-workflow; needs subagent delegation with isolated context. (Bonus signal: no existing agent + instruction combination covers it.)
2. `label` (button text the user sees), `agent` (target agent name), `prompt` (optional template sent to the target), `send` (`true` fires the prompt automatically instead of leaving it staged for the user).
3. Because the body is advisory prose the model can drift from, while `tools:` is enforced by the host — omitting edit/terminal tools makes modification structurally impossible rather than merely discouraged. (Omitting `tools:` entirely grants full access.)
4. `name` must be lowercase kebab-case and must exactly match the skill's directory name. `description` is what Copilot matches against the current request to decide whether to load the skill — it controls activation.
5. Pairs because teammates run different operating systems and the skill must behave identically on both (same args, same exit codes). `references/` holds reference-heavy material (schemas, checklists, anything pushing past ~2000 tokens) so progressive disclosure loads it only when the SKILL.md body points at it.

## §6 Close (3:25–3:30)

- Rapid group quiz: one question per module; save M2's question 5 (the pen-test answer) for last — it's the sentence you most want leaving the room intact.
- Lab briefing: 3A is the PM/TPM capstone and everyone else's chain rehearsal; 3B is the engineering core — **tell students explicitly to commit and keep their Lab 3B artifact set: Level 4's collection lab packages exactly those four files into a distributable collection.** 3C is choose-your-track; steer at least a few students to each track so the Level 4 cohort has all three experiences in the room.
- Preview Level 4: "You now author artifacts. Next time: distributing them — collections, adoption, governance — and teaching this material yourself."
