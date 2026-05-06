# loxdb_pro – ESP32-S3 SD Stress Bench SOAK Report

Date: 2026-05-06  
Scope: `examples/esp32_sd_stress_bench/esp32_sd_stress_bench.ino` (SD_MMC 1-bit + persistent SD image)

## Executive Summary

A 30-minute SOAK run was executed on real ESP32-S3 hardware with an SD card on `COM19` at `115200`.

Result:

- **PASS (SOAK 30m)**: continuous mixed workload (`mode=all`) ran for ~30 minutes with `verify=on` and **no verification failures** (`fail=0`).
- Auto-compaction was exercised under sustained pressure (`compact=192`), with worst observed compaction latency ~`6887 ms`.

## Evidence / Artifacts

- Raw serial log: `docs/test_reports/esp32_s3_sd_stress_bench_COM19_2026-05-06_soak30m.log`

## Run Configuration

Commands executed (high level):

- `pause`
- `profile soak`
- `verify on`
- `mode all`
- `formatdb`
- `stats`
- `run`
- 30 × (wait ~60s, `stats`)
- `pause`
- `stats`

Sketch key configuration points:

- SD storage image: `/loxdb_stress_store.bin`
- SD backend: `SD_MMC` (1-bit)
- `kStorageBytes=32 MiB`, `erase_size=4096`, `write_size=1`
- `cfg.ram_kb=2048` (with deterministic fallback ladder on `LOX_ERR_NO_MEM`)
- WAL sync: `LOX_WAL_SYNC_FLUSH_ONLY`

## Results (derived from log)

### Verification

From the final bench summary lines:

- `ok=600`
- `fail=0` (max observed in log: 0)

### Compaction

- `compact=192` (max observed in log: 192)
- `last_compact_ms=6689`
- max observed `last_compact_ms`: `6887`

### Pressure envelope

Maximum observed (from `[PRESSURE]` lines in the log):

- `kv=100`
- `ts=35`
- `rel=100`
- `wal=74`
- `risk=100`

Last observed pressure snapshot:

- `kv=100 ts=35 rel=100 wal=0 risk=100 ops=55047`

Interpretation:

- Under SOAK workload the system frequently operated near full (`risk=100`) and triggered compaction.
- Despite high pressure, verification remained stable (`fail=0`), indicating functional correctness under sustained SD-backed persistence for this run window.

## Notes / Known Constraints

- This report is a **system-level soak validation**, not a formal endurance qualification.
- For release-grade endurance evidence, consider repeating with:
  - multiple SD card vendors / capacities
  - controlled power supply and temperature conditions
  - longer run windows (e.g., 4–24 hours) with periodic log rotation

