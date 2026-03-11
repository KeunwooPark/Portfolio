# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```shell
# Install dependencies
bundle install

# Local development server (live reload at http://localhost:4000)
bundle exec jekyll serve

# Build only
bundle exec jekyll build

# If gem errors occur
bundle update && bundle exec jekyll build
```

## Architecture

This is a **Jekyll static site** deployed to `kwpark.io` via GitHub Pages.

### Layouts (in `_layouts/`)
- `default.html` — base HTML shell. Handles all `<head>` content: title, canonical URL, meta description, Open Graph tags, JSON-LD structured data, and GA4. All other layouts extend this.
- `blog_post.html` — wraps blog posts. Do NOT put `<title>` here; it's emitted by `default.html`.
- `post.html` — wraps research/publication project pages.

### Content (in `_posts/`)
All content lives here regardless of type. The `categories` frontmatter field determines where a post appears:
- `[blog]` → shown on `/blog/`
- `[conference]`, `[journal]`, `[poster_demo]` → shown on `/publications/`
- `[misc]` → shown in "Other Projects" on `/publications/`

### Key frontmatter fields
**Blog posts** (`layout: blog_post`): `title`, `permalink`, `publish_date`, `excerpt`, `categories`

**Publication/project posts** (`layout: post`): `title`, `permalink`, `thumbnail`, `publish_date`, `venue`, `authors`, `award`, `video_embed_link`, `categories`

`excerpt` is used for meta description and Open Graph description. If a page has no `excerpt`, add a `description` field to frontmatter instead (used as fallback in `default.html`).

### SEO
- `sitemap.xml` and `robots.txt` are in the project root and served as-is (Jekyll processes the sitemap's frontmatter).
- `search.json` generates a JSON index of all blog posts for client-side search on `/blog/`.
- After deploying, submit the sitemap at `https://kwpark.io/sitemap.xml` to Google Search Console.

### Adding a new post
1. Create a file in `_posts/` following the naming convention `YYYY-MM-DD-slug.md`.
2. Set required frontmatter (see above for fields by type).
3. Place images in `assets/img/{project-name}/`. Use `thumbnail` frontmatter for the OG image.
4. Update `last_updated` in `index.html`.
