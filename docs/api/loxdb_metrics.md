# `loxdb_metrics` (collection + export)

- Public header: `modules/loxdb_metrics/include/loxdb_metrics.h`
- CMake target: `loxdb::metrics`
- Category: Observability

## Purpose

Collects runtime DB metrics into a snapshot and exports it into a transport-agnostic payload:

- fixed-size binary frame (`LOXDB_METRICS_EXPORT_SIZE`)
- optional CBOR map payload

## Core API

- `loxdb_metrics_err_t loxdb_metrics_collect(loxdb_t *db, loxdb_metrics_snapshot_t *out_snapshot);`
- `loxdb_metrics_err_t loxdb_metrics_export(const loxdb_metrics_snapshot_t *snapshot, uint8_t *buffer, size_t buffer_len, size_t *written);`
- `loxdb_metrics_err_t loxdb_metrics_export_cbor(const loxdb_metrics_snapshot_t *snapshot, uint8_t *buffer, size_t buffer_len, size_t *written);`
- `const char *loxdb_metrics_err_name(loxdb_metrics_err_t err);`

## Periodic scheduler API

- `loxdb_metrics_err_t loxdb_metrics_scheduler_init(loxdb_metrics_scheduler_t *scheduler, const loxdb_metrics_scheduler_cfg_t *cfg, uint64_t start_time_ms);`
- `loxdb_metrics_err_t loxdb_metrics_scheduler_tick(loxdb_metrics_scheduler_t *scheduler, uint64_t now_ms, uint8_t *buffer, size_t buffer_len, size_t *written);`

The scheduler calls a `loxdb_metrics_publish_fn` callback when it emits a payload.

