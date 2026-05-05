# `loxdb_ftl_basic` (basic NAND FTL)

- Public header: `modules/loxdb_ftl_basic/include/loxdb_ftl_basic.h`
- CMake target: `loxdb::ftl_basic` (optional)
- Category: Storage backend (optional)

## Purpose

Provides a basic translation layer so raw NAND can be used as a byte-addressable `lox_storage_t` for `loxdb`.

## API

- `lox_storage_t *loxdb_ftl_basic_create(const loxdb_ftl_basic_cfg_t *cfg);`
- `void loxdb_ftl_basic_destroy(lox_storage_t *ftl);`

