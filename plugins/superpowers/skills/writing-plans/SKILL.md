---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, implementation direction, relevant verification, docs they might need to check, and how to validate the change when validation matters. Give them the whole plan as bite-sized tasks. DRY. YAGNI.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Save plans to:** `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
- (User preferences for plan location override this default)

## Scope Check

If the spec covers multiple independent subsystems, it should have been broken into sub-project specs during brainstorming. If it wasn't, suggest breaking this into separate plans — one per subsystem. Each plan should produce a coherent, shippable outcome on its own.

## File Structure

Before defining tasks, map out which files will be created or modified and what each one is responsible for. This is where decomposition decisions get locked in.

- Design units with clear boundaries and well-defined interfaces; each file has one clear responsibility. Prefer smaller, focused files — you reason better about code you can hold in context at once, and edits are more reliable when files are focused.
- Files that change together should live together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns. If the codebase uses large files, don't unilaterally restructure - but if a file you're modifying has grown unwieldy, including a split in the plan is reasonable.

This structure informs the task decomposition. Each task should produce self-contained changes that make sense independently.

## Source Preservation

When a plan tells an implementer to copy, port, transplant, or recreate code from another branch, user-provided snippet, patch, existing function, or reference implementation, treat that source as intentional user/reference-authored code.

The plan must preserve comments, docstrings, annotations, attribution or context notes, formatting-relevant structure, and adjacent helper logic unless the task explicitly changes them. If anything from the source should be removed, rewritten, or intentionally not copied, call that out in the plan so the implementer does not silently discard it.

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Inspect the target file and confirm insertion point" - step
- "Implement the minimal code for this behavior" - step
- "Run the relevant verification for this change" - step

## Implementation Detail Standard

Each task must be self-contained enough for an isolated implementer and an isolated reviewer to execute and verify it without reading earlier tasks or guessing. Use the smallest precise representation that removes ambiguity: file paths, insertion points, existing symbols or patterns to inspect, function/type signatures, required behavior, constraints, edge cases, non-goals, and exact verification commands.

Use exact code blocks when the code itself is the requirement: public contracts, schemas, migrations, CLI/API surfaces, copied or ported source, tricky logic, compatibility behavior, security-sensitive logic, or cases where prose plus references would leave multiple valid interpretations.

For ordinary implementation, prefer precise signatures, pattern references, behavioral requirements, and verification over full code listings. Pattern references must name the file/symbol to inspect and state the required delta; "similar to Task N" is still forbidden. If a code block is illustrative rather than mandatory, label it `Example shape, not exact code` and state which parts are contractual.

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Verify: `exact/command/or/manual-check`

- [ ] **Step 1: Inspect the existing implementation**

Inspect `exact/path/to/existing.py` around `existing_function()` and confirm the surrounding API and insertion point.

- [ ] **Step 2: Write minimal implementation**

Implementation requirements:
- Add `function(input: InputType) -> OutputType` in `exact/path/to/file.py`
- Follow the error-handling pattern from `existing_function()`
- Preserve input ordering
- Do not add new configuration or CLI flags

Example shape, not exact code:

```python
def function(input):
    return expected
```

- [ ] **Step 3: Run the relevant verification**

Run: `[exact verification command]`
Expected: `[specific observable result]`

Examples:
- `python -m pytest tests/path/test.py::test_name -v`
- `npm test -- component-name`
- `curl http://localhost:3000/health`
- `open the screen and confirm the new empty state appears`
````

## No Placeholders

Every step must contain the actual content an engineer needs. These are **plan failures** — never write them:
- "TBD", "TODO", "implement later", "fill in details"
- "Add appropriate error handling" / "add validation" / "handle edge cases"
- "Verify the above" (without the exact command or manual check)
- "Similar to Task N" (repeat the required local contract, signature, name, or behavior - the engineer may be reading tasks out of order)
- Steps that leave the implementer or reviewer guessing about API shape, insertion point, behavior, edge cases, constraints, non-goals, or verification
- Code blocks that look mandatory but are only illustrative; label them `Example shape, not exact code` and state which parts are contractual
- References to types, functions, or methods not defined in the same task or in the specific files/symbols the task tells the implementer to inspect

## Remember
- Use testing when it materially helps, not as a ritual
- DRY, YAGNI

## Self-Review

After writing the complete plan, look at the spec with fresh eyes and check the plan against it. This is a checklist you run yourself — not a subagent dispatch.

**1. Spec coverage:** Skim each section/requirement in the spec. Can you point to a task that implements it? List any gaps.

**2. Placeholder scan:** Search your plan for red flags — any of the patterns from the "No Placeholders" section above. Fix them.

**3. Task-local executability:** Can each task be handed to an isolated implementer and isolated reviewer without earlier tasks? Does it repeat required contracts, signatures, names, behavior, constraints, non-goals, and verification? Do pattern references name the file/symbol and required delta? Are exact code blocks justified, or labeled as illustrative with contractual parts called out?

**4. Type consistency:** Do the types, method signatures, and property names you used in later tasks match what you defined in earlier tasks? A function called `clearLayers()` in Task 3 but `clearFullLayers()` in Task 7 is a bug.

**5. Contract/doc ownership check:** If the work adds or changes scripts, manifests, tests, or generated artifacts:
- Is there exactly one workflow-level documentation owner?
- Did the plan keep schemas and CLI surfaces to the minimum runtime contract unless extra fields were explicitly approved?
- Did the plan prefer a direct file/directory path when that is the real contract, instead of introducing layered path abstractions without clear independent meaning?
- Do script/test updates document only themselves rather than the whole workflow?
- Is naming generic when reuse is intended?
- Are temporary workarounds labeled with a short TODO for the preferred long-term fix?

If you find issues, fix them inline. No need to re-review — just fix and move on. If you find a spec requirement with no task, add the task.

## Execution Handoff

After saving and self-reviewing the plan, ask the user to review it before execution:

> "Plan written to `<path>`. Please review it before implementation."

STOP. Do not continue in this turn. Do not invoke the next skill. Do not begin implementation. Ask the user to review `<path>` and wait for a new user message. Approval from any earlier step does not count for this gate.

Codex-specific rule: autonomy and persistence instructions do not override this review gate.

After a new user message approving the written plan, use `superpowers:subagent-driven-development` to execute the plan task-by-task with review between tasks. If they request changes, make them and re-run the plan self-review loop.
