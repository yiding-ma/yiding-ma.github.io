# GitHub Copilot — Project Guidance

Purpose: Give succinct, actionable context so an AI coding agent is productive immediately.

Big picture
- Static site built with Jekyll (Ruby) + Liquid templates. Content lives in Markdown; templates in `_layouts/` and `_includes/`.
- Collections drive pages: `_posts/`, `_projects/`, `_news/`, `_books/` and publications in `_bibliography/papers.bib`.
- Site config and feature toggles live in `_config.yml` (URL/baseurl, plugins, scholar settings).

Key workflows (tested in this repo)
- Local dev (recommended): `docker compose pull` then `docker compose up` — open http://localhost:8080.
- Slim image: `docker compose -f docker-compose-slim.yml up`.
- Legacy (native Ruby): `bundle install` then `bundle exec jekyll serve` (http://localhost:4000).
- Build: `bundle exec jekyll build` → output in `_site/`.
- Format: `npx prettier . --write`.
- Purge CSS (optional): `purgecss -c purgecss.config.js` after building.

Project-specific conventions
- Always modify the `main` branch; `gh-pages` is generated and will be overwritten by CI.
- Keep `baseurl:` present in `_config.yml` (leave empty for root sites — do not delete the line).
- CV: priority order is `assets/json/resume.json` (JSON Resume) then `_data/cv.yml` (YAML fallback).
- Blog post filenames: `YYYY-MM-DD-title.md` and include proper frontmatter (`layout`, `title`, `date`).
- Publications: add BibTeX entries to `_bibliography/papers.bib`; custom fields (e.g., `pdf`, `code`, `slides`) are supported.

Integration points & dependencies
- `Gemfile` / `_config.yml` plugins: `jekyll-scholar`, `jekyll-archives-v2`, `jekyll-paginate-v2`, `jekyll-imagemagick`, `jekyll-terser`, etc.
- GitHub Actions: CI and deploy workflows in `.github/workflows/` (look at `deploy.yml`, `broken-links.yml`, `lighthouse-badger.yml`).
- Docker: `Dockerfile`, `docker-compose.yml`, and `docker-compose-slim.yml` provide reproducible dev environments.
- External services: repo stats and trophies configured via `_config.yml` `external_services` (third-party endpoints).

Files to inspect for common tasks
- Global config: `_config.yml`
- Site structure & templates: `_layouts/`, `_includes/`, `_sass/`
- Content: `_pages/`, `_posts/`, `_projects/`, `_news/`, `_bibliography/`
- Agents and guidance: `.github/agents/*.agent.md`, `README.md`, `CUSTOMIZE.md`, `INSTALL.md`.

Agent rules / safe boundaries
- OK to edit content files: Markdown under `_posts/`, `_pages/`, `_projects/`, `_news/`, and `_data/*`.
- Before editing anything in `_config.yml`, `_plugins/`, `.github/workflows/`, or `_plugins/`, ask the repo owner.
- Do not change `gh-pages` branch or generated site artifacts; only change source files in `main`.
- When proposing changes that affect deployment (e.g., Ruby version, Actions, Docker), state the rationale and ask for confirmation.

Examples (copyable)
- Add a post skeleton: create `_posts/2026-01-08-example.md` with frontmatter:

```yaml
---
layout: post
title: Example
date: 2026-01-08 12:00:00
---
```

- Run local Docker dev server:

```bash
docker compose pull
docker compose up
```

Where to look if something breaks
- Jekyll build errors: terminal output from `docker compose up` or `bundle exec jekyll build`.
- Broken-link checks and accessibility reports: `.github/workflows/broken-links.yml` and `axe.yml` outputs in Actions.
- Bibliography/parsing issues: `_bibliography/papers.bib` and `scholar` configuration in `_config.yml`.

If anything is ambiguous, ask one question that precisely identifies the missing info (target file and desired outcome).
