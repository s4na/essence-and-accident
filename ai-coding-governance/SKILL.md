---
name: ai-coding-governance
description: Governance workflow for AI-assisted coding that separates design decisions from routine implementation. Use when planning, implementing, or reviewing AI-generated code; when the user complains about overengineering, non-idiomatic Rails/TypeScript/JavaScript choices, excessive abstractions, broad PRs, repeated review cost, schema/status-column/STI concerns, or wants a minimal, conventional, decision-diff-first coding process.
---

# AI Coding Governance

Use this skill to keep AI coding inside the project's allowed design space. Treat AI as a fast implementer under strong constraints, not as an unconstrained architect.

## Reference lookup

Before producing a decision diff, read the reference that matches the change:

- Always read [`references/core.md`](./references/core.md).
- For Rails, backend, model, controller, API, or server-side changes, read [`references/rails.md`](./references/rails.md).
- For UI, component, client-side state, type, or frontend dependency changes, read [`references/frontend.md`](./references/frontend.md).
- For Terraform, Kubernetes, CI/CD, environment, or cloud resource changes, read [`references/infrastructure.md`](./references/infrastructure.md).

Use the references as concise operational rules. If a repository's established pattern conflicts with a reference, surface that conflict in the decision diff instead of silently choosing a new convention.

## Core rule

Separate **decision diff** from **routine implementation** before editing code.

- Decision diff: choices humans must approve because they affect concepts, data models, invariants, failure behavior, or reversibility.
- Routine implementation: mechanical code that follows approved decisions and existing local patterns.

Do not mix these in one step. If a new decision appears during implementation, stop and ask for approval instead of silently redesigning.

## Default objective function

Optimize for the smallest conceptual change that satisfies the current requirement.

Prefer minimizing new concepts over minimizing lines of code. Each new class, table, column, status, abstraction, dependency, flag, job, callback, or configuration option creates permanent review and maintenance cost.

## Pre-implementation protocol

Before changing code, produce this plan and wait for approval unless the user explicitly asks to skip planning:

1. Requirement interpretation
2. Existing files/patterns to follow
3. Files expected to change
4. New files, if any
5. Database/schema changes, if any
6. New concepts introduced, if any
7. Invariants and invalid states to preserve or eliminate
8. Alternatives rejected
9. Decisions requiring human approval
10. Test/check plan

Keep the plan concrete and short. The plan must show why the change is local and conventional for this repository.

The detailed default constraints and conventions are maintained in the references above. Apply them unless the user explicitly approves a justified exception.

## Implementation protocol after approval

After the plan is approved:

1. Implement only the approved plan.
2. Do not add unapproved abstractions, files, columns, states, or dependencies.
3. If implementation reveals a missing decision, stop and present a new decision diff.
4. Prefer touching each existing line once. Avoid refactor-then-implement-then-cleanup churn over the same code.
5. Keep commits meaning-based and reviewable.

## Review protocol

When reviewing an AI-generated diff, start with the decision diff rather than line-by-line comments:

- What new concepts were introduced?
- What persistence or invariant changed?
- Which choices are hard to reverse?
- Which changes are merely mechanical?
- Where did the diff depart from local convention?
- Are there duplicated truths or newly representable invalid states?
- Can any file, abstraction, state, or schema change be removed?

Request revisions by narrowing the allowed design space, not by saying “write better code.”

## Commit guidance

Prefer meaning-based commits:

1. Mechanical cleanup, only if necessary
2. Data model or invariant change, only if approved
3. Behavior change
4. UI/API wiring
5. Tests

Avoid splitting so that the same code must be reviewed repeatedly. If a commit sequence repeatedly rewrites the same lines, squash or re-plan.

## Useful prompt template

Use this when asking an AI agent to work under this skill:

```text
Before editing code, produce a decision diff only.

Include:
1. Requirement interpretation
2. Existing files/patterns to follow
3. Files expected to change
4. New files
5. DB/schema changes
6. New concepts
7. Invariants and invalid states
8. Alternatives rejected
9. Decisions requiring approval
10. Test plan

Constraints:
- Minimize new concepts, not just lines.
- Read and apply the relevant `references/` before making design choices.
- Implement only current requirements.
- Follow local repository patterns over generic best practices.
- If a new design decision appears during implementation, stop.
```
