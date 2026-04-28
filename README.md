# Shihtzu Factory

Static site for [www.shihtzufactory.com](https://www.shihtzufactory.com) — a small, family-run Shih Tzu house in Abilene, Texas.

## Stack

- Static HTML/CSS/JS, no build step
- Hosted on **GitHub Pages** from `main` branch root
- Custom domain via `CNAME` file
- DNS via **Cloudflare** (already on Cloudflare nameservers)
- Commerce via **Stripe Payment Links** (no server required)

## Local preview

```bash
python3 -m http.server 4200
# open http://localhost:4200
```

## Wiring real Stripe Checkout

The reserve buttons currently point at placeholder URLs. To make them live:

1. Go to https://dashboard.stripe.com/payment-links → **Create payment link**
2. Create one Payment Link per puppy / accessory:
   - **Gucci deposit** — one-time $500
   - **Parsley deposit** — one-time $500
   - **Tyra deposit** — one-time $500
   - **Mini collar** — one-time $48
   - **Top-knot bow set** — one-time $24
   - **Brush & comb** — one-time $62
   - **Welcome-home bundle** — one-time $145
3. For each link, copy the URL Stripe gives you (looks like `https://buy.stripe.com/abc123...`)
4. In `index.html`, search for `PLACEHOLDER_GUCCI`, `PLACEHOLDER_PARSLEY` etc. and replace with the real URL
5. Commit + push — GitHub Pages redeploys automatically

## Wiring the contact form

The contact form points at a Formspree placeholder. To make it work:

1. Sign up at https://formspree.io (free tier supports basic forms)
2. Create a form, get the endpoint URL like `https://formspree.io/f/abc123`
3. In `index.html` find `PLACEHOLDER` in the form `action` attribute and replace
4. Remove the `onsubmit` handler that shows the alert

## DNS — Cloudflare records

Domain is already on Cloudflare nameservers. To point it at GitHub Pages:

| Type | Name | Value | Proxy |
|------|------|-------|-------|
| A | @ | 185.199.108.153 | DNS only (gray cloud) initially, orange after cert issued |
| A | @ | 185.199.109.153 | DNS only |
| A | @ | 185.199.110.153 | DNS only |
| A | @ | 185.199.111.153 | DNS only |
| CNAME | www | topaz-zc.github.io | DNS only |

After GitHub Pages issues the Let's Encrypt cert (5–15 minutes after the DNS propagates), you can flip the records to **Proxied** (orange cloud) for Cloudflare CDN + analytics + DDoS protection.

## Photo source

All puppy/show photos are from the original shihtzufactory.com site. Backup of the full 176-photo archive lives at `~/Mocks/shihtzu-rebrand/full-scrape/photos-final/` and `~/Desktop/Shihtzu-Factory-Photos/`.
