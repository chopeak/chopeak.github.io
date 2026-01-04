# Copilot Instructions for chopeak.github.io

Quick context for an AI code assistant to be immediately productive with this repository.

## Big picture
- This repo is a Jekyll site using the **Type on Strap** theme (see `type-on-strap.gemspec` and `_config.yml`). The site uses a combination of:
  - Ruby/Jekyll for content and site generation
  - Node/Gulp for asset pipeline (JS/CSS/image processing) located in `assets/`
- Key directories:
  - `_posts/`, `pages/`, `_layouts/`, `_includes/` — Jekyll content & templates
  - `assets/` — JS, CSS, images and `gulpfile.js` for build tasks
  - `_data/` — authors, language and social configuration used by templates

## Dev / build workflows (explicit examples)
- Install Node deps for asset tasks:
  - cd `assets/` && `npm install`
  - Build assets: `npx gulp` or `cd assets && npx gulp default` (generates `assets/js/main.min.js`, minifies CSS, optimizes images)
- Common gulp tasks (examples from `assets/gulpfile.js`):
  - `gulp post "My New Post"` — create a new post file in `_posts/` named `YYYY-MM-DD-My-New-Post.md`
  - `gulp js` — concatenate & minify JS -> `assets/js/main.min.js`
  - `gulp css` — minify CSS
  - `gulp img` / `gulp webp` / `gulp thumbnails` — image optimizations and thumbnails
  - `gulp sharp_img` — alternative image pipeline using `sharp` if `imagemin` fails
- Jekyll build / serve (standard):
  - `bundle install` (Gemfile exists)
  - Typical commands: `bundle exec jekyll build` or `bundle exec jekyll serve --livereload` (containerized `Dockerfile` exposes port 4000)
  - Note: repo uses `remote_theme: sylhare/Type-on-Strap` in `_config.yml` which may alter local dev if theme is used as a gem or remote theme.

## Project-specific conventions & patterns
- Post creation: gulp `post` uses `new Date().toLocaleDateString('en-CA')` so filenames use `YYYY-MM-DD-title.md` (en-CA date format)
- Front matter and features:
  - Posts use `layout: post` and optional fields: `feature-img`, `thumbnail`, `tags` (see existing files in `_posts/`)
  - Config flags in `_config.yml` enable features: `katex`, `mermaid`, `comments.{disqus,cusdis,utterances}`
- Assets structure:
  - JS partials in `assets/js/partials/` concatenated to `assets/js/main.min.js` (referenced in `_includes/default/head.liquid`)
  - SCSS in `_sass/` and `assets/css/main.scss` — variables in `_sass/base/_variables.scss`
- Social/comment integration:
  - Templates for Disqus / Cusdis / Utterances exist in `_includes/social/`. Configuration is toggled via `_config.yml` (`comments:` section) and `_data/social.yml`.

## Integration & packaging notes
- The repo includes `type-on-strap.gemspec` and a `Gemfile` which indicates publishing/support as a theme gem; be careful when changing theme internals that are meant to be public API for consumers.
- `assets/Dockerfile` (older developer workflow): installs the theme and exposes Jekyll on port 4000 — useful for reproducing the environment.

## What to change or be cautious about
- Keep Jekyll-specific config in `_config.yml` (site URL, pagination, comment IDs). Changes here affect site generation and external integrations.
- When editing images, prefer using the gulp image tasks to avoid accidentally committing large or unoptimized files.
- Avoid breaking the public API declared by `type-on-strap.gemspec` if you intend to keep gem compatibility.

## Useful files to reference when making changes
- Site configuration: `_config.yml`
- Templates & layout: `_layouts/`, `_includes/`
- Asset tasks & examples: `assets/gulpfile.js`, `assets/package.json`
- Theme packaging: `type-on-strap.gemspec`, `Gemfile`
- Data-driven content: `_data/authors.yml`, `_data/social.yml`, `_data/language.yml`

---
If you'd like, I can draft a short PR with this file or expand sections (e.g., add exact commands for local development, CI, or a small checklist for making a release). Please tell me which areas you want more detail or examples for.