---
title: "I Built a 160,000-Line Software Modernization Framework for €300"
date: 2026-03-02
draft: false
tags: ["ai", "code-generation", "ddd", "legacy-modernization"]
description: "How one architect used AI agents to build Muscovite — a DDD code generation framework — entirely through prompting, in 4 months for the cost of a nice dinner."
ShowToc: true
---

*Zero lines of code written by hand. Every file — Racket, C++, SQL, Protobuf, QML, CMake, documentation — was prompted.*

---

In November 2025, I started an experiment. I had been carrying ideas about legacy software modernization for roughly 20 years — partial implementations, architectural sketches, patterns I'd validated across dozens of enterprise engagements. With AI coding agents becoming genuinely capable, I decided to test a thesis: **could I build the framework I'd always wanted, entirely through prompting?**

Six weeks and approximately €300 in API credits later, I had Muscovite: a domain-driven code generation framework at version 1.41, with 1,048 git commits, 111,000 lines of Racket generators, a 34,600-line C++ framework, 170 Architecture Decision Records, and a 26-bounded-context geosciences domain model derived from USGS best practices.

## What Muscovite Does

Muscovite takes a declarative domain model written in a custom DSL and generates a complete, deployable application stack. Here is what a domain model looks like — from the geosciences project, not a tutorial example:

```
CONTEXT Volcanology @uuid "f763d73f-..." {

    VALUE_OBJECT EruptionParameters @uuid "54d02d65-..." (
        vei                   INTEGER,
        column_height_km      DECIMAL,
        volume_km3            DECIMAL,
        duration_days         INTEGER,
        magma_composition     TEXT,
        explosivity           TEXT
    )
    CHECK vei_valid: vei IS NULL OR (vei >= 0 AND vei <= 8)
    DOC "Volcanic Explosivity Index and eruption characteristics";
}
```

From this, Muscovite generates: PostgreSQL schemas with constraints and seed data, C++ entity structs with strong types, gRPC service definitions with pagination, Qt desktop views, CMake build targets, and Sqitch deploy/revert/verify scripts.

**The amplification ratio**: 11,582 lines of `.ddd` model generate 55,755 lines of deployable code. That's roughly 5× — but the real metric is what it costs to *produce* the `.ddd` model, which with AI assistance is measured in hours, not months.

## The Architecture That Makes It Work

Most model-driven development efforts fail because the generated code becomes a prison you can't escape. Muscovite addresses this with a three-layer architecture borrowed from how compilers work:

| Layer | Purpose | Rule |
|-------|---------|------|
| **Layer 0** — Racket generators | DSL parser, IR model, code generation | The brain — all templates live here |
| **Layer 1** — C++ framework | Reusable base classes | No domain logic — infrastructure only |
| **Layer 2** — Generated output | PostgreSQL, C++, gRPC, Qt, CMake | **Never manually edited** |

Changes flow downward only. If the output is wrong, fix the generator or the model. Regeneration is always safe — you can't lose manual customizations because there are none.

### Grammar-First Governance

The PEG grammar is the authoritative specification. Every syntax change starts with a grammar edit, followed by parser implementation, followed by a conformance test. When AI agents write code over hundreds of commits, they drift. The grammar acts as a formal contract that prevents this drift from compounding across all nine targets.

### UUID-Based Semantic Migrations

Every element carries a UUID. When you rename `family_name` to `last_name`, the UUID stays. The semantic diff engine compares model versions by UUID identity — producing typed migration operations, not ambiguous string-based diffs.

## Evidence at Scale: 26 Bounded Contexts of Geoscience

To test whether the abstraction holds under a real, complex domain, I pointed AI agents at a USGS best practices PDF and had them produce a comprehensive geosciences domain model: 26 bounded contexts, 182 entities, 130 value objects, 4 sagas, 39 seed data files with real scientific data.

The domain knowledge is accurate. The VEI scale constraint (0–8) is correct. The Barrovian metamorphic sequence follows the standard mineralogical progression. The ICS chronostratigraphic color codes match the 2024 chart. The seed data for chemical elements includes verified atomic masses and electron configurations.

This was produced by AI agents extracting structured knowledge from a PDF, guided by my architectural judgment about bounded context boundaries.

## Brownfield Extraction: Legacy Database to DDD Model

I pointed an AI agent at a live Gitea PostgreSQL instance and told it: discover the database, find the simplest bounded context, and produce a Muscovite project.

The result: a 60-line `.ddd` file with one entity, one aggregate, and one `MAPPING` block declaring six field-level transformations. From this, the generator produces the target schema, a migration extractor binary, a transformer binary, and NATS JetStream infrastructure — the strangler-fig pattern, generated from a declarative model.

## The "Promptable 20%" Thesis

Every MDD system hits the same wall: you generate 80% (CRUD, schema, basic UI), but real applications need business logic. Muscovite's answer: the generated code creates an *unusually constrained context* for AI agents writing the remaining logic. Typed inputs, typed repository methods, typed event emission, typed error returns — that's dramatically less ambiguous than writing business logic from scratch.

The generator doesn't solve the 80% problem. It makes the remaining 20% reliably AI-completable.

## What This Means for Economics

Muscovite compresses a modernization engagement that traditionally takes 6–18 months into weeks with one architect and AI agents. The €300 figure is real. The geosciences project alone would represent months of domain modeling workshops in the traditional model.

Three findings deserve wider attention:

1. **Grammar-first governance works** — formal specs prevent AI drift across hundreds of commits
2. **Documentation is context persistence** — 170 ADRs function as external long-term memory for stateless agents
3. **AI-generated code accumulates technical debt at the same rate as human-generated code, just faster**

---

Muscovite is dual-licensed: **AGPL-3.0** for open-source use, **commercial license** for proprietary applications.

[Source code on GitHub](https://github.com/opensourcedruidhills/muscovite) · [Commercial licensing](mailto:muscovite-licensing@johanneslochmann.net)

*Johannes Lochmann is a Staff Engineer specializing in legacy software modernization but doesn't shy away from greenfield projects, either. He has been building enterprise systems for 20+ years and prompting AI agents to build them for approximately 6 months.*
