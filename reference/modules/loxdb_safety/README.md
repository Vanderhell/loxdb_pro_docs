# loxdb_safety

## Purpose

`loxdb_safety` is a certification-support layer for controlled `loxdb_pro` releases.

## Main API

- runtime validation: `loxdb_safety_validate_runtime_ex(...)`
- profile/metadata helpers from `loxdb_safety.h`

## Evidence workflow

- `loxdb_safety_evidence` target
- `loxdb_safety_bundle` target

## Notes

- Provides evidence artifacts and traceability support.
- Does not claim certification by itself.
