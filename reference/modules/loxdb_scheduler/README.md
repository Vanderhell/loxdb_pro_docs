# loxdb_scheduler

## Purpose

`loxdb_scheduler` provides a lightweight job scheduler with retry semantics for embedded runtimes.

## Main API

- `loxdb_scheduler_init(...)`
- `loxdb_scheduler_add_job(...)`
- `loxdb_scheduler_tick(...)`
- `loxdb_scheduler_set_enabled(...)`
- `loxdb_scheduler_get_job(...)`
- `loxdb_scheduler_get_stats(...)`

## Job state model

- `IDLE -> RUNNING -> DONE`
- `IDLE -> RUNNING -> FAILED`
- `RUNNING -> RETRY` (until retry budget is exhausted)

## Notes

- callback-driven, no heap allocation
- caller provides time (`now_ms`) and drives execution via `tick`
- retry delay uses `microres` backoff calculation (`mres_delay_calc`)
