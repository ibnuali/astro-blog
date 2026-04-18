# Astro Blog Agents

## Key Commands

| Command | Action |
|---------|--------|
| `npm run dev` | Start dev server at `localhost:4321` |
| `npm run build` | Build to `./dist/` |
| `npm run preview` | Preview production build locally |
| `npm run deploy` | Deploy to Cloudflare Workers |
| `npm run check` | Build + typecheck + dry-run deploy |

## Architecture

- **Framework**: Astro 5 with Cloudflare Workers adapter
- **Content**: Astro Content Collections with glob loader (`src/content/blog/*.md`/*.mdx`)
- **Styling**: Plain CSS with CSS custom properties (no Tailwind)
- **Dark mode**: Theme toggle sets `data-theme` on `<html>` element; script in `ThemeToggle.astro` handles persistence and `prefers-color-scheme`

## URL Structure

| Path | Source |
|------|--------|
| `/` | `src/pages/index.astro` (lists all posts) |
| `/:post-id/` | `src/pages/[slug].astro` (single post) |
| `/rss.xml` | `src/pages/rss.xml.js` |

Post URLs use `post.id` which equals the filename without extension (e.g., `first-post` for `first-post.md`).

## Content Collections

Blog posts live in `src/content/blog/` and use this frontmatter schema:
```ts
{
  title: string
  description: string
  pubDate: Date (coerced from string)
  updatedDate?: Date
  heroImage?: string
}
```

## Important Config

- `astro.config.mjs`: Update `site` to your production URL for correct canonical URLs/sitemaps
- `src/consts.ts`: Update `SITE_TITLE` and `SITE_DESCRIPTION`
- `src/components/Footer.astro`: Update copyright name and GitHub URL

## Styling Conventions

- CSS variables defined in `:root` in `src/styles/global.css`
- Dark theme variables under `[data-theme="dark"]`
- Max content width: `720px`, prose width: `680px`
- Header height: `60px`
