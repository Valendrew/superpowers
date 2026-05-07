Assume these skills are behavior-shaping code, not prose. A shorter but weaker skill is a failed optimization.

You are optimizing skill instructions for agent efficacy. Do not do a general writing cleanup.

Work ONLY on these files under `@skills`:

1. `using-superpowers`
2. `brainstorming`
3. `systematic-debugging`
4. `writing-plans`
5. `subagent-driven-development`
6. `verification-before-completion`

## Objective

For each skill, reduce token usage and improve clarity while preserving:

* semantics
* trigger behavior
* workflow logic and order
* enforcement strength
* practical efficacy
* cross-skill coherence

Optimize for behavior, not elegance.

## Non-negotiable constraints

Do NOT:

* weaken hard gates, red flags, stop conditions, or enforcement language
* remove behavior-affecting steps
* change workflow order unless the removed material is exactly redundant and removal is clearly safe
* rewrite trigger text in a way that broadens or narrows activation
* replace concrete instructions with abstractions
* convert behavior-shaping language into bland documentation prose
* harmonize tone if that reduces force
* make generic “skill-writing best practice” edits that change behavior
* change semantics to save tokens

Preserve:

* all requirements
* all decision points
* all workflow transitions
* all explicit prohibitions
* all review and verification expectations
* all cross-skill handoffs
* the repo’s philosophy and voice where behaviorally important
* wording that enforces sequence or stage-gate order
* named statuses, outcome classes, and decision branches unless the mapping remains one-to-one and equally actionable

**Frontmatter descriptions are trigger-critical and hard-gated. Default to `LEAVE VERBATIM`. Edit only if the change is clearly activation-neutral, and provide a separate justification block.**

When uncertain, preserve behavior over token savings.

## Block review rules

Review each skill block by block. Complete both passes for one skill before moving to the next.

Treat each of the following as a separate block when present:

* frontmatter
* section intro paragraph
* each prose paragraph
* each bullet cluster with one behavioral purpose
* hard-gate / red-flag / anti-pattern lists
* workflow or handoff text
* diagram-adjacent explanatory prose
* examples
* status taxonomies / branch logic

Do not merge blocks if doing so would weaken sequencing, salience, or enforcement.

Examples are behaviorally important when they clarify:

* trigger boundaries
* workflow order
* escalation handling
* review order
* anti-pattern avoidance
* operational specificity not present in the rule alone

Only remove an example if it merely paraphrases an already-concrete instruction. If the example makes an abstract instruction concrete, it is behaviorally necessary.

Maintain a compact running terminology and handoff log as you go so cross-skill consistency is checked continuously, not only at the end.

## Method

Work skill by skill.

For each skill, do 2 passes.

### Pass 1: Block-by-block analysis

For each block, classify it as one of:

* `KEEP`
* `EDIT`
* `DELETE`
* `MERGE`
* `LEAVE VERBATIM`

Definitions:

* `KEEP`: leave unchanged for this rewrite; block is acceptable as-is
* `EDIT`: keep meaning, but tighten or shorten
* `DELETE`: redundant within the same skill or pure wording overhead with no behavioral effect
* `MERGE`: can combine with adjacent block without losing force or clarity
* `LEAVE VERBATIM`: leave unchanged because the wording itself is behaviorally tuned or high-risk to alter; this is a stronger “do not touch” signal than `KEEP`

Record for each reviewed block:

1. section name
2. short excerpt or description
3. classification
4. brief reason
5. risk level: `low`, `medium`, or `high`
6. proposed action in one sentence

To control output size:

* You may group adjacent blocks in one row only if they share the same decision, risk, and rationale.
* You may summarize a clean section as a grouped entry when every block in it is `KEEP` and there are no meaningful reduction opportunities.
* Give full individual attention to any block that is `EDIT`, `DELETE`, `MERGE`, `LEAVE VERBATIM`, or higher risk.

Be especially careful with:

* frontmatter descriptions
* hard gates
* red flags
* anti-patterns
* flowchart-adjacent prose
* workflow handoffs
* examples that may be operationally important
* repeated imperatives that may be intentional reinforcement

### Pass 2: Coherent rewrite

Then produce a revised full skill that:

* applies only safe improvements
* preserves behavior and structure unless structural simplification is clearly safe
* keeps terminology coherent across the six skills
* removes redundancy without reducing force
* keeps transitions and handoffs explicit
* prefers exactness over prettiness

Risk gating for Pass 2:

* High-risk blocks should default to `LEAVE VERBATIM`
* Use `EDIT` on a high-risk block only when the reduction is trivially safe and clearly semantics-preserving
* If you cannot make that case, leave it unchanged

## Required output format

For each skill, output these sections in order.

### Skill: `<name>`

#### A. Risk Summary

Provide 3–7 bullets identifying:

* the most dangerous parts to touch
* the safest token reductions

#### B. Block Review

Use a table with columns:

* Section
* Block
* Decision
* Reason
* Risk
* Proposed change

Use grouped rows whenever allowed above to control bloat. Keep reasons concise.

#### C. Rewrite Strategy

Briefly explain what will change and what will not.

#### D. Revised Skill

Provide the full revised text.

#### E. Semantic Diff Check

List any change that might affect:

* triggering
* workflow order
* strictness
* scope
* verification burden
* cross-skill consistency

If none, write exactly:

`No intended semantic change.`

#### F. Verification Check

Run explicit post-rewrite checks:

1. Trigger checks: walk through 2–3 concrete triggering scenarios and state whether the revised skill should still activate exactly as before.
2. Workflow check: walk through at least one workflow handoff or stage transition and state whether the sequencing is still explicit.
3. Frontmatter check: if frontmatter changed, justify why activation is unaffected; if not, state that it was left verbatim.

## Global consistency check

After all six skills, provide a final cross-skill review covering:

* shared terminology
* workflow sequence coherence
* handoff consistency
* duplicated concepts that should remain duplicated for reinforcement
* duplicated concepts that can safely be reduced
* places where token reduction would likely hurt efficacy

If you find a real cross-skill inconsistency, do not silently harmonize it by changing local meaning. Preserve each skill’s semantics, flag the inconsistency explicitly, and propose the minimal fix.

Use the running terminology and handoff log to support this review.

## Optimization heuristics

These heuristics do **not** override the non-negotiable constraints. Their overlap is intentional reinforcement.

Prefer reducing:

* repeated explanation after a rule is already explicit
* duplicated rationale that does not change behavior
* wordy transitions
* examples that only paraphrase an already-concrete instruction
* long sentences that can be split or tightened
* repeated “why” prose when the rule is already clear

Avoid reducing:

* trigger text
* gate language
* reviewer / verification requirements
* stop conditions
* escalation instructions
* cross-skill routing text
* anti-rationalization pressure
* ordering cues and prerequisite language
* examples that add operational specificity

## Decision rule

When uncertain, preserve behavior over token savings.

If a block is expensive but likely behaviorally important, mark it `LEAVE VERBATIM` or `EDIT` with a high-risk warning rather than compressing aggressively.

Do not force shrinkage. Larger reductions require stronger justification.