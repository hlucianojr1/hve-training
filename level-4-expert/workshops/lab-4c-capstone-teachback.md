# Lab 4C — Capstone Teach-Back

**Time:** 60–90 min (30–40 prep if Module 4's loop was followed, ~20 delivery, ~15 feedback) · **Self-runnable:** yes (solo variant below)
**Goal:** deliver [Level 1 Module 1 — Why HVE](../../level-1-foundational/modules/01-why-hve.md) to a peer or small group, compressed to ~20 minutes, and be graded against the rubric. This is the program's capstone: you pass Level 4 by teaching, not by answering questions about teaching.

## Prerequisites

- Module 4 completed; [Instructor Handbook §2](../../instructor-handbook.md) re-read
- The source module and its [answer key](../../level-1-foundational/instructor-guide.md#module-1-answer-key) studied
- An audience: 1–3 colleagues (they do *not* need HVE experience — a naive audience is better) — or the solo variant
- A repo you can project, for the demo

## Part 1 — Preparation Checklist (do this days before, not hours)

- [ ] **Pre-bake the demo artifacts:** run a research task end-to-end in your demo repo; keep the research doc. Prepare the side-by-side: a plain-chat answer vs the research doc *for the same task* — the citations do the arguing
- [ ] **Plan the live-start:** know exactly which invocation you'll start live for 2–3 minutes before switching to the pre-baked doc, and the honest switch line ("research runs ~20 minutes; here's one I ran earlier")
- [ ] **Prepare the outage fallback:** confirm you can deliver the whole demo narrating over the pre-baked artifacts with Copilot closed
- [ ] **Feynman pass:** explain the why-rpi thesis out loud, from memory, in under 2 minutes — the constraint ("when AI knows it cannot implement, it optimizes for verified truth, not plausible code") must be in your own words
- [ ] **Know the hard lines:** be ready to state, unprompted, at least two things HVE is *not* and one Transparency-Note never-use (developer performance assessment is the one clients ask about)
- [ ] **Dry-run with a timer:** full delivery to an empty room; a 20-minute slot means ~4 min problem, ~4 min insight, ~6 min demo, ~4 min what-HVE-is/is-not + honest trade, ~2 min questions
- [ ] **Prepare your audience:** ask them to bring one story of AI-generated code that was confidently wrong (the module opens with it), and to ask at least one hard question each

## Part 2 — Delivery (~20 min)

Teach the module's actual arc: the problem they already have → the counterintuitive insight → the RPI answer → what HVE is and is not → the honest trade. Use the discussion prompt, land the core sentence twice, run your demo with the live-start-then-switch, and finish with 2–3 of the module's knowledge-check questions run as a discussion.

Your graders should note timestamps at each segment boundary — timing is a rubric dimension.

## Part 3 — Structured Feedback (~15 min)

Immediately after, the audience grades the rubric below **independently first, then discusses** — dimension by dimension, evidence required ("you said X at minute 12"), not vibes. You self-grade too; a large gap between self-grade and audience grade is itself a finding.

## The Grading Rubric

| Dimension | 1 — Below bar | 2 — Competent | 3 — Instructor-ready |
|-----------|---------------|----------------|----------------------|
| **Accuracy of the why-rpi thesis** | Restates "RPI = phases" without the causal claim, or gets it wrong (e.g., "research makes the model smarter") | States the constraint correctly: forbidden from implementing → optimizes for verified truth; phases and artifacts accurate | Lands it in their own words with a concrete plausible-vs-verified example (e.g., `prefix` vs `resource_prefix`), and can restate it under challenge |
| **Demo artifact quality** | No pre-baked artifacts, or demo attempted cold/live and stalled | Pre-baked research doc walked competently; citations shown | Live-start-then-switch executed with an honest switch line; side-by-side comparison lets citations argue; fallback plan evident |
| **Handling of questions** | Bluffs, or answers a different question than was asked | Answers correctly, admits uncertainty when real | Uses questions as teaching moments; cites source docs ("the Transparency Note draws that line"); redirects out-of-scope items honestly |
| **Honest treatment of limitations** | Limitations skipped or hand-waved ("it basically always works") | States what HVE is not and at least one never-use line when prompted | Volunteers the honest trade (first workflow feels slower), the no-safety-layer point, and the developer-performance red line — unprompted, integrated, not a disclaimer slide |
| **Timing discipline** | >6 min over/under the 20-minute target, or a segment consumed the session | Within ~3 min of target; all segments delivered | On target with deliberate pacing; visibly cut or expanded material in-flight without losing the arc |

**Passing bar: ≥11 of 15, with no dimension scored 1.** A single 1 fails the attempt regardless of total — each dimension is a failure mode that sinks real sessions on its own. Retakes are expected and cheap: remediate the weak dimension (the Module 4 prep loop tells you how) and redeliver within a week.

## Solo Variant (Feynman fallback)

No peers available? Record yourself:

1. Run the full prep checklist — no shortcuts because nobody's watching; the recording will tell on you.
2. Record a complete 20-minute delivery (screen + audio), demo included, to an imagined audience.
3. Wait at least a day, then watch it as a skeptical student and grade the rubric honestly, timestamping evidence for every score.
4. For the questions dimension: pause the recording at three points and answer, cold and out loud, the three anticipated questions from the [Level 1 instructor guide §2](../../level-1-foundational/instructor-guide.md) ("isn't this waterfall for AI?", "we already review AI code carefully", "does this work with other IDEs?").

Same passing bar. If you can, send the recording and your self-graded rubric to a Level 4 peer for a second opinion.

## Verify Your Work

- [ ] Every prep-checklist box was checked *before* delivery
- [ ] Delivery happened — live or recorded — covering the module's full arc within timing tolerance
- [ ] A completed rubric exists with evidence notes (audience-graded, or timestamped self-grade for solo)
- [ ] Score ≥11/15 with no 1s — or a named weak dimension and a scheduled retake
- [ ] You can say what you'd change in the next delivery (there is always something)

## If Something Went Wrong

| Symptom | Fix |
|---------|-----|
| Demo stalled waiting on live research | The classic — you skipped pre-baking or trusted the live run; re-read Handbook §2, pre-bake, redeliver ([ref #5](../../resources/limitations-and-workarounds.md)) |
| Ran 10+ minutes over | Dry-run wasn't timed, or discussion went unbounded; script your segment boundaries and practice cutting the demo walkthrough, not the limitations |
| Audience asked something you couldn't answer | Fine *if* handled honestly ("I don't know — the Transparency Note is where I'd check") — that scores 2, bluffing scores 1 |
| You realize mid-delivery you don't actually believe a claim you're making | Best possible failure: you found a Feynman gap live; note it, finish honestly, remediate with the source doc, retake |
| Audience graded everything 3 without evidence | Politeness, not assessment — require a timestamp per score and re-run the discussion |

## What You Learned

- Teaching is the strictest test of understanding: every gap Level 1–3 left becomes visible under an audience's questions.
- The pre-baked demo pattern isn't stagecraft avoidance — it's honest engineering around a real constraint, and modeling that honesty is itself the lesson.
- Rubric-based feedback turns "that went okay" into named, fixable dimensions — the same move HVE makes on AI output: verify, don't vibe.

## Stretch Goal

Schedule the real thing: a 45-minute brown-bag of the full Module 1 (not the compressed cut) for your team or client, using your graded rubric's weak dimension as the prep focus. Then start assembling your own Level 1 delivery kit: demo repo with a `demo-start` branch, pre-baked artifact set, and your two-minute thesis — the [Instructor Handbook](../../instructor-handbook.md) is now *your* handbook.
