# `loxdb_replication` (transport-agnostic core)

- Public header: `modules/loxdb_replication/include/loxdb_replication.h`
- CMake target: `loxdb::replication`
- Category: Connectivity

## Purpose

Replication core that does not own any network transport:

- leader assigns monotonic offsets and keeps an in-memory WAL window
- follower applies frames in strict order and detects gaps
- ACK and resume helpers allow reconnect flows
- state can be persisted via export/import

## API

- init:
  - `loxdb_replication_err_t loxdb_replication_init(loxdb_replication_t *r, const loxdb_replication_cfg_t *cfg);`
- leader:
  - `loxdb_replication_err_t loxdb_replication_leader_append(loxdb_replication_t *r, const uint8_t *payload, size_t payload_len, uint64_t *out_offset);`
  - `loxdb_replication_err_t loxdb_replication_leader_peek_unacked(const loxdb_replication_t *r, loxdb_replication_frame_t *out_frame);`
  - `loxdb_replication_err_t loxdb_replication_leader_ack(loxdb_replication_t *r, uint64_t acked_offset);`
- follower:
  - `loxdb_replication_err_t loxdb_replication_follower_apply(loxdb_replication_t *r, uint64_t offset, const uint8_t *payload, size_t payload_len, uint8_t *out_applied);`
- offsets / persistence:
  - `loxdb_replication_err_t loxdb_replication_get_resume_offset(const loxdb_replication_t *r, uint64_t *out_resume_offset);`
  - `loxdb_replication_err_t loxdb_replication_get_applied_offset(const loxdb_replication_t *r, uint64_t *out_applied_offset);`
  - `loxdb_replication_err_t loxdb_replication_export_state(const loxdb_replication_t *r, loxdb_replication_persist_state_t *out_state);`
  - `loxdb_replication_err_t loxdb_replication_import_state(loxdb_replication_t *r, const loxdb_replication_persist_state_t *state);`
  - `loxdb_replication_err_t loxdb_replication_plan_resume(const loxdb_replication_t *r, uint64_t follower_applied_offset, uint64_t *out_resume_offset, uint8_t *out_requires_snapshot);`
- introspection:
  - `loxdb_replication_err_t loxdb_replication_get_pending_count(const loxdb_replication_t *r, uint32_t *out_pending_count);`
  - `loxdb_replication_err_t loxdb_replication_get_stats(const loxdb_replication_t *r, loxdb_replication_stats_t *out_stats);`
- `const char *loxdb_replication_err_name(loxdb_replication_err_t err);`
