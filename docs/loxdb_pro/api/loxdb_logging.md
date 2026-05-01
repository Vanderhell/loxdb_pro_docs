# `loxdb_logging` (audit + trace events)

- Public header: `modules/loxdb_logging/include/loxdb_logging.h`
- CMake target: `loxdb::logging`

## Purpose

Emits structured log/audit/trace events via a user-provided hook.

## API

- hook management:
  - `void loxdb_logging_set_hook(loxdb_log_hook_t hook, void *ctx);`
  - `loxdb_log_hook_t loxdb_logging_get_hook(void);`
- emit:
  - `int loxdb_logging_emit(const loxdb_log_event_t *event);`
  - `int loxdb_logging_audit_query(uint64_t ts_ms, const char *query_name, int32_t rc, uint32_t latency_us);`
  - `int loxdb_logging_audit_query_ex(uint64_t ts_ms, const char *component, const char *query_name, int32_t rc, uint32_t latency_us, uint32_t rows_affected, uint32_t payload_bytes);`
  - `int loxdb_logging_trace_span(uint64_t ts_ms, const char *component, const char *action, uint32_t trace_id, uint16_t span_id, uint8_t is_begin, int32_t rc, uint32_t latency_us);`
- stats:
  - `void loxdb_logging_reset_stats(void);`
  - `int loxdb_logging_get_stats(loxdb_log_stats_t *out_stats);`
