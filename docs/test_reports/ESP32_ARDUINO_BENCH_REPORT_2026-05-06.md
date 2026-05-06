# loxdb_pro – ESP32 (Arduino) Full Bench Report

Date: 2026-05-06  
Scope: `examples/esp32_arduino_bench/loxdb_pro_esp32_bench` (Serial-driven micro-bench + integrity checks)

## Executive Summary

This report documents a full bench run on a real ESP32-S3 device over Serial (COM20, 115200 baud).

Pass criteria:

- Database initializes and deinitializes without error.
- KV bench: all `put`/`get` operations succeed with data integrity preserved.
- TS bench: inserts and queries remain correct up to the configured storage limit; integrity checks remain clean.
- REL bench: inserts and indexed lookups succeed with correct counts and clean integrity checks.
- `loxdb_selfcheck()` reports OK (`kv_ok=1 ts_ok=1 rel_ok=1 wal_ok=1`) after each phase.

Result:

- **PASS** for KV and REL flows within configured limits.
- TS inserts correctly saturate at the storage limit and return `LOX_ERR_STORAGE (-6)` thereafter; selfcheck remains clean.

## Test Artifacts

- Sketch: `examples/esp32_arduino_bench/loxdb_pro_esp32_bench/loxdb_pro_esp32_bench.ino`
- Raw serial log: `docs/test_reports/esp32_arduino_bench_COM20_2026-05-06_v9.log`

## Test Environment

### Host

- OS: Microsoft Windows 11 Pro for Workstations (build 26100)
- Shell: Windows PowerShell 5.1
- Time zone: Central Europe Standard Time

### Toolchain

- `arduino-cli`: 1.4.1 (commit e39419312, 2026-01-19)
- ESP32 Arduino core: `esp32:esp32` 3.3.8
- Upload tool: `esptool` v5.2.0 (as reported by `arduino-cli upload`)

### Target (from `info` command)

- `chip_model=ESP32-S3`
- `chip_rev=2`
- `cpu_mhz=240`
- `free_heap=304920`
- `psram=no`

### Firmware Under Test

- Git revision: `2e60699050757c24da1e90157d23914d92339d5f`
- Board FQBN used for build/upload: `esp32:esp32:esp32s3`
- Serial port: `COM20` @ `115200`

### Runtime Configuration (sketch defaults)

- Storage backend: RAM (`loxdb_port_ram`)
- `storage_bytes=196608` (192 KiB)
- `ram_kb=64`

Notes:

- KV max value length is determined by the build profile: `LOX_KV_VAL_MAX_LEN=128`. Larger values are rejected.

## Method

The test was executed by uploading the sketch and running the `fullbench` command over Serial.

The `fullbench` command is structured into isolated phases (KV → TS → TS → REL), reinitializing the DB between phases to avoid cross-phase WAL/storage saturation and to keep results interpretable.

At the end of each phase the sketch runs `loxdb_selfcheck()` to validate internal invariants across KV/TS/REL/WAL.

## Results

All timing is reported by the device using `millis()`; throughput numbers below are derived from the raw output.

### KV (`put+get`)

Throughput definition: `(iters * 2) / dt_s` (each iteration performs one `put` and one `get`).

| iters | value_len (B) | keyspace | dt (ms) | put_ok | get_ok | fail | throughput (ops/s) |
|------:|--------------:|---------:|--------:|-------:|-------:|-----:|-------------------:|
| 1000  | 16            | 16       | 34      | 1000   | 1000   | 0    | 58823.53 |
| 5000  | 16            | 128      | 277     | 5000   | 5000   | 0    | 36101.08 |
| 5000  | 64            | 128      | 577     | 5000   | 5000   | 0    | 17331.02 |
| 5000  | 128           | 128      | 1228    | 5000   | 5000   | 0    | 8143.32 |
| 5000  | 16            | 1024     | 407     | 5000   | 5000   | 0    | 24570.02 |

Integrity:

- No KV errors observed (`first_err=0` for all KV cases).
- `selfcheck` after KV phase: `kv_ok=1 ts_ok=1 rel_ok=1 wal_ok=1`, anomalies all zero.

### TS (single stream `bench_u32`)

The stream is registered as `U32`, cleared, and then the benchmark attempts `iters` inserts (timestamps `0..iters-1`).
The implementation stops inserting after `LOX_ERR_FULL` but continues to count and report other failures.

Observed behavior for this configuration:

- Inserts succeed up to `ins_ok=2197` samples, then fail with `LOX_ERR_STORAGE (-6)` due to storage/WAL constraints of the 192 KiB RAM backend configuration.
- Query remains correct (`q_count == ins_ok`) and last sample validation passes (`last_ok=1`).

| iters | dt_ins (ms) | ins_ok | fail | first_err | dt_q (ms) | q_count | last_ok | insert rate (ins/s) |
|------:|------------:|-------:|-----:|----------:|----------:|--------:|--------:|--------------------:|
| 5000  | 29869       | 2197   | 2803 | -6        | 3         | 2197    | 1       | 73.55 |
| 20000 | 188237      | 2197   | 17803| -6        | 3         | 2197    | 1       | 11.67 |

Integrity:

- `selfcheck` after each TS phase reports OK with zero anomalies.

Interpretation:

- `LOX_ERR_STORAGE (-6)` indicates the configured in-RAM storage budget (including WAL) is exhausted for this workload.
- This is expected under a fixed-size storage backend and serves as a capacity validation point.

### REL (`users_256` table)

Schema:

- Table: `users_256`
- `id: U32` (indexed)
- `age: U8`
- `name: STR(16)`
- Capacity: `max_rows=256`

Workload:

- Insert `iters` rows and validate indexed lookups by `id` for all inserted rows.

| iters | max_rows | dt_ins (ms) | ins_ok | dt_find (ms) | find_ok | count | fail | insert rate (rows/s) | lookup rate (rows/s) |
|------:|---------:|------------:|-------:|-------------:|--------:|------:|-----:|---------------------:|---------------------:|
| 256   | 256      | 17          | 256    | 13           | 256     | 256   | 0    | 15058.82 | 19692.31 |

Integrity:

- `selfcheck` after REL phase reports OK with zero anomalies.

## Reproducibility

### Build + Upload

```powershell
arduino-cli compile --fqbn esp32:esp32:esp32s3 .\examples\esp32_arduino_bench\loxdb_pro_esp32_bench
arduino-cli upload  -p COM20 --fqbn esp32:esp32:esp32s3 .\examples\esp32_arduino_bench\loxdb_pro_esp32_bench
```

### Run

Open a serial monitor at 115200 baud and run:

```text
info
init
fullbench
deinit
```

## Conclusions and Next Steps

- KV and REL functionality + integrity checks are validated on ESP32-S3 in a pure-RAM configuration.
- TS insertion saturates at ~2197 samples for this configuration due to storage budget; correctness is preserved up to the limit.

If higher TS capacity is required on-device:

- Increase `storage_bytes` (bounded by available heap), or
- Adjust the TS/WAL configuration and/or compaction strategy to reduce WAL pressure for TS workloads.

