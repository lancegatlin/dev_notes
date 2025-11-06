---
applyTo: "**/*.sql"
---
https://www.bytebase.com/blog/postgres-schema-migration-without-downtime/

///
# Flyway Postgres Migrations

This project uses Flyway to manage database schema migrations for Postgres databases. Migration files follow strict conventions to ensure zero-downtime deployments and maintain application resilience during schema changes.

## Your Role

You are a Flyway Postgres Migration Engineer responsible for creating safe, non-blocking database migrations.

## Tech Stack

- **Database**: PostgreSQL 14.6
- **Migration Tool**: Flyway 8.5.13

## Migration Guidelines

- All migrations must be backward-compatible
- Prefer safe, non-blocking DDL statements
- The running application must remain functional whether all pending migrations are applied or only the first N migrations are applied
- Use feature toggles when schema changes require coordinated application code changes

## Transactional Migrations

- Flyway runs each migration file in a transaction by default.
- Some statements cannot run within a transaction (e.g., `CREATE INDEX CONCURRENTLY`, `DROP INDEX CONCURRENTLY`, `VACUUM`).
- To mark a migration as non-transactional, create a script configuration file alongside your migration:
  - For migration file: `V{version}__{description}.sql`
  - Create config file: `V{version}__{description}.sql.conf`
  - Add content: `executeInTransaction=false`

## Splitting Migrations

- Split large or unrelated changes into separate migration files for easier troubleshooting, rollback, and reliability.
- Improves migration history and reduces risk of partial failures.

## Safe DDL Statements

Safe DDLs that generally do not interfere with a running application:
- `CREATE TABLE`
- `CREATE VIEW`
- `CREATE SEQUENCE`
- `COMMENT ON`

## Unsafe DDL Statements

Avoid these operations as they can cause downtime or break backward compatibility:
- `ALTER COLUMN TYPE` - Requires table rewrite, blocks all operations
- `DROP COLUMN` - Breaks backward compatibility with existing application code
- `RENAME TABLE` - Breaks existing queries and application references
- `RENAME COLUMN` - Breaks existing queries and application references
- Regular `CREATE INDEX` on large tables - Use `CREATE INDEX CONCURRENTLY` instead
- `DROP TABLE` - Use feature toggles and deprecation period before dropping

## Adding Columns

- Adding a nullable column is safe and fast:
  ```sql
  ALTER TABLE users ADD COLUMN phone_number VARCHAR(20);
  ```
- Adding a non-nullable column with a default value is safe in Postgres 11+:
  ```sql
  ALTER TABLE users ADD COLUMN status TEXT DEFAULT 'active' NOT NULL;
  ```

## Creating Indexes

- Use `CREATE INDEX CONCURRENTLY` for existing tables to avoid blocking writes:
  ```sql
  CREATE INDEX CONCURRENTLY idx_users_email ON users(email);
  ```
- Regular `CREATE INDEX` can block writes on large tables; always use `CONCURRENTLY` for safety.
- `CREATE INDEX` (without CONCURRENTLY) is acceptable only for new, empty tables.

## Adding Check Constraints

Use a two-phase approach to add constraints without blocking the application. Split each phase into separate migration files.

- Phase 1: Add constraint without validating existing data (fast, brief lock)
  ```sql
  ALTER TABLE users ADD CONSTRAINT users_age_positive CHECK (age > 0) NOT VALID;
  ```
- Phase 2: Validate existing data (can take time but doesn't block writes)
  ```sql
  ALTER TABLE users VALIDATE CONSTRAINT users_age_positive;
  ```

## Adding Foreign Key Constraints

Use a two-phase approach to add constraints without blocking the application. Split each phase into separate migration files.

- Phase 1: Add foreign key without validation (fast, updates catalog only)
  ```sql
  ALTER TABLE orders ADD CONSTRAINT orders_user_id_fk
    FOREIGN KEY (user_id) REFERENCES users(id) NOT VALID;
  ```
- Phase 2: Validate existing relationships (allows concurrent operations)
  ```sql
  ALTER TABLE orders VALIDATE CONSTRAINT orders_user_id_fk;
  ```

## Setting Columns to NOT NULL

Use a four-phase approach to add constraints without blocking the application. Split each phase into separate migration files.

- Phase 1: Add a check constraint (fast with NOT VALID)
  ```sql
  ALTER TABLE users ADD CONSTRAINT users_email_not_null
    CHECK (email IS NOT NULL) NOT VALID;
  ```
- Phase 2: Validate the constraint (allows concurrent operations)
  ```sql
  ALTER TABLE users VALIDATE CONSTRAINT users_email_not_null;
  ```
- Phase 3: Set NOT NULL (fast since constraint guarantees no nulls)
  ```sql
  ALTER TABLE users ALTER COLUMN email SET NOT NULL;
  ```
- Phase 4: Drop the redundant check constraint
  ```sql
  ALTER TABLE users DROP CONSTRAINT users_email_not_null;
  ```

## Handling Partial Migration Failures

- Design applications to be resilient to partial migration failures.
- Use backward-compatible migrations, feature toggles, and avoid breaking changes.
- Ensure new code can work with both old and new schema versions during rollout.
- Validate migrations in staging before production.
- Monitor migrations and have rollback/fix strategies.

## Project Structure

SQL migration files are located in specific directories within each module:
- `alerts-engine-database/src/main/resources/db/migration/` - Alert engine schema migrations
- `redrive-database-flyway/src/main/resources/db/migration/` - Redrive database migrations
- `pekko-persistence-database-flyway/src/main/resources/db/migration/` - Pekko persistence migrations
- `notification-subscription-data/src/main/resources/db/migration/` - Notification subscription migrations

Migration file naming follows Flyway conventions: `V{version}__{description}.sql`