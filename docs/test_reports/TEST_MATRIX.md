# Test Matrix (Hardware + Host)

This document defines a practical, GitHub-presentable validation matrix for `loxdb_pro`, combining:

- host automated tests and micro-benchmarks
- SD-card adapter tests and latency benchmark
- real-device (ESP32) serial-driven bench evidence
- optional CLI/TUI “hardware validation” operator runbook

## Conventions

- Each matrix row defines: **target**, **scope**, **commands**, **artifacts**, **pass criteria**.
- Store raw outputs under `docs/test_reports/` (or the downstream docs mirror repo) and link them from PRs/releases.
- When a test is not applicable to a given environment, mark it `N/A` explicitly in the run log.

## Matrix

### 1) Host: Core Library + Unit/Integration Tests

Target:

- Windows (primary dev machine)
- Linux (CI runner or local)

Scope:

- Core DB correctness, regression coverage, safety evidence bundle (if enabled in the repo)

Commands (example, adjust to your presets):

```powershell
cmake --preset debug
cmake --build build/debug -j
ctest --test-dir build/debug -C Debug --output-on-failure
```

Artifacts:

- `ctest` output (console capture)
- optional: `build/<preset>/releases/safety/` if generating safety evidence bundles

Pass criteria:

- all tests pass
- no crashes or sanitizer assertions (if enabled)

### 2) Host: SD-card Adapter (Real FatFS Vendor) + Latency Benchmark

Target:

- Windows host + vendor FatFS headers (or Linux/macOS equivalent)

Scope:

- SD-card adapter integration with vendor FatFS, plus latency micro-benchmark (`sdcard_bench_latency`)

Commands (Windows runner):

```powershell
.\scripts\run_sdcard_real_vendor.ps1 -FatFsIncludeDir "C:\vendor\fatfs\include"
cmake --build build/sdcard-real --config Debug --target sdcard_bench_latency
.\build\sdcard-real\Debug\sdcard_bench_latency.exe
```

Artifacts:

- `ctest` output from `build/sdcard-real`
- `sdcard_bench_latency` console output

Pass criteria:

- all `sdcard*` tests pass
- `sdcard_bench_latency` exits successfully and reports plausible latency values (no error messages)

### 3) Host: SD-card Soft Simulation (File-backed Medium)

Target:

- Windows / Linux / macOS

Scope:

- SD-card functional flows on a file-backed “virtual medium”

Commands:

```bash
ctest --test-dir build_sdcard_profile -C Debug -R sdcard_softsim --output-on-failure
```

Artifacts:

- `ctest` output capture

Pass criteria:

- tests pass

### 4) Hardware: ESP32 (Arduino) Full Bench + Integrity Checks

Target:

- ESP32-S3 over USB Serial

Scope:

- KV: `put+get` timing + correctness
- TS: saturation behavior under fixed storage budget + correctness of query/last
- REL: indexed inserts and lookups + correctness
- Integrity: `loxdb_selfcheck()` after each phase

Commands:

- Upload using Arduino CLI (example FQBN for ESP32-S3):

```powershell
arduino-cli compile --fqbn esp32:esp32:esp32s3 .\examples\esp32_arduino_bench\loxdb_pro_esp32_bench
arduino-cli upload  -p COM20 --fqbn esp32:esp32:esp32s3 .\examples\esp32_arduino_bench\loxdb_pro_esp32_bench
```

- Serial monitor @ `115200`:

```text
info
init
fullbench
deinit
```

Artifacts:

- raw serial log under `docs/test_reports/` (capture the entire session)
- a summary report (tables + environment + methodology)

Pass criteria:

- `db_init` returns `[OK]`
- KV cases have `fail=0` and `first_err=0`
- TS cases preserve correctness (`last_ok=1`, `q_count==ins_ok`) and fail with a documented capacity error once storage is saturated
- REL cases have `fail=0`, correct counts, and correct indexed lookups
- `selfcheck` always reports OK and anomalies remain zero

### 5) Optional: CLI/TUI Hardware Validation (Operator Checklist)

Target:

- Real machines (Windows/Linux, slower hardware if applicable)

Scope:

- Release-binary smoke test, secure DB flows, TUI rendering, watch mode behavior, write gating

Runbook:

- `modules/loxdb_cli/docs/HARDWARE_VALIDATION.md`

Artifacts:

- operator log + terminal captures (screenshots if needed)

Pass criteria:

- checklist “Exit Criteria” satisfied per device

## Reporting Requirements (GitHub-ready)

For each completed row:

- record **exact date**, **device/OS**, **toolchain versions**, and **git revision**
- include raw logs plus a short summary table
- explicitly state `PASS/FAIL/N/A`

