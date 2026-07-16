# AutoSpotting Website Review — 2026-07-16

Full review of the saasify-theme migration branch (`migrate-to-saasify-theme`), run by four
parallel review agents (broken links, SEO, best practices, in-browser visual QA) against the
local dev server, plus a copy/asset comparison with the archived autospotting.io page.

**Canonical domain decision: `https://autospotting.io`** (confirmed 2026-07-16).
`autospotting.org` and both `www.` variants now 301-redirect to it via `netlify.toml`.

---

## Review scope and numbers

- 185 pages crawled, 470 internal URLs and 242 external URLs checked
- Rendered HTML and production build (`hugo --gc --minify`) audited for SEO
- Netlify/build/deploy configuration audited
- Homepage, blog index + pagination, old and new blog posts, FAQ and 404 reviewed
  visually in Chrome (desktop; mobile via markup analysis — the extension couldn't
  emulate a real 390px viewport)

## Fixed in this session (one commit per concern)

| Commit | Fix |
|---|---|
| `3d994db` | `.gitignore` covers `node_modules/`, `resources/_gen/`, backups, build artifacts |
| `9cd05c1` | Baseline commit of the in-progress theme migration (324 files) |
| `6a9b11d` | **FAQ**: nav pointed at `/faq.html`, a stale old-theme artifact that would 404 in production. The FAQ section on the homepage now has an `id="faq"` anchor, and a contact CTA ("Still have questions?" with Email Us / Book a Call buttons, copy reused from the old site's "Need help?" section) is built into the FAQ section styling |
| `3533794` | All menu anchors (`#features`, `#testimonials`, `#pricing`, FAQ) are now `/#...` — the bare fragments dead-ended on every non-homepage page (~1,800 broken anchor occurrences) |
| `911510b` | `baseURL` was `"/"` → canonicals, og:url, sitemap `<loc>` and RSS links were all relative (schema-invalid, ignored by Google). Also: robots.txt now generated; deprecated `paginate` keys replaced (they made Hugo emit junk `/6/` and `/page/` pages into the sitemap); duplicated `content/blog/leanercloud-backup/` section (29 posts with identical titles/descriptions competing with the real ones) excluded from the build |
| `39f82bb` | Head/meta overhaul: home `<title>` uses the real page title (was bare "AutoSpotting"); `og:image`/`twitter:image` now fall back to each post's `featured_image` (previously **no social image at all, sitewide**); `og:type=article` only on posts; `article:published_time`/`modified_time`; RSS `<link rel=alternate>`; JSON-LD (Organization + WebSite on home, BlogPosting on posts); GA loads only in production |
| `4413bcc` | 79 broken tag/category links: theme hardcodes extensionless `/tags/x` which 404s under `uglyURLs=true`; overridden templates resolve real permalinks |
| `04bcd34` | Styled 404 page (production had none — visitors got Netlify's default; the dev server's old-meghna 404 was a stale artifact) |
| `97ce953` | Footer "Privacy" link removed — `/privacy.html` never existed on either live domain (soft-404s) |
| `893ad35` | **Deploy-breaking**: `tailwind.config.js` referenced a machine-local Hugo cache path; on Netlify every theme class would be purged and styling would silently break. Theme vendored into `_vendor/` |
| `3fce8a3` | `HUGO_VERSION` 0.152.2 → 0.164.0 (what the migration was tested on), Node 18 (EOL) → 20; security headers (X-Frame-Options, nosniff, Referrer-Policy, Permissions-Policy); cache headers; `.org` → `.io` domain 301s |
| `8fcd59c` | `favicon.ico` was a symlink (Hugo doesn't publish those, so production served the theme author's favicon); real multi-size ICO + apple-touch-icon shipped |
| `db80371` | Broken content links: mangled beehiiv/hemingwayapp URLs, dead Gitter link, dead mp4 embeds (files gone from old host), malformed relative link, de-linked an SQS URL that leaked an AWS account id |
| `e94b674` | Reuse from archived site: testimonials by Pierre Allétru (postale.io), Jacob Cooper (Flip CX), Klas Wikblad (APPRL) with recovered avatars/logos; avatar-less entries get an initials fallback; marquee duplicate hidden from screen readers |
| `0bb23d4`, `45454f6` | Reuse: "See AutoSpotting in Action" homepage section with the intro and 85%-savings demo videos (privacy-enhanced youtube-nocookie embeds) |
| `9008527` | Post body images lazy-load via render hook; above-the-fold featured image now `eager` + `fetchpriority=high` (was lazy — hurt LCP); dead mobile-menu script removed |
| `1fd5e6e` | Five 0.5–1.4 MB PNGs recompressed to JPEG (~5.1 MB → ~0.5 MB, 13+ posts affected); 2400px screenshot resized; legacy `/faq`, `/products`, `/author/*` 301s |

## Decisions needed from you

1. **Google Analytics**: the old `UA-27789976-3` is a Universal Analytics property — dead
   since July 2023, it collected nothing while loading gtag.js on every page. The loader
   code is still in place; add a GA4 measurement id in `hugo.toml` (`googleAnalytics =
   "G-..."`) to re-enable analytics. If you'd rather keep the UA id for sentimental
   reasons it can be restored, but it does nothing.
2. **Slack invite links**: the `join.slack.com/t/cloudutil/...zt-...` invite used in ~5 blog
   posts (and the old site's Need help section) returns 403 — almost certainly expired.
   Provide a fresh permanent invite URL and I'll update the posts and can add a "Join our
   Slack" button to the FAQ contact CTA.
3. **Privacy policy**: no privacy page has ever existed. Once GA4 (or any tracking) is
   active, a real privacy policy is advisable (GDPR). I removed the dead footer link;
   say the word and I'll draft a page for review.
4. **Old-theme leftovers** (all report-only, nothing deleted): `config.toml` +
   `config.toml.backup` (meghna config, ignored by Hugo 0.164 but confusing),
   `themes/meghna-hugo` + `themes/universal` submodules, `data/en/*.yml` (unreferenced),
   `.backup/`, `i18n/`, and the `Makefile` that still deploys to S3/CloudFront (conflicts
   with the Netlify pipeline — risk of clobbering if someone runs `make`). The five
   original oversized PNGs are also still on disk next to their `.jpg` replacements.
   All of these can be removed once you confirm.
5. **Feature illustrations**: the seven homepage feature blocks use large emoji as
   illustrations, which reads as placeholder. `static/images/features/` contains unused
   images — want me to wire them in (or generate proper illustrations)?
6. **Netlify domain setup**: the `.org → .io` redirects only work if both domains are
   attached to the Netlify site. Also note **the live autospotting.org currently serves
   from S3/CloudFront (nginx headers), not Netlify** — the DNS/hosting cutover plan needs
   to include pointing both domains at Netlify (or porting the redirects to CloudFront).

## Remaining nice-to-haves (not blocking)

- **Taxonomy page bloat**: 120 of ~150 sitemap URLs are thin `/tags/*`/`/categories/*`
  pages with auto-generated titles. Consider `noindex` on term pages or trimming the tag
  vocabulary.
- **Long post titles**: several posts exceed ~60 chars (SERP truncation), e.g. the
  Vantage post at 92 chars. A `seo_title` param or shorter titles would help.
- **Missing image alts**: ~8 body images across two leanercloud posts lack alt text.
- **Self-host fonts**: Google Fonts is render-blocking (preconnects exist); self-hosted
  woff2 with `font-display: swap` would remove the third-party dependency.
- **Old post redirects**: if Google Search Console shows indexed URLs from the old meghna
  site structure beyond `/faq.html`/`/author/*`, add matching 301s.
- **Accessibility**: no skip-link or `<main>` landmark; nav toggle lacks
  `aria-expanded`; `btn-primary` white-on-`primary-600` is ~3.7:1 contrast (AA needs
  4.5:1 — consider `primary-700`); related-post card images reuse the current post's
  title as alt; `assets/css/main.css` references an undefined `secondary` palette.
- **Header translucency**: hero text visibly bleeds through the `bg-white/80` fixed
  header when scrolling (cosmetic).
- **Editorial**: some imported LeanerCloud posts are personal rather than
  AutoSpotting-related ("Declined a Job Offer...", OpenTofu commentary) — review whether
  they belong on the company blog.
- **Savings tools**: the old site linked a savings calculator spreadsheet
  (`bit.ly/LCSavingsCalculator`) and the [savings-estimator GUI]
  (https://github.com/LeanerCloud/savings-estimator) plus a demo video (`VXfCOXXtLwA`) —
  none are on the new site; could make a good "Estimate your savings" section.

## Verification performed

- `hugo --gc --minify` builds clean: 0 errors, 0 warnings, 206 pages (was 238 with the
  duplicate/junk pages)
- Full Netlify-equivalent build (`tailwindcss` + `hugo`) run locally end-to-end
- robots.txt, sitemap (absolute URLs, no junk/duplicate entries), RSS discovery, JSON-LD,
  og:image, titles all verified in the built output
- Tag/category links, FAQ anchor, contact CTA, testimonials and video embeds verified on
  the running dev server
- Browser QA (Chrome) confirmed homepage/blog/posts render cleanly with zero console errors
