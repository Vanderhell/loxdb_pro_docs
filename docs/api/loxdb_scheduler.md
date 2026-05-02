# `loxdb_scheduler` (lightweight job scheduler)

- Public header: `modules/loxdb_scheduler/include/loxdb_scheduler.h`
- CMake target: `loxdb::scheduler`

## Purpose

Runs small periodic jobs (callbacks) with:

- interval scheduling
- retry states and retry delay
- job info and aggregate stats

## API

- `loxdb_scheduler_err_t loxdb_scheduler_init(loxdb_scheduler_t *scheduler);`
- `loxdb_scheduler_err_t loxdb_scheduler_add_job(loxdb_scheduler_t *scheduler, const loxdb_scheduler_job_cfg_t *cfg, uint32_t *out_job_id);`
- `loxdb_scheduler_err_t loxdb_scheduler_tick(loxdb_scheduler_t *scheduler, uint64_t now_ms);`
- `loxdb_scheduler_err_t loxdb_scheduler_get_job(const loxdb_scheduler_t *scheduler, uint32_t job_id, loxdb_scheduler_job_info_t *out_job);`
- `loxdb_scheduler_err_t loxdb_scheduler_set_enabled(loxdb_scheduler_t *scheduler, uint32_t job_id, uint8_t enabled, uint64_t now_ms);`
- `loxdb_scheduler_err_t loxdb_scheduler_get_stats(const loxdb_scheduler_t *scheduler, loxdb_scheduler_stats_t *out_stats);`
- `const char *loxdb_scheduler_err_name(loxdb_scheduler_err_t err);`

