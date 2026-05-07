# Superpowers

Superpowers is a software development workflow for coding agents. It is organized as a suite of composable skills under `skills/`.

Each skill is self-contained in `skills/<skill-name>/SKILL.md`. This README is only the suite overview; the skill files are the source of truth for operational instructions.

## Workflow

The core workflow is:

1. **using-superpowers** - Checks for applicable skills before every task and establishes skill invocation rules.
2. **brainstorming** - Turns rough ideas into an approved design before implementation starts.
3. **writing-plans** - Converts approved requirements into a detailed implementation plan.
4. **subagent-driven-development** - Executes implementation plans task by task with isolated agents and review gates.
5. **requesting-code-review** - Reviews completed work for requirement gaps, regressions, and quality issues.
6. **verification-before-completion** - Requires fresh evidence before claiming work is complete.
7. **finishing-a-development-branch** - Guides final integration decisions after implementation is complete and verified.

Supporting skills apply when their trigger conditions match:

- **systematic-debugging** - Root-cause process for bugs, test failures, and unexpected behavior.
- **test-driven-development** - Strict RED-GREEN-REFACTOR workflow when TDD is explicitly requested.
- **dispatching-parallel-agents** - Parallelizes independent investigations or implementation tasks.
- **receiving-code-review** - Handles review feedback with verification and technical judgment.
- **refactoring-integration** - Adds contract, branch, edit, validation, and git discipline for high-risk refactors and integrations.

## Skills

### Workflow

- **using-superpowers** - Establishes how and when agents invoke skills.
- **brainstorming** - Explores requirements, locks design decisions, and produces a reviewed spec.
- **writing-plans** - Produces implementation plans with concrete files, steps, and verification.
- **subagent-driven-development** - Runs approved plans through isolated implementation and review loops.
- **finishing-a-development-branch** - Presents merge, PR, cleanup, or continuation options after completion.

### Quality

- **requesting-code-review** - Requests focused review before work proceeds or merges.
- **receiving-code-review** - Evaluates and implements review feedback without blind agreement.
- **verification-before-completion** - Verifies claims with fresh command output or concrete checks.

### Debugging And Testing

- **systematic-debugging** - Finds root cause before applying fixes.
- **test-driven-development** - Enforces test-first development when explicitly requested.

### Coordination

- **dispatching-parallel-agents** - Delegates independent work to concurrent agents with isolated context.

### Refactoring And Integration

- **refactoring-integration** - Preserves behavior and auditability during branch alignment, migrations, backend integrations, and high-risk code movement.

## Repository Layout

```text
skills/
  <skill-name>/
    SKILL.md
```

Some skills include focused reference files or prompts beside `SKILL.md`. Load those only when the skill instructs you to.
