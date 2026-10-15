# TheFoundersBrain.com: static site

> **Live hosting verified 15 October 2026:** GitHub reports this existing deployment as **Workers Builds: businessbrainpro-site**, despite the historical Pages instructions below. Hosting and DNS have not been migrated. `wrangler.jsonc` preserves Cloudflare’s existing generated configuration and enables `assets.not_found_handling: "404-page"` so missing URLs serve the branded error page with HTTP 404.

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
- `install.html`: the short setup-help page (intentionally noindex)
- `404.html`: branded missing-page response for Cloudflare Pages
- `robots.txt` and `sitemap.xml`: crawl guidance and the canonical public homepage
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
- **`_redirects`** ✅ *live in this repo*: the stable Cloudflare Pages redirects. `/template` (and the alias `/template-notion`) → the published Command Center — Template v1 Notion link, 302. The Command Center skills ship ONLY this stable `/template` path, so it is load-bearing for a fresh client install. Destination is recorded in the umbrella repo's `versions.md`; change it here, never in the plugins. `/install` points clients to Start Here and provides prerequisites, troubleshooting and contact details. The kit holds the full checklist and prompts.
- **`updates.json`** ✅ *live in this repo*: the update-discovery file, served at `businessbrainpro.com/updates.json`. Generated with `gen-updates-json.sh` from the umbrella repo (never edited by hand) and copied here on every release. session-starter Step 0.5b reads it (weekly cache, fail-silent, read-only) to nudge clients when a newer version ships. Exposes only product names + versions — no client data. **On every release: regenerate in the umbrella, copy the file here, push.**

These unlock the remaining steps in `launch-checklist.md` (the update endpoint; verifying the redirects resolve after deploy).

## Delivery alignment, 15 October 2026

The homepage and `/install` follow the client kit: choose Claude Cowork or Codex, install, run shared technical setup, then choose onboarding. Cowork uploads the included `.plugin` files. Codex registers the bundled local marketplace, restarts the app and installs from Plugins. The private client kit remains the source for copyable prompts and plugin files; this public site does not host the kit.

Reports are opt-in, with three core schedules and two optional Ghostwriter schedules. Compass and Outreach add sections to approved core briefs. Runtime availability and connection access are checked during setup; no exact-minute or automatic catch-up guarantee is made. The kit’s Start Here guide explains dashboard consent, scheduling and updates. Keep detailed instructions there rather than duplicating them on `/install`.

The original design, guided-service offer, contact address and template redirect destination are preserved. The generated `updates.json` must match the released umbrella version file. Before publishing, check links, HTML structure, copy claims and JSON parity. A push to main publishes both pages and the update feed.

## Local quality audit, 15 October 2026

The local draft includes a skip-to-content link, reduced-motion CSS, a counter fallback when IntersectionObserver is unavailable, and narrow-screen sizing fixes. Orange buttons use the existing darker brand orange for white text (calculated contrast 5.08:1). Both pages use the existing branded cover for social previews. The new 404 page follows [Cloudflare Pages serving behavior](https://developers.cloudflare.com/pages/configuration/serving-pages/); existing redirect destinations are unchanged. The sitemap includes only the indexable homepage, not setup help, the backup or 404.

Verified locally: HTML structure, titles/descriptions, heading counts, local links and fragments, social-image file/dimensions, sitemap/robots consistency, preserved approved copy, script behavior in normal/reduced-motion/no-observer cases, and external destination/font HTTP responses. These are source and isolated-script checks, not a rendered accessibility certification.

Still unverified: rendered desktop/mobile layouts, actual keyboard traversal, social-platform cropping, Lighthouse/Core Web Vitals, and new 404/SEO-file responses after deployment. The browser tool denied access to the local file URL; do not infer visual or performance results from source checks. Nothing was deployed. Publishing requires separate approval because pushing the connected branch deploys automatically.
