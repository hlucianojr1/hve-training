# HVE Framework Training Curriculum

A four-level training program for Microsoft's **Hypervelocity Engineering (HVE)** framework — the prompt/agent library for GitHub Copilot built around the RPI methodology (Research → Plan → Implement → Review).

This curriculum is designed for **mixed-role audiences**: engineers, tech leads, and PMs/TPMs all start together at Level 1 and split into role tracks where the material diverges.

## Curriculum at a Glance

| Level | Theme | Audience | Instructor Session | Self-Paced Labs | Exit Criteria |
|-------|-------|----------|-------------------|-----------------|---------------|
| [1 — Foundational](level-1-foundational/README.md) | Why HVE + your first workflow | All roles | 3–4 h | ~2 h | One full guided RPI cycle completed; can locate any agent in the catalogs |
| [2 — Intermediate](level-2-intermediate/README.md) | Operating HVE well, every day | All roles, role tracks begin | 3–4 h | ~3 h | Unaided strict RPI on your own repo; can name the four context-degradation symptoms and the fix for each |
| [3 — Advanced](level-3-advanced/README.md) | Full lifecycle + authoring your own artifacts | Engineers, tech leads (PMs: modules 1–2) | 3–4 h | ~3 h | Custom artifact set loads and works in Copilot Chat; full planning chain produced |
| [4 — Expert](level-4-expert/README.md) | Distribution, adoption, and teaching HVE | Champions, leads, future instructors | 3–4 h | ~3 h | Collection validates; adoption plan drafted; teach-back delivered against rubric |

Total program: **4 half-day sessions + ~11 hours of labs**, typically delivered one level per week.

## Prerequisites (all students)

- VS Code with the **GitHub Copilot** and **GitHub Copilot Chat** extensions, active license
- Git installed; a work repository you can safely experiment in (labs use your own repos)
- The **HVE Core** extension installed from the VS Code Marketplace (`ise-hve-essentials.hve-core`) — covered in [Lab 1A](level-1-foundational/workshops/lab-1a-install-verify.md) if not done in advance
- `.copilot-tracking/` added to your repo's `.gitignore` (Lab 1A verifies this)

## How to Use This Curriculum

**As an instructor:**

1. Read the [Instructor Handbook](instructor-handbook.md) first — session logistics, demo preparation (critical: live research runs 20–60 minutes; pre-bake artifacts), and mixed-role facilitation.
2. Each level has an `instructor-guide.md` with a minute-by-minute agenda, talk tracks, demo scripts, discussion prompts, and knowledge-check answer keys.
3. Modules are student-facing; teach from them directly. Instructor-only cues appear in the instructor guides, never in module files.

**As a self-paced student:**

1. Work through each level's `modules/` in order, then complete the `workshops/` labs.
2. Every lab is fully self-runnable: prerequisites, numbered steps, expected outputs, a Verify Your Work checklist, and troubleshooting.
3. Check yourself against each level's exit criteria (in the level README) before moving on.

**Role tracks:** Levels 1 is common to all roles. Level 2 Module 4 and Lab 2C split into engineer / PM-TPM / tech-lead tracks. Level 3 is authoring-heavy (PMs typically take modules 1–2 plus Lab 3A only). Level 4 is for anyone who will champion, govern, or teach HVE.

## Directory Map

```text
training/
├── README.md                     ← you are here
├── instructor-handbook.md        ← read first if teaching
├── level-1-foundational/         ← why HVE, setup, artifact types, first RPI
├── level-2-intermediate/         ← strict RPI, context engineering, mode selection, role tracks
├── level-3-advanced/             ← lifecycle, specialty planners, authoring all 4 artifact types
├── level-4-expert/               ← collections, adoption/governance, quality engineering, teaching
└── resources/
    ├── cheat-sheet.md            ← 1-page RPI + command reference (print for every session)
    ├── limitations-and-workarounds.md  ← consolidated limitations table
    └── glossary.md               ← HVE terminology
```

## Grounding

Every module teaches from — and links to — the official HVE Core documentation in this repository (`docs/`). The curriculum adds sequencing, facilitation, exercises, and assessment; it deliberately does not duplicate reference content that would drift out of date. Key sources: [`docs/rpi/`](https://github.com/microsoft/hve-core/blob/main/docs/rpi/README.md), [`docs/getting-started/`](https://github.com/microsoft/hve-core/blob/main/docs/getting-started/README.md), [`docs/architecture/ai-artifacts.md`](https://github.com/microsoft/hve-core/blob/main/docs/architecture/ai-artifacts.md), [`docs/hve-guide/`](https://github.com/microsoft/hve-core/blob/main/docs/hve-guide/README.md), [`docs/customization/team-adoption.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/team-adoption.md), and [`TRANSPARENCY-NOTE.md`](https://github.com/microsoft/hve-core/blob/main/TRANSPARENCY-NOTE.md).
