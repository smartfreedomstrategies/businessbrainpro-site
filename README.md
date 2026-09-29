# TheFoundersBrain.com: static site

> **Rebrand 2026-07-15:** the product is now **The Founders' Brain** on **thefoundersbrain.com**. The GitHub repo and the Cloudflare Pages project keep the old name (`businessbrainpro-site`); only the served copy and the primary domain change. businessbrainpro.com stays attached and 301s to the new domain once it's wired (see `DEPLOY-cloudflare-pages.md`).

The site for The Founders' Brain. Self-contained HTML files, Satoshi from Fontshare (CDN). No build, no local dependencies, no dashboard. Plain files.

Deployed on **Cloudflare Pages** from Git, exactly like `razvanpopescu-site` and `creierulafacerii-site`. Any push to `main` republishes automatically.

## Current state: the full page is the homepage (since 2026-09-29)

- **`index.html`** = the full landing page (LIVE at the root).
- **`coming-soon-backup.html`** = the old holding page, kept with `noindex` in case it is ever needed again.
- `/full-page` and `/full-page.html` 301 to the root (`_redirects`), so links sent during the warm-intro phase keep working.
- **Contact:** every CTA is a mailto to `razvan@razvanpopescu.com` (Google mail, receives). No address is shown as text. `thefoundersbrain.com` has no MX, so no `hello@` address is used.
- The design origins in the umbrella dev repo (`coming-soon.html`, `the-business-brain.html`) are historical. This repo is the source of truth for the live copy.

## What's in the box
- `index.html`: the live landing page
- `coming-soon-backup.html`: the old holding page (noindex)
- `install.html`: the install and rescue page
- `_redirects`: /template, /template-notion, /full-page
- `DEPLOY-cloudflare-pages.md` — the Cloudflare Pages deploy steps
- `DNS-checklist-cloudflare.md` — a DNS verification template to fill in before attaching the domain
- `README.md` — this file
- `.gitignore`

---

## Cloudflare Pages

1. Go to dash.cloudflare.com, **Workers & Pages, Create, Pages, Connect to Git**.
2. Authorize GitHub, pick the `businessbrainpro-site` repo.
3. Build settings:
   - **Framework preset:** None
   - **Build command:** leave empty
   - **Build output directory:** `/` (the root)
4. **Save and Deploy.** In about 30 seconds you get a live `businessbrainpro-site.pages.dev` link.

Any new push to GitHub republishes automatically.

## The domain (thefoundersbrain.com)

1. In the Pages project, **Custom domains, Set up a domain**, enter `thefoundersbrain.com`.
2. Cloudflare tells you which DNS record to add. Full details plus the email safety net are in `DEPLOY-cloudflare-pages.md` and `DNS-checklist-cloudflare.md`.
3. HTTPS turns on by itself.

Repeat for `www.thefoundersbrain.com` if you want www to resolve too. Keep `businessbrainpro.com` (+www) attached so the 301 redirect to the new domain keeps old links alive.

---

## Also going on this domain (next steps, out of scope for now)

`businessbrainpro.com` is also the EN ecosystem infrastructure:
- **`_redirects`** ✅ *live in this repo*: the stable Cloudflare Pages redirects. `/template` (and the alias `/template-notion`) → the published Command Center — Template v1 Notion link, 302. The Command Center skills ship ONLY this stable `/template` path, so it is load-bearing for a fresh client install. Destination is recorded in the umbrella repo's `versions.md`; change it here, never in the plugins. (`/install` is still a future addition.)
- **`updates.json`** ✅ *live in this repo*: the update-discovery file, served at `businessbrainpro.com/updates.json`. Generated with `gen-updates-json.sh` from the umbrella repo (never edited by hand) and copied here on every release. session-starter Step 0.5b reads it (weekly cache, fail-silent, read-only) to nudge clients when a newer version ships. Exposes only product names + versions — no client data. **On every release: regenerate in the umbrella, copy the file here, push.**

These unlock the remaining steps in `launch-checklist.md` (the update endpoint; verifying the redirects resolve after deploy).
