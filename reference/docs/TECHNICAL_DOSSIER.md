# LoxDB PRO Technical Dossier

Version: public README extraction draft  
Date: 2026-04-29  
Repository: `loxdb_pro`  
Scope: public technical documentation for the PRO layer above `loxdb` core

---

## 1. System overview

`loxdb_pro` is a commercial modular extension layer above the `loxdb` core.

It focuses on:

- security-at-rest through encryption and integrity checks,
- safety evidence and runtime validation,
- operational capability through backup, retention, scheduler, alerting, metrics, and logging,
- policy, admission, and monitoring layer,
- replication and wire framing,
- OTA and schema migration flow,
- optional storage backends such as SD card and basic FTL,
- production CLI for import/export and diagnostics.

Architecturally, LoxDB PRO is composed of C libraries, mainly static targets, plus test executables and CLI tooling. The public integration surface is exposed through CMake aliases under the `loxdb::` namespace.

---

## 2. Module topology

### 2.1 PRO modules

1. `loxdb_codec`
2. `loxdb_secure`
3. `loxdb_safety`
4. `loxdb_api`
5. `loxdb_admission`
6. `loxdb_logging`
7. `loxdb_metrics`
8. `loxdb_backup`
9. `loxdb_retention`
10. `loxdb_scheduler`
11. `loxdb_alerting`
12. `loxdb_schema_migrate`
13. `loxdb_policy`
14. `loxdb_replication`
15. `loxdb_monitor`
16. `loxdb_net`
17. `loxdb_ota`
18. `loxdb_adapter_sdcard` optional
19. `loxdb_ftl_basic` optional
20. `loxdb_cli` tooling/executable module

### 2.2 CMake alias surface

```text
loxdb::codec
loxdb::secure
loxdb::safety
loxdb::api
loxdb::admission
loxdb::logging
loxdb::metrics
loxdb::backup
loxdb::retention
loxdb::scheduler
loxdb::alerting
loxdb::schema_migrate
loxdb::policy
loxdb::replication
loxdb::monitor
loxdb::net
loxdb::ota
loxdb::adapter_sdcard
loxdb::ftl_basic
loxdb::core
```

---

## 3. Public API map

Detailed function signatures are kept in the commercial headers. This public overview documents the API domains and module responsibilities.

| Module | Public header | Primary API surface | Responsibility |
|---|---|---|---|
| `loxdb_codec` | `modules/loxdb_codec/include/loxdb_codec.h` | `mc_encode`, `mc_decode`, `mc_*` family | Compression/decompression of payloads using RLE, VARINT, DELTA, LZSS, and HUFF families. |
| `loxdb_secure` | `modules/loxdb_secure/include/loxdb_secure.h` | secure init/deinit, secure KV/TS/REL wrappers, DB identity handling | Transparent data encryption and tamper detection. |
| `loxdb_safety` | `modules/loxdb_safety/include/loxdb_safety.h`, `loxdb_safety_profile.h` | runtime validation and extended validation output | Runtime safety checks and evidence output for certification-oriented workflows. |
| `loxdb_api` | `modules/loxdb_api/include/loxdb_api.h` | lifecycle, KV, TS, REL, stats, selfcheck, runtime operations | Unified API gateway over plain and secure modes. |
| `loxdb_admission` | `modules/loxdb_admission/include/loxdb_admission.h` | admission evaluation and status helpers | Admission, quota, and backpressure gate. |
| `loxdb_logging` | `modules/loxdb_logging/include/loxdb_logging.h` | audit/trace emit, stats, hook | Audit trail and diagnostic events. |
| `loxdb_metrics` | `modules/loxdb_metrics/include/loxdb_metrics.h`, `loxdb_metrics_microtimer.h` | collect/export/scheduler hooks | Runtime metrics and export formats. |
| `loxdb_backup` | `modules/loxdb_backup/include/loxdb_backup.h` | export/import/stream and error naming | Logical backup/restore layer. |
| `loxdb_retention` | `modules/loxdb_retention/include/loxdb_retention.h` | init, policy, tick, run once, stats | Data lifecycle enforcement. |
| `loxdb_scheduler` | `modules/loxdb_scheduler/include/loxdb_scheduler.h` | init, add job, tick, state | Embedded-friendly periodic job orchestration. |
| `loxdb_alerting` | `modules/loxdb_alerting/include/loxdb_alerting.h` | create, evaluate, destroy, state | Alert state machine over metrics. |
| `loxdb_schema_migrate` | `modules/loxdb_schema_migrate/include/loxdb_schema_migrate.h` | plan, backup, apply, history | Controlled schema migrations. |
| `loxdb_policy` | `modules/loxdb_policy/include/loxdb_policy.h` | init, eval, before operation, after operation | Policy enforcement point over operation flow. |
| `loxdb_replication` | `modules/loxdb_replication/include/loxdb_replication.h` | leader append/ack, follower apply, resume planning | Transport-agnostic replication state logic. |
| `loxdb_monitor` | `modules/loxdb_monitor/include/loxdb_monitor.h` | evaluate and action mapping | Health evaluation and recoverability decisions. |
| `loxdb_net` | `modules/loxdb_net/include/loxdb_net.h` | frame encode/decode, sync/replication handshake builders | Lightweight wire framing. |
| `loxdb_ota` | `modules/loxdb_ota/include/loxdb_ota.h` | plan and runtime chunk/finalize flow | OTA update orchestration and rollback-safe flow boundaries. |
| `loxdb_adapter_sdcard` | `modules/loxdb_adapter_sdcard/include/loxdb_adapter_sdcard.h` | create/destroy adapter | FatFS-backed `lox_storage_t` adapter. |
| `loxdb_ftl_basic` | `modules/loxdb_ftl_basic/include/loxdb_ftl_basic.h` | create/destroy wrapper | Basic FTL abstraction for flash-like backend. |
| `loxdb_cli` | `modules/loxdb_cli/include/cli_*.h` | command runner, import/export, TUI, watch paths | Operational CLI toolchain. |

