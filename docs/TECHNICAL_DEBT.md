# Technical Debt & Backlog

Living document. Captures everything pending after the v1.0.0 ship of the Guardrails First landing site (live at <https://www.guardrailsfirst.com>).

**Last updated**: 2026-05-03 (post-v1.0.0 perf fix).

## Status snapshot at v1.0.0

- Live at `https://www.guardrailsfirst.com`
- Lighthouse mobile: **Performance 100 · Accessibility 98 · Best Practices 100 · SEO 100**
- Custom domain validated, TLS auto-issued by SWA
- DNSSEC enabled at Namecheap (clean chain on `guardrailsfirst.com`; `araujoinnovations.ch` still propagating to `.ch` parent)
- Single CTA → `https://guardrailsfirst.substack.com`
- OG card validated via opengraph.xyz

---

## Pre-launch (before first newsletter publish)

These genuinely block sending mail or shipping the show. Sort before publishing essay #1.

### Mailing address — set up Postfach

- **Why**: CAN-SPAM Act requires every commercial email to include a physical mailing address. Substack pre-fills theirs (548 Market Street, San Francisco) as a placeholder for new publications.
- **What**: Set up a Swiss Post Postfach (~CHF 80-160/year) at post.ch. Replace the Substack default with the Postfach address.
- **Where to update**: Substack → Settings → Publication details → Mailing address.
- **Trigger**: before the first newsletter goes out. Until then, the placeholder isn't being sent anywhere.
- **Alternatives considered**: virtual office (Regus / IWG) at CHF 50-100/month — pricier but a real street address; coworking membership (Impact Hub, Westhive) at CHF 100-300/month with extras. Postfach is the cheapest legitimate option for a solo content side-project.

### Substack publication header — wife review + upload

- **Files**: `pictures/substack-header.png` (composited final), `pictures/substack-header-bg.png` (GPT-generated background, before logo + text overlay).
- **Why uncommitted**: pending wife-review pass; may iterate on aesthetics. Once approved, lock by committing.
- **Action when approved**:
  1. Upload via Substack → Settings → Publication details → Cover image
  2. Commit both PNGs:

     ```powershell
     git add pictures/substack-header*.png
     git commit -m "Lock Substack publication header as versioned brand asset"
     git push
     ```

### Substack About page

Already populated, but verify content reads well next time you log in. Edit at Substack → Pages → About if needed.

---

## Deferred social icons (from original v1 brief)

V1 ships with three icons in the footer: **Substack, YouTube, email**. The original brief listed eight properties; the deferred five are captured below so we don't lose track. Source of truth: `src/components/SocialIcons.astro`.

| Property | Handle / URL | Status | Add when |
|---|---|---|---|
| **X / Twitter** | `https://x.com/guardrailsfirst` | In progress | Account is active and at least one post exists |
| **Bluesky** | `https://bsky.app/profile/guardrailsfirst.com` (domain-as-handle) | Planned | After registering and adding `_atproto` TXT DNS record (see below) |
| **LinkedIn** | TBD — Real Agents Academy company page URL | Not yet created | When the RAA company page is set up. Note: this lives under the umbrella, not the GF show. |
| **Instagram** | TBD handle | Open question | Open question: does Instagram fit a regulated-AI practitioner audience? Decide before adding. |
| **Threads** | TBD handle | Same as Instagram | Same as Instagram |
| **GitHub** | `https://github.com/realagents-academy` | Org exists | Probably skip — GitHub org is operational, not consumer-facing. Open decision. |

### Bluesky `_atproto` TXT setup

When ready to claim `guardrailsfirst.com` as a Bluesky handle:

| Type | Host | Value | TTL |
|---|---|---|---|
| TXT | `_atproto` | `did=<your-bluesky-DID>` | Automatic |

The record sits on a unique host label and won't conflict with existing M365, SWA, or GitHub TXT records. After publishing, claim the handle in Bluesky settings.

### Adding a new social icon

1. Add the icon's SVG path to `src/components/SocialIcons.astro` (the JSX-style conditional rendering block)
2. Add an entry to the `links` array at the top of the same file
3. `npm run build` to verify, then commit and push — auto-deploys

Keep the icon set tight. Three icons working is still better than eight with broken links.

---

## Performance & polish

### WebP/AVIF logo conversion (low priority)

- **Lighthouse audit said**: 642 KiB additional savings available via modern image format
- **Current state**: PNG at 52.7 KB after the v1.0.0 perf fix. Lighthouse Performance is 100/100.
- **Why deferred**: No regression to fix. Only worth the work if perf score drops or LCP creeps back up.
- **How**: Generate `public/logo.webp` alongside `public/logo.png`. Use `<picture>` element in `Hero.astro` with `<source type="image/webp">` and the PNG as fallback.

### `favicon-512.png` compression

- **Current**: 233 KB (System.Drawing's PNG encoder doesn't apply aggressive compression)
- **Target**: < 30 KB
- **Why it matters less**: not loaded on main page render — only fetched by Android home screen / PWA install. Lighthouse may flag eventually but doesn't right now.
- **Fix**: run `pngquant` or `oxipng` on the file. Quickest path: `npx pngquant --quality=80-95 --output public/favicon-512.png public/favicon-512.png`.

### Inter font weight trim (low priority)

- Currently load 400 / 500 / 600 / 700 (4 weight files via `@fontsource/inter`)
- Could trim to 400 + 600 if visual impact is minimal — saves ~80 KB across requests
- Verify visually before trimming: Hero uses 700 (bold) for the title, 500 (medium) for the tagline, 600 for the CTA, 400 for body

### SVG version of GF monogram

- Source PNG at `pictures/GuardrailsFirstLogo.png`. **No vector source exists** — designer-output PNG only.
- Vectorize via auto-trace (Adobe Illustrator's Image Trace, vectorizer.io, or potrace) or hand-rebuild from the geometry
- Once we have an SVG: use as `public/favicon.svg` (modern browsers prefer this), inline it in Hero for crisper scaling, derive other sizes deterministically

---

## Brand assets & identity

### Real Agents Academy visual identity (umbrella brand)

- Currently using a placeholder orange "RA" mark at `pictures/RealAgentsAcademyLogo.png` — uploaded as the GitHub org avatar
- **No defined RAA color palette, typography, or aesthetic direction** — distinct from Guardrails First's navy/teal
- Decision needed before launching paid courses or building the RAA umbrella site
- Should be intentionally different from GF (umbrella ≠ show) but coherent enough that they read as related properties

### Substack accent color

- Already changed to brand teal (verified during v1.0.0 session)
- If the wife review of the header surfaces any other Substack styling concerns, capture them here

---

## DNS / infrastructure

### DNSSEC chain on `araujoinnovations.ch`

- Currently in **Insecure delegation** state — DS record not yet at `.ch` parent (SWITCH)
- Recheck at <https://dnsviz.net/d/araujoinnovations.ch/dnssec/> from **2026-05-04** onward
- **Action if still Insecure after 2026-05-05**: contact Namecheap support about DS publication to SWITCH. Compare what Namecheap → Domain → DNSSEC tab shows vs what `dig DS araujoinnovations.ch @ns1.switch.ch` returns. Mismatches → support ticket.

### Apex domain HTTPS coverage (only if it bites)

- Currently: bare-apex `https://guardrailsfirst.com` returns no cert (TLS not served at apex). HTTP redirect via Namecheap URL Redirect goes to https-www.
- **Edge case**: users with HTTPS-First browser mode who type `https://guardrailsfirst.com` get a connection error. Workaround: type bare apex or use www directly.
- **Fix when needed**: attach apex to SWA properly via TXT validation (`_dnsauth`) + ALIAS record pointing at `<your-swa>.azurestaticapps.net`. Apply to all his sites for consistency.
- **Why deferred**: chose www-only with apex URL Redirect to mirror the existing araujoinnovations.ch pattern. If this pattern bites either site, fix both together.

### HSTS preload submission

- After the site has been live for a few weeks with stable HSTS headers (already present in `staticwebapp.config.json`), submit `guardrailsfirst.com` to <https://hstspreload.org/>
- Embeds the domain in browser HSTS preload lists — every visitor's browser knows to use HTTPS even on first visit
- **One-way decision**: removing from preload takes weeks-to-months. Only submit when confident the domain stays HTTPS forever.

### Future Bluesky `_atproto` TXT

Captured under "Deferred social icons → Bluesky" above. Add when claiming the handle.

---

## Future architecture (v2)

### Essay archive

- Drop `.mdx` files into `src/pages/essays/[slug].mdx`
- Add `src/layouts/Essay.astro` for essay-specific layout (title, publish date, reading time, author byline, related-essay links)
- Astro routes them automatically based on filename — no router config needed
- This is the migration the Astro choice was specifically made to support cleanly

### Dynamic per-essay OG images

- Replace static `public/og-image.png` with build-time generation
- Use `@vercel/og` or a satori-based Astro integration
- Each essay gets a unique 1200×630 card: essay title + tagline + author byline + brand background
- ~30-min refactor when essay archive ships

### Sitemap + RSS

- Add `@astrojs/sitemap` integration to `astro.config.mjs`
- Restore the `Sitemap:` line in `public/robots.txt` (currently removed because the file referenced doesn't exist yet)
- Add `@astrojs/rss` for newsletter-style RSS feed (subscribers using readers like Feedly / Inoreader / NetNewsWire)

### Analytics

- Add Cloudflare Web Analytics when there's content cadence to measure
- Free, no third-party script needed if you proxy DNS through Cloudflare; otherwise one tiny script
- Don't add anything until the first essay ships — nothing to measure yet

### Real Agents Academy umbrella site

- New repo: `realagents-academy/realagents-academy-site`
- New SWA in same RG (`rg-realagents-academy-prod`)
- Same DNS pattern (www-only, apex URL Redirect at Namecheap, `realagents-academy.azurestaticapps.net` target)
- **Trigger**: when first paid course is ready to launch

### Course funnel mechanics

- Payment + content delivery TBD (Stripe + Substack paid? gumroad? podia? a custom integration?)
- Won't ship under the Guardrails First brand — lives under Real Agents Academy
- Affects the umbrella site's architecture, not the GF show site

---

## Code / repo housekeeping

### GitHub Action checkout version

- Currently `actions/checkout@v3` in `.github/workflows/azure-static-web-apps-agreeable-mushroom-08e58f503.yml`
- Bump to `@v4` or `@v5` whenever convenient. `@v3` still works.

### npm vulnerabilities

- 5 moderate-severity vulns reported during `npm install`
- Run `npm audit` to enumerate; almost certainly transitive dev-only (vite, esbuild internals)
- Verify they don't affect runtime / production output. If clean, accept and move on.
- Re-check periodically — some get patched upstream without intervention.

### Astro version range

- Currently `"astro": "^6.2.1"` (caret accepts any 6.x)
- Tighten to `"~6.2.1"` (patch only) if breaking changes in minor versions become a concern. Astro 6 → 7 will absolutely require manual review.

---

## Out of scope (not on the roadmap)

- CMS / admin panel
- Comment system on essays (Substack handles this)
- User auth / login on the landing site
- Newsletter list management outside Substack
- Cookie banner (no cookies set, no analytics)
- Self-hosted email (M365 + Substack handle inbound and outbound)
- Browser support for IE / pre-2020 Edge / pre-Chromium browsers
