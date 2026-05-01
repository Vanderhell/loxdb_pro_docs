# loxdb_replication

## Purpose

`loxdb_replication` provides a transport-agnostic replication core for:

- stable monotonic WAL offsets on leader
- ACK + resume offset tracking
- idempotent follower replay with gap detection

## Main API

- `loxdb_replication_init(...)`
- `loxdb_replication_leader_append(...)`
- `loxdb_replication_leader_peek_unacked(...)`
- `loxdb_replication_leader_ack(...)`
- `loxdb_replication_follower_apply(...)`
- `loxdb_replication_get_resume_offset(...)`
- `loxdb_replication_export_state(...)`
- `loxdb_replication_import_state(...)`
- `loxdb_replication_plan_resume(...)`

## Notes

- this module intentionally does not own network transport
- persistence is supported via export/import of `next_offset`/`acked_offset`/`applied_offset`
- replay is strictly ordered (`offset == applied_offset + 1`) and duplicates are safely ignored
- resume handshake helper can signal when follower is behind available WAL window (`requires_snapshot=1`)
