# BusinessBrainPro.com — static site

The hero page for The Business Brain. One self-contained file, `index.html`. Satoshi from Fontshare (CDN). No build, no local dependencies, no dashboard. Plain file.

Deployed on **Cloudflare Pages** from Git, exactly like `razvanpopescu-site` and `creierulafacerii-site`. Any push to `main` → Cloudflare republishes automatically.

## What's in the box
- `index.html` — the whole page (HTML + CSS + JS inline, SVG icons inline)
- `DEPLOY-cloudflare-pages.md` — the Cloudflare Pages deploy steps
- `DNS-checklist-cloudflare.md` — a DNS verification **template** to fill in before attaching the domain
- `README.md` — this file
- `.gitignore`

## Source of truth
The `index.html` here is the **live version** of the page. Its design origin is `the-business-brain.html` in the development (umbrella) repo, but from now on edits meant for the live site are made **here** and pushed. Cloudflare republishes on its own. Don't maintain two copies that drift — keep the umbrella file as design origin only.

---

## Step 1 — GitHub (done)

The `businessbrainpro-site` repo exists on `smartfreedomstrategies`, with the files on `main`. Nothing to do here.

*(Going forward: change the text in `index.html`, `git add`, `git commit`, `git push`. Cloudflare republishes on its own.)*

---

## Step 2 — Cloudflare Pages

1. Go to dash.cloudflare.com → **Workers & Pages → Create → Pages → Connect to Git**.
2. Authorize GitHub, pick the `businessbrainpro-site` repo.
3. Build settings:
   - **Framework preset:** None
   - **Build command:** leave empty
   - **Build output directory:** `/` (the root)
4. **Save and Deploy.** In ~30 seconds you get a live `businessbrainpro-site.pages.dev` link.

Any new push to GitHub → Cloudflare republishes automatically.

---

## Step 3 — The domain (businessbrainpro.com)

1. In the Pages project → **Custom domains → Set up a domain** → enter `businessbrainpro.com`.
2. Cloudflare tells you which DNS record to add. Full details + the email safety net are in `DEPLOY-cloudflare-pages.md` and `DNS-checklist-cloudflare.md`.
3. HTTPS turns on by itself.

Repeat for `www.businessbrainpro.com` if you want www to resolve too.

---

## Also going on this domain (next steps, out of scope for the "just the hero" pass)

`businessbrainpro.com` is also the EN ecosystem infrastructure. When you get there, these live here (in this repo) too:
- **`updates.json`** — the update-discovery file, served at `businessbrainpro.com/updates.json`. Generated with `gen-updates-json.sh` from the umbrella repo (never by hand) and copied here on every release.
- **`_redirects`** — the stable Cloudflare Pages redirects: `/template` → the published Notion template link (and the future `/install`). Cloudflare Pages format: one line per rule — `/template  https://…notion…  302`.

These unlock the remaining steps in `launch-checklist.md` (pointing the redirects + the update endpoint). Deliberately not included now — the page is "just index" until we grow the site.

## Pre-launch
The CTAs in `index.html` point to `hello@businessbrainpro.com`. That mailbox must exist before launch, or the install email bounces.
