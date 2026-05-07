---
name: refactoring-integration
description: Use for high-risk refactoring, branch alignment, feature migration, backend integration, or architecture migration where behavior must be preserved, history must stay clean, and changes need staged planning, semantic commits, and validation gates. Trigger when users ask to align branches, port features across divergent code, modernize structure, integrate a backend, refactor safely, or preserve project-specific functionality while adopting another branch's architecture.
---

# Refactoring Integration

## Overview

High-risk code movement fails when agents treat it as ordinary editing. Missing one file, guessing a schema, or mixing unrelated changes can silently break behavior or destroy useful history.

Use this skill for refactors, branch alignments, backend integrations, migrations across divergent structure, or any work where behavior preservation and auditability matter.

**Core principle:** Preserve behavior and make every change auditable.

Do not start by editing code. First identify the contract, the authoritative implementation, and the smallest sequence of reviewable steps that keeps the system working.

## When To Use

Use this skill when the user asks to:

- Align a feature branch with a divergent base branch
- Port features across a changed directory, build, or runtime structure
- Modernize architecture while preserving project-specific behavior
- Integrate a backend or replace a backend dispatch path
- Move, split, or consolidate code where behavior must not change
- Preserve branch ancestry while producing a clean final tree

This skill is additive. If the work also triggers design, planning, debugging, review, or final verification workflows, follow those skills normally and apply this skill's contract, ledger, edit discipline, branch discipline, and validation gates inside that workflow.

## Start With The Contract

Before planning or editing:

1. Identify the goal in one sentence.
2. List what must be preserved.
3. List what is explicitly out of scope.
4. Identify the authoritative structure or reference implementation.
5. Identify the final branch/history requirement.
6. Identify required validation commands or manual checks.

If any item is ambiguous and cannot be inferred safely from the repository, ask before touching files.

For integration work, prefer a contract like:

```text
Goal: <what the final tree must support>
Preserve: <feature behavior, public API, models/assets, user workflows>
Adopt: <reference branch/module/build structure>
Drop: <obsolete demos/tests/assets/history explicitly excluded>
Final history: <which branch ancestry must be preserved>
Validation: <build/test/runtime checks>
```

## Refactoring Plan Content

For multi-step work:

1. Check whether the current task already has an approved plan or an in-progress planning workflow in the conversation or repository.
2. If one exists, defer to that plan and add the refactoring/integration details below when they are missing.
3. If none exists, create an implementation plan before editing.

Each refactoring task must be a semantic batch that can be reviewed and committed independently. Avoid placeholders such as "handle edge cases", "update callers", or "verify"; name the exact files, edits, callers, and checks.

Include these details in the plan when they apply:

- Contract and invariants: runtime behavior, public APIs, build outputs, branch/history requirements, and explicit out-of-scope items.
- File ledger: create, modify, move, restore, drop, and verify paths.
- Execution protocol: when to inform the user, pause, validate, stage, and commit.
- Validation protocol: exact narrow and broad checks, with expected observable results.

## Branch Alignment Pattern

When aligning a feature branch with a divergent base branch, do not rely on cherry-picking individual commits if file structure changed significantly. Use a tree/output based workflow:

1. Create a backup branch for the original feature head.
2. Create the final history-preserving branch from the feature branch.
3. Create a scratch/reference branch from the target base branch.
4. Generate an exhaustive delta ledger from the feature branch point:

```bash
MB=$(git merge-base <base> <feature>)
git diff --name-status "$MB"..<feature> > /tmp/<feature>-delta.txt
git diff --stat "$MB"..<feature> > /tmp/<feature>-delta-stat.txt
git log --oneline --reverse "$MB"..<feature> > /tmp/<feature>-commits.txt
```

5. Classify every delta path exactly once:

```text
Restore Whole Path: feature-owned paths/assets that should be replayed as-is.
Manual Port: shared files where the target base structure must remain authoritative.
Drop: obsolete tests/demos/assets/history explicitly out of scope.
New Alignment Work: code needed because the target base has new structure or APIs.
```

