# loxdb_pro documentation

This folder contains the **official product documentation** for `loxdb_pro` (the commercial integration layer around the `loxdb` core).

Start here:

- `MODULE_CATALOG.md` – what modules exist and what they do
- `API_SURFACE.md` – which headers are intended as public API and how to consume them
- `BUILD_AND_INTEGRATION.md` – CMake integration, presets, and recommended build profiles
- `api/_INDEX.md` – per-module API reference index
- `COMMERCIAL_OVERVIEW.md` – product overview (positioning + integration flow)
- `LICENSING.md` – packaging/licensing notes

## Recommended entrypoint (most users)

If you want one stable API that can switch between **plain** and **secure** modes without changing your app code, use:

- module `loxdb_api` (`modules/loxdb_api/include/loxdb_api.h`)

## Scope

This documentation describes the **`loxdb_pro` modules under `modules/`** and their public headers in `modules/*/include/`.
The underlying `loxdb` core API is intentionally treated as a dependency and documented separately.
