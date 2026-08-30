# BUILD-NOTES — sltk-news-mock

Built: 2026-08-30  
Builder: Lasagne (claude-sonnet-4-6 subagent)  
Purpose: Mock news section for SL Tervisekeskus website, ready for review and later deployment to sltk-live.

---

## Files created

```
sites/sltk-news-mock/
  assets/
    site.css              — copied from sltk-live/assets/site.css
    main.js               — copied from sltk-live/assets/main.js
    favicon.svg           — copied from sltk-live/assets/favicon.svg
    apple-touch-icon.png  — copied from sltk-live/assets/apple-touch-icon.png
    site.webmanifest      — copied from sltk-live/assets/site.webmanifest
  uudised.html            — News listing page (1 article card)
  uudised/
    taiskasvanute-vaktsineerimine.html  — Full article page
  BUILD-NOTES.md          — This file
```

---

## Design elements copied from live site

- **emergency-bar** — identical markup to sltk-live
- **site-header** with logo, desktop nav (all links preserved), mobile hamburger
- **mobile-nav** panel with full link list
- **page-header** section with breadcrumb + h1
- **site-footer** — identical 4-column grid, footer-legal, copyright
- **mobile-cta** floating bar
- CSS classes: `.section`, `.container`, `.prose`, `.btn`, `.epak-box`, `.contact-row`, etc. (from site.css)
- Typography: Poppins (Google Fonts), same weights as live site
- GA4 tag: G-E8RS05GMN6 (same as live; noindex mock so no skew)

## Design additions (news-specific)

Both pages include `<style>` blocks with news-specific CSS:
- **uudised.html**: `.news-list`, `.news-card`, `.news-card-meta`, `.news-card-date`, `.news-card-tag`, `.news-card-title`, `.news-card-teaser`, `.news-card-cta`
- **article page**: `.article-meta`, `.source-note` (green left-border), `.notice-box` (new-info highlight), `.references-section`, `.article-cta` (dark green CTA box), `.article-timestamp`, `.back-link`

These supplement site.css without modifying it and are ready to be moved into site.css at deployment.

## Navigation changes vs. live site

Added **"Uudised"** link between Materjalid and Kontakt in both desktop nav and mobile nav. This is the only nav addition — all other links unchanged.

Footer: added `<li><a href="/uudised.html">Uudised</a></li>` to the "Patsiendile" column.

---

## Relative asset paths

| Page | CSS | JS | Favicon |
|------|-----|----|---------|
| `uudised.html` | `assets/site.css` | `assets/main.js` | `assets/favicon.svg` |
| `uudised/taiskasvanute-vaktsineerimine.html` | `../assets/site.css` | `../assets/main.js` | `../assets/favicon.svg` |

Internal links between pages use relative paths and resolve correctly.

---

## Content sourcing

**Article**: täiskasvanute-vaktsineerimine.html  
**Source facts from**: https://www.terviseportaal.ee/haiguste-ennetus/vaktsineerimine/taiskasvanute-vaktsineerimine (fetched 2026-08-30)  
**Secondary source linked**: https://terviseamet.ee/nakkushaigused/vaktsineerimine  
**Regulation PDF linked**: https://www.riigiteataja.ee/aktilisa/1150/7202/6005/SOM_m32_lisa2_2026.pdf  

All facts are:
- Sourced from official Estonian state health portals (Terviseportaal, Terviseamet, Riigi Teataja)
- Attributed inline with source notes
- No statistics invented; no Cleveland Clinic cited; no fabricated claims
- Written in SLTK's own voice — not verbatim copy from Terviseportaal
- Contact info (address, phone, hours) copied exactly from live kontakt.html: Sepapaja 12/1 III k, +372 613 8887, E-R 8:00-18:00

---

## DEPLOYMENT-CHECKLIST.md status (mock pass)

| Item | Status | Notes |
|------|--------|-------|
| `<title>` unique, keyword-rich | ✅ | Both pages |
| `<meta description>` ~140-160 chars | ✅ | Both pages |
| `<link rel="canonical">` | ✅ | Absolute URLs to sltervisekeskus.ee |
| `hreflang="et"` + `x-default` | ✅ | Both pages |
| OG tags (type, url, title, desc, image, locale, site_name) | ✅ | Both pages |
| Twitter Card tags | ✅ | Both pages |
| Schema.org JSON-LD | ✅ | BreadcrumbList on listing; NewsArticle + BreadcrumbList on article |
| Sitemap entry | ⏭ SKIPPED | Mock only — add at deployment |
| IndexNow submission | ⏭ SKIPPED | After deployment |
| Internal inbound links | ⏭ PENDING | Add link from index.html at deployment |
| `robots` meta | ⚠️ noindex,nofollow | Change to `index,follow` at deployment |
| GA4 / GTM | ✅ | Same tag as live site |
| Header/footer | ✅ | Identical to live site |
| `lang="et"` on `<html>` | ✅ | Both pages |
| Mobile-responsive | ✅ | Uses site.css |
| Accessibility | ✅ | skip-link, ARIA labels, hamburger button |
| CTA | ✅ | e-Perearstikeskus CTA on article page |

---

## Asset caveats

1. **`favicon.ico`** — not copied (not needed for mock; favicon.svg covers modern browsers).  
   At deployment: copy `sltk-live/favicon.ico` to root.
2. **`og-default.jpg`** — not copied (referenced via absolute URL `https://www.sltervisekeskus.ee/assets/og-default.jpg`). No issue at deployment.
3. **hero.mp4** — not referenced by news pages; not copied.
4. **Navigation links** (e.g. `/broneerimine.html`, `/materjalid.html`) use absolute `/`-prefixed paths matching the live site structure. These won't navigate locally in the mock but the design renders correctly since all CSS/JS assets are relative and present.
5. **`style.css`** in assets — not copied (unused; site.css is the active stylesheet).

---

## Deployment steps (when ready)

1. Copy `uudised.html` → `sltk-live/uudised.html`
2. Copy `uudised/taiskasvanute-vaktsineerimine.html` → `sltk-live/uudised/taiskasvanute-vaktsineerimine.html`
3. In both files: change `noindex,nofollow` → `index,follow`
4. Add inbound link in `sltk-live/index.html` (e.g. in resources section or footer)
5. Add sitemap entries (priority 0.7 for listing, 0.8 for article)
6. Submit to IndexNow
7. Add news-specific CSS classes from `<style>` blocks into `assets/site.css` (optional cleanup)
