---
name: ai-coding-governance
description: Governance workflow for AI-assisted coding that defines the ordinary way to write Rails, React, TypeScript, Terraform, Terraform AWS/GCP, and Go code, and separates design decisions from routine implementation. Use when planning, implementing, or reviewing AI-generated code; when the user complains about overengineering, non-idiomatic choices, excessive abstractions, broad PRs, repeated review cost, schema/status-column/STI concerns, or wants a minimal, conventional, decision-diff-first coding process with explicit approval for deviations.
---

# AI Coding Governance

Use this skill to define and enforce the project's **ordinary way** of building software. Keep AI coding inside the project's allowed design space. Treat AI as a fast implementer under strong constraints, not as an unconstrained architect.

The default behavior is to follow the ordinary patterns defined here and in the relevant reference files. Deviate only when the user explicitly instructs it or when a decision diff explains the necessity and receives approval.

## Ordinary references

Load the relevant reference before planning or editing in these domains:

- Rails: `references/rails.md`
- React: `references/react.md`
- TypeScript: `references/typescript.md`
- Terraform general: `references/terraform.md`
- Terraform AWS: load both `references/terraform.md` and `references/terraform-aws.md`
- Terraform GCP: load both `references/terraform.md` and `references/terraform-gcp.md`
- Go: `references/golang.md`

These references define the default ordinary choices. If the repository's local patterns conflict with a reference, follow the repository unless the user explicitly asks to change the convention. If the user explicitly requests an approach that deviates from ordinary practice, follow the user's instruction and record the deviation in the decision diff.

## Core rule

Separate **decision diff** from **routine implementation** before editing code. The decision diff must call out any deviation from ordinary practice or from the relevant reference file.

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

## Hard default constraints

Assume these are forbidden unless explicitly justified and approved:

- New tables
- New columns, especially nullable columns
- Persisted `status`/state columns or enums
- STI or inheritance hierarchies for domain modeling
- Generic base classes or interfaces
- Service objects, concerns, callbacks, background jobs, feature flags, or state machines not already idiomatic in the local code
- New dependencies
- New framework/layer conventions
- Future-proofing for unrequested requirements
- Rewriting nearby code for aesthetic consistency

These tools are not absolutely banned. They require proof that existing structures cannot satisfy the current requirement with a smaller, reversible change.

## Design principles

### 1. Minimum conceptual diff

Implement the current requirement with the fewest new concepts, files, states, and persistence changes. Avoid “general and extensible” designs unless the generality is already required now.

### 2. Burden of proof for new concepts

For every new concept, answer:

- What current requirement makes it necessary?
- Why cannot an existing concept hold this responsibility?
- What invalid states or review burden does it add?
- How hard is it to reverse later?

If the answer is weak, do not introduce it.

### 3. Current requirements only

Do not implement predicted future variants. Prefer a reversible, boring implementation now and refactor when real requirements arrive.

### 4. Local consistency beats generic best practice

Follow this repository's nearby patterns over abstract best practices. Inspect adjacent code first. If the local code avoids a pattern, avoid it too unless asked otherwise.

### 5. Do not make invalid states representable

Persist facts, not derived status, when possible. Derive state from existing facts such as timestamps, associations, or records. Avoid duplicated truth such as `status = active` plus `activated_at` unless there is a clear invariant and enforcement plan.

### 6. Security and correctness are non-negotiable

Never accept injection-prone code, unsafe input handling, type widening that hides invalid data, or broad exception swallowing as a “minimal” shortcut.

### 7. Preserve review signal

Make it obvious which lines encode decisions. Keep mechanical changes boring, localized, and aligned with existing names and helpers.

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
- Do not add tables, columns, statuses, enums, STI, generic base classes, service objects, concerns, callbacks, jobs, flags, or dependencies unless you prove they are necessary.
- Implement only current requirements.
- Follow local repository patterns over generic best practices.
- If a new design decision appears during implementation, stop.
```
