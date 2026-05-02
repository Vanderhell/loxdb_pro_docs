# `loxdb_secure` (encryption + integrity wrapper)

- Public header: `modules/loxdb_secure/include/loxdb_secure.h`
- CMake target: `loxdb::secure`

## Purpose

`loxdb_secure` is an encryption-at-rest + integrity layer that wraps a `loxdb_t` instance and exposes KV/TS/REL APIs that mirror the plain calls.

The `loxdb_api` module can use this internally, but you can also use `loxdb_secure` directly.

## Key limits (compile-time)

- `MDBS_KV_PLAINTEXT_MAX` (default 32 bytes)
- `MDBS_REL_ROW_PLAINTEXT_MAX` (default 512 bytes)
- `MDBS_TS_SAMPLE_PLAINTEXT_MAX` (default 4 bytes)
- DB id size: `MDBS_DB_ID_SIZE` (16 bytes)
- handle storage: `MDBS_HANDLE_SIZE` (256 bytes)

## Key types

- `loxdb_secure_t` – opaque handle storage
- `mdbs_cfg_t` – binds secure wrapper to `loxdb_t`, master key, and optional DB id
- `mdbs_err_t` – error codes (invalid, corrupt, tampered, key, storage, ...)

## Lifecycle

- `mdbs_err_t loxdb_secure_init(loxdb_secure_t *s, const mdbs_cfg_t *cfg);`
- `void loxdb_secure_deinit(loxdb_secure_t *s);`
- `mdbs_err_t loxdb_secure_rotate_key(loxdb_secure_t *s, const uint8_t *new_master_key, size_t new_master_key_len);`

## KV API

- `mdbs_err_t loxdb_secure_kv_set(loxdb_secure_t *s, const char *key, const void *val, size_t plaintext_len, uint32_t ttl);`
- `mdbs_err_t loxdb_secure_kv_get(loxdb_secure_t *s, const char *key, void *buf, size_t buf_len, size_t *out_len);`
- `mdbs_err_t loxdb_secure_kv_del(loxdb_secure_t *s, const char *key);`
- `mdbs_err_t loxdb_secure_kv_exists(loxdb_secure_t *s, const char *key);`
- `mdbs_err_t loxdb_secure_kv_iter_keys(loxdb_secure_t *s, mdbs_kv_key_iter_cb_t cb, void *ctx);`
- `mdbs_err_t loxdb_secure_kv_clear(loxdb_secure_t *s);`
- convenience: `loxdb_secure_kv_put(s, key, val, len)` (TTL = 0)

## TS API

- `mdbs_err_t loxdb_secure_ts_register(loxdb_secure_t *s, const char *name, loxdb_ts_type_t type, size_t raw_size);`
- `mdbs_err_t loxdb_secure_ts_insert(loxdb_secure_t *s, const char *name, LOXDB_TIMESTAMP_TYPE ts, const void *val);`
- `mdbs_err_t loxdb_secure_ts_last(loxdb_secure_t *s, const char *name, loxdb_ts_sample_t *out);`
- `mdbs_err_t loxdb_secure_ts_query(loxdb_secure_t *s, const char *name, LOXDB_TIMESTAMP_TYPE from, LOXDB_TIMESTAMP_TYPE to, mdbs_ts_query_cb_t cb, void *ctx);`
- `mdbs_err_t loxdb_secure_ts_clear(loxdb_secure_t *s, const char *name);`

## REL API

- `mdbs_err_t loxdb_secure_table_create(loxdb_secure_t *s, loxdb_schema_t *schema);`
- `mdbs_err_t loxdb_secure_table_get(loxdb_secure_t *s, const char *name, loxdb_table_t **out_table);`
- `mdbs_err_t loxdb_secure_rel_insert(loxdb_secure_t *s, loxdb_table_t *table, const void *row_buf);`
- `mdbs_err_t loxdb_secure_rel_find(loxdb_secure_t *s, loxdb_table_t *table, const void *search_val, mdbs_rel_iter_cb_t cb, void *ctx);`
- `mdbs_err_t loxdb_secure_rel_find_by(loxdb_secure_t *s, loxdb_table_t *table, const char *col_name, const void *search_val, void *out_buf);`
- `mdbs_err_t loxdb_secure_rel_delete(loxdb_secure_t *s, loxdb_table_t *table, const void *search_val, uint32_t *out_deleted);`
- `mdbs_err_t loxdb_secure_rel_iter(loxdb_secure_t *s, loxdb_table_t *table, mdbs_rel_iter_cb_t cb, void *ctx);`
- `mdbs_err_t loxdb_secure_rel_count(loxdb_secure_t *s, const loxdb_table_t *table, uint32_t *out_count);`
- `mdbs_err_t loxdb_secure_rel_clear(loxdb_secure_t *s, loxdb_table_t *table);`

## DB id & lock/unlock

- `mdbs_err_t loxdb_secure_store_db_id(loxdb_secure_t *s);`
- `mdbs_err_t loxdb_secure_load_db_id(loxdb_secure_t *s, uint8_t out_db_id[MDBS_DB_ID_SIZE]);`
- `void loxdb_secure_lock(loxdb_secure_t *s);`
- `mdbs_err_t loxdb_secure_unlock(loxdb_secure_t *s, const uint8_t *master_key, size_t master_key_len);`

