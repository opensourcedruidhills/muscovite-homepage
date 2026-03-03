---
title: "Architecture"
date: 2026-03-02
draft: false
ShowToc: true
---

## The Compiler Analogy

Muscovite works like a compiler: source language (`.ddd`) → intermediate representation (typed IR with 70+ struct types) → multiple code backends. The DSL is the "source code"; PostgreSQL, C++, gRPC, and Qt are "target architectures."

## Three-Layer Hierarchy

```
Layer 0 — dslgen/        (Racket)    DSL parser, IR model, 9 code generators
Layer 1 — framework/     (C++23)     Reusable base classes, no domain logic
Layer 2 — generated/     (output)    100% generated, never manually edited
```

**The iron rule**: changes flow downward only. If generated output is wrong, fix the generator (Layer 0) or the framework (Layer 1) — never edit Layer 2.

## The DSL Pipeline

```
.ddd file → Lexer → Parser → Typed IR (70 structs) → Validation → Generators → Output
                                   ↑
                           PEG Grammar (261 rules)
                           authoritative spec
```

### PEG Grammar (Authoritative)

The formal grammar in `dslgen/grammar/muscovite.peg` defines all accepted syntax. Every language change starts here. The grammar conformance test enforces parity between grammar keywords and lexer keywords.

### Typed Intermediate Representation

The parser produces typed Racket structs — not an AST, but a **domain-specific IR**:

- `bounded-context` — top-level module with aggregates, entities, events, sagas
- `entity` — identity object with fields, invariants, commands, relationships
- `aggregate` — consistency boundary with root entity and children
- `field` — typed attribute with UUID identity for rename tracking
- `type-spec` — type info: name, nullable?, collection?, precision, scale, SRID
- `relationship` — explicit FK declaration (direction, target, cascade, embedded)
- `value-object` — immutable type with storage mode (inline/owned/shared)
- `saga` — long-running process with steps, compensation, and correlation
- `state-machine` — entity lifecycle graph with states and transitions

Every IR element carries a UUID for semantic diff tracking.

### Strict Type Registry

Unknown types cause generation to **fail** — no silent fallbacks. The type registry maps every DSL type to all target representations:

| DSL Type | PostgreSQL | C++ | gRPC Proto | Qt Widget |
|----------|------------|-----|------------|-----------|
| UUID | `UUID` | `boost::uuids::uuid` | `muscovite.common.UUID` | `QLineEdit` |
| String | `TEXT` | `std::string` | `string` | `QLineEdit` |
| Integer | `INTEGER` | `std::int32_t` | `int32` | `QSpinBox` |
| DateTime | `TIMESTAMPTZ` | `muscovite::dba::Timestamp` | `muscovite.common.Timestamp` | `QDateTimeEdit` |
| Email | `TEXT` (DOMAIN) | `std::string` | `muscovite.common.Email` | `QLineEdit` |
| Point | `GEOGRAPHY(POINT, 4326)` | `std::string` (WKT) | `muscovite.common.Point` | `QLineEdit` |

## The C++ Framework (Layer 1)

Generated code inherits from production-grade base classes:

### ConnectionPool
RAII connection management with health checks, idle timeout, validate-on-borrow, and pool exhaustion handling. Maps PostgreSQL SQLSTATE codes to typed `DbError` variants (`SerializationFailure` = retryable, `UniqueViolation` = not retryable).

### UnitOfWork
Implements the outbox pattern — data and domain events commit atomically in the same transaction. RAII rollback-on-destruction ensures consistency.

### Repository\<T\> (CRTP)
Type-safe repository base using `std::expected<T, Error>` for error handling. Separate `CommandRepository` (pqxx::work) and `QueryRepository` (pqxx::nontransaction) enforce CQRS at the type level.

### Domain Services
All aggregate access through `CommandService` / `QueryService`. No stored procedures. Single transaction per command. Event emission inline within the same transaction.

```
gRPC Request → CommandService → CommandRepository → PostgreSQL
                     └── emit event to domain_events table
```

## Target Independence

The IR supports arbitrary retargeting. Current targets are an implementation choice, not a limitation:

| Current Target | Possible Alternatives |
|----------------|----------------------|
| PostgreSQL | Oracle, SQL Server, CockroachDB |
| C++ / pqxx | .NET / EF Core, Java / Spring, Rust / SQLx |
| gRPC / Protobuf | REST / OpenAPI, GraphQL |
| Qt Widgets / QML | React, Blazor, SwiftUI |

Adding a target = writing a generator (~1K–5K lines of Racket).

## Semantic Diff Migrations

Every DSL element has a UUID. The migration engine compares two model versions by UUID identity:

| Change | Detection | Generated SQL |
|--------|-----------|---------------|
| Add field | New UUID | `ALTER TABLE ADD COLUMN` |
| Rename field | Same UUID, different name | `ALTER TABLE RENAME COLUMN` |
| Change type | Same UUID, different type | `ALTER COLUMN TYPE` |
| Add entity | New UUID | `CREATE TABLE` |
| Remove entity | Missing UUID | `DROP TABLE` (with confirmation) |

No ambiguity. No guesswork. The UUID is the stable identity across all schema versions.

## Project Metrics (v1.41.0)

| Metric | Value |
|--------|-------|
| DSL model lines | 11,582 |
| Generated output lines | 55,755 |
| DSL amplification | ~5× |
| Generator targets | 9 |
| Racket source | 111,309 lines |
| C++ framework | 34,615 lines |
| Architecture Decision Records | 167 |
| Git commits | 1,048 |
