# loxdb_logging

## Purpose

`loxdb_logging` is a structured logging facade for query audit and trace events.

## Main API

- `loxdb_logging_audit_query(...)`
- `loxdb_logging_audit_query_ex(...)`
- `loxdb_logging_trace_span(...)`
- `loxdb_logging_get_stats(...)`, `loxdb_logging_reset_stats(...)`

## Features

- unified event envelope
- latency/result metadata capture
- lightweight in-process stats counters
