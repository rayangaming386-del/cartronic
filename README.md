# CarTronic — interactive prototype

One self-contained file: **`index.html`**. Open it in a browser (double-click works; no server or build step needed).
The layout follows the approved CarTronic design screens (desktop) and the reference phone screenshots (mobile):
same section order, Helvetica Neue/Arial typography (system fonts, no external font requests), measured sizes and
colours. Company facts come from cartronic.nl and its public Klantenvertellen profile.

## What to review

| Area | Where |
| --- | --- |
| Homepage: hero, brands, finder (merk/model, kenteken one click away), categories, services, technique, projects, reviews, webshop, CTA | `#/` |
| Catalog with filters, search, statuses, shareable selection | `#/mogelijkheden` |
| Retrofit detail with compatibility, specifications (warranty, price) and related upgrades | e.g. `#/mogelijkheden/audi-a3-8v-virtual-cockpit` |
| Smart search (model + option) | e.g. search "Golf 7 camera", "A4 B9 CarPlay", "Kodiaq camera" |
| Brands → models | `#/merken` |
| Portfolio with filters | `#/projecten` |
| Expertise, services | `#/expertise`, `#/diensten/kalibratie-rijhulpsystemen` |
| Smart contact flow (topic choice incl. optielijst € 10, VIN, WhatsApp / e-mail prefill) | `#/contact`, `#/contact?onderwerp=optielijst` |
| Customer reviews (Klantenvertellen 9,4 / 229 reviews, verbatim quotes) | `#/beoordelingen` |
| Knowledge articles (online connection, ACC/Lane Assist calibration, SCM alarm, CarPlay, models not listed, optielijst) | `#/kennis` |
| Product information (OEM parts, warranty, prices, compatibility statuses, optielijst) | `#/productinformatie` |
| Privacy (what is stored, RDW lookup, wipe everything) | `#/privacy` |
| Saved options, recent items, vehicle context | `#/mijn-auto` |
| Webshop, product, cart, checkout, payment states | `#/webshop` |
| Admin (demo) | `#/admin` — user `demo`, password `cartronic-demo` |
| Website management in Beheer (texts, company data, options, services, articles, reviews, display, SEO, publishing) | `#/admin/website`, `#/admin/teksten`, `#/admin/opties`, `#/admin/seo` |

**Kenteken:** real plates are looked up live through RDW Open Data. To test every state without a real plate,
open "Demo-kentekens" under the plate field (e.g. `TE-ST-01` Golf 7, `TE-ST-06` generation choice,
`TE-ST-05` unsupported brand, `TE-ST-08` not found, `TE-ST-09` RDW unavailable). Demo results are labelled as demo data.

**Payments:** checkout ends on a clearly labelled *demo* payment page where the tester picks the outcome
(paid / processing / failed / cancelled). Nothing is charged and this is not Mollie.

## Real vs. demo content

- **Real (from cartronic.nl):** address (Patrijsweg 22, 2289 EX Rijswijk), phone/WhatsApp 070 383 9836,
  info@cartronic.nl, opening hours, services (retrofits, online programming, ACC/Lane Assist/Night Vision
  calibration, diagnostics), and the CarTronic retrofit catalog: **285 options for 38 models** (Volkswagen, Audi,
  SEAT, Škoda): **327 options for 43 models**, merged from the approved selection, the full product list and 42
options from the newer cartronic.nl product pages (Tiguan AD, Golf 8, ID.3/ID.4, Sharan, A3 8Y, A6 C8, A7 C8, Q8,
Q4 e-tron, Kamiq, Enyaq) — see *Catalog data* below.
- **Reviews:** score 9,4 from 229 reviews and four verbatim quotes from the public Klantenvertellen profile.
  The star row shows 4,7 of 5 stars (= 9,4/10). Please update when the profile changes.
- **"Meer van ons werk":** texts from the reference site; the links open CarTronic's Facebook page (the individual
  posts could not be located), the video card links two verified Facebook videos (Q7, 360° camera).
- **Photos:** taken from the approved design screens (hero, workshop projects, catalog images). A catalog photo is
  only reused for the same product (e.g. the Audi Smartphone Interface photo for Audi Smartphone Interface options).
  Everything else shows a technical line drawing labelled "Illustratie".
- **Demo only:** all webshop products, prices, stock, shipping rates and the seeded orders.
  Products carry `isDemo: true`; the shop shows a "Testomgeving" notice.
- **Before/after slider:** built and working, but only shown when a project has real before *and* after photos
  (none exist yet). They can be added under Admin → Projecten.

### Please verify before going live
- Opening hours: directory listings show di–za **9:30–17:00**; new.cartronic.nl lists **09:00–17:00**. The prototype uses 9:30.
- "Ruim 14 jaar ervaring" is CarTronic's own wording; business registers give August 2005 as start date, so the
  number may be outdated.
