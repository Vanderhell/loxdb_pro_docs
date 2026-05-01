# loxdb_net

## Purpose

`loxdb_net` provides lightweight framing helpers for MCU-to-MCU transport.

## Main API

- `loxdb_net_encode_frame(...)`
- `loxdb_net_decode_frame(...)`
- `loxdb_net_build_sync_hello(...)`
- `loxdb_net_parse_sync_hello(...)`
- `loxdb_net_build_sync_hello_frame(...)`
- `loxdb_net_parse_sync_hello_frame(...)`
- `loxdb_net_build_replication_handshake(...)`
- `loxdb_net_parse_replication_handshake(...)`
- `loxdb_net_build_replication_handshake_frame(...)`
- `loxdb_net_parse_replication_handshake_frame(...)`
- `loxdb_net_build_replication_sync_flow_frames(...)`
- `loxdb_net_parse_replication_sync_flow_frames(...)`

## Features

- compact fixed header
- CRC16 integrity check
- CBOR payload flag handling
- sync hello payload + frame helpers
- replication handshake payload + frame helpers (`resume_from`, `requires_snapshot`)
- combined two-frame helper for full replication sync flow
