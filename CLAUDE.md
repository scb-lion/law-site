# CLAUDE.md — law-site

Guidance for Claude Code (and humans) working in this project.

## ⚠️ Git identity — READ FIRST
- This repo and folder use the GitHub account **`scb-lion` ONLY**.
- **Never** associate `Tiz20lion` (the machine's global git identity) with this
  repo, its commits, or its pushes — not as author, committer, or credential.
- Local git config already pins this:
  - `user.name = scb-lion`
  - `user.email = scb-lion@users.noreply.github.com`
  - `credential.https://github.com.username = scb-lion`
  - `credential.useHttpPath = true`  (so the machine-wide `github.com` credential
    for the other account is not reused for this repo)
- Remote: `https://github.com/scb-lion/law-site.git`
- The scb-lion credential is now stored scoped to `…/law-site.git`, so routine
  `git push` authenticates as scb-lion non-interactively. If a push ever
  authenticates as anyone other than `scb-lion`, stop and re-auth as scb-lion.
- After any push, verify: `curl -s https://api.github.com/repos/scb-lion/law-site/commits/main | grep '"login"'` → must be `scb-lion`.

## What this is
A **static `wget` mirror** of **https://www.scfederaldefense.com** (The Law
Offices of Nathan S. Williams — Federal Criminal Defense, South Carolina),
captured 2026-07-05 for hosting on **Vercel** as pure static files (no build).

- 53 HTML pages at repo root + 4 PDFs
- Assets under `content/`, `static/`, `universal/`, `@sqs/`, `website-component-definition/`
- `vercel.json` enables clean URLs (`/about` serves `about.html`)
- `_mirror-logs/` holds the wget logs (gitignored, deletable)

## Source is Squarespace → NOT fully self-contained
Squarespace loads many assets at runtime from its own CDNs by absolute URL.
Even after mirroring, these still load from the **live Squarespace CDN**, not
local copies: lazy `data-src` images, several `assets.squarespace.com` JS
bundles, the `definitions.sqspcdn.com` component runtime, and the hero
`video.squarespace-cdn.com` video. The site looks pixel-perfect as long as
Squarespace keeps serving them, but it hotlinks the original firm's live assets.
A deeper self-containment pass (rewriting `data-src`/component URLs) is possible
but fragile and was intentionally not done.

## Local customizations already applied
- Contact email → `lawyfirm@nathawilliams.online` (was `nathan@scfederaldefense.com`)
- Phone → `(412) 378-7368` / `tel:+14123787368` / `sms:4123787368`
  (was `(843) 473-7000` / `+18434737000`)
- **Do NOT touch** long numeric strings like `1673365537…` — those are
  Squarespace asset-ID timestamps, not phone numbers.

## Editing notes (bulk find/replace across all pages)
```bash
find . -name "*.html" -type f -print0 | xargs -0 sed -i \
  -e 's/OLD_EMAIL/NEW_EMAIL/g' \
  -e 's/(OLD) PHONE-DISPLAY/(NEW) PHONE-DISPLAY/g' \
  -e 's/OLDPHONEDIGITS/NEWPHONEDIGITS/g'
```
Always grep for leftovers afterward (HTML + JS + CSS) and verify `tel:`/`sms:`/
`mailto:` link forms are still well-formed.

## Deploy (Vercel free / Hobby)
1. Push to `https://github.com/scb-lion/law-site.git` (as scb-lion).
2. Vercel → New Project → import repo → Framework preset **Other**,
   Root Directory = repo root. No build step (static). $0 on Hobby.

## Legal note
This is a copy of another firm's live site — text, branding, attorney photos,
and logo are that firm's copyright/trademark. Use as a scaffold; replace firm
name, content, images, and logo before anything goes public.

## How this site was cloned
See the reusable recipe in the workspace root: `../CLAUDE.md` → "Cloning a
website into a static, Vercel-ready mirror".
