# Project Instructions

This repository packages the `zzz-code-constraints` Codex Skill and its public documentation.

## Scope

- Keep `SKILL.md` focused on rules that apply to ordinary code design, implementation, refactoring, and review.
- Put database, C# test, and domain-specific examples in `references/` and load them only when the task needs them.
- Keep the Skill discoverable, but do not make it a universal policy for unrelated coding tasks.
- Preserve the distinction between risk signals and mandatory thresholds. Method-call depth and direct-call count must not force a refactor by themselves.

## Decision Boundaries

- When the requested change is clear, inspect the relevant files and proceed with a concise plan.
- For low-risk ambiguity, make a reasonable assumption and state it; do not force a plan-versus-edit question.
- Ask before editing only when the user requests plan confirmation or the change affects a public API, database schema, architecture boundary, security, authentication, payment, production data, or an irreversible external action.
- Preserve explicit user requirements that are stricter than these defaults.

## Safety and Validation

- Do not modify secrets, credentials, authentication, payment logic, deployment configuration, or production data without explicit authorization.
- Do not perform destructive or externally mutating operations without the required authorization.
- Run `scripts/quick_validate.py` for Skill structure changes.
- Run validation proportionate to the change. Documentation-only changes do not require application tests; report checks that were not applicable or not run.
- Update `outputs/README.md` when public Skill behavior or usage changes.

## Editing

- Make the smallest change that keeps the Skill readable and internally consistent.
- Do not add placeholder references, duplicate rules, or a checklist that repeats the body.
- Keep comments on methods, properties, and constants. Add comments inside methods for non-obvious business decisions, state changes, exception handling, algorithms, and compatibility logic.
- Keep temporary test artifacts local unless the user explicitly requests that they be committed.
