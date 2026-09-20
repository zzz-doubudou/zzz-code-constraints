# C# Test and Code Organization Rules

Use these rules only when modifying C# test code or when the repository's existing conventions require them.

- Keep test-only files and generated test artifacts local unless the user explicitly requests a committed test.
- A separate partial class may hold test logic when it keeps production code focused; follow the repository's naming and ignore conventions.
- Production code created solely for local tests must be enclosed in `#if DEBUG` and `#endif`. Business code must not depend on it. Verify that the non-DEBUG build excludes it when relevant.
- Follow existing conventions for `#region`. Do not add regions only to satisfy this Skill.
- Keep reflection-based test helpers with their reflection tests when the repository uses a dedicated section; do not impose a location on a repository without that convention.
