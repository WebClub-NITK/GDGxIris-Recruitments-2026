## Task ID: SchemaShift

#### `Backend Engineering`, `Databases`, `Developer Tools`, `ORM`

**Mentor:** Nishant

**Difficulty:** `Medium-Hard`

### Description

Build **SchemaShift**, a developer tool that helps teams migrate an application from a **non-SQL (NoSQL) database to a SQL database**.

Teams often start with a document store for speed of iteration, then need relational structure, stronger consistency, and better performance at larger scale. SchemaShift should make that transition practical: inspect the source data, generate the migration scripts, move the data, and update the application's ORM / data-access layer so the app can run against the new database.

As a stretch goal, support the reverse direction (**SQL → NoSQL**) and a **zero-downtime** migration path.

### Features to Implement

1. **Source Inspection & Schema Inference**

   - Connect to a sample NoSQL database (e.g. MongoDB) containing realistic nested documents and collections.
   - Infer a target relational schema: tables, columns, primary keys, foreign keys, and indexes.
   - Handle nested objects and arrays with a clear, documented strategy (embedding vs. separate tables / join tables).
   - Present the proposed schema in a readable format (CLI output, JSON/YAML report, or a simple UI).

2. **Migration Script Generation**

   - Automatically generate **all** scripts needed for the move, including at least:
     - SQL DDL to create the target schema
     - Data transformation / load scripts or a documented ETL pipeline
     - Rollback or cleanup scripts where appropriate
   - Scripts should be deterministic, reviewable, and runnable without hand-editing for the sample dataset.
   - Persist generated artifacts in a clear directory structure (e.g. `migrations/`, `etl/`, `orm/`).

3. **Data Migration**

   - Execute the migration against a target SQL database (e.g. PostgreSQL or MySQL).
   - Preserve referential integrity and handle type conversions (ObjectIds, dates, nested values, nulls).
   - Provide progress reporting and a post-migration verification step that compares record counts and spot-checks critical fields.
   - Fail loudly with actionable errors when data cannot be mapped cleanly.

4. **ORM / Application Layer Migration**

   - After data is migrated, update (or generate) the application's data-access layer for the new SQL database.
   - Support at least one concrete path, for example:
     - Mongoose / native Mongo driver → Prisma, Drizzle, TypeORM, or Sequelize
   - Generate models/schemas, connection setup, and enough repository/query examples that the sample app can perform basic CRUD on the new database.
   - Document what was changed and what still needs manual review.

5. **Sample Application**

   - Include a small demo app (or seed a provided one) that originally talks to the NoSQL database.
   - After running SchemaShift, the same app (or a clearly documented successor) should work against the SQL database using the migrated ORM layer.
   - Provide seed data large and nested enough to exercise inference, relations, and edge cases.

### Bonus Features

*Implementing any of the following elevates the submission; completing **both** of the first two makes the task count as* `Hard`.

1. **Zero-Downtime Migration**

   - Design a dual-write / shadow-read / cutover strategy so the application can keep serving traffic during migration.
   - Document the phases (e.g. schema create → backfill → dual write → verify → cutover → decommission).
   - Provide tooling or scripts that support at least one safe cutover path with rollback.

2. **Bidirectional Migration (SQL → NoSQL)**

   - Support migrating from SQL back to a document store.
   - Generate the corresponding scripts, schema/document model, and ORM updates for the reverse direction.

3. **Interactive Schema Review**

   - Let the user approve or tweak inferred mappings (rename tables/columns, choose embedding vs. relation) before scripts are generated.

### Tips

- Start with a narrow domain (e.g. users, posts, comments) before generalizing the inferencer.
- Make generated SQL and ORM output human-readable — reviewers will read the scripts.
- Prefer explicit mapping rules over magic; document every nested-document decision.
- For zero downtime, study expand/contract and dual-write patterns before coding.
- Keep configuration in a file (source URI, target URI, mapping overrides) rather than hardcoding credentials.

### Useful Resources

- [PostgreSQL Documentation](https://www.postgresql.org/docs/current/)
- [MongoDB Schema Design Patterns](https://www.mongodb.com/docs/manual/data-modeling/)
- [Prisma Schema Reference](https://www.prisma.io/docs/orm/prisma-schema)
- [Expand/Contract Database Migration Pattern](https://openpracticelibrary.com/practice/expand-and-contract-pattern/)
- [Dual Writes and Data Consistency](https://martinfowler.com/articles/patterns-of-distributed-systems/dual-writes.html)
