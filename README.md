# Muscovite Homepage

The official homepage for [Muscovite](https://github.com/opensourcedruidhills/muscovite) — a declarative DDD code generation framework.

Built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme. Deployed to GitHub Pages.

## Local Development

```bash
# Clone with submodules (for the theme)
git clone --recurse-submodules https://github.com/opensourcedruidhills/muscovite-homepage.git
cd muscovite-homepage

# Local preview with drafts
hugo server --buildDrafts

# Build for production
hugo --gc --minify
```

## Deployment

Pushes to `main` trigger automatic deployment via GitHub Actions → GitHub Pages.

## License

Content © 2025-2026 Johannes Lochmann. Site theme (PaperMod) is MIT-licensed.

