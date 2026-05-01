# sdcard_fatfs example

Reference integration for running `loxdb` on SD card file storage through FatFS.

## Prerequisites

- `loxdb_pro` installed with `LOXDB_PRO_BUILD_SDCARD_ADAPTER=ON`
- FatFS and SD low-level driver integrated in your BSP
- mounted volume (for example `"0:"`)

## Build

```bash
cmake -S . -B build \
  -DFATFS_INCLUDE_DIR=/path/to/fatfs/include \
  -DFATFS_LIBRARY=/path/to/libfatfs.a
cmake --build build
```

## Runtime flow

1. Mount volume: `f_mount(&fs, "0:", 1)`
2. Create storage adapter:
   - file path: `"0:/loxdb.bin"`
   - cache size: `4096` (or `0` to disable cache)
3. Pass adapter as `lox_cfg_t.storage` into `lox_init`
4. Use database normally, then `lox_flush` / `lox_deinit`
5. Destroy adapter with `loxdb_adapter_sdcard_destroy`

