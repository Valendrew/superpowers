# Repository Guidelines

## Purpose

This repository contains the Superpowers skill suite for coding agents. Treat it as a documentation-first project: the primary deliverables are clear, consistent `SKILL.md` files and focused supporting prompt/reference files.

## Project Structure & Module Organization

- `skills/<skill-name>/SKILL.md` is the source of truth for each skill.
- Supporting files live beside the skill that uses them, such as `skills/systematic-debugging/root-cause-tracing.md` or `skills/subagent-driven-development/implementer-prompt.md`.
- `README.md` is only the suite overview. Do not duplicate operational skill instructions there.
- `docs/` contains platform or supporting documentation, currently including Codex installation notes and prompt optimization material.

## Skill Authoring Rules

Each skill must have YAML frontmatter with `name` and `description`, followed by concise Markdown instructions. Use lowercase hyphenated skill names, matching the folder name exactly, for example `refactoring-integration`.

Write skills in the same style as the existing suite:

- Start with a clear title and a short overview.
- State the core principle when the workflow needs discipline.
- Use direct instructions, not generic advice.
- Keep cross-skill references consistent with existing skills.
- Avoid extra README files inside skill folders.

## Documentation Scope

Keep documentation single-purpose. If guidance controls how a skill operates, place it in that skill’s `SKILL.md` or a directly referenced sibling file. If guidance only explains the repository at a high level, place it in `README.md` or this file.

Do not add stale references to removed skills. Before documenting a skill, confirm it exists with:

```bash
find skills -maxdepth 2 -name SKILL.md -print | sort
```

## Agent-Specific Instructions

Before changing behavior or workflows, inspect nearby skills and preserve their tone, structure, and naming conventions. Keep edits small and auditable. Do not rewrite unrelated files, and do not revert user changes unless explicitly asked.

Before reporting completion, verify the touched Markdown files with:

```bash
git diff --check -- <paths>
rg -n "<removed-or-renamed-skill>" README.md AGENTS.md skills || true
```
