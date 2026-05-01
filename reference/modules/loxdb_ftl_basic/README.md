# loxdb_ftl_basic

## Purpose

`loxdb_ftl_basic` is a basic NAND translation layer over `lox_storage_t`.

## Main API

- `loxdb_ftl_basic_create(...)`
- `loxdb_ftl_basic_destroy(...)`

## Features

- byte-write emulation (`write_size = 1`) via block-level read-modify-erase-write
- factory bad block scan (optional OOB marker callback)
- runtime bad block marking and relocation into reserve area
- relocation on both erase-failure and program(write)-failure paths
- writes spanning page/block boundary are handled by per-block RMW flow

## Limits

- no ECC
- no wear-leveling
- no runtime bad-block persistence across reboot
- no garbage collection
