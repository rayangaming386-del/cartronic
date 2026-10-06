# CarTronic — interactive prototype

One self-contained file: **`index.html`**. Open it in a browser (double-click works; no server or build step needed).
The redesign follows the approved CarTronic design screens. Company facts come from cartronic.nl.

## What to review

| Area | Where |
| --- | --- |
| Homepage, kenteken finder, brands, expertise story, projects | `#/` |
| Catalog with filters, search, statuses, shareable selection | `#/mogelijkheden` |
| Retrofit detail with compatibility, specifications (warranty, price) and related upgrades | e.g. `#/mogelijkheden/audi-a3-8v-virtual-cockpit` |
| Smart search (model + option) | e.g. search "Golf 7 camera", "A4 B9 CarPlay", "Kodiaq camera" |
| Brands → models | `#/merken` |
| Portfolio with filters | `#/projecten` |
| Expertise, services | `#/expertise`, `#/diensten/kalibratie-rijhulpsystemen` |
| Smart contact flow (WhatsApp / e-mail prefill) | `#/contact` |
| Saved options, recent items, vehicle context | `#/mijn-auto` |
| Webshop, product, cart, checkout, payment states | `#/webshop` |
| Admin (demo) | `#/admin` — user `demo`, password `cartronic-demo` |

**Kenteken:** real plates are looked up live through RDW Open Data. To test every state without a real plate,
open "Demo-kentekens" under the plate field (e.g. `TE-ST-01` Golf 7, `TE-ST-06` generation choice,
`TE-ST-05` unsupported brand, `TE-ST-08` not found, `TE-ST-09` RDW unavailable). Demo results are labelled as demo data.

**Payments:** checkout ends on a clearly labelled *demo* payment page where the tester picks the outcome
(paid / processing / failed / cancelled). Nothing is charged and this is not Mollie.

## Real vs. demo content

- **Real (from cartronic.nl):** address (Patrijsweg 22, 2289 EX Rijswijk), phone/WhatsApp 070 383 9836,
  info@cartronic.nl, opening hours, services (retrofits, online programming, ACC/Lane Assist/Night Vision
  calibration, diagnostics), and the CarTronic retrofit catalog: **285 options for 38 models** (Volkswagen, Audi,
  SEAT, Škoda), merged from the approved selection and the full product list (see *Catalog data* below).
- **Photos:** taken from the approved design screens (hero, workshop projects, catalog images). A catalog photo is
  only reused for the same product (e.g. the Audi Smartphone Interface photo for Audi Smartphone Interface options).
  Everything else shows a technical line drawing labelled "Illustratie".
- **Demo only:** all webshop products, prices, stock, shipping rates and the seeded orders.
  Products carry `isDemo: true`; the shop shows a "Testomgeving" notice.
- **Before/after slider:** built and working, but only shown when a project has real before *and* after photos
  (none exist yet). They can be added under Admin → Projecten.

### Please verify before going live
- Opening hours: cartronic.nl lists di–za **9:30–17:00**; new.cartronic.nl lists **09:00–17:00**. The prototype uses 9:30.
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

- **Redirects:** `aliases` holds older URLs (32, e.g. cartronic.nl slugs such as `audi-a4-b9-8w-virtual-cockpit` or
  `aanbieding-1-…`). They redirect to the current page; no duplicate pages exist.
- **Not shown:** the source URLs are kept only for reconciliation; there is no "bronpagina" link on the site.
- **Retrofits are not webshop products:** catalog options link to the contact/WhatsApp flow, never to the cart.
- **Adding an option:** add one line to `RETROFITS` with `slug`, `feature`, `brand`, `models` (+ `years`, `specs`,
  `price` when CarTronic publishes them). New models go in `MODELS` with RDW match tokens and year range.

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

**SEO note:** hash routes simulate the production URL structure. In production every retrofit, model, product and
project should be its own server-rendered, indexable page (e.g. `/mogelijkheden/volkswagen/golf-8/achteruitrijcamera`).

## QA performed
Automated browser tests (Playwright) covered all routes at 1440/768/390 px and the header at 1280–1920 px (no console errors, no horizontal
overflow), kenteken states (live RDW mocked + all demo states), manual selection, search/autocomplete with keyboard,
filters and share state, cart/drawer/checkout validation, duplicate-order protection, all four payment results,
stock decrement and order snapshots, the full admin flow (login, quick edit, create/validate/archive/delete products,
categories, order status, promotions, stock, settings, reset), mobile menu/sheets/drawers, motion with and without the
GSAP CDN, and an axe-core accessibility audit (no violations on the audited pages).

Catalog QA (after the catalog merge): data validation of all 285 entries (unique slugs and titles, valid
brand/model/category/illustration, no "Aanbieding" text as a model, no empty or generic texts, existing images,
aliases resolve), every detail page rendered without errors, example searches ("Golf 7 camera", "A4 B9 CarPlay",
"T-Roc inklapbare spiegels", "A3 Virtual Cockpit", "Kodiaq camera", "Ateca parkeerhulp"), 16 representative
RDW records across VW, Audi, SEAT and Škoda, redirects with back/forward, and all earlier suites re-run.
