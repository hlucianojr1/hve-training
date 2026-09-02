# Module 2.2 — Context Engineering

**Time:** 50 min · **Roles:** all · **Source docs:** [`docs/rpi/context-engineering.md`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md)

This is the load-bearing module of the whole curriculum. Level 1 gave you the rule (`/clear` between phases). This module gives you the *mechanism* — so you can diagnose degraded behavior instead of being surprised by it, and choose the right context tool instead of clearing reflexively.

## Objectives

- Explain recency bias: why 3K tokens of instructions lose to 50K+ tokens of output.
- Recognize the four degradation symptoms on sight.
- Choose correctly between `/clear`, `/compact`, `/checkpoint`, and a new chat.
- Restore context after clearing — per transition and across sessions.

## 1. The Mechanics: Recency Bias

At the start of a session, an agent's system prompt is roughly **3K tokens** — and it's most of what the model can see, so the model follows it closely. After one full RPI cycle, the conversation holds **50K–200K tokens** of implementation output: code, file contents, tool results, validation logs.

The model doesn't *forget* the instructions. It **deprioritizes** them: recent tokens receive disproportionate attention weight. A 3K system prompt that was 30% of a 10K conversation is 1.5% of a 200K one — background noise. The dominant pattern in the conversation is now "implement and validate," so a new request pattern-matches to that instead of to the phase-ordering instructions.

The concrete failure sequence, verbatim from the docs because you will live it in Lab 2B:

1. First `/rpi` request works correctly, executing all phases in order.
2. Conversation grows to 50K+ tokens of implementation output, file contents, tool results.
3. Second `/rpi` request **skips directly to implementation**, producing shallow output that misses edge cases.

The output still compiles. Tests still pass. It *looks* right — that's what makes this failure mode dangerous.

## 2. The Four Degradation Symptoms

Memorize these — they're an exit criterion for this level. Any one of them means: stop, `/clear`, restore from artifacts.

1. **Skips phases** — jumps from your request straight to writing code, no research or planning.
2. **Ignores its own instructions** — phase ordering, formatting rules, or convention requirements vanish from output.
3. **Shallow output** — thin analysis, unaddressed edge cases, repeats the same patterns instead of investigating alternatives.
4. **Echoes the previous task** — reproduces the structure and approach of the *last* task instead of following instructions for the new one.

Degradation is not a bug to report. It's physics to manage.

## 3. The Context Toolbox

Four tools, four situations. Reflexive `/clear` is better than nothing, but the intermediate skill is picking deliberately:

| Command | Effect | Use when |
|---------|--------|----------|
| `/clear` | Removes all conversation history | Between phases; before every new `/rpi`; switching tasks; any degradation symptom |
| `/compact` | Summarizes history into condensed form (lossy) | Mid-phase, conversation growing long but you must continue the current task |
| `/checkpoint` | Persists state to disk via the memory agent | Between sessions; before a risky `/clear` when progress notes matter |
| New chat | Fresh session, same effect as `/clear` | Starting unrelated work |

Two honesty notes on `/compact`:

- It's **lossy by design** — the model decides what survives summarization, and critical nuance may not. That's why phase transitions use `/clear` + artifacts (deterministic), never `/compact` (probabilistic).
- It was **removed from agent handoff buttons** because Autopilot mode could trigger *compaction loops* that degraded context unpredictably. It's still available as a typed command; the removal is a design lesson: automated summarization compounds its own losses.

## 4. Restoring Context After /clear

`/clear` wipes chat, not disk. Artifacts in `.copilot-tracking/` survive; your job is to bring the right one back into view. Two reliable mechanisms: **open the file in the editor** before invoking the next agent (Copilot Chat reads visible editor tabs), or **reference its path** in your prompt. The `/task-*` prompts attempt to auto-discover recent artifacts, but that's unreliable when multiple topics coexist — opening the file wins.

| Transition | Open or reference |
|------------|-------------------|
| Research → Plan | `.copilot-tracking/research/<topic>-research.md` |
| Plan → Implement | `.copilot-tracking/plans/<topic>-plan.instructions.md` |
| Implement → Review | `.copilot-tracking/changes/<topic>-changes.md` (plan and research help) |
| Review → Rework/Iterate | `.copilot-tracking/reviews/<topic>-review.md` |

## 5. Cross-Session Persistence

Artifacts carry *task* knowledge, but not *session* state — which phase you were in, what you decided, what's next. For work spanning days:

- Click **💾 Save** (RPI Agent) or invoke the **memory** agent — writes `.copilot-tracking/memory/YYYY-MM-DD/<description>-memory.md` with task overview, current phase, completed work, next steps, and key file paths.
- To resume: new chat → `/clear` → `/checkpoint continue <description>` → the memory agent restores context and offers to continue.
- Save **before** any `/clear` between phases when you have progress notes worth keeping. The checkpoint *supplements* the planning artifacts; it never replaces them.

## Use Case Spotlight

The opening scenario of [context-engineering.md](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md): a developer's first `/rpi` produces excellent code, so they trust the same session with "now add input validation to the API endpoint." The agent skips research, misses three edge cases research would have caught, ignores the codebase's existing validation patterns, and invents a naming convention contradicting every other validator in the project. Nothing failed loudly. The cost surfaced weeks later as inconsistency and rework — which is exactly why you learn to see symptoms early.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Context degradation / phase skipping on second request | `/clear` before every new `/rpi`; restore from artifacts ([ref #1](../../resources/limitations-and-workarounds.md)) |
| `/compact` is lossy; removed from handoff buttons after Autopilot compaction loops | Prefer `/clear` + artifact restore between phases; `/checkpoint` for cross-session state ([ref #3](../../resources/limitations-and-workarounds.md)) |
| rpi-agent accumulates tokens across all phases in one session | Watch for the four symptoms; `/clear` or `/compact` before a second `/rpi` ([ref #2](../../resources/limitations-and-workarounds.md)) |

## Knowledge Check

1. Why does a 3K-token system prompt "lose" in a 200K-token conversation, if the model never actually forgets it?
2. Recite the three-step concrete failure sequence for two `/rpi` requests in one session.
3. Name all four degradation symptoms.
4. Mid-implementation, your conversation is getting long but you need to finish the current phase. Which command, and why not `/clear`?
5. Why was `/compact` removed from agent handoff buttons, and what should you use for cross-session persistence instead?

*(Answers: [instructor guide](../instructor-guide.md#module-2-answer-key))*

## Further Reading

- [Context Engineering](https://github.com/microsoft/hve-core/blob/main/docs/rpi/context-engineering.md) — this module's source, worth a full read
- [Using RPI Agents Together — Session Persistence](https://github.com/microsoft/hve-core/blob/main/docs/rpi/using-together.md) — Save button and `/checkpoint continue` flows
