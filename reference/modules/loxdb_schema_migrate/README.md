# loxdb_schema_migrate

## Purpose

`loxdb_schema_migrate` provides a backup-first migration flow with step-by-step schema transitions and local migration history.

## Main API

- `loxdb_schema_plan_migration(...)`
- `loxdb_schema_backup_before_apply(...)`
- `loxdb_schema_apply_migration(...)`

## Behavior

- computes up/down migration plan from current schema to target schema
- optionally enforces backup-before-apply (`require_backup_before_apply`)
- applies one migration callback per schema step (`N -> N+1` or `N -> N-1`)
- stores bounded migration history (`LOXDB_SCHEMA_MIGRATE_MAX_HISTORY`)

## Typical flow

1. initialize module with current schema
2. plan migration to target
3. call backup API before apply
4. apply migration steps through caller callback
5. inspect migration history for audit/diagnostics
