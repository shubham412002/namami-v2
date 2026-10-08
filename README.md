# Namami Import & Export — Website

A 2-page marketing site for Namami Import & Export: a global import-export
trading company that sits under the wider Namami Parent Group. No build
step, no dependencies to install — every page is a single self-contained
HTML file (inline CSS + JS) so it can be opened directly or dropped onto
any static host as-is.

## Structure

```
namami-website/
├── index.html          Homepage — hero, services, about, team, stats,
│                        why-us, products, industries, global network,
│                        Namami Family / parent group, testimonials,
│                        news teaser, contact form, CTA popups
├── blog.html            Separate Blog route — newsletter signup +
│                        article listing (shares the same header,
│                        footer and design system as index.html)
└── assets/
    └── images/
        └── README.txt   Exact filenames the site expects for real
                          photography, and what each one is for
```

## Running it

There's nothing to build or install. Either:

- Open `index.html` directly in a browser, or
- Serve the folder with any static file server, e.g.:
  ```
  npx serve .
  # or
  python3 -m http.server 8080
  ```

Opening `index.html` via `file://` works fine too — the only thing that
behaves differently is that the `assets/images/...` fetches will 404
faster/slower depending on the browser, which just means you'll see the
illustrated fallback instead of a real photo (expected until real images
are added — see below).

## What's implemented

- Full one-page homepage per the site structure in the brief: hero,
  6-service quick strip, About + animated stat counters, Why Namami,
  client-logo marquee, Services (click-through detail modals),
  Industries (click-through detail modals with a catalogue-download
  stub), Products, Global Network map, Namami Family + Namami Parent
  Group explainer, Team / working professionals section, testimonial
  carousel, News teaser, full contact form, and a footer.
- **Blog route** (`blog.html`) — a genuinely separate page (not an
  anchor), with a newsletter subscribe block and an 8-article listing
  that loads 6 then reveals the rest, each opening a read-more modal.
- **CTA lead-capture popup** — fires once, 20s after page load, and
  captures Name / Email / WhatsApp Number only (separate from the full
  contact form). Recently tightened up so all three fields fit without
  scrolling on a normal laptop viewport, with a slimmer styled
  scrollbar for anyone on a shorter screen.
- **Namami Parent Group** panel above the sister-brand grid, and the
  four real sister brands (My Ocean Logistics, All About Cargo, Let's
  Unite, Ocean Tech Journal) with their real focus areas.
- **Typography: Roboto** throughout (headings at weight 900, body copy
  at 400–700), loaded from Google Fonts.
- **Real photography.** Every photo spot (hero, about, all 6 service
  cards, 3 product cards, 3 news cards, 8 blog articles) is wired to a
  real, licensed Pexels stock photo by URL — see
  `assets/images/README.txt` for exactly which photo is used where and
  how to swap in the client's own photography later. If a photo URL
  ever fails to load, the page automatically falls back to the same
  brand-colored SVG illustration used previously, so nothing breaks.
- **Animated 3D globe (amCharts 5)** in the Global Network section:
  a real-geography rotating globe (orthographic projection) built from
  `am5map` + `am5geodata_worldLow`, with pulsing pins at India (hub),
  UAE, Europe, Australia and the Americas, and animated dashed lines
  radiating from the India hub to each region. It auto-rotates slowly
  and can be dragged. This loads from the amCharts CDN at runtime; if
  that CDN is ever unreachable (offline demo, restrictive network),
  the original static illustrated map already in the page is shown
  instead automatically — see the "Note on external CDNs" section
  below.
- Fixed a clipping bug on the service-card icon badges: the round icon
  badge that straddles the photo/body boundary on each of the 6
  service cards was previously being cut off by its parent's rounded-
  corner clipping. It's now a sibling element with its own stacking
  order, so it always renders as a full circle.
- Animation stack: IntersectionObserver scroll reveals, count-up stat
  counters, CSS marquee, magnetic buttons, 3D tilt cards, GSAP
  ScrollTrigger parallax (all degrade gracefully — nothing breaks if
  GSAP fails to load or `prefers-reduced-motion` is set).

### Latest round: video, smooth scroll, SEO & Core Web Vitals

- **"Freight In Motion" video section** — a new full-bleed cinematic section
  (between "Why Namami" and "Services") with a real, licensed Pexels cargo-
  ship video looping muted in the background, à la the video-heavy sections
  on Apple's product pages. Performance-conscious by design: the video has
  `preload="none"` and no source until the section actually scrolls into
  view (via `IntersectionObserver`), so it never competes with the hero
  image for bandwidth on initial load; a fast-loading poster image is shown
  immediately; and it's skipped entirely (poster stays static) for visitors
  with `prefers-reduced-motion` or Data Saver mode on. For production, swap
  the embedded Pexels URL for a properly transcoded/compressed version of
  the client's own footage served from a real video host (Cloudflare
  Stream, Mux, Bunny, etc.) — hotlinking the raw Pexels file works but a
  dedicated video CDN with adaptive bitrate will perform better at scale.
