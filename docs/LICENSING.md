# Licensing notes (commercial distributions)

`loxdb_pro` is described in this repository as a **commercial** integration layer.

Because licensing differs between:

- the `loxdb` core dependency,
- individual embedded modules, and
- commercial distributions,

do **not** treat this documentation folder as the source of truth for legal terms.

## What to ship with a distribution

When producing a distribution, include:

- the license text that applies to that distribution
- third-party notices (if any)
- a version identifier (tag/commit) that matches the shipped headers

## Code headers

Some modules may embed an SPDX line in headers (example: `modules/loxdb_codec/include/loxdb_codec.h`).
If you rely on SPDX headers for compliance, verify them against the shipped package and your intended commercial terms.
