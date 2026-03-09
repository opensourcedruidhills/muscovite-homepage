# Muscovite Homepage — Agent Operations Guide

Operational how-to for AI agents working on the Muscovite marketing website.
For product context, see `.github/copilot-instructions.md`.

**CRITICAL: This is a PUBLIC repository. Never commit secrets, internal paths, or sensitive data.**

---

## Quick Start

### Prerequisites

- Hugo extended (v0.128+): `snap install hugo` or `brew install hugo`
- Git (for submodule theme checkout)

### Local Development

```bash
# Clone with submodules (PaperMod theme)
git clone --recurse-submodules https://github.com/opensourcedruidhills/muscovite-homepage.git
cd muscovite-homepage

# If submodules weren't cloned:
git submodule update --init --recursive

# Run local dev server
hugo server -D
# Site available at http://localhost:1313/

# Build for production
hugo --minify
# Output in public/
```

### Adding Content

```bash
# New blog post
hugo new blog/my-new-post.md

# New page
hugo new my-page.md
```

### Front Matter Template (Blog Posts)

```markdown
---
title: "Post Title"
date: 2026-03-09
summary: "Brief summary for listing pages"
tags: ["ddd", "code-generation", "architecture"]
author: "Johannes Lochmann"
ShowToc: true
TocOpen: false
---
```

### Front Matter Template (Pages)

```markdown
---
title: "Page Title"
layout: "single"
ShowToc: true
---
```

---

## Deployment

GitHub Actions deploys to GitHub Pages automatically on push to `main`.

Workflow: `.github/workflows/` — uses `peaceiris/actions-hugo` + `peaceiris/actions-gh-pages`.

Custom domain: `muscovite.johanneslochmann.net` (configured via `CNAME` file).

---

## Content Architecture

| Page           | File                   | Purpose                                      |
|----------------|------------------------|----------------------------------------------|
| Home           | `hugo.toml` homeInfo   | Hero section with tagline and key metrics    |
| About          | `content/about.md`     | Product description, vision, team            |
| Features       | `content/features.md`  | Capability matrix and feature descriptions   |
| Architecture   | `content/architecture.md` | Technical deep-dive (three-layer arch)    |
| Documentation  | `content/docs.md`      | Getting started, links to main repo docs     |
| License        | `content/license.md`   | Dual licensing (AGPL + Commercial) explained |
| Blog           | `content/blog/*.md`    | Technical blog posts                         |

---

## Theme Customization

PaperMod is used as-is via git submodule. To customize:

1. **Override templates**: Create files in `layouts/` matching the theme structure
2. **Custom CSS**: Add to `assets/css/extended/` (PaperMod convention)
3. **Custom JS**: Add to `assets/js/`
4. **Do NOT modify** files inside `themes/PaperMod/` — they're a submodule

---

## Quality Checklist

Before committing any content:

- [ ] No sensitive data (paths, keys, usernames, IPs)
- [ ] All metrics are accurate and verifiable
- [ ] Links to external resources are valid
- [ ] Hugo builds without errors (`hugo --minify`)
- [ ] Content renders correctly (`hugo server -D`)
- [ ] Spelling and grammar checked
- [ ] SPDX header not needed for content markdown (only for code files)
