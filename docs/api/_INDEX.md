# API reference index

These pages document the public, integration-facing APIs for each `loxdb_pro` module.

For a one-table overview (targets + public headers), see `../MODULE_CATALOG.md`.

## Facade / security

- [`loxdb_api.md`](loxdb_api.md) - unified facade for plain vs secure mode
- [`loxdb_secure.md`](loxdb_secure.md) - encryption + integrity wrapper
- [`loxdb_safety.md`](loxdb_safety.md) - runtime safety validation hooks
- [`loxdb_codec.md`](loxdb_codec.md) - compression/encoding helpers

## Observability

- [`loxdb_metrics.md`](loxdb_metrics.md) - metrics snapshot/export
- [`loxdb_metrics_microtimer.md`](loxdb_metrics_microtimer.md) - metrics scheduler adapter
- [`loxdb_logging.md`](loxdb_logging.md) - audit/trace hooks
- [`loxdb_monitor.md`](loxdb_monitor.md) - health evaluation policy
- [`loxdb_alerting.md`](loxdb_alerting.md) - alert state machine on metrics

## Policies / lifecycle

- [`loxdb_admission.md`](loxdb_admission.md) - quotas/rate-limit decisions + backpressure
- [`loxdb_policy.md`](loxdb_policy.md) - central policy gate for operations
- [`loxdb_retention.md`](loxdb_retention.md) - periodic retention and cleanup
- [`loxdb_scheduler.md`](loxdb_scheduler.md) - periodic job scheduler
- [`loxdb_backup.md`](loxdb_backup.md) - logical backup export/import
- [`loxdb_schema_migrate.md`](loxdb_schema_migrate.md) - migration planning/execution

## Connectivity / fleet ops

- [`loxdb_replication.md`](loxdb_replication.md) - transport-agnostic replication core
- [`loxdb_net.md`](loxdb_net.md) - framing + handshake helpers
- [`loxdb_ota.md`](loxdb_ota.md) - OTA planning and runtime state machine

## Storage backends (optional)

- [`loxdb_adapter_sdcard.md`](loxdb_adapter_sdcard.md) - FatFS-backed `lox_storage_t`
- [`loxdb_ftl_basic.md`](loxdb_ftl_basic.md) - basic NAND FTL-backed `lox_storage_t`

## Tooling

- [`loxdb_cli.md`](loxdb_cli.md) - CLI helpers + terminal tool

