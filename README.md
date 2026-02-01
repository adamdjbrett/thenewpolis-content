# The New Polis Content Import
## Converted WordPress to Eleventy

Bold, typographic Eleventy 3.x blog with:
- Montserrat (headings)
- Poppins (body)
- Pagination
- Atom / RSS / JSON / twtxt feeds
- Sitemap + humans.txt
- JSON-LD schema
- Cloudflare Pages deployment

## Install

npm install
npm start

## Build

npm run build

## Deploy

Push to `main` branch — GitHub Actions will deploy to Cloudflare Pages automatically.

## Work Summary (This Chat)

High-level summary of completed work across the New Polis Eleventy migration:

- Migrated layouts to data files: added `content/posts/posts.11tydata.cjs` and `content/pages/pages.11tydata.cjs`; removed `layout:` from individual front matter.
- Added post navigation and ShareOpenly integration in the post template, plus breadcrumb/schema includes and hero image rendering from `image:` front matter.
- Implemented conferences collection (layout, sample items, index page) and wired pagination/back-forward nav like the main posts.
- Added site footer metadata and Luxon year display; created `LICENSE` (MIT).
- Added heading anchors (web component) and TOC plugin; configured markdown-it + footnote plugin.
- Fixed URL generation for localhost vs production with `baseUrl`/`absUrl` filters and updated feeds/sitemap/robots/twtxt to avoid `//` and `0.0.7.231` URLs.
- Rebuilt `/public/images` and `/public/docs` from SiteSucker export; normalized content references to `/images/` and `/docs/` paths; generated redirects for moved assets.
- Built `missing.md` inventory of still-missing assets and attempted recovery from SiteSucker; filled several missing images from `public/images/missing_images`.
- Ensured all posts now have `image:` front matter (from `baby-feature-image` in SiteSucker HTML; defaulted where missing).
- Populated `categories` and `tags` for all posts by parsing SiteSucker HTML classes; applied fallback category and derived tags from content.
- Fixed tag/category index templates: tags now use `post.data.tags`, categories/tags lists now populate.
- Added site description beneath the title in header.
- Added author + categories (clickable, with separator and label) under breadcrumbs; added tags line above “Share this Post” with `#` formatting and normalization rules.

## WordPress → Eleventy: Import, Build, Update (Based on This Project)

This section documents the practical workflow we used here to convert a WordPress site to Eleventy and keep it updated.

### 1) Import WordPress Content

1. Export or download the WordPress site HTML (SiteSucker or a similar crawler).
2. Convert posts/pages to Markdown and place them in:
   - `content/posts/YYYY/MM/DD/slug.md`
   - `content/pages/*.md`
3. Add Eleventy data defaults:
   - `content/posts/posts.11tydata.cjs` (layout: `post.njk`)
   - `content/pages/pages.11tydata.cjs` (layout: `base.njk`)
4. Normalize internal links:
   - Replace any `index.html` post URLs with trailing `/`
   - Replace old WP asset paths with `/images/…` or `/docs/…`

### 2) Import Assets (Images & Docs)

1. Clear existing `public/images` and `public/docs`.
2. Copy media from the SiteSucker export:
   - WordPress images → `public/images/wp-content/uploads/...`
   - WordPress docs   → `public/docs/wp-content/uploads/...`
3. Update content links to point at `/images/…` or `/docs/…`.
4. Create redirects for moved assets (via `_redirects` or an Eleventy `redirects.njk`).

### 3) Add Metadata & Site Features

1. Define site metadata in `_data/metadata.js` (title, description, url, footer).
2. Add header/footer, breadcrumbs, and schema includes.
3. Enable markdown-it + footnotes, TOC, and heading anchors.
4. Ensure post hero images render using `image:` in front matter.

### 4) Populate Categories/Tags

1. Parse SiteSucker HTML for taxonomy:
   - Extract `category-*` and `tag-*` from `<article class="...">`.
2. Add `categories:` and `tags:` to post front matter.
3. Use fallback defaults when missing:
   - Category: `Analysis`
   - Tags: derive from content (or defaults like “Critical Theory”, etc.).

### 5) Build & Run Locally

```bash
npm install
npm start
```

### 6) Production Build

```bash
npm run build
```

### 7) Ongoing Updates

- Re-run asset sync when new images/docs are added.
- Re-run taxonomy script when new posts arrive.
- Rebuild feeds and sitemap after major changes.
- Keep `metadata.url` accurate for production; use baseUrl/absUrl filters for localhost.
