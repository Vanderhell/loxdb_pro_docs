# loxdb_retention

## Purpose

`loxdb_retention` is a central retention policy layer for periodic cleanup workflows.

## Main API

- `loxdb_retention_init(...)`
- `loxdb_retention_set_policy(...)`
- `loxdb_retention_run_once(...)`
- `loxdb_retention_tick(...)`
- `loxdb_retention_get_stats(...)`

## Current behavior

- optional expired KV purge via `lox_kv_purge_expired(...)`
- optional TS/REL rule lists in policy
- optional TS/REL cleanup hooks through per-rule callbacks
- optional TS/REL trim hooks through per-rule callbacks (`action = TRIM`)
- interval scheduler logic with caller-provided `now_ms`

## Reference cleanup callbacks

- `loxdb_retention_ts_cleanup_max_age(...)`
  - clears TS stream when latest sample is older than `max_age_ms`
- `loxdb_retention_rel_cleanup_max_rows(...)`
  - clears REL table when row count exceeds `max_rows`

## Trim strategy hook points

- set `rule.action = LOXDB_RETENTION_ACTION_TRIM`
- provide:
  - `cfg.ts_trim_cb` for TS rules
  - `cfg.rel_trim_cb` for REL rules
- this allows caller-owned "trim" behavior without forcing full clear

## Notes

- transport/thread model stays caller-owned
- designed to be later wired to `microtimer` and policy config storage
