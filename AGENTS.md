# ITDrafts Agent Guide

## Repository Overview

Sphinx documentation site for IT articles (Russian language). Published at https://pages.ksomov.ru

## Build Commands

```bash
# Install dependencies first
pip install -r requirements.txt

# Build HTML
make html

# Build PDF (requires LaTeX)
make latexpdf
```

Output goes to `build/` directory.

## Content Structure

- `source/` — all RST content files
- `source/index.rst` — main page with toctree
- Content organized by topic: `linux/`, `bsd/`, `mikrotik/`, `homeassistant/`, etc.
- `source/_static/` — static assets (CSS, images, favicons)

## Deployment

- Push to `master` triggers GitHub Actions workflow
- Deploys to `gh-pages` branch
- Also SSHs to host server to run `update.sh`
- CNAME: `pages.ksomov.ru`

## Docker Images

Two custom Dockerfiles for local builds:
- `Dockerfile-html` — HTML generator
- `Dockerfile-ru-latexpdf` — PDF generator with Cyrillic support

Build: `docker build -f Dockerfile-html -t sphinx-html:latest .`

## Key Configuration

- `source/conf.py` — Sphinx config
- Theme: `sphinx_rtd_theme`
- Language: Russian (`language = 'en'` in conf.py is misleading — content is Russian)
- Branch: `master` (not `main`)
