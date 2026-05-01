# loxdb_secure

## Purpose

`loxdb_secure` is a transparent encryption/integrity wrapper for `loxdb` data paths.

## Main API

- lifecycle: `loxdb_secure_init(...)`, `loxdb_secure_deinit(...)`, `loxdb_secure_rotate_key(...)`
- KV: `loxdb_secure_kv_*`
- TS: `loxdb_secure_ts_*`
- REL: `loxdb_secure_rel_*`, table helpers
- metadata helpers: `loxdb_secure_store_db_id(...)`, `loxdb_secure_load_db_id(...)`

## Features

- AES-128-CBC encryption
- HMAC-SHA256 integrity tags
- derived key separation for ENC/MAC/IV material
- allocation-free core implementation
