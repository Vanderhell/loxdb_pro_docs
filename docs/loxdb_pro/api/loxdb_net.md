# `loxdb_net` (framing + sync helpers)

- Public header: `modules/loxdb_net/include/loxdb_net.h`
- CMake target: `loxdb::net`

## Purpose

Provides a minimal wire framing format and helpers for building/parsing frames, including replication sync flows.

Key constants:

- `LOXDB_NET_MAGIC` (`LXDP`)
- `LOXDB_NET_VERSION` (currently `1`)
- flags: `LOXDB_NET_FLAG_CBOR`, `LOXDB_NET_FLAG_ACK`, `LOXDB_NET_FLAG_SYNC`

## API (selected)

- `void loxdb_net_init_header(loxdb_net_frame_header_t *header, uint16_t stream_id, uint32_t payload_len, uint8_t flags);`
- `int loxdb_net_validate_header(const loxdb_net_frame_header_t *header);`
- `uint16_t loxdb_net_crc16(const uint8_t *data, uint32_t len);`
- `int loxdb_net_encode_frame(uint16_t stream_id, uint8_t flags, const uint8_t *payload, uint32_t payload_len, uint8_t *out_frame, size_t out_frame_cap, size_t *out_frame_len);`
- `int loxdb_net_decode_frame(const uint8_t *frame, size_t frame_len, loxdb_net_frame_header_t *out_header, const uint8_t **out_payload, uint32_t *out_payload_len);`
- sync/replication helpers:
  - `int loxdb_net_build_sync_hello_frame(uint16_t stream_id, uint8_t role, uint16_t node_id, uint32_t capabilities, uint8_t *out_frame, size_t out_frame_cap, size_t *out_frame_len);`
  - `int loxdb_net_parse_sync_hello_frame(const uint8_t *frame, size_t frame_len, uint16_t expected_stream_id, loxdb_net_sync_hello_t *out_hello);`
  - `int loxdb_net_build_replication_handshake_frame(uint16_t stream_id, uint64_t resume_from, uint8_t requires_snapshot, uint8_t *out_frame, size_t out_frame_cap, size_t *out_frame_len);`
  - `int loxdb_net_parse_replication_handshake_frame(const uint8_t *frame, size_t frame_len, uint16_t expected_stream_id, loxdb_net_replication_handshake_t *out_handshake);`
  - `int loxdb_net_build_replication_sync_flow_frames(uint16_t stream_id, uint8_t role, uint16_t node_id, uint32_t capabilities, uint64_t resume_from, uint8_t requires_snapshot, uint8_t *out_frames, size_t out_frames_cap, size_t *out_frames_len, size_t *out_first_frame_len);`
  - `int loxdb_net_parse_replication_sync_flow_frames(const uint8_t *frames, size_t frames_len, uint16_t expected_stream_id, loxdb_net_replication_sync_flow_t *out_flow);`
