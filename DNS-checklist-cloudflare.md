# DNS checklist — businessbrainpro.com on Cloudflare

⚠️ **Template, not live data.** The values here are NOT known. **The first step is to read the current DNS** at the registrar where `businessbrainpro.com` lives and fill in the tables below before hitting "Continue to activation" in Cloudflare. Do NOT invent records.

## Find out first (you fill in)
- Registrar / where DNS is managed now: **_______**
- Does the domain have email configured? (e.g. `hello@businessbrainpro.com`) **YES / NO**
  - If YES: on what service? (Google Workspace / Zoho / cPanel / other) **_______**
- Does it already have a live site (an A record pointing to an IP)? **YES / NO** → IP: **_______**

If the domain is completely empty (no site, no email), the move is simple: add the domain in Cloudflare, move the nameservers, attach Pages. Skip the "email" section below.

---

## VITAL — if the domain has email, these must exist in Cloudflare

(Fill in from the current DNS. If you don't have email on the domain yet, this section doesn't apply until you set up `hello@businessbrainpro.com`.)

| Type | Name | Value | Proxy |
|---|---|---|---|
| MX | businessbrainpro.com | `_______` (priority `__`) | DNS only |
| TXT (SPF) | businessbrainpro.com | `v=spf1 ... ~all` | DNS only |
| TXT (DMARC) | _dmarc | `v=DMARC1; p=none;` | DNS only |
| TXT (DKIM) | `*._domainkey` | the long DKIM key | DNS only |

**Watch the DKIM:** the Cloudflare scan sometimes misses the long `*._domainkey` key. Verify manually that it's there. If it's missing, sent email can land in spam.

---

## SITE — after the domain is in Cloudflare

Do NOT edit the A record by hand. Go to Workers & Pages → **businessbrainpro-site** → **Custom domains** tab → **Set up a domain** → enter `businessbrainpro.com`. Cloudflare creates the correct record to the Pages project itself. Repeat for `www`.

| Name | Target | Proxy |
|---|---|---|
| businessbrainpro.com | Pages project `businessbrainpro-site` | Proxied (auto) |
| www | Pages project `businessbrainpro-site` | Proxied (auto) |

---

## The right order of steps

1. **Read the current DNS** at the registrar and fill in the tables above.
2. **Add the domain in Cloudflare** (the "Add a site" / Review DNS records flow).
3. **Verify** in the review screen that nothing vital is missing (MX/SPF/DMARC/DKIM, IF you have email). Add manually whatever's missing.
4. **Change the nameservers** at the registrar (to the two Cloudflare gives you). This activates Cloudflare.
5. **Wait for propagation** (minutes–hours).
6. **Workers & Pages → businessbrainpro-site → Custom domains → Set up a domain** → `businessbrainpro.com` + `www`.
7. **Test:** open the site; if you have email on the domain, send yourself a test.

## Safety net
If something breaks, switch the nameservers back at the registrar and return to the previous state. Reversible.
