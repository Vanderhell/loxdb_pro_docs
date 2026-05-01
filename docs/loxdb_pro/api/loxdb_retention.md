# `loxdb_retention` (periodic cleanup)

- Public header: `modules/loxdb_retention/include/loxdb_retention.h`
- CMake target: `loxdb::retention`

## Purpose

Retention policy runner that can:

- purge expired KV entries
- cleanup / trim TS streams based on configured rules
- cleanup / trim REL tables based on configured rules

## API

- `loxdb_retention_err_t loxdb_retention_init(loxdb_retention_t *ret, const loxdb_retention_cfg_t *cfg, uint64_t start_time_ms);`
- `loxdb_retention_err_t loxdb_retention_set_policy(loxdb_retention_t *ret, const loxdb_retention_policy_t *policy, uint64_t now_ms);`
- `loxdb_retention_err_t loxdb_retention_run_once(loxdb_retention_t *ret, uint64_t now_ms);`
- `loxdb_retention_err_t loxdb_retention_tick(loxdb_retention_t *ret, uint64_t now_ms);`
- `loxdb_retention_err_t loxdb_retention_get_stats(const loxdb_retention_t *ret, loxdb_retention_stats_t *out_stats);`
- helper callbacks:
  - `loxdb_retention_ts_cleanup_max_age(...)`
  - `loxdb_retention_rel_cleanup_max_rows(...)`
- `const char *loxdb_retention_err_name(loxdb_retention_err_t err);`

