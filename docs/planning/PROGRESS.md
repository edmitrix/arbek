# ARBEK — Build log

Working file: `index.html` (single self-contained page). Preview locally with `npx serve -l 4173 .` and open http://localhost:4173.
Original v6 prototype kept at `docs/design/prototype-v6-reference.html`.

---

## 2026-09-16 / 17 — Session 1 (baseline established)

### Project setup
- Created the full folder structure (Photos by category, public/images, src/app routes, components, lib, content, db, scripts, tests, docs, .github, .vscode).
- README.md documents the layout, category slugs, photo naming, and next steps. `.gitignore`, `.env.example`, VS Code settings added.
- Domain still undecided: arbek.co or arbek.us (decide at Vercel go-live). Contact address shown on site as hello@arbek.co.

### Site rebuilt from the v6 prototype
- Logo vectorised as SVG symbols (full, emblem-only, wordmark-only); source at `public/images/brand/arbek-logo.svg`.
- Design: black / champagne gold / ivory / bordeaux accent. Cormorant Garamond for display, Jost for UI (Google Fonts).
- Sections: preloader, announcement bar, sticky header with Shop mega-menu, full-height hero ("Wear the presence."), marquee,
  five-category tile grid, Featured Pieces rail, House statement + four pillars, Hair Atelier spotlight, Fragrance & Beauty spotlight,
  Client Services, Q&A accordion, Private List newsletter, footer with large wordmark.
- Working client-side features: bag drawer (localStorage, sized lines), wishlist, live search, mobile menu, toasts, back-to-top.

### Shopping flow
- Category tile / nav / footer link -> **Collection view** (all pieces in that category). "Collection" in the nav shows every piece.
- Piece -> **Gallery modal**: three pictures, arrows, thumbnails, swipe, arrow keys, size picker (clothing XS–XL), Add to bag, Close.
- Clothing requires a size; the size chosen in the gallery is remembered for the card's quick Add.

### Content and photos
- Clothing: four real outfits from `Photos/clothing/`, three shots each ->
  `gold-dot-midi`, `grey-power-suit`, `olive-pleat-set`, `ivory-dot-tiered` (web JPGs in `public/images/products/clothing/`).
- Category tile photos for hair, perfumes, lip-balms, beauty from the single image in each Photos folder (`category.jpg`).
- Other products (wig, bundles, two perfumes, lip balm, glow oil) are still placeholder entries with generated art.
- Conversion snippet for new photos: `scripts/README.md`.

### Fixes along the way
- Shop mega-menu closed before it could be reached: item now spans the full header height, panel spans the header width, hover-intent delay.
- Photo "loaded" flag no longer depends on requestAnimationFrame (worked badly in background tabs).
- Announcement bar no longer wraps on tablets.

### Design decisions settled
- Tile copy stays: number + kicker top, heading + blurb bottom-left, Discover on hover (tried photo-only tiles; reverted by request).
- Navigation: Shop | Collection | Hair | Fragrance | The House, plus Services / Search / Account / Bag on the right.

---

## 2026-09-17 — Session 2 (shipped to production)

### Git + hosting
- Repo initialised on `main` and pushed to GitHub `edmitrix/arbek` (public). 106 files in the first commit; `Photos/` originals are tracked (32 MB, not used by the site).
- Vercel project `arbek` imported from GitHub, Framework Preset "Other", no build step. Every push to `main` deploys to production in about a minute; other branches get preview URLs.
- `vercel.json` sets `"outputDirectory": "."`. Without it Vercel served `public/` as the site root (because that folder exists) and `index.html` returned 404.

### Domain
- **www.arbek.co** is live with HTTPS; `arbek.co` 308-redirects to www. Fallback: `arbek.vercel.app`.
- Registrar is GoDaddy, but the nameservers are delegated to `ns1/ns2.vercel-dns.com`, so **all DNS records live in Vercel** (project → Settings → Domains → DNS Records). GoDaddy's DNS page is dead weight and refuses edits with `DNSZoneExternalNameserver`.
- Gotcha hit on the way: the domain was first added to Vercel as `arbek.com` instead of `arbek.co`.
- `.env.example` `NEXT_PUBLIC_SITE_URL` now points at `https://www.arbek.co`.

### Hero photo
- Hero background is now a photo: `public/images/banners/hero.jpg` (214 KB, 1672×941, from `Photos/banners/hero.png`). Preloaded with `fetchpriority="high"`.
- Layers over it: `.veil` gradient (dark bottom + left for the copy), glow at .45, grain at .32. The heron `.hero-emblem` is hidden while the photo is in place (JS still targets it harmlessly).
- Phone (≤900px): hero gets `46vh` extra top padding so the face sits clear above the headline; image anchored `44% 0%`.
- Flagged: the hero shows Bath & Body Works and Nine West branding, same concern as the category photos.

---

## Open items / next steps
- [ ] Photos for the remaining products (wig, bundles, perfumes, lip balm, beauty), three per piece, named `<id>-1..3.jpg`.
- [ ] Replace branded third-party product photos (Chanel, Victoria's Secret, Bath & Body Works) before launch unless ARBEK retails them.
- [ ] Real product names, descriptions and prices (current ones are placeholders).
- [x] Domain: arbek.co, live on Vercel (see Session 2).
- [ ] Checkout / payments (Stripe assumed), accounts, newsletter backend (Supabase was mentioned in the v6 prototype).
- [ ] Account page, Privacy, Terms, Instagram/TikTok links are still `#` placeholders.
- [ ] Consider self-hosting the two Google Fonts.
- [ ] Move into Next.js on Vercel when the single-file prototype is signed off.
