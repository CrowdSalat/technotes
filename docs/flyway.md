# Flyway


## motivation

Flyway aims to streamline the database migration process, making it faster and less error-prone for development teams. It integrates perfectly with with CI/CD pipelines.

- **Safe and reliable deployments**: Flyway ensures that database schema changes are applied in the correct order across all environments (development, testing, production). This reduces the risk of errors and unexpected behavior.
- **Safety checks**: Flyway verifies if the scripts have already been applied to the database before running them again. This prevents accidental re-runs that could cause issues.
- **Rollback support**: If a migration causes problems, Flyway allows you to easily rollback to a previous state.

## alternatives

- Liquibase: Like flyway but uses xml for configuration
- manual execution of sql scripts

## basics

1. Write migration scripts
2. Execute flyway commands

### write migration scripts

- Flyway uses versioned filenames to ensure migrations are applied in the correct order. Typically: `V<version number>__<description>.sql` (e.g. V001__create_users_table.sql).
- Inside the script file, write your SQL statements that modify the database schema. This could include creating tables, adding columns, or altering existing structures.

### execute flyway commands

- `migrate`: This command instructs Flyway to scan for new or pending migrations and apply them to your database.
- `info`: This command displays information about the current state of Flyway, such as which migrations have been applied and the Flyway version.
- `baseline`: This command is typically used for initial setup. It helps Flyway create the internal tracking table in your database to manage the migration history.
- `clean`: This command removes applied migrations from your database, essentially rolling back all changes. Use with caution!
