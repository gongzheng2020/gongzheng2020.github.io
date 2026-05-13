# Project Guidelines

A Jekyll blog theme ("Not Pure Poole") built on [Poole](https://github.com/poole/poole) + [Pure.css](https://purecss.io/). See [README.md](README.md) for full feature list.

## Build and Serve

```bash
bundle install                          # Install dependencies
bundle exec jekyll serve --draft --trace  # Dev server at http://localhost:4000
scripts/serve                           # Shortcut for the above
scripts/draft "my-post-title"           # Create a new draft
scripts/publish "my-post-title"         # Publish a draft
```

The site builds to `_site/`. Ruby 2.7+ and Bundler required.

## Architecture

### Layout Hierarchy

```
_layouts/default.html     → Root template (head, sidebar, content wrapper)
  ├── home.html           → Homepage with paginated posts
  ├── post.html           → Individual post (tags, related posts, Disqus)
  ├── page.html           → Static pages (About, etc.)
  └── archive-*.html      → Archive views (dates/tags/categories)
```

Three-column responsive layout: left sidebar (fixed, 100vh) → center content → right sidebar (optional TOC). Pure.css grid classes control the responsive breakpoints.

### Key Includes

| Include | Purpose |
|---------|---------|
| `head.html` | Meta tags, stylesheets, plugin output |
| `sidebar-left.html` | Avatar, navigation, social links |
| `sidebar-right.html` | TOC (shown when `page.toc: true`) |
| `post-tags.html` | Tag links for a post |
| `home-header.html` | Archive menu on homepage |

## Conventions

### Creating a Post

Use `jekyll-compose` (`scripts/draft` then `scripts/publish`) or manually create `_posts/YYYY-MM-DD-slug.md`:

```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD HH:MM +0800
last_modified_at: YYYY-MM-DD HH:MM:SS +0800
tags: [tag1, tag2]
toc: true        # Enable right-sidebar TOC
math: true       # Enable MathJax rendering
comments: false  # Disable Disqus for this post (default: true)
---
```

Permalink style is `pretty` — posts appear at `/YYYY/MM/DD/slug/`.

### Navigation & Data

- **Navigation links**: Edit `_data/navigation.yml` (array of `{title, url}`). Current: Project, Blog, About.
- **Project page**: `project.md` — lists posts with `categories: [project]`. To add a project post, create a post in `_posts/` with `categories: [project]` in frontmatter.
- **Social links**: Edit `_data/social.yml` (array of `{title, url, icon}` — icons are Font Awesome classes)
- **Archive types**: Edit `_data/archive.yml` (determines which archive pages appear in the home header menu)

### Configuration

Site settings in `_config.yml`. Notable options:
- `paginate: 5` — posts per page on homepage
- `cover_image` / `cover_bg_color` / `cover_color` — sidebar appearance
- `google_analytics` / `disqus` — third-party integrations (only active in production, i.e. `JEKYLL_ENV=production`)

Per-post frontmatter overrides: `cover_bg_color`, `cover_color`, `cover_image`.

### Styles

SASS entry point: `assets/styles.scss` → imports all partials from `_sass/`. Dark mode via `@media (prefers-color-scheme: dark)` in `_syntax-dark.scss` and `_variables.scss`. CSS custom properties defined in `_sass/_variables.scss`.

## Important Constraints

- **Jekyll ~3.9** (not 4.x) — some newer Jekyll features and plugins are unavailable
- **GitHub Pages compatible** — avoid plugins not in the [Pages whitelist](https://pages.github.com/versions/)
- **`_site/` is the build output** — never edit files here; they are regenerated on each build
- **Drafts live in `_drafts/`** — created via `scripts/draft`, not manually in `_posts/`
