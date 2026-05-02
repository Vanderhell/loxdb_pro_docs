# `loxdb_alerting` (metrics-driven state machine)

- Public header: `modules/loxdb_alerting/include/loxdb_alerting.h`
- CMake target: `loxdb::alerting`

## Purpose

Evaluates `loxdb_metrics_snapshot_t` and emits alert events:

- `OK` → `PENDING` → `FIRING` → `RESOLVED`

Policy supports thresholds and cooldown.

## API

- `loxdb_alerting_t *loxdb_alerting_create(const loxdb_alerting_cfg_t *cfg);`
- `void loxdb_alerting_destroy(loxdb_alerting_t *alerting);`
- `loxdb_alerting_err_t loxdb_alerting_eval(loxdb_alerting_t *alerting, const loxdb_metrics_snapshot_t *snapshot, loxdb_alerting_event_t *out_event);`
- `loxdb_alerting_state_t loxdb_alerting_state(const loxdb_alerting_t *alerting);`
- name helpers:
  - `const char *loxdb_alerting_err_name(loxdb_alerting_err_t err);`
  - `const char *loxdb_alerting_state_name(loxdb_alerting_state_t state);`

