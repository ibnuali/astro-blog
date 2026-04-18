# Astro Blog

A modern, dark-themed blog built with Astro and deployed on Cloudflare Workers.

## Features

- **Dark-first design** with purple accent (#bc52ee)
- **Responsive 3-column grid** that adapts to 2-column (tablet) and 1-column (mobile)
- **Content Collections** for type-safe blog posts with frontmatter validation
- **Dark/Light mode toggle** — persists via localStorage, respects `prefers-color-scheme`
- **SEO-ready** — canonical URLs, OpenGraph, sitemap, RSS feed
- **MDX support** — write Markdown with embedded components
- **Cloudflare Workers deployment** — fast global edge deployment

## Project Structure

```
src/
├── components/     # Reusable Astro components (Header, Footer, ThemeToggle, etc.)
├── content/blog/   # Blog posts in Markdown/MDX
├── layouts/        # Page layouts (BlogPost.astro)
├── pages/          # Astro pages and routes
│   ├── index.astro      # Homepage with hero + post grid
│   ├── [slug].astro     # Individual blog post pages
│   └── rss.xml.js       # RSS feed
├── consts.ts       # Site-wide constants (title, description)
└── styles/
    └── global.css  # Design tokens, dark theme, typography, responsive styles

public/
├── fonts/          # Atkinson font files
└── blog-placeholder-*.jpg  # Default hero images
```

## URL Structure

| Path | Description |
|------|-------------|
| `/` | Homepage — hero section + responsive post grid |
| `/:post-id/` | Individual blog post (e.g., `/first-post/`) |
| `/rss.xml` | RSS feed |

Post URLs use the filename without extension (e.g., `first-post.md` → `/first-post/`).

## Content Schema

Blog posts in `src/content/blog/` require this frontmatter:

```yaml
---
title: "Post Title"
description: "A brief description of the post"
pubDate: "2024-01-15"
heroImage: "/path/to/image.jpg"  # optional
updatedDate: "2024-01-20"        # optional
---
```

## Commands

| Command | Action |
|---------|--------|
| `npm run dev` | Start dev server at `localhost:4321` |
| `npm run build` | Build to `./dist/` |
| `npm run preview` | Preview production build locally |
| `npm run deploy` | Deploy to Cloudflare Workers |
| `npm run check` | Build + typecheck + dry-run deploy |

## Configuration

Key files to update after deployment:

- **`astro.config.mjs`** — Update `site` to your production URL for correct canonical URLs/sitemaps
- **`src/consts.ts`** — Update `SITE_TITLE` and `SITE_DESCRIPTION`
- **`src/components/Footer.astro`** — Update copyright name and social links

## Design System

- **Accent color**: `#bc52ee` (purple)
- **Dark theme default** with toggle to light mode
- **CSS custom properties** in `src/styles/global.css` for all colors, spacing, and typography
- **Responsive breakpoints**: 1100px (desktop), 1024px, 900px, 768px, 640px, 480px (mobile)
- **Typography**: Inter for body, monospace for dates/labels

## Getting Started

```bash
npm install
npm run dev
```

Deploy to Cloudflare:

```bash
npm run deploy
```