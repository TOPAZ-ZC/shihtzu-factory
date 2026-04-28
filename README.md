# Shihtzu Factory

Static site for [www.shihtzufactory.com](https://www.shihtzufactory.com) — a small, family-run Shih Tzu house in Abilene, Texas.

## Stack

- Static HTML/CSS/JS, no build step
- Hosted on **GitHub Pages** from `main` branch root
- Custom domain via `CNAME` file
- DNS via **Cloudflare** (already on Cloudflare nameservers)
- Commerce via **PayPal Hosted Payment Buttons** (Venmo included as a payment option inside PayPal Checkout — no server required)

## Local preview

```bash
python3 -m http.server 4200
# open http://localhost:4200
```

## Wiring real PayPal/Venmo checkout

The reserve buttons currently fall back to placeholder URLs when no payment link is set. To make them live:

1. Sign in at https://www.paypal.com/business → **Pay & Get Paid** → **PayPal Buttons** (or **Hosted Payment Buttons**)
2. Create one button per puppy / accessory. For puppies use a $500 deposit (the remaining balance is collected in person at pickup):
   - **Gucci deposit** — fixed $500
   - **Parsley deposit** — fixed $500
   - **Tyra deposit** — fixed $500
   - **Mini collar** — fixed $48
   - **Top-knot bow set** — fixed $24
   - **Brush & comb** — fixed $62
   - **Welcome-home bundle** — fixed $145
3. For each button, copy the **payment link URL** PayPal gives you (looks like `https://www.paypal.com/ncp/payment/XXXXXXX`)
4. Sign in at https://www.shihtzufactory.com/admin, click each puppy/item, paste the URL into the **PayPal/Venmo Payment Link** field, save
5. **Venmo is included automatically** — PayPal Checkout shows a "Pay with Venmo" button alongside PayPal at checkout, so customers can pick either

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
