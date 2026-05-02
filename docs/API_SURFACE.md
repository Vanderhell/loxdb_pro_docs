# Public API surface

`loxdb_pro` is intended to be consumed as a set of **stable C headers** as part of a commercial distribution.

## Public headers (product API)

All headers below are considered the public API for the corresponding module:

- `modules/loxdb_api/include/loxdb_api.h`
- `modules/loxdb_secure/include/loxdb_secure.h`
- `modules/loxdb_safety/include/loxdb_safety.h`
- `modules/loxdb_safety/include/loxdb_safety_profile.h`
- `modules/loxdb_codec/include/loxdb_codec.h`
- `modules/loxdb_metrics/include/loxdb_metrics.h`
- `modules/loxdb_metrics/include/loxdb_metrics_microtimer.h`
- `modules/loxdb_logging/include/loxdb_logging.h`
- `modules/loxdb_monitor/include/loxdb_monitor.h`
- `modules/loxdb_admission/include/loxdb_admission.h`
- `modules/loxdb_policy/include/loxdb_policy.h`
- `modules/loxdb_retention/include/loxdb_retention.h`
- `modules/loxdb_scheduler/include/loxdb_scheduler.h`
- `modules/loxdb_backup/include/loxdb_backup.h`
- `modules/loxdb_schema_migrate/include/loxdb_schema_migrate.h`
- `modules/loxdb_replication/include/loxdb_replication.h`
- `modules/loxdb_net/include/loxdb_net.h`
- `modules/loxdb_alerting/include/loxdb_alerting.h`
- `modules/loxdb_ota/include/loxdb_ota.h`
- `modules/loxdb_cli/include/cli_*.h` (CLI library + app struct)
- `modules/loxdb_adapter_sdcard/include/loxdb_adapter_sdcard.h` (optional)
- `modules/loxdb_ftl_basic/include/loxdb_ftl_basic.h` (optional)

## Recommended high-level integration approach

- If your app wants *one* stable call surface: use `loxdb_api` and select `LOXDB_API_MODE_PLAIN` vs `LOXDB_API_MODE_SECURE`.
- If your app wants a dedicated secure wrapper: use `loxdb_secure` directly.
- If you need runtime gates (auth/encryption/limits): use `loxdb_policy` before/after operations.
- If you need audit/trace: install a `loxdb_logging` hook and emit structured events.

## Versioning note

The public API is defined by the header contents. When producing a commercial distribution, treat these headers as your ABI/API contract:

- additions are backwards compatible
- removals/renames require a major version bump on the distribution package
