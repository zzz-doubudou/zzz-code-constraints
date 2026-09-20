# Database Script Rules

Use these rules only when generating or modifying database scripts.

- Allow primary keys when required by the data model.
- Do not add ordinary, unique, clustered, or other non-primary-key indexes.
- Do not add foreign-key constraints or foreign-key indexes.
- Add native database metadata comments for every created or materially modified object. At minimum cover the database or schema, tables, and columns; cover views, procedures, functions, triggers, sequences, and constraints when supported.
- Use the target dialect's native comment mechanism. SQL line comments are not a substitute for metadata comments.
- Match existing naming, type, migration, and transaction conventions.
- If the dialect cannot express a required comment, report the limitation instead of silently omitting it.

Before completion, inspect the script for forbidden index and foreign-key statements and verify required metadata comments.
