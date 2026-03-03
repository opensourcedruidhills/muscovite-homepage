---
title: "Documentation"
date: 2026-03-02
draft: false
ShowToc: true
---

## Getting Started

### Prerequisites

- **Racket** 8.x+ (for DSL generation)
- **C++23 compiler** (GCC 13+ or Clang 17+)
- **CMake** 3.28+
- **Conan** 2.x (C++ package management)
- **PostgreSQL** 16+ with PostGIS (for database targets)
- **Hugo** (for documentation generation)

### Quick Start

```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/opensourcedruidhills/muscovite.git
cd muscovite

# Byte-compile Racket (avoids 71s cold-start)
cd dslgen && raco make generate.rkt && cd ..

# Generate from the FHIR Health example
cd dslgen
racket generate.rkt --project ../examples/fhir-health/project.mus --full

# Build C++
conan install --profile conan_profiles/debug .
cmake --preset debug
cmake --build --preset debug

# Run tests
ctest --preset debug --output-on-failure
```

## Project DSL (.mus)

Project configuration files define build targets, capabilities, and bounded context references:

```
PROJECT "fhir-health"
  VERSION "1.0.0"
  CAPABILITIES: grpc, qt-widgets, qt-qml, documentation;
  
  CONTEXT Patient FROM "patient.ddd";
  CONTEXT Observation FROM "observation.ddd";
  CONTEXT Encounter FROM "encounter.ddd";
```

## Domain Model DSL (.ddd)

Domain models use DDD tactical patterns:

```
CONTEXT Patient @uuid "a1b2c3d4-..." {

    ENTITY Patient @uuid "..." IDENTIFIED_BY id (
        id              UUID        NOT_NULL,
        mrn             TEXT        NOT_NULL @unique,
        family_name     TEXT        NOT_NULL,
        given_name      TEXT        NOT_NULL,
        birth_date      DATE,
        active          BOOLEAN     NOT_NULL DEFAULT true
    )
    DOC "A person receiving healthcare services";

    AGGREGATE Patient ROOT Patient;

    EVENT PatientRegistered (
        patient_id UUID,
        mrn TEXT,
        registered_at TIMESTAMPTZ
    );
}
```

## Key Resources

| Resource | Location |
|----------|----------|
| **DSL Reference** | `doc/DSL.md` (auto-generated from PEG grammar) |
| **Project DSL Reference** | `doc/PROJECT-DSL.md` |
| **Architecture Context** | `doc/CONTEXT.md` |
| **IR / Type Reference** | `doc/GROUND-TRUTH.md` |
| **Vision Document** | `doc/VISION.md` |
| **PEG Grammar** | `dslgen/grammar/muscovite.peg` |
| **Architecture Decision Records** | `doc/adr/` (167 ADRs) |
| **C++ Pattern Catalog** | `doc/CPP-PATTERNS.md` |
| **PostgreSQL Patterns** | `doc/POSTGRES-PATTERNS.md` |
| **Qt Widget Patterns** | `doc/QT-WIDGETS-PATTERNS.md` |

## Example Projects

### FHIR Health (`examples/fhir-health/`)
Healthcare domain: 9 bounded contexts (Patient, Observation, Encounter, Condition, etc.). Based on HL7 FHIR R4 resource model.

### Geosciences (`examples/geosciences/`)
Geology domain: 26 bounded contexts (Chemistry, Mineralogy, Petrology, Drilling, etc.). 182 entities, 130 value objects, 4 sagas. Derived from USGS best practices.

### Hello World (`examples/hello-world/`)
Minimal example demonstrating the full generation pipeline with a single bounded context.

## Generator Targets

```bash
# Generate specific targets
racket generate.rkt --project ../examples/fhir-health/project.mus --cpp-dba      # C++ DBA only
racket generate.rkt --project ../examples/fhir-health/project.mus --grpc          # gRPC only
racket generate.rkt --project ../examples/fhir-health/project.mus --qt-frontend widgets  # Qt Widgets
racket generate.rkt --project ../examples/fhir-health/project.mus --qt-frontend qml      # Qt QML
racket generate.rkt --project ../examples/fhir-health/project.mus --qt-frontend both     # Both
racket generate.rkt --project ../examples/fhir-health/project.mus --doc           # Documentation
racket generate.rkt --project ../examples/fhir-health/project.mus --full          # All targets
```

## Database Workflow

```bash
# Start PostgreSQL container
cmake --build --preset debug --target docker-start

# Generate + deploy migrations
cmake --build --preset debug --target db-setup-fhir

# Reset (revert + deploy)
cmake --build --preset debug --target db-reset-fhir

# Fresh (drop + create + deploy) — GREENFIELD ONLY
cmake --build --preset debug --target db-fresh-fhir
```
