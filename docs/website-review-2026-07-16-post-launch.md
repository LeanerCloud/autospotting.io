# Post-Launch Review — autospotting.io — 2026-07-16

Second thorough review, run against the **live production site** (S3 + CloudFront)
after the migration launch, covering SEO and web best practices. Method note: the
review agents were repeatedly interrupted by API capacity issues, so the audit was
completed with deterministic crawler/checker scripts over every sitemap URL plus
header, redirect, weight, and edge-behavior probes; findings below are all
verified against production responses.

## Verified clean ✅

- **All 40 sitemap URLs return 200**; no redirects or 404s inside the sitemap
- **Canonicals** absolute, suffix-free, self-consistent on every page
- **Titles/descriptions** present and unique sitewide (no duplicates)
- **JSON-LD parses as valid JSON** on all pages (Organization + WebSite on home, BlogPosting on posts)
- **og:image / twitter:image** present everywhere and resolve 200 (branded 1200x630 card as default)
- **Redirects**: single-hop 301s for http→https, both www hosts, autospotting.org (query strings preserved), and all legacy URLs (`/faq.html`→`/#faq`, `/products`→`/#pricing`, `/author/*` and deleted meghna posts→`/blog/`)
- **robots.txt** with sitemap reference; `/tags/*` and `/categories/*` are noindex,follow and excluded from the sitemap
- **Security headers** on every response: X-Frame-Options, nosniff, Referrer-Policy, HSTS (CloudFront managed policy)
- **Compression** (gzip) on text responses; real 404s (styled page) for garbage paths; feeds, favicon, apple-touch-icon all 200 with correct content types
- **GA4 live** (G-ZRE3WPMMNK, production-only); **sitemap submitted to Search Console** — fetched same-day: Success, 40 pages discovered

## Found in this round → fixed (commit `8350553`)

1. **No Cache-Control headers on any object** — browsers re-downloaded everything
   on every visit. Deploy workflow now sets 7-day caching for assets and
   `must-revalidate` for html/xml/txt. Verified live.
2. **Homepage weight ~962KB**, dominated by client logos shipped at up to 94KB
   while rendering 32px tall. Logos/avatars resized to display size:
   **homepage now 339KB** (-65%).
3. **7 more titles over ~65 chars** once the ` | AutoSpotting` suffix is added —
   tightened via `seo_title` (on-page H1s unchanged).
4. **~40 body images with empty alts** in older newsletter posts — the
   render-image hook now falls back to a page-title-derived alt. (Hand-written
   alts for the top posts remain a nice-to-have.)
5. One meta description under 50 chars — extended.

## Remaining (non-blocking)

- **CSP header**: the CloudFront managed policy doesn't include one. A starter
  policy is feasible (`script-src 'self' 'unsafe-inline' https://www.googletagmanager.com`)
  via a custom response-headers policy in the website-hosting Terraform.
- **Google Fonts is render-blocking** (preconnects in place). Self-hosting
  Inter + Plus Jakarta Sans as woff2 would remove the third-party dependency.
- **`/index.html` duplicates** serve 200 alongside `/` — harmless (canonicals
  point to the clean URL); could 301 in the CloudFront function for tidiness.
- **GA4 stream URL** still says `autospotting.org` — cosmetic; edit in GA Admin.
- **Accessibility backlog** (from the first review): skip link, `<main>`
  landmark, `aria-expanded` on the nav toggle, `primary-600` button contrast.
- **Watch Search Console** over the coming week for indexing of the new URLs
  and any crawl anomalies from the domain consolidation.
