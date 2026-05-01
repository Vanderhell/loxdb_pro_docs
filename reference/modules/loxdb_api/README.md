# loxdb_api

## Purpose

`loxdb_api` is a unified facade over plain `loxdb` and `loxdb_secure` access.

## Main API

- lifecycle: `loxdb_api_open(...)`, `loxdb_api_close(...)`
- KV: `loxdb_api_kv_*`
- TS: `loxdb_api_ts_*`
- REL: `loxdb_api_rel_*`
- runtime/safety/stats: `loxdb_api_get_runtime(...)`, `loxdb_api_validate_safety(...)`, `loxdb_api_stats(...)`

## Modes

- `LOXDB_API_MODE_PLAIN`
- `LOXDB_API_MODE_SECURE`

## Notes

- Keeps app-side call surface stable when switching secure/plain runtime modes.
