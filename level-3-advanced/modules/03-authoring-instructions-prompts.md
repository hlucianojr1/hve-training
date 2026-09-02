# Module 3.3 — Authoring Instructions & Prompts

**Time:** 45 min · **Roles:** engineers, tech leads (PMs welcome — prompts are your artifact type) · **Source docs:** [`docs/customization/instructions.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/instructions.md), [`docs/customization/prompts.md`](https://github.com/microsoft/hve-core/blob/main/docs/customization/prompts.md), [`docs/contributing/instructions.md`](https://github.com/microsoft/hve-core/blob/main/docs/contributing/instructions.md)

## Objectives

- Author an instruction file that encodes a real team standard and fires automatically via `applyTo`.
- Author a prompt with input variables and agent delegation.
- Know where these files live in *your* repo and which VS Code settings load them.

You consumed these artifact types in Level 1. Today you produce them. Start with these two — they're the simplest, and they deliver the highest return: an instruction file is the lowest-effort, highest-leverage customization in the whole system.

## 1. Instructions: Encoding Standards That Fire Themselves

An instruction file is YAML frontmatter plus the standard itself:

```markdown
---
description: "Terraform module conventions for the platform team"
applyTo: '**/*.tf'
---

# Terraform Instructions

* Every module exposes `resource_prefix`, never `prefix`.
* State backends are declared in `backend.tf`, one per environment.
```

Rules that make instructions work:

- **`description` is required; `applyTo` is what makes it automatic.** Without `applyTo`, nothing fires. The glob is evaluated against the file you're editing or referencing: `'**/*.py'`, `'**/*.test.ts'`, `'**/src/api/**'`, or comma-combined `'**/*.py, **/*.ipynb'`. Prefer the narrowest glob that covers the standard — `'**/*.test.ts'` over `'**/*.ts'` for test rules.
- **Write real standards, not aspirations.** The content should be what a senior reviewer actually flags in PRs: naming rules, structural conventions, forbidden patterns. If your team doesn't enforce it, don't encode it.
- **Include an Anti-Patterns section** — the convention from [`docs/contributing/instructions.md`](https://github.com/microsoft/hve-core/blob/main/docs/contributing/instructions.md): show the thing to avoid (❌ with a short why) next to the correct alternative (✅). Models follow contrastive examples better than prohibitions alone.
- **Stacking and specificity:** when multiple instruction files match one file, all of them load and merge — `copilot-instructions.md` first (the always-on global baseline), broad globs next, specific globs on top. On conflict, the more specific pattern wins. Design for stacking: keep each file scoped to one concern so overlap is additive, not contradictory.

**Instructor walkthrough (10 min):** live-author a tiny instruction (e.g., "commit messages use Conventional Commits" with `applyTo: '**'`, or a language rule for a stack in the room), save it under `.github/instructions/`, open a matching file, ask Copilot Chat for a change, and watch the standard show up uninvoked. Then open [`markdown.instructions.md`](https://github.com/microsoft/hve-core/blob/main/.github/instructions/hve-core/markdown.instructions.md) as the production-grade version of the same shape.

## 2. Prompts: Reusable Entry Points

A prompt turns a recurring team task into a `/command`:

```markdown
---
description: "Generates release notes from recent commits and merged pull requests"
---

# Generate Release Notes

1. Group changes by category: Features, Bug Fixes, Breaking Changes, Documentation.
2. Include PR numbers and brief descriptions for each entry.
3. Highlight breaking changes at the top with migration guidance.
```

The three mechanisms that lift a prompt above a saved snippet:

- **Input variables:** `${input:varName}` for required inputs, `${input:varName:defaultValue}` for optional-with-default. Document them in an `## Inputs` section — that's the convention every HVE prompt follows (open [`task-research.prompt.md`](https://github.com/microsoft/hve-core/blob/main/.github/prompts/hve-core/task-research.prompt.md): two inputs, four requirements, nothing else). `argument-hint` frontmatter shows usage hints in the picker.
- **Agent delegation:** the `agent:` frontmatter field hands execution to an agent, referenced by the agent's human-readable `name:` (e.g., `agent: Task Planner`). The agent's full protocol governs execution; your prompt body supplies scope and context without duplicating the workflow. This is the pattern behind every `/task-*` command you've used.
- **File injection:** `#file:path/to/file.md` pulls another file's contents in at runtime — e.g., reviewing a change against `#file:.github/instructions/coding-standards/typescript.instructions.md`.

Division of labor to remember: the *prompt* captures what the user wants; the *instruction* captures standards that apply regardless of what anyone wants. If you find yourself writing "always" in a prompt, it probably belongs in an instruction.

## 3. Where Files Live in Your Repo, and What Loads Them

In a consuming repo (yours, not hve-core), the convention is a team subdirectory per artifact type:

```text
.github/
├── instructions/<your-team>/*.instructions.md
└── prompts/<your-team>/*.prompt.md
```

VS Code discovers these through the `chat.*FilesLocations` settings — each maps a directory to `true`:

```json
{
  "chat.instructionsFilesLocations": { ".github/instructions/your-team": true },
  "chat.promptFilesLocations": { ".github/prompts/your-team": true }
}
```

When you add a new directory, register it in the matching setting (workspace `.vscode/settings.json` so teammates inherit it). Lab 3B's troubleshooting table starts here: an authored artifact that "doesn't load" is almost always an unregistered directory or a filename missing the `.instructions.md` / `.prompt.md` suffix.

Finally: the **prompt-builder** agent (`/prompt-build`, `/prompt-analyze`, `/prompt-refactor`) can generate, grade, and consolidate these files for you — Module 4 covers it as the authoring meta-tool. Author your first ones by hand anyway; you can't review what you can't write.

## Use Case Spotlight

A Go team — a stack outside HVE's bundled coding-standards coverage — writes one instruction file: `applyTo: '**/*.go'`, twelve rules lifted from their last quarter of PR review comments, plus an Anti-Patterns section contrasting `errors.Wrap` misuse with correct error wrapping. Within a week the "same three review comments on every PR" pattern stops, because Copilot output arrives pre-conformant. Nobody on the team invokes anything: the glob does the work. This is exactly the workaround for coverage limitation #16 — you just authored the coverage.

## Limitations & Workarounds in This Module

| Limitation | Response |
|-----------|----------|
| Bundled instruction coverage is uneven across stacks | This module *is* the workaround — author your own for your stack ([ref #16](../../resources/limitations-and-workarounds.md)) |
| CLI plugins never auto-apply instructions (no `instructions` component in the plugin spec) | Standards enforcement needs the VS Code extension; on CLI, reference instruction files explicitly in prompts ([ref #6](../../resources/limitations-and-workarounds.md)) |
| Instructions influence, they don't enforce — output can still violate an encoded standard | Keep linters and CI checks as the enforcement layer; instructions reduce violations, gates catch them ([ref #15](../../resources/limitations-and-workarounds.md)) |
| Overlapping `applyTo` globs with contradictory rules produce unpredictable merges | Scope each file to one concern; rely on specificity-wins only for deliberate overrides |

## Knowledge Check

1. What single frontmatter field makes an instruction file activate with zero user action, and what happens if you omit it?
2. Your global baseline says "use tabs"; a `'**/*.py'` instruction says "4-space indentation." What happens when editing a `.py` file, and why?
3. Write the variable syntax for a required `sprintNumber` input and an optional `stylePath` input defaulting to `docs/style.md`.
4. What does `agent: Task Planner` in prompt frontmatter change about execution, and what should the prompt body contain (and not contain)?
5. You add `.github/prompts/platform-team/` to your repo and the prompt never appears under `/`. Name the two most likely causes.

*(Answers: [instructor guide](../instructor-guide.md#module-3-answer-key))*

## Further Reading

- [Customizing with Instructions](https://github.com/microsoft/hve-core/blob/main/docs/customization/instructions.md) — stacking, targeting, role scenarios
- [Creating Custom Prompts](https://github.com/microsoft/hve-core/blob/main/docs/customization/prompts.md) — variables, delegation, `#file:` injection
- [Contributing: Instructions](https://github.com/microsoft/hve-core/blob/main/docs/contributing/instructions.md) — full frontmatter schema and section conventions (including Anti-Patterns)
- [Environment Customization](https://github.com/microsoft/hve-core/blob/main/docs/customization/environment.md) — the complete `chat.*FilesLocations` reference
