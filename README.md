# Glow Grid Marketing — One-Page Site

Single static site + one Vercel serverless function for the contact form. No third-party form services, no build step.

## Files

- `index.html` — the entire site, single file
- `api/contact.js` — Vercel serverless function that emails form submissions
- `_redirects` — Cloudflare Pages / Netlify redirect config
- `vercel.json` — Vercel config + security headers + function settings
- `robots.txt` — search engine instructions
- `sitemap.xml` — sitemap (update domain before deploy)

## Local Preview

For just viewing the static site:

```bash
python3 -m http.server 8000
# or
npx serve .
```

To test the contact form locally with the API working:

```bash
npm install -g vercel
vercel dev
```

`vercel dev` runs both the static files and the serverless function on localhost.

## Contact Form Setup (Resend, ~5 min, free)

The form posts to `/api/contact`, which sends email via Resend. You need to:

1. **Sign up at [resend.com](https://resend.com)** — free tier is 3,000 emails/month, way more than needed
2. **Verify a domain** (optional but recommended) — adds DNS records to `glowgridmarketing.com` so emails come from `contact@glowgridmarketing.com` instead of Resend's test address. Without this, emails work but come from `onboarding@resend.dev`.
3. **Create an API key** — Resend dashboard → API Keys → Create. Copy it.
4. **Add environment variables in Vercel dashboard** (Project → Settings → Environment Variables):
   - `RESEND_API_KEY` — paste the key from step 3
   - `TO_EMAIL` — `glow.grid.marketing@gmail.com`
   - `FROM_EMAIL` — `contact@glowgridmarketing.com` (if you verified the domain) or `onboarding@resend.dev` (for testing)
5. **Redeploy** so env vars take effect: `vercel --prod`

The form will:
- Email submissions to `TO_EMAIL` with reply-to set to the visitor's email
- Show inline success/error messages without a page redirect
- Block bots via a hidden honeypot field

## Deploy — Vercel

```bash
npm install -g vercel
vercel login
vercel --prod
```

First deploy creates the project. Subsequent deploys reuse it.

## Domain Setup (after deploy)

For `glowgridmarketing.com`:

1. Have client register the domain in their own account (Cloudflare Registrar recommended, ~$10/yr at wholesale, no upsells)
2. In Vercel dashboard → Project → Domains → add `glowgridmarketing.com` and `www.glowgridmarketing.com`
3. Vercel gives exact DNS records to add at the registrar:
   - `A` record on apex `@` pointing to Vercel's IP
   - `CNAME` on `www` pointing to `cname.vercel-dns.com`
4. Wait 5–60 min for DNS to propagate. SSL is automatic.

## Things to Swap Before Going Live

Search the file for these and replace:

1. `[Name]` — founder's name (in About section heading)
2. `[Founder bio goes here]` — actual bio text (2 paragraphs)
3. `founder.jpg` — replace with actual founder photo (drop file in same folder)
4. Work tiles in `#work` section — placeholder titles like "Brand Launch Series" should be replaced with real client work
5. Social links — `instagram.com`, `tiktok.com`, `linkedin.com` placeholders. Swap for real handles.
6. `https://www.glowgridmarketing.com` (in JSON-LD schema and sitemap.xml) — replace with actual domain when registered

## Email Forwarding (recommended)

Once `glowgridmarketing.com` is registered, set up:
- `hello@glowgridmarketing.com` → her Gmail
- `contact@glowgridmarketing.com` → her Gmail

Free options:
- **Cloudflare Email Routing** (free, if domain on Cloudflare)
- **ImprovMX** (free for low volume)

This makes contact emails look more professional than the gmail address.

## Maintenance

The site is intentionally simple. Updates = edit `index.html`, redeploy with `vercel --prod`. Takes 30 seconds.
