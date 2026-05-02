# `loxdb_safety` (runtime validation + safety profile)

- Public headers:
  - `modules/loxdb_safety/include/loxdb_safety.h`
  - `modules/loxdb_safety/include/loxdb_safety_profile.h`
- CMake target: `loxdb::safety`

## Purpose

`loxdb_safety` is a certification-support layer used in controlled PRO releases:

- provides a fixed `LOXDB_SAFETY_PROFILE_ID`
- validates runtime invariants and can emit a report into a `FILE*`

## API

- `const char *loxdb_safety_profile_id(void);`
- `int loxdb_safety_validate_runtime(loxdb_t *db, FILE *report_out);`
- `int loxdb_safety_validate_runtime_ex(loxdb_t *db, FILE *report_out, loxdb_safety_report_t *report);`

## Build-time profile gates

`loxdb_safety_profile.h` defines deterministic compile-time checks (for example RAM floor and mandatory WAL enablement) and is intended to be used by release build presets.

