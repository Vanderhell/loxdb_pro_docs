# `loxdb_adapter_sdcard` (FatFS storage backend)

- Public header: `modules/loxdb_adapter_sdcard/include/loxdb_adapter_sdcard.h`
- CMake target: `loxdb::adapter_sdcard` (optional)
- Category: Storage backend (optional)

## Purpose

Exposes a FatFS file as `lox_storage_t` so `loxdb` can use it as a storage backend.

## API

- `lox_storage_t *loxdb_adapter_sdcard_create(const loxdb_adapter_sdcard_cfg_t *cfg);`
- `void loxdb_adapter_sdcard_destroy(lox_storage_t *storage);`

