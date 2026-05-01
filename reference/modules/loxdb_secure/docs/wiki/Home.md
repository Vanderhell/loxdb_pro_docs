# loxdb_secure Wiki

`loxdb_secure` is a transparent encryption layer for `loxdb`. It protects KV values, TS samples, and REL rows before they are persisted.

## Contents

- [Build and Test](Build-and-Test.md)
- [Architecture Notes](Architecture.md)

This content is mirrored to the GitHub Wiki by `.github/workflows/wiki.yml` after the repository Wiki feature is enabled.

## Quick Summary

- Cipher: AES-128-CBC
- Integrity: HMAC-SHA256, stored as a truncated 16-byte tag
- Record format: `[IV | tag | ciphertext]`
- No dynamic allocation in `src/`
- GitHub Actions CI covers Linux, Windows, and macOS
- Tagged releases publish per-platform zip assets automatically