- **Smooth scroll on every in-page anchor link** (nav, hero/CTA buttons,
  footer links, "Explore" links) — clicking now eases the page into the
  target section (`scrollIntoView({behavior:'smooth'})`) instead of
  jumping instantly, with the URL hash still updating. `prefers-reduced-
  motion` visitors get an instant jump instead, by design. A base CSS
  `scroll-behavior: smooth` is also set as a fallback. Cross-page links
  (e.g. Blog → `index.html#services`) still land the normal browser way on
  initial page load, matching standard behaviour on effectively every
  site, Apple's included.
- **On-page SEO**: proper `<title>`/meta description per page, `robots`
  meta, canonical links, Open Graph + Twitter Card tags (so links look
  right when shared), an `Organization` JSON-LD block, a favicon, and a
  `sitemap.xml` + `robots.txt` at the project root.
- **Core Web Vitals attention**: the hero photo (the page's Largest
  Contentful Paint element) is preloaded with `fetchpriority="high"` and
  explicit `width`/`height` to avoid layout shift; preconnects are set up
  for every third-party origin actually used (fonts, Pexels images/video,
  amCharts, cdnjs); below-the-fold images are `loading="lazy"`; and the
  new video section is fully deferred as described above so it can't hurt
  load performance.
- **Honest note on "getting the site to the top" of search results**: the
  changes above are the on-page/technical SEO and performance work that's
  within a website's own code — they help a lot, but search ranking also
  depends on things outside the HTML: real content depth, backlinks/
  domain authority, business listings (Google Business Profile), site
  age, and competition for the target keywords. There's no code change
  that guarantees a #1 ranking; treat this as a strong technical
  foundation, not a ranking guarantee.
- **PLACEHOLDER DOMAIN**: canonical links, Open Graph URLs, the JSON-LD
  block and `sitemap.xml`/`robots.txt` all currently point at
  `https://www.namamiimportexport.com/` as a placeholder — search for
  that string across the files and replace it with the real production
  domain before launch.

### Note on external CDNs (fonts, photos, amCharts)

This build intentionally uses real external URLs for fonts (Google
Fonts), photography (Pexels) and the animated globe (amCharts CDN).
These all load normally in a regular browser with internet access —
open `index.html` on a normal laptop/phone and everything renders as
designed. Every one of these has a graceful fallback if it can't load
(system font, SVG illustration, static map respectively), so the site
never breaks — it just looks plainer. If you need the site to run with
zero external dependencies (fully offline, or inside a locked-down
preview tool), ask and it can be reverted to the local-asset-only
pattern used in the previous round.

## Known placeholders (things to swap in when available)

These are called out in the UI itself (small "placeholder" notes near
stats, the marquee, testimonials, and the Family section) so nothing is
silently fake:

- **Photography is generic stock, not the client's own.** Real Pexels
  photos are now wired in (ports, warehouses, freight, offices) so the
  site no longer looks empty/illustrated, but none of it is Namami's
  own branded photography yet — see `assets/images/README.txt` for
  what's used where and how to swap it out.
- **Sister-brand URLs** — the four Namami Family cards link to `#` with
  a "coming soon" toast until real domains are supplied (names/focus
  areas are already real, from the brief).
- **Team members** — About's team section has 4 placeholder profiles
  (initials avatar, generic role titles); swap in real names, roles and
  LinkedIn URLs in the `TEAM` array near the bottom of `index.html`.
- **Contact details** — phone/email/address/support-hours in the
  Contact section and footer are still `[COMPANY PHONE]` etc.
  placeholders.
- **WhatsApp number** — the floating WhatsApp button shows a "coming
  soon" toast until `WHATSAPP_NUMBER` is set (search for it near the
  bottom of the script in `index.html` and `blog.html`).
- **Stats** (50+ countries, 1,200+ shipments, 850+ clients, 25+ years)
  are layout placeholders, flagged as such under the stats band.
- **Forms have no backend.** The contact form, CTA lead form, and blog
  newsletter signup all validate client-side and show a success
  message, but nothing is sent anywhere yet — wire them to your email
  service / CRM / ESP of choice (Mailchimp, etc. for the newsletter).
- **Client logos** in the "Trusted By" marquee are text placeholders.

## Editing content

Nearly everything is data-driven from small arrays near the bottom of
each file's `<script>`, so most content changes don't require touching
HTML:

- `SERVICES`, `INDUSTRIES`, `PRODUCTS`, `NEWS`, `TEAM`, `FAMILY_BRANDS`,
  `TESTIMONIALS` in `index.html`
- `ARTICLES` in `blog.html`

Each entry carries its own copy, icon, color tone, and (where relevant)
its `assets/images/...` filename — add, remove or edit an entry and the
grid/card re-renders automatically.
