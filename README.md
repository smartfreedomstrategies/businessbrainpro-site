# BusinessBrainPro.com: static site

The site for The Business Brain. Self-contained HTML files, Satoshi from Fontshare (CDN). No build, no local dependencies, no dashboard. Plain files.

Deployed on **Cloudflare Pages** from Git, exactly like `razvanpopescu-site` and `creierulafacerii-site`. Any push to `main` republishes automatically.

## Current state: coming-soon page in front

Right now the domain serves a clean **"Coming soon" holding page**, with the full hero page kept as a reachable backup. The full page still has a few wordings to polish, so we show the tidy holding page in the meantime.

- **`index.html`** = the "Coming soon" holding page (LIVE, what visitors see).
- **`full-page.html`** = the full hero page (backup, reachable at `businessbrainpro.com/full-page.html`).

### Swap the full page back to the front at launch
When the full page is polished and ready to be the homepage:

```
git mv index.html coming-soon-backup.html
git mv full-page.html index.html
git add -A && git commit -m "Launch: full hero page to the front"
git push
```

Cloudflare republishes on push. (Keep `coming-soon-backup.html` around in case you want the holding page again later.)

## Pre-launch notes

- **Temporary email on the holding page.** The live `index.html` CTA points to `razvan@smartfreedomstrategies.com`, because `hello@businessbrainpro.com` is not a live mailbox yet and the holding page invites people to write NOW (a dead CTA loses those emails). **At launch: create `hello@businessbrainpro.com`, then change the `index.html` CTA back to it.** `full-page.html` already uses `hello@businessbrainpro.com`, so it needs the mailbox live before it goes to the front.
- The design origins live in the umbrella dev repo: `coming-soon.html` (holding page) and `the-business-brain.html` (full hero). Edits meant for the live site are made here and pushed. Do not let the copies drift.

## What's in the box
- `index.html` — the live "Coming soon" holding page
- `full-page.html` — the full hero page (backup)
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

## The domain (businessbrainpro.com)

1. In the Pages project, **Custom domains, Set up a domain**, enter `businessbrainpro.com`.
2. Cloudflare tells you which DNS record to add. Full details plus the email safety net are in `DEPLOY-cloudflare-pages.md` and `DNS-checklist-cloudflare.md`.
3. HTTPS turns on by itself.

Repeat for `www.businessbrainpro.com` if you want www to resolve too.

---

## Also going on this domain (next steps, out of scope for now)

`businessbrainpro.com` is also the EN ecosystem infrastructure:
- **`_redirects`** ✅ *live in this repo*: the stable Cloudflare Pages redirects. `/template` (and the alias `/template-notion`) → the published Command Center — Template v1 Notion link, 302. The Command Center skills ship ONLY this stable `/template` path, so it is load-bearing for a fresh client install. Destination is recorded in the umbrella repo's `versions.md`; change it here, never in the plugins. (`/install` is still a future addition.)
- **`updates.json`**: the update-discovery file, served at `businessbrainpro.com/updates.json`. Generated with `gen-updates-json.sh` from the umbrella repo (never by hand) and copied here on every release. Still to add.

These unlock the remaining steps in `launch-checklist.md` (the update endpoint; verifying the redirects resolve after deploy).
