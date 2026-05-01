# loxdb_safety Manual

## Scope

This manual defines integration assumptions and release gates for certified
`loxdb_pro` safety deliveries.

## Required Build Profile

- deterministic profile id: `loxdb_safety.v1`
- WAL enabled
- minimum RAM profile: 32 KB
- TS policy support: log retain available and validated

## Integrator Obligations

- run core selfcheck before production image creation
- keep power-loss and corruption replay tests in qualification run
- keep WCET report synchronized with exact release tag
- do not change compile-time profile macros without change-impact review

## Forbidden Configurations

- WAL disabled in safety delivery
- ad-hoc compile definitions that bypass profile checks
- untracked dependency updates in release branch

## Release Evidence Minimum Set

- CMake cache snapshot
- test results (`ctest --output-on-failure`)
- traceability CSVs (requirements and tests)
- signed package hash and SBOM
