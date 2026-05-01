# LoxDB PRO Module Map

This document describes the public module topology of LoxDB PRO.

LoxDB PRO is built as a set of C modules, primarily static library targets, integrated through CMake aliases under the `loxdb::` namespace.

---

## Module table

| Module | Header | CMake alias | Primary responsibility |
|---|---|---|---|
| `loxdb_codec` | `modules/loxdb_codec/include/loxdb_codec.h` | `loxdb::codec` | Compression/decompression of payloads using RLE, VARINT, DELTA, LZSS, and HUFF families. |
| `loxdb_secure` | `modules/loxdb_secure/include/loxdb_secure.h` | `loxdb::secure` | Transparent secure KV/TS/REL paths, encrypted storage context, integrity verification, and tamper detection. |
| `loxdb_safety` | `modules/loxdb_safety/include/loxdb_safety.h` | `loxdb::safety` | Runtime safety checks and evidence-friendly validation output. |
| `loxdb_api` | `modules/loxdb_api/include/loxdb_api.h` | `loxdb::api` | Unified API gateway over plain and secure modes. |
| `loxdb_admission` | `modules/loxdb_admission/include/loxdb_admission.h` | `loxdb::admission` | Admission, quota, and backpressure decision point. |
| `loxdb_logging` | `modules/loxdb_logging/include/loxdb_logging.h` | `loxdb::logging` | Audit and diagnostic event emission. |
| `loxdb_metrics` | `modules/loxdb_metrics/include/loxdb_metrics.h` | `loxdb::metrics` | Runtime metric collection, export, and scheduler hooks. |
| `loxdb_backup` | `modules/loxdb_backup/include/loxdb_backup.h` | `loxdb::backup` | Logical backup, restore, and stream operations. |
| `loxdb_retention` | `modules/loxdb_retention/include/loxdb_retention.h` | `loxdb::retention` | Data lifecycle and retention policy enforcement. |
| `loxdb_scheduler` | `modules/loxdb_scheduler/include/loxdb_scheduler.h` | `loxdb::scheduler` | Embedded-friendly periodic job orchestration. |
| `loxdb_alerting` | `modules/loxdb_alerting/include/loxdb_alerting.h` | `loxdb::alerting` | Alert state machine driven by metric evaluation. |
| `loxdb_schema_migrate` | `modules/loxdb_schema_migrate/include/loxdb_schema_migrate.h` | `loxdb::schema_migrate` | Schema migration planning, backup, apply, and history tracking. |
| `loxdb_policy` | `modules/loxdb_policy/include/loxdb_policy.h` | `loxdb::policy` | Policy enforcement before and after operation flow. |
| `loxdb_replication` | `modules/loxdb_replication/include/loxdb_replication.h` | `loxdb::replication` | Transport-agnostic replication state logic. |
| `loxdb_monitor` | `modules/loxdb_monitor/include/loxdb_monitor.h` | `loxdb::monitor` | Health evaluation and recoverability/action mapping. |
| `loxdb_net` | `modules/loxdb_net/include/loxdb_net.h` | `loxdb::net` | Lightweight frame encode/decode and handshake builders. |
| `loxdb_ota` | `modules/loxdb_ota/include/loxdb_ota.h` | `loxdb::ota` | OTA planning and runtime chunk/finalize orchestration. |
| `loxdb_adapter_sdcard` | `modules/loxdb_adapter_sdcard/include/loxdb_adapter_sdcard.h` | `loxdb::adapter_sdcard` | Optional FatFS-backed `lox_storage_t` adapter. |
| `loxdb_ftl_basic` | `modules/loxdb_ftl_basic/include/loxdb_ftl_basic.h` | `loxdb::ftl_basic` | Optional basic FTL abstraction for flash-like backends. |
| `loxdb_cli` | `modules/loxdb_cli/include/cli_*.h` | executable/tooling module | Import/export, TUI, hex utilities, and blackbox test flows. |

---

## Core product surface

The main product-level entry point is `loxdb_api`.

It groups access into these domains:

1. lifecycle,
2. KV,
3. time-series,
4. relational tables,
5. telemetry,
6. diagnostics,
7. runtime validation.

---

## Optional modules

The following modules are optional and can be excluded from the build depending on product requirements:

- `loxdb_adapter_sdcard`,
- `loxdb_ftl_basic`,
- selected CLI tooling paths,
- real FatFS test integration.

---

## CMake alias surface

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
