# `loxdb_codec` (compression library)

- Public header: `modules/loxdb_codec/include/loxdb_codec.h`
- CMake target: `loxdb::codec`
- Category: Data codec

## Purpose

`loxdb_codec` provides small, allocation-free compression primitives designed for embedded payloads.

Algorithms (compile-time toggles):

- RLE (`LOXDB_CODEC_ENABLE_RLE`)
- VARINT (`LOXDB_CODEC_ENABLE_VARINT`)
- DELTA (`LOXDB_CODEC_ENABLE_DELTA`)
- LZSS (`LOXDB_CODEC_ENABLE_LZSS`)
- HUFFMAN (`LOXDB_CODEC_ENABLE_HUFF`)

## Unified API

- `mc_err_t mc_encode(mc_alg_t alg, mc_slice_t src, mc_buf_t *dst, void *ctx);`
- `mc_err_t mc_decode(mc_alg_t alg, mc_slice_t src, mc_buf_t *dst, void *ctx);`
- `size_t mc_max_encoded_size(mc_alg_t alg, size_t input_len);`
- `const char *mc_alg_name(mc_alg_t alg);`

Each algorithm also exposes its direct encode/decode entrypoints when enabled (see header).

