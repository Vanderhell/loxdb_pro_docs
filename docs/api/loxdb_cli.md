# `loxdb_cli` (terminal tool + helpers)

- Public headers: `modules/loxdb_cli/include/cli_*.h`
- CMake target: `loxdb::cli`
- Category: Tooling

## Purpose

`loxdb_cli` is a terminal tool/library for operating on plain and secure `loxdb` files:

- non-interactive commands for KV/TS/REL/info/export/import/hex/watch
- interactive TUI mode
- secure open with key / keyfile
- export JSON/CSV and import JSON

## Public helper entrypoints

- `int cli_run_command(cli_app_t *app, const char *command, int argc, char **argv);`
- `void cli_print_help(FILE *f);`
- `bool cli_app_load_file(cli_app_t *app, const char *path, uint32_t ram_kb, FILE *err);`
- `int cli_app_enable_secure(cli_app_t *app, const uint8_t *master_key, size_t key_len, FILE *err);`
- `int cli_export_app(cli_app_t *app, FILE *out, cli_format_t format, uint32_t engine_mask);`
- `int cli_import_file(cli_app_t *app, const char *path, uint32_t engine_mask, bool dry_run, bool overwrite, bool assume_yes, FILE *err);`

