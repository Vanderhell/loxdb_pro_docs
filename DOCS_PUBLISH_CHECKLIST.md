# DOCS Publish Checklist

1. No absolute local paths (e.g., `C:\Users\...`).
2. No private emails or personal identities in operational context.
3. No secrets, tokens, internal URLs, or internal hostnames.
4. No references to unpublished internal source paths.
5. Links resolve within this repo or public repos only.
6. Version and release references are consistent.
7. New docs are placed under the correct section (`docs/spec/roadmap/releases`).
8. CHANGELOG/release notes updated when behavior changed.

## Suggested pre-push check (PowerShell)

```powershell
Get-ChildItem -Recurse -File -Filter *.md |
  Select-String -Pattern 'C:\\Users\\|@|internal\\|localhost|127\.0\.0\.1'
```