---
name: zzz-code-constraints
description: Apply focused code design and review constraints when the user explicitly requests this Skill or the project instructions route a relevant task to it.
---

# Concise Code Constraints

Use this Skill as a narrow constraint layer for code design, implementation, refactoring, and review. Apply `karpathy-guidelines` as the general coding baseline when it is active. Do not make this Skill a universal workflow for unrelated coding tasks.

## Scope and Decision Boundaries

- Start with targeted searches and read only the context needed for the requested change.
- Keep edits limited to the requested behavior and proven direct dependencies.
- When the request is clear, inspect the relevant code, state a concise plan, and proceed.
- For low-risk ambiguity, make a reasonable assumption and state it instead of forcing a plan-versus-edit question.
- Ask before editing only when the user requests plan confirmation or the change affects a public API, database schema, architecture boundary, security, authentication, payment, production data, or an irreversible external action.
- Preserve stricter, explicit user requirements.

## Methods and Encapsulation

- Make the smallest change that matches existing project conventions.
- Do not add a one-use abstraction, wrapper, configuration point, or extension point.
- Do not split code into a method that only renames a few lines, forwards parameters, assigns a few properties, wraps one condition, or hides a short expression.
- Keep a method separate when it has real reuse, an independent responsibility or contract, a lifecycle or failure boundary, meaningful complexity, or a clear readability benefit.
- Extract a public method only when there is clear business reuse, an independent public boundary, and a material maintainability or readability benefit.
- Keep important business lifecycles visible when a method coordinates locks, transactions, status changes, failure writeback, compensation, persistence, or post-commit work.
- Preserve existing responsibility boundaries. Do not add a service, coordinator, or wrapper merely because an existing method is long.

## Method Call Chains

A method call chain is a sequence in which one method calls another and the called method continues calling other methods until the behavior completes. Direct-call count within one method and chain depth are risk signals only; they are not mandatory thresholds.

Assess the actual responsibilities, state changes, exception handling, business decisions, boundaries, readability, and maintenance cost. A long chain becomes more concerning when it contains thin pass-through methods or over-encapsulated decisions that add jumps without an independent rule, contract, reuse case, or lifecycle.

Do not pause, refactor, or ask the user to decide solely because a count or depth signal is high. Report the method path and recommendation only when the risk materially affects the implementation decision or maintainability. Ask the user only when extraction, merging, or inlining would change the business structure and the tradeoff is genuinely unclear.

## Comments and Validation

- Add or update comments for public APIs, business boundaries, non-obvious state, important invariants, failure behavior, algorithms, and compatibility workarounds.
- Follow repository conventions. Do not add comments that merely repeat obvious code.
- Run validation proportionate to the change risk. If a relevant check is not run, state why.
- For conditional rules, read only the matching reference below.

## Conditional References

- Database script work: read `references/database.md`.
- C# test or code-organization work: read `references/testing-csharp.md`.
- Stock-operation service work: read `references/stock-operation-example.md` only when that domain matches the task.
