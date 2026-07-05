# law-site

Static mirror of **https://www.scfederaldefense.com** (The Law Offices of Nathan S. Williams — Federal Criminal Defense, South Carolina), captured for hosting on Vercel.

## What this is

A full-site `wget` mirror: all HTML pages plus CSS, JS, fonts, images, and PDFs downloaded locally. Captured 2026-07-05.

- **53+ HTML pages** at the repo root (`index.html`, `about.html`, `contact.html`, practice-area pages, city pages, etc.)
- Assets under `content/`, `static/`, `universal/`, `@sqs/`, `website-component-definition/`
- 4 PDFs, Google Fonts, Squarespace CSS/JS bundles

## Deploying to Vercel

This is a **static site** — no build step.

1. Point a Vercel project at this folder (Framework preset: **Other**, Output/Root directory: the folder itself).
2. `vercel.json` enables clean URLs (`/about` serves `about.html`).
3. Deploy.

Or with the CLI from this folder: `vercel` (then `vercel --prod`).

## Important: this is a Squarespace site (CDN dependency)

The source is built on Squarespace, which loads many assets at runtime from its own CDNs by absolute URL. Even after mirroring, the following still load from Squarespace's **live CDN**, not local copies:

- Lazy-loaded images (`data-src` / Squarespace ImageLoader)
- Several runtime JS bundles (`assets.squarespace.com`)
- The web-component runtime (`definitions.sqspcdn.com`)
- Hero background **video** (`video.squarespace-cdn.com`)

**Consequence:** the deployed site looks pixel-perfect *as long as Squarespace keeps serving those assets*. It is not fully self-contained, and it hotlinks the original firm's live assets. If the source site changes or blocks hotlinking, parts may break.

## Legal note

This is a copy of another law firm's live site — its text, branding, attorney photos, and logo are that firm's copyright/trademark. Use as a scaffold only; replace the firm name, content, images, and logo before publishing anything public.

## Logs

Mirror logs are in `_mirror-logs/` (not needed for deployment; safe to delete).
