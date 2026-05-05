# `loxdb_schema_migrate` (backup-first migrations)

- Public header: `modules/loxdb_schema_migrate/include/loxdb_schema_migrate.h`
- CMake target: `loxdb::schema_migrate`
- Category: Lifecycle

## Purpose

Plans and applies schema migrations and records a local history of applied migrations.

Optionally enforces “backup before apply” using `loxdb_backup`.

## API

- `loxdb_schema_migrate_err_t loxdb_schema_migrate_init(loxdb_schema_migrate_t *m, const loxdb_schema_migrate_cfg_t *cfg);`
- `loxdb_schema_migrate_err_t loxdb_schema_migrate_set_current_schema(loxdb_schema_migrate_t *m, uint16_t current_schema);`
- `loxdb_schema_migrate_err_t loxdb_schema_migrate_get_current_schema(const loxdb_schema_migrate_t *m, uint16_t *out_schema);`
- `loxdb_schema_migrate_err_t loxdb_schema_plan_migration(const loxdb_schema_migrate_t *m, uint16_t target_schema, loxdb_schema_migrate_plan_t *out_plan);`
- `loxdb_schema_migrate_err_t loxdb_schema_backup_before_apply(loxdb_schema_migrate_t *m, uint16_t target_schema, const loxdb_backup_export_cfg_t *backup_cfg, loxdb_backup_manifest_t *out_manifest);`
- `loxdb_schema_migrate_err_t loxdb_schema_apply_migration(loxdb_schema_migrate_t *m, uint16_t target_schema, loxdb_schema_migrate_step_fn step_cb, void *step_ctx, uint64_t now_ms);`
- `loxdb_schema_migrate_err_t loxdb_schema_migrate_get_history(const loxdb_schema_migrate_t *m, size_t index, loxdb_schema_migrate_history_t *out_entry);`
- `size_t loxdb_schema_migrate_history_count(const loxdb_schema_migrate_t *m);`
- `const char *loxdb_schema_migrate_err_name(loxdb_schema_migrate_err_t err);`

