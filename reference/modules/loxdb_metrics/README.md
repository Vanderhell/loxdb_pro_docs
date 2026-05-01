# loxdb_metrics

## Purpose

`loxdb_metrics` collects runtime DB metrics and exports them in a compact binary frame.

## Main API

- `loxdb_metrics_collect(...)`
- `loxdb_metrics_export(...)`
- `loxdb_metrics_export_cbor(...)`
- scheduler: `loxdb_metrics_scheduler_init(...)`, `loxdb_metrics_scheduler_tick(...)`
- microtimer adapter:
  - `loxdb_metrics_microtimer_adapter_init(...)`
  - `loxdb_metrics_microtimer_adapter_on_tick(...)`
  - `loxdb_metrics_microtimer_adapter_get_status(...)`
  - `loxdb_metrics_microtimer_adapter_bind_external(...)`
  - `loxdb_metrics_microtimer_adapter_unbind_external(...)`

## Collected data

- summary stats (`loxdb_stats_t`)
- db/kv/ts/rel stats
- pressure snapshot
- selfcheck result

## Export format

- fixed-size binary packet (`LOXDB_METRICS_EXPORT_SIZE`)
- magic `LMET`, version byte, packed little-endian fields
- optional compact CBOR map export profile (implemented via `microcbor`)
- transport-agnostic: caller can forward buffer via UART/TCP/HTTP/MQTT/file

## Periodic flow

- scheduler API is transport-agnostic and callback-based
- call `loxdb_metrics_scheduler_tick(...)` from your loop/timer source
- choose binary or CBOR payload format per scheduler config
- microtimer adapter wrapper allows direct timer-callback wiring without changing scheduler API
- optional bind/unbind bridge allows integration with external microtimer runtime without hard compile dependency
