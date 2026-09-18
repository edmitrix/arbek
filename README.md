# Arbek

Fashion and beauty store selling clothing, perfumes, hair (wigs and weaves), lip balms, and beauty products.

- **Domain:** `www.arbek.co` or `www.arbek.us` (decided at go-live on Vercel)
- **Hosting:** Vercel
- **Planned stack:** Next.js (App Router) + TypeScript + Tailwind CSS, Stripe for payments

## Product categories

| Slug        | Category                 |
|-------------|--------------------------|
| `clothing`  | Clothing                 |
| `perfumes`  | Perfumes                 |
| `hair`      | Hair (wigs and weaves)   |
| `lip-balms` | Lip balms                |
| `beauty`    | Beauty products          |

The same five slugs are used everywhere: `Photos/`, `public/images/products/`, `src/data/`, and the `/collections/[category]` routes.

## Working prototype

- **[index.html](index.html)** is the live prototype we work from until launch. It is a single self-contained file:
  the ARBEK logo is inlined once as SVG symbols (`#arbek-logo`, `#arbek-emblem`, `#arbek-word`), the catalogue lives in the
  `CATEGORIES` and `PRODUCTS` arrays at the top of the script, and the cart, wishlist, search, Q&A and newsletter all work client-side.
- Clicking a category tile opens its collection; clicking a piece opens a gallery of three pictures with size selection (clothing) and Add to bag.
- Placeholder art is generated in CSS per category. Drop a real photo at the matching path and it replaces the placeholder automatically:
  - category tiles: `public/images/products/<category>/category.jpg`
  - product galleries (3 pictures each): `public/images/products/<category>/<product-id>-1.jpg`, `-2.jpg`, `-3.jpg`
    (`-1.jpg` is also the card image; change `images` on a product to use more or fewer)
  - spotlights: `public/images/products/hair/spotlight.jpg`, `public/images/products/perfumes/spotlight.jpg`
- Preview locally with `npx serve -l 4173 .` and open http://localhost:4173 (or use the "static" entry in `.claude/launch.json`).
- The original v6 prototype is kept for reference at `docs/design/prototype-v6-reference.html`.

## Folder layout

```
Arbek/
├── index.html               Working prototype (single file) — see "Working prototype" above
├── Photos/                  Raw, original product photos (source of truth, not served to the site)
│   ├── clothing/            Drop photos into the matching category folder
│   ├── perfumes/
│   ├── hair/
│   ├── lip-balms/
│   └── beauty/
│
├── public/                  Static files served as-is by Next.js / Vercel
│   ├── images/
│   │   ├── products/        Web-ready (resized, compressed) product images, by category
│   │   ├── banners/         Hero and promo banners
│   │   ├── brand/           Logo, favicon source, social share image
│   │   └── lookbook/        Editorial / lifestyle shots
│   ├── icons/               SVG icons, favicons, PWA icons
│   └── fonts/               Self-hosted font files
│
├── src/
│   ├── app/                 Next.js App Router (routes)
│   │   ├── (shop)/          Storefront: products/[slug], collections/[category], cart, checkout, search
│   │   ├── (account)/       Customer area: account, orders, wishlist, addresses
│   │   ├── (auth)/          login, register, forgot-password
│   │   ├── (marketing)/     about, contact, faq, shipping-returns, privacy, terms
│   │   ├── admin/           Internal admin: products, orders, customers
│   │   └── api/             Route handlers: products, checkout, newsletter, contact, webhooks/stripe
│   ├── components/
│   │   ├── ui/              Buttons, inputs, modals, badges (design primitives)
│   │   ├── layout/          Header, footer, nav, mobile menu
│   │   ├── product/         Product card, gallery, variant picker, reviews
│   │   ├── cart/            Cart drawer, line items, totals
│   │   ├── checkout/        Address form, payment step, order summary
│   │   ├── account/         Order history, profile forms
│   │   ├── forms/           Shared form fields and validation wrappers
│   │   └── marketing/       Hero, category tiles, newsletter signup, testimonials
│   ├── lib/
│   │   ├── db/              Database client and queries
│   │   ├── payments/        Stripe client, checkout session helpers
│   │   ├── utils/           Formatting (currency, dates), slugify, helpers
│   │   └── validations/     Zod schemas for forms and API input
│   ├── hooks/               React hooks (useCart, useMediaQuery, ...)
│   ├── store/               Client state (cart, wishlist) e.g. Zustand
│   ├── types/               Shared TypeScript types (Product, Order, Customer)
│   ├── config/              Site config: name, domain, nav links, category list, currency
│   ├── data/
│   │   ├── products/        Product catalog (JSON/TS) until a database is wired up
│   │   └── categories/      Category metadata and ordering
│   ├── styles/              Global CSS, Tailwind config extensions, theme tokens
│   └── emails/              Transactional email templates (order confirmation, shipping)
│
├── content/                 Markdown/MDX content
│   ├── lookbook/            Seasonal lookbooks
│   ├── blog/                Style and beauty articles
│   └── pages/               Long-form policy pages (privacy, terms) if kept as content
│
├── db/
│   ├── migrations/          Schema migrations
│   └── seed/                Seed data scripts
│
├── scripts/                 One-off tooling: import photos from Photos/ -> public/images, resize, generate catalog
├── tests/
│   ├── unit/                Component and utility tests
│   └── e2e/                 Browser tests (Playwright) for browse -> cart -> checkout
├── docs/
│   ├── brand/               Brand guidelines, colours, typography, voice
│   ├── design/              Wireframes, mockups, page inventory
│   └── planning/            Roadmap, launch checklist, domain decision
├── .github/workflows/       CI (lint, test, build) on push and PR
└── .vscode/                 Editor settings and recommended extensions
```

## Photo workflow

Clothing is live: the four outfits in `Photos/clothing/` were converted to `public/images/products/clothing/` as
`gold-dot-midi`, `grey-power-suit`, `olive-pleat-set` and `ivory-dot-tiered` (three shots each, plus `category.jpg`).
To convert more originals, run the snippet in `scripts/README.md`.

1. Save originals into `Photos/<category>/` using the naming convention `<category>-<product-slug>-<n>.jpg`
   (example: `hair-body-wave-wig-22in-1.jpg`).
2. A script in `scripts/` will later resize and compress these into `public/images/products/<category>/`.
3. Only the optimised copies in `public/` ship with the site.

## Next steps

1. Scaffold the app: `npx create-next-app@latest . --typescript --tailwind --app --src-dir`
2. Fill in `src/config/site.ts` with name, domain, categories, and currency.
3. Copy `.env.example` to `.env.local` and add keys.
4. Connect the repo to Vercel and add the chosen domain.
