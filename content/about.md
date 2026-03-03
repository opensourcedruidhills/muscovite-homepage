---
title: "About Muscovite"
date: 2026-03-02
draft: false
ShowToc: true
---

## What Is Muscovite?

Muscovite is a **model-driven Domain-Driven Design framework** that transforms declarative domain models into a complete, production-ready full-stack application. You write a `.ddd` domain model once — Muscovite generates **9 targets** from it.

**Dual-licensed**: AGPL-3.0 for open-source use, commercial license for proprietary applications.

## The Problem

Enterprise software modernization is expensive because the same domain knowledge gets manually re-implemented at every layer: database schema, backend services, API contracts, frontend forms, build systems, documentation. When the model changes, every layer must be updated by hand — a process that is slow, error-prone, and prohibitively expensive for legacy systems with hundreds of tables.

## The Solution

Muscovite eliminates this duplication. A single `.ddd` file describes your domain model using DDD tactical patterns (entities, aggregates, value objects, sagas, events, specifications). The generator produces:

| Target | Output |
|--------|--------|
| **PostgreSQL** | Sqitch migrations, DOMAINs, constraints, seed data |
| **C++ DBA** | Entity structs, repositories, services (pqxx) |
| **gRPC** | Protocol Buffer definitions + service implementations |
| **Qt Widgets** | Desktop master-detail views |
| **Qt Quick/QML** | Modern ViewModel-based UI |
| **CMake/Conan** | Complete build system |
| **Documentation** | AsciiDoc system manuals |
| **OpenSearch** | Index mappings |
| **Migration** | Brownfield extraction + transformation |

## Who Built It

**Johannes Lochmann** — Staff Engineer with 20+ years in enterprise systems, specializing in DDD, CQRS, and legacy software modernization. Muscovite was built entirely through AI-assisted development: zero lines of code written by hand, approximately €300 in API credits, 1,000+ commits over 4 months.

## The Thesis

> Generated typed scaffolding makes the remaining business logic reliably AI-completable. The generator doesn't solve the 80% problem — it makes the remaining 20% trivially promptable.

Traditional MDD generates 80% and struggles with 20%. Muscovite generates 80% that *constrains* AI agents into producing correct code for the remaining 20% — achieving a 100% AI-assisted workflow.

## Contact

- **Source**: [github.com/opensourcedruidhills/muscovite](https://github.com/opensourcedruidhills/muscovite)
- **Commercial licensing**: [muscovite-licensing@johanneslochmann.net](mailto:muscovite-licensing@johanneslochmann.net)
- **Copyright**: © 2025-2026 Johannes Lochmann
