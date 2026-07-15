# Deploy on Cloudflare Pages — thefoundersbrain.com

> **Rebrand 2026-07-15 — what's left to do by hand (operator):**
> 1. Buy/confirm `thefoundersbrain.com`, add it to Cloudflare, then Workers & Pages → `businessbrainpro-site` → Custom domains → **Set up a domain** → `thefoundersbrain.com` + `www.thefoundersbrain.com`.
> 2. Once the new domain serves the site, add the old-domain 301 as a **Redirect Rule** on the `businessbrainpro.com` zone (NOT in `_redirects`; Pages ignores host-based sources there): dashboard → businessbrainpro.com → Rules → Redirect Rules → create rule → match **All incoming requests** → **Dynamic** redirect → expression `concat("https://thefoundersbrain.com", http.request.uri.path)` → status **301** → tick **Preserve query string**.
> 3. Create the `hello@thefoundersbrain.com` mailbox/alias before `full-page.html` goes to the front (its CTAs point there).
> The Pages project itself keeps the name `businessbrainpro-site`; everything below documents the original old-domain deploy and still applies mechanically.

Static site, a single `index.html`. Deployed from Git, like `razvanpopescu.com`. Any push to `main` → Cloudflare republishes on its own.

**Before anything:** check where `businessbrainpro.com` is registered and whether it already has DNS/email configured. If it's a new domain with no site and no email yet, the move is clean (point the nameservers at Cloudflare and you're done). If it already has email (e.g. `hello@businessbrainpro.com`), do NOT break the MX/SPF/DKIM/DMARC records — see `DNS-checklist-cloudflare.md`.

---

## Step 1 — Connect the repo to Cloudflare Pages

1. Cloudflare → **Workers & Pages → Create → Pages → Connect to Git**
2. Authorize GitHub (if asked) and pick the **businessbrainpro-site** repo
3. Build settings:
   - Framework preset: **None**
   - Build command: **(empty)**
   - Build output directory: **/**
4. **Save and Deploy**
5. In ~30 sec you get a `https://businessbrainpro-site.pages.dev` link
6. **Test there first.** Open it and check:
   - the hero counters (7, 25, 4) animate on scroll; "7:00" is fixed
   - every section is visible after load — even without scrolling, everything appears within ~1.5s (the reveal is fail-open with a hard backstop)
   - the language switcher "Română" goes to creierulafacerii.ro
   - the "Install the Brain" buttons open an email to `hello@businessbrainpro.com`
   - the favicon shows in the tab
   - no horizontal scroll at desktop (1280px) or mobile (375px)
   If it looks right → move on.

---

## Step 2 — Attach the domain (Custom domain)

1. In the Pages project → **Custom domains** tab → **Set up a domain**
2. Enter `businessbrainpro.com` → Continue
3. Cloudflare shows you **exactly** which DNS record to add. Usually:
   - if the domain is ALREADY on Cloudflare (Cloudflare nameservers): one click — Cloudflare creates the correct record to the Pages project itself
   - if it's NOT on Cloudflare: it gives you a CNAME (to `businessbrainpro-site.pages.dev`) or an A record (to an IP) to add at your current registrar/DNS
4. Repeat for `www.businessbrainpro.com` if you want www to work (a CNAME to `businessbrainpro-site.pages.dev`).
5. HTTPS turns on by itself.

---

## Step 3 — The ecosystem redirects (when you get there)

This domain also serves the EN plugin infrastructure. Add a `_redirects` file at the repo root (Cloudflare Pages format, one rule per line):

```
/template   https://<the-Notion-Template-v1.0.0-link>   302
```

And copy `updates.json` (generated with `gen-updates-json.sh` from the umbrella repo) into the root, so it's served at `businessbrainpro.com/updates.json`. See `README.md` → "Also going on this domain".

---

## What NOT to touch if the domain already has email

If `hello@businessbrainpro.com` (or any address on the domain) is already working, leave these untouched:
- **MX** (mail delivery)
- **TXT**: SPF (`v=spf1...`), DKIM (`*._domainkey`), DMARC (`_dmarc`)
- the mail/webmail/autoconfig/autodiscover subdomains

Details in `DNS-checklist-cloudflare.md`.

---

## Final check

- DNS propagation: minutes to a few hours.
- Check `businessbrainpro.com` in the browser (hard refresh: Cmd+Shift+R).
- If you have email on the domain, send yourself a test to confirm it still works.

**Safety net:** test on `*.pages.dev` first. Attach the domain only once you're happy. If something breaks, you can revert the DNS record to its previous value. Reversible.