6. Verify classification exhaustiveness with a script: zero missing paths, zero duplicates.
7. Replay whole-path owned files onto the scratch branch first and commit that batch.
8. Apply manual ports and new alignment work as semantic commits.
9. Validate the scratch tree.
10. Record the scratch branch in the final feature-history branch with an `ours` merge.
11. Replace the final branch tree from the verified scratch tree and verify no diff remains.
12. Only then optionally move the original feature branch name to the aligned branch, keeping the backup.

This pattern preserves feature ancestry while making the final implementation match a clean reference tree.

## Edit Discipline

Follow the existing codebase before inventing new structure.

- Preserve existing behavior unless the contract explicitly changes it.
- Change one thing per step: movement, build wiring, backend dispatch, API adaptation, or validation.
- Keep diffs small enough to inspect.
- Prefer additive scaffolding before rewiring existing callers.
- Do not add speculative abstractions.
- Do not refactor unrelated working code.
- Do not add compatibility shims after all callers are updated unless the contract requires backward compatibility.
- Do not guess model schemas, tensor names, build flags, or generated artifacts; inspect them with local tooling.
- Use structured APIs/parsers when available instead of ad hoc text manipulation.
- If a patch/build script owns a dependency checkout, make that ownership explicit and deterministic; otherwise do not reset or overwrite external local edits.

For backend or architecture integration:

1. Inspect the reference module that already follows the target architecture.
2. Mirror its public API shape, build-system pattern, and runtime setup.
3. Preserve feature-specific behavior behind the new structure.
4. Keep backend-specific code behind compile-time or runtime dispatch already used by the project.
5. Verify both the old backend/path and the new backend/path unless one is explicitly out of scope.

## Validation Gates

Do not claim completion without evidence.

After each semantic batch:

1. Run the narrowest useful verification first.
2. Run broader build/test checks when the batch touches shared behavior, build logic, public API, or cross-module contracts.
3. Summarize pass/fail and the exact blocker if validation fails.
4. Stop for user inspection before committing when the user requested a validation-gated workflow.
5. Commit only after approval, and commit only the files belonging to that semantic batch.

Useful checks include:

```bash
git status --short --branch
git diff --check -- <paths>
<focused build or test command>
<full relevant build or test command>
```

For generated artifacts or model preloads, verify the generated output references the exact expected names/paths.

## Git Hygiene

- Never use `git add .` for mixed changes; stage exact files by semantic purpose.
- Do not include local plan files, scratch ledgers, dirty dependency submodules, or generated build outputs unless the contract says they are deliverables.
- Make asset/model commits explicit and early when large binaries or runtime assets are part of the feature.
- Keep each commit reviewable: replay, build integration, API adaptation, backend implementation, binding changes, build-script changes, and final transfer should be separate when practical.
- Before replacing branch refs, create or verify backup refs.
- Do not push or force-push unless the user explicitly asks.

## Collaboration Protocol

When the user wants inspection-driven work:

1. Announce the task starting and files/refs likely to be touched.
2. Make the edit.
3. Run the planned verification.
4. Stop and ask for inspection/approval before staging or committing.
5. After approval, commit the semantic batch.
6. Announce the next task before proceeding.

Do not print large diffs, status dumps, or command output unless the user asks. Give concise status updates and actionable blockers.

## Review And Debugging

If a build or test fails:

1. Read the exact error and identify the failing component.
2. Compare against a working reference in the same codebase.
3. Form one hypothesis and make the smallest fix that addresses the root cause.
4. Re-run the failing command.
5. Do not stack unrelated fixes.

Before final branch transfer or merge:

- Review the changed tree against the original contract.
- Confirm out-of-scope paths did not re-enter.
- Confirm the final branch/tree matches the verified reference when using a scratch branch.
- Confirm backup refs exist before branch replacement.
