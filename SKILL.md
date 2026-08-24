---
name: concise-code-constraints
description: Apply karpathy-guidelines as the general coding standard and add personal constraints during design, plan generation, implementation, testing, database script generation, business interaction, refactoring, and code review. Use when a task changes source code or database scripts and an unclear modification plan must prompt a user choice between plan-first and direct implementation, the plan must name affected classes and members, the implementation must stay simple and readable, search and reading must stay narrowly scoped to related code, unnecessary abstraction is forbidden, service-layer call chains deeper than three levels must be reported for user decision, all code elements need comments regardless of visibility, business interactions should prefer code, local test code must remain uncommitted, C# test-only logic may use separate partial classes, and database scripts may add primary keys but no extra indexes or foreign keys and must include database comments.
---

# Concise Code Constraints

Apply this skill as a passive constraint layer whenever it is selected for a source-code task. Do not initiate, select, or require a workflow or another skill. If the task is also being handled by `$ask-matt` or another technical skill, keep this skill's rules as additional constraints. Apply **`$karpathy-guidelines`** as the general coding standard and apply the rules below as personal additions. Keep the solution minimal, explicit, readable, and easy to review.

This skill does not automatically invoke `$ask-matt`. Use `$ask-matt` separately only when the user explicitly requests skill or workflow selection.

## Plan Stage

For every task involving source-code or database-script changes, first determine whether the user has provided a clear modification plan or explicitly chosen plan-first or direct implementation. Treat a modification plan as clear only when it identifies which classes, files, or members will change and what will change in them.

If the modification plan is not clear and the user has not already chosen an execution mode, ask the user before editing, in the user's language, to choose exactly one of these options:

- **Plan first:** Output the concrete modification plan and wait for confirmation before editing.
- **Directly modify code:** Inspect and implement without waiting for separate plan confirmation.

Keep the choice labels concise and equivalent to "Plan first" and "Directly modify code." Targeted read-only inspection may continue when needed to understand the request, but do not edit code before the user chooses. Do not ask this workflow question when the user has already supplied a clear modification plan or explicitly selected one of the two modes.

Before implementation, inspect the relevant code and state a concise plan. For every affected source file, identify:

- the class, interface, record, or module being changed;
- the methods, properties, fields, or other members being added or modified;
- the behavior or responsibility that changes;
- any related tests and the verification command.

Do not list speculative files or classes. If the affected type is not yet known, say that the codebase inspection will determine it and resolve it before implementation.

## Scope Discipline

Keep code modification work focused on the requested behavior and its direct dependencies.

- Start with targeted searches for the exact feature, type, method, error, or database object involved. Read only the matching files and the smallest surrounding context needed to understand the change.
- Expand the search or reading scope only when a reference, contract, test, build error, or runtime dependency proves that more context is required. State the reason for each meaningful expansion.
- Do not scan the whole repository, generated output, large logs, unrelated modules, or broad configuration trees before there is evidence they matter.
- Keep the plan's file and symbol scope explicit. Do not inspect, refactor, reformat, or modify unrelated code discovered incidentally.
- Report unrelated issues separately without widening the implementation. Keep verification commands focused on the affected behavior unless a broader check is required by the project.

## Personal Implementation Additions

In addition to `$karpathy-guidelines`, use the smallest change that satisfies the request and matches existing project conventions.

- Do not create abstractions, wrappers, helper layers, configuration points, or extension points for one use or for hypothetical future needs.
- Keep a method focused, but do not split logic into methods that merely rename a few lines or hide a simple expression.
- Merge methods when their logic can be combined without reducing readability. Keep a method separate only when it is genuinely reused, has an independent contract or lifecycle, isolates meaningful complexity, or materially improves clarity.
- Preserve existing public contracts unless the request requires changing them.
- Remove only unused code made unused by this change.

## Business Interaction

Within the business domain, prefer code-driven interaction whenever a stable code entry point exists. Use business services, domain APIs, SDKs, application APIs, command-line interfaces, scripts, or other automatable interfaces before manual UI operations.

- Reuse existing business services and interfaces instead of bypassing them with direct database edits or ad hoc state changes.
- Keep business rules in the application/domain layer; do not move business decisions into one-off scripts or UI actions merely for convenience.
- Make code-driven operations repeatable, reviewable, testable, and auditable.
- Use manual UI interaction only when no suitable code/API entry point exists, the operation genuinely requires human judgment, or the user explicitly requests UI interaction. State the reason briefly.
- Do not create a new abstraction solely to avoid one simple manual step; follow the existing codebase's level of automation and keep any new code minimal.

## Service-Layer Call Depth

During design, implementation, refactoring, and review, inspect only the service-layer call paths directly touched by the task. Count the entry service method as level 1 and each consecutive service-to-service method call as the next level. A path that reaches a fourth service method exceeds the three-level limit.

When a relevant path exceeds three levels:

