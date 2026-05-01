# LoxDB PRO Validation Matrix

Latest known internal validation run: **2026-04-29**

The public repository does not include validation logs. Logs can be provided during commercial evaluation.

---

## Validation variants

| Build variant | Configure | Build | Validation targets | Result |
|---|---:|---:|---:|---:|
| GCC Debug | PASS | PASS | 36/36 | PASS |
| GCC Release | PASS | PASS | 36/36 | PASS |
| Clang Debug | PASS | PASS | 36/36 | PASS |
| Clang Release | PASS | PASS | 36/36 | PASS |
| Clang ASAN + UBSAN | PASS | PASS | 36/36 | PASS |

---

## Validation script scope

The Linux validation script executes these defined steps:

1. GCC Debug: configure, build, PRO regex tests.
2. GCC Release: configure, build, PRO regex tests.
3. Clang Debug: configure, build, PRO regex tests.
4. Clang Release: configure, build, PRO regex tests.
5. Clang Sanitizers: configure, build, PRO regex tests with ASAN + UBSAN.

Note: the internal script label text may show `[1/8]` through `[5/8]`, but the currently defined validation flow contains five active steps.

---

## PRO regex test gate

The shared PRO validation gate contains **36 test targets / test executables**. This is not the total number of individual test cases. Each target can contain multiple assertions, data scenarios, negative-path checks, integration flows, and blackbox cases:

1. `loxdb_codec_test_rle`
2. `loxdb_codec_test_varint`
3. `loxdb_codec_test_delta`
4. `loxdb_codec_test_lzss`
5. `loxdb_codec_test_huff`
6. `loxdb_codec_test_integration`
7. `secure_kv`
8. `secure_ts`
9. `secure_rel`
10. `secure_integration`
11. `safety`
12. `api`
13. `admission`
14. `monitor`
15. `net`
16. `ota`
17. `logging`
18. `metrics`
19. `backup`
20. `retention`
21. `scheduler`
22. `alerting`
23. `schema_migrate`
24. `policy`
25. `replication`
26. `sdcard`
27. `sdcard_softsim`
28. `ftl_basic`
29. `pro_full_stack`
30. `export`
31. `import`
32. `hex`
33. `cli_integration`
34. `cli_tui`
35. `cli_blackbox_plain`
36. `cli_blackbox_secure`

---

## Test count interpretation

The `36/36` number means that all common PRO gate targets passed in the selected build variant.

It should be read as:

- 36 validation targets passed,
- not "only 36 tests exist",
- not a full count of module-local test cases,
- not a full count of assertions.

The implementation has broader internal coverage inside those targets, especially around codec behavior, secure reopen/tamper handling, API integration, persistence/recovery, storage simulation, and CLI blackbox flows.

---

## Test classes

### Unit tests

Module-local behavior:

- codec,
- policy,
- admission,
- monitor,
- metrics,
- scheduler,
- alerting,
- backup,
- retention,
- network framing.

### Integration tests

Cross-module flows:

- secure lifecycle,
- API gateway behavior,
- full stack path,
- CLI blackbox path,
- import/export path.

### Persistence and recovery tests

Storage and durability-related flows:

- storage adapter soft simulation,
- power-loss style scenario,
- secure reopen/reinit,
- tamper scenario.

### Tooling tests

Operational tooling:

- import correctness,
- export correctness,
- TUI behavior,
- secure/plain CLI end-to-end modes.
