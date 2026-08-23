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

### Content

**Blog posts live in `_blog/` as page bundles** — one folder per post, holding the
markdown *and* its images together:

```
_blog/<slug>/
├── index.md      # the post; reference images relatively: ![](photo.jpg)
└── photo.jpg
```

`_blog` is a Jekyll collection (`output: true` in `_config.yml`). The folder name is
the slug and the images are served at `/blog/<slug>/photo.jpg`, i.e. right next to the
post itself, so relative references just work. Set `permalink: /blog/<slug>/` in each
post's front matter — **never** set a `permalink:` on the collection in `_config.yml`,
because a collection-level permalink flattens every bundle's images into `/blog/`.

Ordering is by the `publish_date` front matter field (there is no date prefix in the
folder name), so `/blog/` and `search.json` sort with `sort: 'publish_date' | reverse`.

**Publications and projects live in `_posts/`**, where the `categories` field decides
where they appear:
- `[conference]`, `[journal]`, `[poster_demo]` → shown on `/publications/`
- `[misc]` → shown in "Other Projects" on `/publications/`

Their images stay in `assets/img/publications/{project-name}/`.

### Key frontmatter fields
**Blog posts** (`layout: blog_post`): `title`, `permalink`, `publish_date`, `excerpt` (no `categories` — membership in the `_blog` collection is what makes it a blog post)

**Publication/project posts** (`layout: post`): `title`, `permalink`, `thumbnail`, `publish_date`, `venue`, `authors`, `award`, `video_embed_link`, `categories`

`excerpt` is used for meta description and Open Graph description. If a page has no `excerpt`, add a `description` field to frontmatter instead (used as fallback in `default.html`).

### SEO
- `sitemap.xml` and `robots.txt` are in the project root and served as-is (Jekyll processes the sitemap's frontmatter).
- `search.json` generates a JSON index of all blog posts for client-side search on `/blog/`.
- After deploying, submit the sitemap at `https://kwpark.io/sitemap.xml` to Google Search Console.

### Adding a new blog post
1. Create `_blog/<slug>/index.md`.
2. Set the frontmatter (see above), including `permalink: /blog/<slug>/`.
3. Drop images into the same folder and reference them relatively: `![alt](photo.jpg)`.
4. Update `last_updated` in `index.html`.

### Adding a new publication/project post
1. Create a file in `_posts/` following the naming convention `YYYY-MM-DD-slug.md`.
2. Set required frontmatter (see above), including `categories`.
3. Place images in `assets/img/publications/{project-name}/`. Use `thumbnail` frontmatter for the OG image.
4. Update `last_updated` in `index.html`.
