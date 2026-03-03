---
title: "Features"
date: 2026-03-02
draft: false
ShowToc: true
---

## Domain-Driven Design Native

Muscovite's DSL captures strategic and tactical DDD patterns natively:

- **Bounded Contexts** — modular schema + service + UI boundaries
- **Entities** with type-safe fields, constraints, documentation
- **Aggregates** — consistency boundaries with root entities
- **Value Objects** — inline (mixin), owned (child table), shared (dimension)
- **Strong Types** — PostgreSQL DOMAINs with validation (Email, Phone, PostalCode)
- **Lookup Tables** — extensible enums with seed data (no CHECK constraints)
- **Domain Events** — unconditional event emission on every write
- **Sourced Events** — typed event store, no JSON, dual-write strategy
- **Sagas** — long-running processes with compensation and correlation
- **State Machines** — entity lifecycle graphs with generated transition logic
- **Specifications** — named query predicates
- **Projections** — CQRS read models (VIEWs, MATERIALIZED VIEWs)
- **Context Maps** — inter-context relationships (Shared Kernel, Customer-Supplier, ACL)

## 9 Generator Targets

One `.ddd` model → nine fully-generated targets:

```
.ddd Domain Model
    ├── PostgreSQL / Sqitch migrations
    ├── C++ DBA layer (entities, repositories, services)
    ├── gRPC / Protobuf (typed APIs with pagination)
    ├── Qt Widgets (desktop master-detail)
    ├── Qt Quick / QML (ViewModel pattern)
    ├── CMake / Conan (complete build system)
    ├── Documentation (AsciiDoc system manuals)
    ├── OpenSearch (index mappings)
    └── Migration (brownfield extraction + transformation)
```

## Grammar-First Language Evolution

The PEG grammar is the authoritative specification. Every syntax change starts with a grammar edit, ensuring the DSL remains a formal language with deterministic parsing. Grammar conformance tests enforce 98%+ keyword parity between grammar and parser.

## UUID-Based Semantic Migrations

Every DSL element carries a UUID. When you rename a field, the UUID stays — the semantic diff engine produces typed migration operations (`rename-field`, not `delete + create`). No more ambiguous ALTER TABLE guesswork.

```
ENTITY Patient @uuid "abc-..." IDENTIFIED_BY id (
    family_name TEXT NOT_NULL @uuid "def-..."  -- rename this...
)
-- Change to last_name, keep the UUID → generates ALTER TABLE RENAME COLUMN
```

## Three-Layer Architecture

| Layer | Location | Purpose |
|-------|----------|---------|
| **Layer 0** | `dslgen/` (Racket) | DSL parser, IR model, all generators |
| **Layer 1** | `framework/` (C++23) | Reusable base classes — no domain logic |
| **Layer 2** | `examples/*/generated/` | 100% generated output — never manually edited |

Changes flow top-down only. If generated code is wrong, fix the generator — not the output.

## Production-Grade C++ Framework

Layer 1 provides infrastructure that generated code inherits:

- **ConnectionPool** — RAII, health checks, idle timeout, validate-on-borrow
- **UnitOfWork** — atomic domain event persistence (outbox pattern)
- **Repository\<T\>** — CRTP base with `std::expected` error handling
- **Optimistic Locking** — version columns with `optimistic_lock_exception`
- **Circuit Breakers** — exponential/linear/fixed backoff with jitter
- **Qt Validation** — structured error codes (VAL001–VAL999), severity levels

## One Shared Library Per Context (OSLPC)

Each bounded context compiles into its own shared library. Changing one context triggers recompilation of only that context and its direct dependents. For large domain models (26+ contexts), this reduces incremental build times from minutes to seconds.

## Climate-Efficient Computation

C++ generates native machine code with no JIT warmup, no GC pauses, and minimal memory pressure. When a domain model generates 300,000+ lines across 26 contexts, the efficiency gains compound: fewer servers, lower energy per transaction, smaller carbon footprint.

## Brownfield Legacy Extraction

Point an AI agent at a live legacy database → it discovers the schema, identifies bounded context boundaries, and produces a `.ddd` model with `MAPPING` blocks declaring field-level transformations. The generator produces extraction/transformation binaries with NATS JetStream transport and dead-letter queues — the strangler-fig pattern, generated from a declarative model.

## AI-Native Development Workflow

170+ Architecture Decision Records serve as persistent context for AI agents. Auto-generated documentation (CONTEXT.md, GROUND-TRUTH.md, DSL.md) ensures every agent session inherits accumulated architectural intent. The project was built 100% through AI-assisted prompting as a proof of concept.
