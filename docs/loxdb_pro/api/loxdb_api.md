# `loxdb_api` (unified facade)

- Public header: `modules/loxdb_api/include/loxdb_api.h`
- CMake target: `loxdb::api`

## Purpose

`loxdb_api` provides one stable API surface over:

- plain `loxdb` access, and
- secure `loxdb_secure` access (encryption + integrity),

so the application can switch modes without rewriting call-sites.

## Key types

- `loxdb_api_t` – runtime handle (wraps `loxdb_t*` and optionally `loxdb_secure_t*`)
- `loxdb_api_cfg_t` – open configuration (mode, keys, DB id, safety validation options)
- `loxdb_api_runtime_t` – runtime state (mode, secure active, safety validated, profile id)
- `loxdb_api_err_t` – unified error codes for plain/secure paths

## Lifecycle

- `loxdb_api_err_t loxdb_api_open(loxdb_api_t *api, const loxdb_api_cfg_t *cfg);`
- `void loxdb_api_close(loxdb_api_t *api);`
- `loxdb_api_err_t loxdb_api_get_runtime(const loxdb_api_t *api, loxdb_api_runtime_t *out_runtime);`
- `loxdb_api_err_t loxdb_api_validate_safety(loxdb_api_t *api, FILE *report_out, loxdb_safety_report_t *out_report);`

## KV API

- `loxdb_api_err_t loxdb_api_kv_set(loxdb_api_t *api, const char *key, const void *val, size_t len, uint32_t ttl);`
- `loxdb_api_err_t loxdb_api_kv_get(loxdb_api_t *api, const char *key, void *buf, size_t buf_len, size_t *out_len);`
- `loxdb_api_err_t loxdb_api_kv_del(loxdb_api_t *api, const char *key);`
- `loxdb_api_err_t loxdb_api_kv_exists(loxdb_api_t *api, const char *key);`
- `loxdb_api_err_t loxdb_api_kv_iter_keys(loxdb_api_t *api, loxdb_api_kv_key_iter_cb_t cb, void *ctx);`
- `loxdb_api_err_t loxdb_api_kv_clear(loxdb_api_t *api);`
- convenience: `loxdb_api_kv_put(api, key, val, len)` (TTL = 0)

## TS API (time-series)

- `loxdb_api_err_t loxdb_api_ts_register(loxdb_api_t *api, const char *name, loxdb_ts_type_t type, size_t raw_size);`
- `loxdb_api_err_t loxdb_api_ts_insert(loxdb_api_t *api, const char *name, LOXDB_TIMESTAMP_TYPE ts, const void *val);`
- `loxdb_api_err_t loxdb_api_ts_last(loxdb_api_t *api, const char *name, loxdb_ts_sample_t *out_sample);`
- `loxdb_api_err_t loxdb_api_ts_query(loxdb_api_t *api, const char *name, LOXDB_TIMESTAMP_TYPE from, LOXDB_TIMESTAMP_TYPE to, loxdb_api_ts_query_cb_t cb, void *ctx);`
- `loxdb_api_err_t loxdb_api_ts_count(loxdb_api_t *api, const char *name, LOXDB_TIMESTAMP_TYPE from, LOXDB_TIMESTAMP_TYPE to, size_t *out_count);`
- `loxdb_api_err_t loxdb_api_ts_clear(loxdb_api_t *api, const char *name);`

## REL API (relational tables)

- `loxdb_api_err_t loxdb_api_rel_table_create(loxdb_api_t *api, loxdb_schema_t *schema);`
- `loxdb_api_err_t loxdb_api_rel_table_get(loxdb_api_t *api, const char *name, loxdb_table_t **out_table);`
- `loxdb_api_err_t loxdb_api_rel_insert(loxdb_api_t *api, loxdb_table_t *table, const void *row_buf);`
- `loxdb_api_err_t loxdb_api_rel_find(loxdb_api_t *api, loxdb_table_t *table, const void *search_val, loxdb_api_rel_iter_cb_t cb, void *ctx);`
- `loxdb_api_err_t loxdb_api_rel_find_by(loxdb_api_t *api, loxdb_table_t *table, const char *col_name, const void *search_val, void *out_buf);`
- `loxdb_api_err_t loxdb_api_rel_delete(loxdb_api_t *api, loxdb_table_t *table, const void *search_val, uint32_t *out_deleted);`
- `loxdb_api_err_t loxdb_api_rel_iter(loxdb_api_t *api, loxdb_table_t *table, loxdb_api_rel_iter_cb_t cb, void *ctx);`
- `loxdb_api_err_t loxdb_api_rel_count(loxdb_api_t *api, const loxdb_table_t *table, uint32_t *out_count);`
- `loxdb_api_err_t loxdb_api_rel_clear(loxdb_api_t *api, loxdb_table_t *table);`

## Stats & self-check

- `loxdb_api_err_t loxdb_api_stats(loxdb_api_t *api, loxdb_stats_t *out_stats);`
- `loxdb_api_err_t loxdb_api_get_db_stats(loxdb_api_t *api, lox_db_stats_t *out_stats);`
- `loxdb_api_err_t loxdb_api_get_kv_stats(loxdb_api_t *api, lox_kv_stats_t *out_stats);`
- `loxdb_api_err_t loxdb_api_get_ts_stats(loxdb_api_t *api, lox_ts_stats_t *out_stats);`
- `loxdb_api_err_t loxdb_api_get_rel_stats(loxdb_api_t *api, lox_rel_stats_t *out_stats);`
- `loxdb_api_err_t loxdb_api_get_effective_capacity(loxdb_api_t *api, lox_effective_capacity_t *out_capacity);`
- `loxdb_api_err_t loxdb_api_get_pressure(loxdb_api_t *api, lox_pressure_t *out_pressure);`
- `loxdb_api_err_t loxdb_api_selfcheck(loxdb_api_t *api, loxdb_selfcheck_result_t *out_selfcheck);`

## Utilities

- `const char *loxdb_api_mode_name(loxdb_api_mode_t mode);`

