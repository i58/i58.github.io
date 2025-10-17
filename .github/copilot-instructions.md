<!-- Copilot / AI agent instructions for the solar-satellite project -->

# Quick agent guide — solar-satellite

This repo is a small Astro blog starter. Use these notes to be productive quickly when editing, fixing, or adding features.

- Project root: content-driven Astro site using Astro 5.x. Key files:
  - `package.json` — scripts: `pnpm dev`, `pnpm build`, `pnpm preview`.
  - `astro.config.mjs` — integrations: MDX and sitemap enabled.
  - `src/content/` — content collections (blog). See `src/content.config.ts` for the `blog` collection schema.
  - `src/layouts/BlogPost.astro` — canonical blog post layout and frontmatter usage.
  - `src/pages/blog/index.astro` — list page that uses `getCollection('blog')` and sorts by `pubDate`.
  - `src/components/` — reusable UI pieces (Header, Footer, BaseHead, FormattedDate).

- Big picture & data flow
  - Markdown/MDX files under `src/content/blog/` are loaded by the Astro Content API (see `src/content.config.ts`). Frontmatter fields are validated by zod in the collection schema.
  - Pages/layouts import content entries via `getCollection('blog')` and render with `BlogPost.astro` or listing pages.
  - Images referenced in frontmatter use Astro's assets (`astro:assets`) and may be passed into the `Image` component (see `BlogPost.astro` and `src/pages/blog/index.astro`).

- Conventions to preserve when changing code
  - Frontmatter fields: `title`, `description`, `pubDate` (coerced to Date), optional `updatedDate` and `heroImage`. Keep names and types consistent with `src/content.config.ts`.
  - Use `getCollection('blog')` to fetch posts; sorting is done in pages (example in `src/pages/blog/index.astro`).
  - Keep layout props typed via `CollectionEntry<'blog'>['data']` when modifying `BlogPost.astro` or similar.

- Common edits & examples
  - Add a new blog post: create `src/content/blog/your-post.md` with frontmatter matching the schema. Use `pubDate: 2025-10-17` (ISO) or another format accepted by zod/coerce.date().
  - Change header links: edit `src/components/Header.astro` (internal links live in the `internal-links` div). Example: add a new link by adding a `HeaderLink` element.
  - Page meta/head changes: update `src/components/BaseHead.astro` (used across pages) to modify canonical/og meta.

- Build / dev / debug commands
  - Install dependencies: `pnpm install` (repo uses pnpm). If pnpm is not available, fallback `npm install` but prefer pnpm.
  - Dev server: `pnpm dev` (starts Astro at default port, usually 4321). Use `pnpm build` then `pnpm preview` to test production build.
  - Lint/typecheck: this repo uses TypeScript (`tsconfig.json`) and the strict Astro config. Use `pnpm astro check` if needed.

- Patterns to be careful with
  - Astro islands / components: this starter uses plain `.astro` components and `astro:assets`. Avoid introducing client frameworks unless wire-up is added to `astro.config.mjs`.
  - Image handling: `heroImage` in frontmatter is typed as `image()` in `content.config.ts`; prefer using `Image` from `astro:assets` and pass width/height (examples in `BlogPost.astro`).
  - Sorting/Date math: pages sort posts by numeric value of `pubDate` (see `src/pages/blog/index.astro`). Preserve `.valueOf()` usage if altering ordering.

- Files to open first when investigating a change
  1. `src/content.config.ts` — content schema and loader
  2. `src/layouts/BlogPost.astro` — core post rendering
  3. `src/pages/blog/index.astro` — list & sorting logic
  4. `src/components/Header.astro` and `Footer.astro` — site chrome
  5. `astro.config.mjs` and `package.json` — integrations and scripts

- When making changes that affect content schema
  - Update `src/content.config.ts` first, then update existing Markdown frontmatter in `src/content/blog/*` to match. Run dev (`pnpm dev`) and watch the dev server for schema validation errors.

If anything is unclear or you need deeper guidance (deploy, CI, or new integrations), tell me which area to expand and I will update this file.