- KvK/BTW numbers are not on the site yet; add them once CarTronic confirms them.
- Retrofit catalog details. cartronic.nl could not be opened directly from the build environment, so product details
  were collected from search-engine results for cartronic.nl pages and could not be re-checked a second time. Please spot-check:
  - prices shown: Golf 7 Active Info Display *€ 1.099 excl. btw* / *€ 1.499 inclusief montage*; VW App Connect
    *vanaf € 199 inclusief montage*; Discover navigatie met CarPlay *€ 899* (btw not stated);
  - Audi Smartphone Interface: the model pages show the price that the catalog and the product page agree on
    (A4 B9 and Q2 GA *€ 399*, Q5 FY and Q7 4M *€ 299*). The offer page says *vanaf € 349 inclusief montage* with
    different per-model prices; that wording is shown on the general Audi Smartphone Interface page. Please align;
  - year ranges added from product pages (e.g. Audi "2015–2019 (ook S4, RS4 en S-line)"); vehicles outside them get
    *Neem contact op*;
  - "Yeti II" options are treated as Yeti 2013–2017 (facelift); Caddy SA = Caddy 4 (2015–2020); a Multivan registered
    from 2022 (T7) is not matched to the Transporter T6.
- RDW type codes used to disambiguate generations (`MODELS[].types` in the data section) should be spot-checked;
  conflicts are flagged to the visitor instead of being trusted.

## Catalog data

All options live in one dataset, `RETROFITS` (data section of `index.html`). Each entry names a `feature`
(e.g. `achteruitrijcamera`); the `FEATURES` table supplies the shared name, category, illustration, description and
search terms. Fields on an entry (title, text, years, specs, warranty, price, offer) always win over those defaults,
so the 58 originally approved entries kept their exact texts, slugs and order.

- **Redirects:** `aliases` holds older URLs (35, e.g. cartronic.nl slugs such as `audi-a4-b9-8w-virtual-cockpit` or
  `aanbieding-1-…`). They redirect to the current page; no duplicate pages exist.
- **Not shown:** the source URLs are kept only for reconciliation; there is no "bronpagina" link on the site.
- **Retrofits are not webshop products:** catalog options link to the contact/WhatsApp flow, never to the cart.
- **Adding an option:** in Beheer → Opties (no code needed), or in code: add one line to `RETROFITS` with `slug`,
  `feature`, `brand`, `models` (+ `years`, `specs`, `price` when CarTronic publishes them). New models go in `MODELS`
  with RDW match tokens and year range (code only, because they drive the kenteken matching).

## Managing the website (Beheer)

Almost everything on the site can be changed in Beheer without touching code. Changes are visible immediately in the
editor's own browser; visitors see them after **publishing** (below).

| Beheer screen | What it manages |
| --- | --- |
| Publiceren | Status of unpublished changes, **Download website (zip)**, backup export/import (JSON), discard draft, restore built-in content, browser storage meter |
| Bedrijfsgegevens | Name, address, route link, phone (display + international), WhatsApp number, e-mail, opening hours (rows), website, Facebook and other profiles, KvK and btw number (shown in the footer only when filled in) |
| Teksten | 177 texts of every page (hero, section titles and intros, page headers, call-to-action band, footer, contact form, product information, privacy text, …), searchable, with **bold**, links and line breaks; "Standaardtekst terugzetten" per field |
| Weergave | Show/hide each homepage section, the closing call-to-action band, "Recent bekeken", the footer Beheer link; switch the webshop off (menu, cart and product links disappear, shop pages explain it); announcement bar with text, link and colour |
| SEO | Website address, homepage title, default description, title suffix, share title/description, Google/Bing verification, "keep search engines out" (test phase); a list of all ~360 pages with their title and description in Google, issues (too long, too short, duplicate) and a per-page editor with a Google preview |
| Opties | The full retrofit catalog: search/filter (brand, model, category, status), edit any field (title, texts, models or brand-wide, years, specifications, warranty, price, offer, keywords, photo, Google title/description), hide/show, duplicate, add new options, restore the built-in text. Built-in options store only the changed fields. |
| Categorieën & merken | Category names, slogans and the "Technische informatie" texts (installed, programmed, calibration, benefits); brand introductions |
| Diensten | Add, edit, reorder (the first three are on the homepage) and remove services |
| Berichten & kennis | Add, edit, reorder and remove articles (headings, lists, links) |
| Beoordelingen | Score, number of reviews, source links and the quotes (real reviews only) |
| Meer van ons werk | The portfolio cards with links on the Projects page |
| Projecten, Producten, Shopcategorieën, Bestellingen, Acties, Voorraad, Instellingen | As before (webshop and portfolio) |

Every value is validated when it is saved and again when the site loads; a damaged or hand-edited value falls back
to the built-in default instead of breaking a page.

## Publishing

The site is static, so Beheer cannot write to the server. Instead **Beheer → Publiceren → Download website (zip)**
rebuilds this exact page in the browser with the new content embedded, and packs:

- `index.html` — the website with all content, texts, photos and settings;
- `404.html` — the same page, for hosts without a rewrite rule (e.g. GitHub Pages);
- `_redirects` — Netlify rule so clean URLs such as `/mogelijkheden/…` work after a reload;
- `robots.txt`, `sitemap.xml` (when the website address is filled in) and `og-image.jpg` (link previews);
- `LEESMIJ.txt` — the upload steps in Dutch.

