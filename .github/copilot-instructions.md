# Copilot Instructions for muscovite-homepage

This is the **public marketing website** for [Muscovite](https://github.com/opensourcedruidhills/muscovite), a declarative DDD code generation framework.

**CRITICAL: This is a PUBLIC repository. Never commit secrets, internal paths, machine names, usernames, API keys, or any sensitive information.**

---

## Project Overview

- **Site**: https://muscovite.johanneslochmann.net/
- **Stack**: Hugo static site with PaperMod theme
- **License**: AGPL-3.0-or-later OR LicenseRef-Muscovite-Commercial (dual-licensed)
- **Copyright**: Johannes Lochmann
- **Hosting**: GitHub Pages via `gh-pages` branch (deployed by GitHub Actions)

## Product Positioning

**Tagline**: "Declare your domain model. Generate the entire stack."

**Core Value Proposition**: Muscovite transforms `.ddd` domain models into PostgreSQL schemas, C++ services, gRPC APIs, Qt desktop clients, and CMake build systems — from a single source of truth.

**Key Metrics** (verified from v1.42 generation):
- 12,000 lines of DSL → 1.1 million lines of production code
- ~97× amplification ratio
- 9 generation targets
- Zero manual edits to generated code

**Target Audiences**: Enterprise architects, DDD practitioners, legacy modernization teams, AI-assisted development teams.

## Content Guidelines

1. **Tone**: Professional, technical, honest. Enterprise audience. No hype — let the numbers speak.
2. **Accuracy**: All metrics must be verifiable from actual generation runs. Don't invent stats.
3. **Honesty**: Muscovite is pre-production. Acknowledge this where relevant.
4. **Terminology**: Use DDD terms correctly (Aggregate, Bounded Context, Entity, Value Object, Domain Event, Saga, etc.).
5. **Links**: Always link to the main GitHub repo for technical details.

## Color Palette (from architecture diagrams)

| Hex       | Name              | Usage                        |
|-----------|-------------------|------------------------------|
| `#2d6a4f` | Dark Forest Green | Domain Models, Data          |
| `#264653` | Slate Blue-Gray   | IR & Core Processing         |
| `#e76f51` | Terra Cotta Orange| Generators & Business Logic  |
| `#2a9d8f` | Teal Cyan         | Framework                    |
| `#e9c46a` | Warm Gold         | Generated Output             |

## Hugo Conventions

- **Theme**: PaperMod (submodule at `themes/PaperMod`)
- **Content**: Markdown files in `content/`
- **Config**: `hugo.toml` (TOML format, not YAML)
- **Menu**: Defined in `hugo.toml` under `[menu]`
- **Blog posts**: `content/blog/` with Hugo front matter (title, date, summary, tags)
- **Static assets**: `static/` directory

## File Structure

```
hugo.toml              # Site configuration
content/
  about.md             # Product description
  features.md          # Feature set
  architecture.md      # Technical architecture deep-dive
  docs.md              # Getting started & documentation links
  license.md           # Dual licensing explanation
  blog/                # Blog posts
themes/PaperMod/       # Theme (git submodule — do NOT modify)
static/                # Static assets (images, favicon, etc.)
.github/workflows/     # CI/CD (GitHub Pages deployment)
```

## Security Rules (PUBLIC REPO — NO EXCEPTIONS)

- **NEVER** commit API keys, tokens, passwords, or secrets
- **NEVER** reference internal file paths (e.g., `/home/...`, `C:\Users\...`)
- **NEVER** include machine names, IP addresses, or internal hostnames
- **NEVER** include personal information beyond the public author name and licensing email
- **ALWAYS** review content for accidental sensitive data before committing
- Public contact: `muscovite-licensing@johanneslochmann.net` (this is intentionally public)
- Public GitHub: `github.com/opensourcedruidhills/muscovite` (this is intentionally public)