- Report the concrete call path, current depth, affected behavior, and practical risk to the user.
- Do not restructure, merge, split, inline, or otherwise change that call chain without the user's decision. Continue unrelated in-scope work when it is safe to do so.
- Give a recommendation based on the observed code, with the smallest reasonable option first. Include the expected impact and tradeoff of each applicable option:
  - Keep the chain when the depth is justified, responsibilities are clear, and tests or observability make the behavior manageable.
  - Merge or inline adjacent pass-through methods when they add no independent rule, contract, reuse, or lifecycle.
  - Move a cohesive business rule to the service or domain component that owns it when responsibility is fragmented across layers.
  - Use an existing application-level orchestrator or workflow entry point when peer services are coordinating a multi-step use case; do not introduce a new abstraction unless the user chooses it and the codebase supports that pattern.
- State which option is recommended and why, then ask the user to choose how to proceed. If the requested change cannot be completed safely without changing the chain, pause that part until the user decides.

## Test Code and Git Boundary

Test code is local verification code and must not be committed to Git unless the user explicitly requests a committed test. Do not stage or commit generated test files. Prefer a temporary test location when the code does not need to remain in the working tree.

For C# code, test logic may be placed in a separate partial-class file when that keeps the production file focused. Follow the repository's existing test-file naming and ignore conventions. Test-only files remain local and must not be staged or committed unless the user explicitly requests otherwise.

Any service, endpoint, registration, or other production-side code created solely to support local test cases must be enclosed in `#if DEBUG` and `#endif`. Keep its declaration, registration, and test-only entry points inside the debug guard. Business code must not call, inject, register, or otherwise depend on this test-only service. Verify that the non-DEBUG build excludes it.

## Code Organization

For languages that support preprocessor regions, organize generated or modified code with meaningful `#region` and `#endregion` blocks. Group related fields, constructors, properties, public methods, private methods, test cases, and test helpers according to the repository's conventions. Do not create a region for every single line or trivial statement.

Place reflection-based test methods and their reflection-only helpers in the final region at the bottom of the containing test file or partial-class file. Keep ordinary tests and ordinary helpers above that final reflection section.

## Database Script Rules

When generating or modifying database scripts:

- Allow primary keys when required by the data model.
- Do not add ordinary indexes, unique indexes, clustered indexes, or any other indexes beyond the primary-key index created by the primary key definition.
- Do not add foreign-key constraints or foreign-key indexes.
- Add database comments for every database object created or materially modified by the script. At minimum, comment the database or schema when the script creates or changes it, every table, and every column. Also comment views, procedures, functions, triggers, sequences, constraints, and other objects when the target database supports comments for them.
- Use the target database dialect's native comment mechanism, such as `COMMENT ON` or table/column comment syntax. Do not treat SQL line comments as a substitute for database metadata comments.
- Match existing naming, type, migration, and transaction conventions. If the database dialect cannot express a required object comment, report the limitation instead of silently omitting it.

Before reporting completion, inspect the script for forbidden index and foreign-key statements, and verify that the required database metadata comments are present.

## Comment Requirements

Add or update comments for every type, class, interface, record, constructor, method, property, field, event, and other declared member that this change creates or materially modifies, regardless of whether it is public, protected, internal, or private. Add comments before non-obvious logic, business rules, state transitions, algorithms, or compatibility workarounds.

Comments must explain intent, constraints, or behavior that is not obvious from the code. Do not add empty narration that merely repeats a name or restates the next line. Follow the repository's comment and documentation conventions, including XML documentation where the language or project uses it.

If an existing comment is made inaccurate by the change, update it. Do not add comments to unrelated code merely to make the file look uniform.

## Review Checklist

Before reporting completion, verify:

1. The plan names each affected class and changed member.
2. Every changed line is traceable to the request or required verification.
3. No single-use abstraction or speculative flexibility was introduced.
4. Methods that could be merged were merged unless separation is justified by reuse, contract, lifecycle, complexity, or clarity.
5. New or materially changed declarations have useful comments at every visibility level.
6. Test-only code is not staged or committed unless explicitly requested; C# partial test files follow the repository's conventions.
7. Test-only services are enclosed in `#if DEBUG` and are not used by business code.
8. Meaningful code sections use `#region`; reflection test code is in the final region at the bottom.
9. Database scripts add only permitted primary keys, contain no extra indexes or foreign keys, and include database metadata comments for required objects.
10. Focused tests, linting, build, script validation, or another relevant check was run; report anything not run.
11. Search, reading, and modification stayed within the related scope; any expansion was justified by a concrete dependency or verification need.
12. Any touched service-layer call path deeper than three levels was reported with its path, risk, options, and recommendation, and no chain restructuring was performed without the user's decision.
13. When the classes, files, or members and their intended changes were not clear, the user chose plan-first or direct implementation before any code edit.

When another skill requires a different pattern, follow that skill only when it is necessary for correctness or explicitly requested. Record the tradeoff briefly and keep the implementation as small as that pattern allows. Treat `$karpathy-guidelines` as the general baseline and this skill as the source of personal additions, not as a replacement for either the selected technical skill or `$karpathy-guidelines`.