Unzip and drag the folder `cartronic-website` onto Netlify (Deploys → drag and drop). Visitors get the new version
on their next visit (their cart and saved items stay). An editor who opens a newer publication while holding
unpublished changes from an older one is asked which version to keep; nothing is overwritten silently.
Make a backup (Publiceren → Exporteer back-up) before large changes.

Production: the same content bundle becomes a CMS/API document and publishing becomes a server action.

## Architecture (single file, production-ready seams)

All adapters are isolated so the UI does not change when they are replaced:

| Concern | Prototype | Production |
| --- | --- | --- |
| Persistence (`Repo`) | `localStorage` | REST API + database |
| Vehicle lookup | RDW Open Data from the browser + demo plates | Own backend proxy (caching, rate limiting) |
| Payments (`Payments`) | `DemoPaymentAdapter` (simulated) | Server-side Mollie, **test mode first** |
| Admin auth (`DemoAuth`) | Browser check — **not secure** | Server sessions, hashed passwords, 2FA, server-side authorisation |
| Analytics | In-memory event layer, nothing sent | Consent-aware endpoint, no plates/personal data |

**Mollie flow (server):** cart → checkout → server validates products & stock → server calculates totals
(never trusts browser prices) → server creates order with snapshots → server creates Mollie payment →
Mollie Checkout → return page shows server status → Mollie webhook → server fetches real status → order *Betaald*.
No API keys or secrets exist in this file.

Other built-in safeguards: prices in integer cents; order line snapshots (later price changes don't alter orders);
idempotency key per checkout (double click / refresh / back don't create duplicate orders); cart re-validation
(inactive, sold out, stock and price changes); archive instead of delete for products that appear in orders;
kenteken kept only in session storage (max. 24 h) and never placed in share links.

## SEO

- **Clean URLs:** on a web server the site uses real paths (`/mogelijkheden/audi-a3-8v-virtual-cockpit`) through the
  History API; old `#/…` links are redirected. Opened as a local file it keeps `#/…` routes. The server must answer
  every path with `index.html` (`_redirects` on Netlify, `404.html` elsewhere — both are in the publish zip and in this repo).
- **Per page:** title, meta description (automatic ones are kept within 160 characters), `robots` (search, cart,
  checkout, garage, 404, admin, demo products and a closed webshop are `noindex`), canonical URL on the website
  address from Beheer → SEO (catalog filters keep only merk/model/cat), Open Graph tags.
- **Structured data:** `AutoRepair` organization built from Bedrijfsgegevens (address, phone, e-mail, opening hours
  parsed from the rows, profiles), `Service` + `BreadcrumbList` for options and services, `Article` for knowledge
  articles, `Product` only for non-demo products. No ratings markup (the reviews are third-party and self-serving
  review markup is not allowed by Google).
- **Sitemap:** every indexable page plus one catalog page per model with options, regenerated at each publication.
- Search engines execute the JavaScript that fills each page. Fully server-rendered pages remain the production
  recommendation for the best results.

## QA performed
Automated browser tests (Playwright) covered all routes at 1440/768/390 px and the header at 1280–1920 px (no console errors, no horizontal
overflow), kenteken states (live RDW mocked + all demo states), manual selection, search/autocomplete with keyboard,
filters and share state, cart/drawer/checkout validation, duplicate-order protection, all four payment results,
stock decrement and order snapshots, the full admin flow (login, quick edit, create/validate/archive/delete products,
categories, order status, promotions, stock, settings, reset), mobile menu/sheets/drawers, motion with and without the
GSAP CDN, and an axe-core accessibility audit (no violations on the audited pages).

Catalog QA (after the catalog merge): data validation of all 327 entries (unique slugs and titles, valid
brand/model/category/illustration, no "Aanbieding" text as a model, no empty or generic texts, existing images,
aliases resolve), every detail page rendered without errors, example searches ("Golf 7 camera", "A4 B9 CarPlay",
"T-Roc inklapbare spiegels", "A3 Virtual Cockpit", "Kodiaq camera", "Ateca parkeerhulp"), 16 representative
RDW records across VW, Audi, SEAT and Škoda, redirects with back/forward, and all earlier suites re-run.

Beheer & SEO round: a snapshot of all 372 public routes (header, page, footer, mobile menu, title, description) was
compared before and after the content layer — identical except the intentionally shortened search descriptions.
New suites: every Beheer screen at 1440 and 390 px (no errors, no overflow), 58 checks that each screen saves,
validates and changes the site, 25 checks of the full publish round trip (zip contents, `unzip -t`, sitemap,
robots, `_redirects`, static head, visitor on the served zip with clean URLs, publishing again from the published
site, outdated drafts), backup/discard/factory reset, path-mode navigation, axe-core on all new screens, and all
earlier suites re-run.

Layout round (reference screenshots): every page re-measured against the phone screenshots at 390 and 428 px and the
desktop screens at 1920 px; all suites re-run (flows, mobile menu as a modal with inert background, catalog with all
327 detail pages, axe-core with the new pages, overflow sweeps at 390/428/768/1440 px).
