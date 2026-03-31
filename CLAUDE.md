# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Site Overview

Personal academic website for Eyal Weiss (postdoctoral scholar, AI & Robotics, Technion). Built with Hugo and the PaperMod theme, deployed to GitHub Pages via GitHub Actions.

## Build & Development

Hugo is not installed locally. To install and run:

```bash
# Install Hugo extended (match CI version: 0.147.6)
wget -O /tmp/hugo.deb https://github.com/gohugoio/hugo/releases/download/v0.147.6/hugo_extended_0.147.6_linux-amd64.deb
sudo dpkg -i /tmp/hugo.deb

# Local dev server
hugo server -D    # -D includes draft posts

# Production build
hugo --minify
```

Deployment is automatic: pushing to `main` triggers the GitHub Actions workflow (`.github/workflows/deploy.yml`) which builds and deploys to GitHub Pages.

## Architecture

- **Hugo config**: `hugo.yaml` — site metadata, navigation menu, home page content (in `homeInfoParams`), social links, theme settings
- **Theme**: PaperMod, included as a git submodule under `themes/PaperMod`
- **Content**: `content/` — each section is a directory with `index.md` (single pages) or dated `.md` files (blog posts)
- **Layout overrides**: `layouts/` overrides PaperMod defaults
  - `layouts/partials/home_info.html` — custom home page with centered profile photo
  - `layouts/_default/_markup/render-link.html` — external links (`http*`) auto-open in new tab
- **Static assets**: `static/` — profile photo, CV PDF, blog images, SVGs
- **Generated output**: `public/` — built site (do not edit manually)

## Content Pages

Navigation menu order (defined in `hugo.yaml`): News, Publications, Talks, Software, Open Access, Contact, Blog, Grad Toolbox, Teaching.

Blog posts use the naming convention `content/blog/YYYY-MM-DD-slug.md`.

## Key Conventions

- Goldmark renderer is set to `unsafe: true` — raw HTML is allowed in markdown content
- The CV is served from `static/cv.pdf` (lowercase) — the home page links to `/cv.pdf`
- Hugo outputs include HTML, RSS, and JSON (for search)
