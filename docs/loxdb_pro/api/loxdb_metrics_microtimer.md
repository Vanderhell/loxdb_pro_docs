# `loxdb_metrics_microtimer` (adapter)

- Public header: `modules/loxdb_metrics/include/loxdb_metrics_microtimer.h`
- CMake target: `loxdb::metrics_microtimer`

## Purpose

Wires `loxdb_metrics_scheduler_t` to a timer callback interface.

## API

- `loxdb_metrics_err_t loxdb_metrics_microtimer_adapter_init(loxdb_metrics_microtimer_adapter_t *adapter, const loxdb_metrics_scheduler_cfg_t *cfg, uint64_t start_time_ms, uint8_t *buffer, size_t buffer_len);`
- `void loxdb_metrics_microtimer_adapter_on_tick(void *ctx, uint64_t now_ms);`
- `loxdb_metrics_err_t loxdb_metrics_microtimer_adapter_get_status(const loxdb_metrics_microtimer_adapter_t *adapter, loxdb_metrics_err_t *out_err, size_t *out_written, uint64_t *out_last_tick_ms);`
- `loxdb_metrics_err_t loxdb_metrics_microtimer_adapter_bind_external(loxdb_metrics_microtimer_adapter_t *adapter, void *timer_ctx, loxdb_metrics_microtimer_bind_fn bind_fn);`
- `loxdb_metrics_err_t loxdb_metrics_microtimer_adapter_unbind_external(loxdb_metrics_microtimer_adapter_t *adapter, void *timer_ctx, loxdb_metrics_microtimer_unbind_fn unbind_fn);`
