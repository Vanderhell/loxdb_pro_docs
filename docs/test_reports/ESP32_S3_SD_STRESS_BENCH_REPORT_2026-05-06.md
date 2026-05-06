# loxdb_pro – ESP32-S3 SD Stress Bench Report

Date: 2026-05-06  
Scope: `examples/esp32_sd_stress_bench/esp32_sd_stress_bench.ino` (SD_MMC 1-bit + persistent loxdb image)

## Executive Summary

This report captures a real-hardware smoke validation of the SD stress bench on an ESP32-S3 with an SD card.

Result:

- **PASS (SMOKE)**: SD mounted successfully, database initialized successfully after lowering RAM budget, stress loop runs, verification probes show `ok>0` and `fail=0` during the captured interval.

## Hardware Pinout (as configured in sketch)

SD_MMC (1-bit):

- `CLK=GPIO17`
- `CMD=GPIO18`
- `D0=GPIO16`
- `D3=GPIO47`

LCD:

- disabled for this run (`SDSTRESS_LCD_ENABLE=0`)

## Toolchain

- `arduino-cli`: 1.4.1
- ESP32 Arduino core: `esp32:esp32` 3.3.8
- FQBN: `esp32:esp32:esp32s3` (ESP32S3 Dev Module)

## Runtime Configuration (key points)

- Storage backend: SD file-backed image at `/loxdb_stress_store.bin` via SD_MMC
- Storage capacity (`kStorageBytes`): 32 MiB (bench-local configuration)
- Erase size: 4096
- Write size: 1
- loxdb RAM budget (`cfg.ram_kb`): 2048 KB (with fallback ladder on `LOX_ERR_NO_MEM`)
- WAL sync mode: `LOX_WAL_SYNC_FLUSH_ONLY`

## Procedure

1. Upload the sketch to the device.
2. Open Serial Monitor at `115200`.
3. Execute:
   - `pause`
   - `profile smoke`
   - `verify on`
   - `mode all`
   - `formatdb` (recreate image)
   - `stats`
   - `run`
   - wait ~60s
   - `pause`
   - `stats`

## Evidence / Artifacts

- Raw serial log: `docs/test_reports/esp32_s3_sd_stress_bench_COM19_2026-05-06_smoke2.log`

## Results (from captured log)

Key events:

- SD mount succeeded: `[OK] SD mounted card=7580MB`
- DB image recreation completed after `formatdb`: `[OK] db reset complete`
- Stress loop ran and advanced op counter.

Final captured snapshot (after ~60s soak, `profile=smoke`, `mode=all`, `verify=on`):

- `[PRESSURE] kv=96 ts=0 rel=0 wal=0 risk=96 ops=535`
- `[STATS] kv=240/248 ts_samples=155 rel_rows=140 wal=0/32736`
- `[BENCH] ... ok=7 fail=0 compact=0 ... streams=8 tables=4`

Interpretation:

- KV pressure dominates in smoke mix for the captured interval (expected with small KV key budget and short run time).
- No verification failures were reported in the captured window (`fail=0`).

## Notes

- Earlier init failures with `lox_init rc=-2` (`LOX_ERR_NO_MEM`) were addressed by lowering `cfg.ram_kb` and adding a deterministic retry ladder. This change is now reflected in the integrated sketch.

