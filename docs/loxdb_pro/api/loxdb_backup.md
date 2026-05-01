# `loxdb_backup` (export/import)

- Public header: `modules/loxdb_backup/include/loxdb_backup.h`
- CMake target: `loxdb::backup`

## Purpose

Logical export/import for KV/TS/REL via user-provided IO callbacks.

Features:

- manifest (`loxdb_backup_manifest_t`) includes counts, CRC32, and optional HMAC-SHA256
- export can be streaming-chunked (`loxdb_backup_export_stream`)

## API

- `loxdb_backup_err_t loxdb_backup_export(const loxdb_backup_export_cfg_t *cfg, loxdb_backup_manifest_t *out_manifest);`
- `loxdb_backup_err_t loxdb_backup_export_stream(const loxdb_backup_export_cfg_t *cfg, const loxdb_backup_stream_cfg_t *stream, loxdb_backup_manifest_t *out_manifest);`
- `loxdb_backup_err_t loxdb_backup_import(const loxdb_backup_import_cfg_t *cfg, loxdb_backup_manifest_t *out_manifest);`
- `const char *loxdb_backup_err_name(loxdb_backup_err_t err);`

