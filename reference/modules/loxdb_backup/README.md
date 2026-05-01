# loxdb_backup

## Purpose

`loxdb_backup` provides logical streaming backup/restore for `loxdb`.

## Main API

- export: `loxdb_backup_export(...)`, `loxdb_backup_export_stream(...)`
- import: `loxdb_backup_import(...)`

## Features

- logical records for `KV`, `TS`, `REL`
- compact manifest in header (CBOR)
- payload integrity with `CRC32`
- optional payload authenticity/integrity with `HMAC-SHA256`
- selective import apply flags (`apply_kv`, `apply_ts`, `apply_rel`)

## Limits

- logical backup only (not physical WAL/snapshot cloning)
- no built-in transport, compression, or encryption layer

## Logical vs physical backup

- logical backup (`loxdb_backup`) exports records through public data APIs (`KV`, `TS`, `REL`) and is safer across schema/storage changes
- physical snapshot means raw storage/WAL image copy and requires strict storage-layout compatibility
- for MCU production flow, logical backup + manifest + integrity check is the recommended default
