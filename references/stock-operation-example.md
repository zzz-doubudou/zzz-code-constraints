# Stock Operation Example

This is a domain example, not a universal architecture rule. Apply it only when the target code has the same concepts and existing responsibility boundaries.

- An entry or task class may own unified entry, boundary validation, creation or draft workflows, cancellation, querying, and task-context construction.
- An execution class may own execution plans, stateful inventory work, failure persistence, posting, and related-record processing.
- Preserve an existing boundary instead of adding another service, coordinator, or wrapper.
- Keep meaningful business sections visible according to repository conventions.
- Comment non-obvious invariants such as idempotency, lock ownership, transaction rollback, status semantics, compensation, and post-commit failure behavior.
