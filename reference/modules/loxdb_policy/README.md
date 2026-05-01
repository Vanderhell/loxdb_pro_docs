# loxdb_policy

## Purpose

`loxdb_policy` is a centralized runtime policy gate for KV/TS/REL operations with explicit hook-coverage checks.

## Main API

- `loxdb_policy_init(...)`
- `loxdb_policy_eval(...)`
- `loxdb_policy_check_hook_coverage(...)`
- `loxdb_policy_before_op(...)`
- `loxdb_policy_after_op(...)`

## Coverage model

- operation coverage is tracked across:
  - `kv_set`, `kv_del`
  - `ts_insert`
  - `rel_insert`, `rel_delete`
- an operation is considered "covered" only when both `pre` and `post` hooks are present
- with `require_full_hook_coverage=1`, missing coverage forces `REJECT` before operation

## Notes

- this module does not patch core internals directly
- intended integration is wrapper/path-level enforcement in app or API boundary
- design goal is to prevent false safety claims when hook wiring is incomplete
