# ITDrafts Agent Guide

## Repository Overview

Sphinx documentation site for IT articles (Russian language). Published at https://pages.ksomov.ru

## Build Commands

```bash
# Install dependencies first
pip install -r requirements.txt

# Build HTML
make html

# Build PDF (requires full LaTeX distribution with Cyrillic support)
make latexpdf
```

Output goes to `build/` directory (gitignored).

## Content Structure

- `source/` — all RST content files
- `source/index.rst` — main page with toctree
- Content organized by topic: `linux/`, `bsd/`, `mikrotik/`, `homeassistant/`, etc.
- `source/_static/` — static assets (CSS, images, favicons)
- All content is Russian RST; Sphinx `language = 'en'` in conf.py is misleading

## Deployment

- Push to `master` triggers GitHub Actions workflow
- Deploys to `gh-pages` branch via `peaceiris/actions-gh-pages`
- Also SSHs to host server to run `update.sh`
- CNAME: `pages.ksomov.ru`

## Docker Images

Two custom Dockerfiles for local builds:
- `Dockerfile-html` — HTML generator
- `Dockerfile-ru-latexpdf` — PDF generator with Cyrillic LaTeX support

Build: `docker build -f Dockerfile-html -t sphinx-html:latest .`

Both Dockerfiles pin Sphinx to the same version as `requirements.txt` (9.0.4).

## Key Configuration

- `source/conf.py` — Sphinx config
- Theme: `sphinx_rtd_theme`
- Branch: `master` (not `main`)
- GitHub user for "Edit on GitHub" links: `jeffscrum`, repo: `itdrafts`
