# `loxdb_ota` (compatibility + update runtime)

- Public header: `modules/loxdb_ota/include/loxdb_ota.h`
- CMake target: `loxdb::ota`

## Purpose

Helps embedded deployments with OTA updates by handling:

- version compatibility checks (major/minor/patch + schema)
- update planning given policy
- runtime update tracking, migration callback, and rollback callback

## API

- `int loxdb_ota_check_compat(const loxdb_ota_version_t *current_v, const loxdb_ota_version_t *target_v, uint16_t min_supported_schema, uint8_t allow_major_upgrade, loxdb_ota_result_t *out_result);`
- `int loxdb_ota_plan_update(const loxdb_ota_version_t *current_v, const loxdb_ota_version_t *target_v, const loxdb_ota_policy_t *policy, loxdb_ota_plan_t *out_plan);`
- `int loxdb_ota_runtime_init(loxdb_ota_runtime_t *runtime, const loxdb_ota_version_t *current_v);`
- `int loxdb_ota_runtime_begin_update(loxdb_ota_runtime_t *runtime, const loxdb_ota_version_t *target_v);`
- `int loxdb_ota_runtime_apply_migration(loxdb_ota_runtime_t *runtime, loxdb_ota_migrate_fn migrate_cb, void *migrate_ctx);`
- `int loxdb_ota_runtime_record_boot(loxdb_ota_runtime_t *runtime, uint8_t boot_ok, const loxdb_ota_policy_t *policy, loxdb_ota_rollback_fn rollback_cb, void *rollback_ctx, loxdb_ota_result_t *out_result);`
- `const char *loxdb_ota_result_name(loxdb_ota_result_t result);`
