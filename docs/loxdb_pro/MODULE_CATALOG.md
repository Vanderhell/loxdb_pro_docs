# Module catalog (loxdb_pro)

All PRO functionality is organized as independent C modules under `modules/` and exported as CMake targets (`loxdb::...`).

| Module | CMake target | Public header | What it provides |
|---|---|---|---|
| `loxdb_api` | `loxdb::api` | `loxdb_api.h` | Unified facade over plain `loxdb` and `loxdb_secure` for KV/TS/REL, runtime mode, stats, and safety validation. |
| `loxdb_secure` | `loxdb::secure` | `loxdb_secure.h` | Transparent encryption + integrity wrapper for KV/TS/REL with key rotation and DB-id binding. |
| `loxdb_safety` | `loxdb::safety` | `loxdb_safety.h` | Runtime safety validation + safety profile identifier used by release evidence bundles. |
| `loxdb_codec` | `loxdb::codec` | `loxdb_codec.h` | Compression helpers for embedded payloads (RLE/VARINT/DELTA/LZSS/HUFF) + unified `mc_encode/mc_decode`. |
| `loxdb_metrics` | `loxdb::metrics` | `loxdb_metrics.h` | Metrics snapshot collection and export (binary fixed-size frame or CBOR). |
| `loxdb_metrics_microtimer` | `loxdb::metrics_microtimer` | `loxdb_metrics_microtimer.h` | Adapter that wires metrics scheduler to a microtimer callback without changing the scheduler API. |
| `loxdb_logging` | `loxdb::logging` | `loxdb_logging.h` | Audit/trace event emission with a hook and basic counters (latency, totals). |
| `loxdb_monitor` | `loxdb::monitor` | `loxdb_monitor.h` | Health evaluation policy (warn/restart/reboot) + optional self-heal/watchdog hooks (extended API). |
| `loxdb_admission` | `loxdb::admission` | `loxdb_admission.h` | Quotas, rate-limit, and predicted usage decisions with backpressure percentage. |
| `loxdb_policy` | `loxdb::policy` | `loxdb_policy.h` | Central policy gate for KV/TS/REL operations (size limits, auth/encryption requirements, hooks coverage). |
| `loxdb_retention` | `loxdb::retention` | `loxdb_retention.h` | Periodic retention layer (KV purge + TS/REL cleanup/trim callbacks) with scheduling + stats. |
| `loxdb_scheduler` | `loxdb::scheduler` | `loxdb_scheduler.h` | Lightweight periodic job scheduler with retries and stats. |
| `loxdb_backup` | `loxdb::backup` | `loxdb_backup.h` | Export/import logical backups via callbacks, with manifest + optional HMAC. |
| `loxdb_schema_migrate` | `loxdb::schema_migrate` | `loxdb_schema_migrate.h` | Backup-first schema migration planning and execution with migration history. |
| `loxdb_replication` | `loxdb::replication` | `loxdb_replication.h` | Transport-agnostic replication core: WAL offsets, ACK/resume, follower replay, persistable state. |
| `loxdb_net` | `loxdb::net` | `loxdb_net.h` | Lightweight framing protocol helpers (CRC16, CBOR flag) and sync/replication handshake frame builders/parsers. |
| `loxdb_alerting` | `loxdb::alerting` | `loxdb_alerting.h` | Alert state machine driven by metrics snapshots (OK/PENDING/FIRING/RESOLVED) with cooldown. |
| `loxdb_ota` | `loxdb::ota` | `loxdb_ota.h` | OTA compatibility checks, update planning, runtime state machine, and rollback hooks. |
| `loxdb_cli` | `loxdb::cli` | `cli_*.h` | Terminal tool and reusable CLI helpers for operating on plain/secure DB files (TUI + export/import). |
| `loxdb_adapter_sdcard` (optional) | `loxdb::adapter_sdcard` | `loxdb_adapter_sdcard.h` | FatFS file-as-storage backend (`lox_storage_t`) for SD card integration. |
| `loxdb_ftl_basic` (optional) | `loxdb::ftl_basic` | `loxdb_ftl_basic.h` | Basic NAND FTL shim to expose a raw NAND as a byte-addressable `lox_storage_t`. |

See per-module API reference in `api/`.

