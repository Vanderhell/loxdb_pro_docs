# loxdb_codec

## Purpose

`loxdb_codec` is a no-heap C99 compression toolkit used in `loxdb_pro` data flows.

## Main API

- generic encode/decode: `mc_encode(...)`, `mc_decode(...)`
- algorithm-level calls: `mc_rle_*`, `mc_varint_*`, `mc_delta_*`, `mc_lzss_*`, `mc_huff_*`

## Algorithms

- RLE
- Varint
- Delta
- LZSS
- Static Huffman

## Notes

- Caller-owned buffers, no hidden allocations.
- Designed for embedded telemetry payloads.