---

## 4. API domains in `loxdb_api`

`loxdb_api` is the primary product integration surface.

### 4.1 Lifecycle

- open and close runtime,
- retrieve runtime information,
- run safety validation passthrough.

### 4.2 KV

- put,
- get,
- delete,
- exists,
- key iteration,
- full clear.

### 4.3 Time-series

- register stream,
- insert sample,
- retrieve latest data,
- query-style access,
- clear stream.

### 4.4 Relational

- create table,
- get table,
- insert row,
- find row,
- iterate rows,
- count rows,
- clear table.

### 4.5 Telemetry and diagnostics

- generic stats,
- domain stats for DB/KV/TS/REL,
- capacity and pressure reports,
- self-check.

---

## 5. Test strategy

### 5.1 PRO validation suite

The common PRO validation gate contains **36 test targets / test executables**. This number is the gate count, not the total count of individual test cases or assertions. Each target may include multiple module-level checks, integration scenarios, persistence/recovery cases, negative-path tests, and CLI blackbox flows:

1. `loxdb_codec_test_rle`
2. `loxdb_codec_test_varint`
3. `loxdb_codec_test_delta`
4. `loxdb_codec_test_lzss`
5. `loxdb_codec_test_huff`
6. `loxdb_codec_test_integration`
7. `secure_kv`
8. `secure_ts`
9. `secure_rel`
10. `secure_integration`
11. `safety`
12. `api`
13. `admission`
14. `monitor`
15. `net`
16. `ota`
17. `logging`
18. `metrics`
19. `backup`
20. `retention`
21. `scheduler`
22. `alerting`
23. `schema_migrate`
24. `policy`
25. `replication`
26. `sdcard`
27. `sdcard_softsim`
28. `ftl_basic`
29. `pro_full_stack`
30. `export`
31. `import`
32. `hex`
33. `cli_integration`
34. `cli_tui`
35. `cli_blackbox_plain`
36. `cli_blackbox_secure`

### 5.2 Linux validation matrix

The validation script performs:

1. GCC Debug: configure, build, PRO regex tests.
2. GCC Release: configure, build, PRO regex tests.
3. Clang Debug: configure, build, PRO regex tests.
4. Clang Release: configure, build, PRO regex tests.
5. Clang Sanitizers: configure, build, PRO regex tests with ASAN + UBSAN.

### 5.3 Latest known status

Latest known internal validation run: **2026-04-29**

- GCC Debug: PASS, 36/36 validation targets.
- GCC Release: PASS, 36/36 validation targets.
- Clang Debug: PASS, 36/36 validation targets.
- Clang Release: PASS, 36/36 validation targets.
- Clang ASAN + UBSAN: PASS, 36/36 validation targets.

The validation target count intentionally represents the shared gate surface. It does not collapse or replace the larger set of internal checks executed inside each module target.

---

## 6. Build and feature toggles

Key build flags:

1. `LOXDB_PRO_ENABLE_TESTS=ON|OFF`
2. `LOXDB_PRO_BUILD_SDCARD_ADAPTER=ON|OFF`
3. `LOXDB_PRO_BUILD_FTL_BASIC=ON|OFF`
4. `LOXDB_PRO_ENABLE_SDCARD_REAL_FATFS_TESTS=ON|OFF`
5. `LOXDB_FATFS_INCLUDE_DIR=<path>`

Compiler/toolchain variants:

1. MSVC / clang-cl on Windows.
2. GCC / Clang on Linux or WSL.
3. Clang sanitizer mode with ASAN + UBSAN.

---

## 7. Storage and platform notes

### 7.1 SD card adapter

`loxdb_adapter_sdcard` provides a FatFS-backed storage adapter.

It is tested through a soft simulation profile and can optionally be tested against a real `ff.h` profile.

### 7.2 Basic FTL

`loxdb_ftl_basic` provides a basic FTL wrapper separated from business logic modules.

### 7.3 Platform portability

The CLI and test harness contain Windows/POSIX branches for process spawning, time handling, and file descriptor redirection.

---

## 8. Security and safety posture

### 8.1 Security

The secure layer provides:

- security-at-rest semantics,
- integrity/tamper detection path,
- key/database identity sensitive reopen scenarios.

### 8.2 Safety

The safety layer provides:

- runtime validation output,
- evidence-friendly reporting.

### 8.3 Policy, admission, and monitor

The governance layer provides:

- runtime governance,
- pre-operation and post-operation hooks,
- allow/deny/alert style decision support,
- recoverability decision support.

---

## 9. CLI and operational surface

`loxdb_cli` covers:

1. import/export flows,
2. hex utilities,
3. integration command runner,
4. TUI interaction mode,
5. blackbox secure/plain flows.

The CLI is a separate executable target and part of the PRO validation gate.

---

## 10. Source traceability

This public document is derived from the internal technical specification and these internal source categories:

1. `CMakeLists.txt` for targets, tests, and aliases.
2. `modules/*/include/*.h` for public API surface.
3. `modules/*/tests/test_*.c` for test coverage map.
4. `scripts/run_pro_validation_linux.sh` for validation orchestration.
5. Internal `README.md`, `MODULES_TECH_SUMMARY.md`, and `PRO_VALIDATION_MATRIX.md` drafts.

The files above are not part of this public overview repository unless explicitly published in a commercial or evaluation package.
