# `loxdb_admission` (quota + rate-limit + prediction)

- Public header: `modules/loxdb_admission/include/loxdb_admission.h`
- CMake target: `loxdb::admission`

## Purpose

Produces admission decisions (`OK/WARN/THROTTLE/REJECT`) from limits + usage, and provides a backpressure percentage for the caller.

## API

- `int loxdb_admission_eval(const loxdb_admission_limits_t *limits, const loxdb_admission_usage_t *usage, loxdb_admission_status_t *out_status, uint32_t *out_backpressure_percent);`
- `int loxdb_admission_eval_ex(const loxdb_admission_limits_ex_t *limits, const loxdb_admission_usage_ex_t *usage, loxdb_admission_decision_t *out_decision);`
- `const char *loxdb_admission_status_name(loxdb_admission_status_t status);`

