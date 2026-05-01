# loxdb_admission

## Purpose

`loxdb_admission` evaluates capacity and rate policies before write operations.

## Main API

- `loxdb_admission_eval(...)`
- `loxdb_admission_eval_ex(...)`

## What it evaluates

- global quota pressure and warning thresholds
- per-stream and per-table limits
- rate-limit decisions
- backpressure signaling

## Output model

- decision class (`ok`, `warn`, `throttle`, `reject`)
- reason flags for policy hits
- predictive warning support
