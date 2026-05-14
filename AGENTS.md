# Repository Guidelines

## Purpose

This repository contains the Superpowers skill suite for coding agents. The primary deliverables are clear, consistent `SKILL.md` files and focused supporting prompt/reference files.

This fork supports Codex only. Do not add compatibility instructions, workflows, metadata, or examples for other agent tools.

## Upstream Sync Reference

Original project: `obra/superpowers`, branch `upstream/main`.

Latest reviewed upstream commit: `917e5f53b16b115b70a3a355ed5f4993b9f8b73d`.

When upstream sync checks are performed, compare changes after this commit against this fork's current skill set. Integrate only changes that are relevant to this Codex-only fork, and update this commit hash to the latest reviewed `upstream/main` commit.

## Project Structure & Module Organization

- `plugins/superpowers/skills/<skill-name>/SKILL.md` is the source of truth for each skill.
- Supporting files live beside the skill that uses them, such as `plugins/superpowers/skills/systematic-debugging/root-cause-tracing.md` or `plugins/superpowers/skills/subagent-driven-development/implementer-prompt.md`.
- `README.md` is only the suite overview. Do not duplicate operational skill instructions there.

## Skill Authoring Workflow

When creating or updating a skill, follow this structure:

1. Confirm the skill location.
   The folder name, frontmatter `name`, and any references to the skill must match exactly. Use lowercase hyphenated names, such as `refactoring-integration`.

2. Write concise frontmatter.
   Include only `name` and `description`. Keep `description` trigger-focused: one sentence that says when the skill applies. Move detailed conditions, examples, and workflow boundaries into the body.

3. Start with the standard body shape.
   Use a clear title, then a short overview explaining the skill's purpose. Add a bold `Core principle` when the workflow needs discipline or a memorable rule.

4. Define when the skill applies.
   Add `When to Use`, `When NOT to Use`, or equivalent trigger guidance when applicability could be confused. Keep cross-skill references explicit, using names like `superpowers:verification-before-completion`.

5. Describe the workflow concretely.
   Prefer direct instructions, checklists, phase lists, command examples, prompt templates, and good/bad examples over abstract advice. Tell the agent what to do, what to avoid, and when to stop.

6. Add enforcement sections only when useful.
   Sections like `Iron Law`, `Red Flags`, `Common Mistakes`, or `Forbidden Responses` are appropriate for strict behavioral workflows, but do not add them as boilerplate.

7. Place supporting material beside the skill.
   Put prompts, references, and scripts in the same skill folder and link them from `SKILL.md`. Avoid extra README files inside skill folders.

Before changing behavior or workflows, inspect nearby skills and preserve their tone, structure, and naming conventions. Keep edits small and auditable. Do not rewrite unrelated files, and do not revert user changes unless explicitly asked.

Do not add stale references to removed skills. Before documenting a skill, confirm it exists.
