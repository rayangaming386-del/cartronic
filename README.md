# CarTronic — interactive prototype

One self-contained file: **`index.html`**. Open it in a browser (double-click works; no server or build step needed).
The redesign follows the approved CarTronic design screens. Company facts come from cartronic.nl.

## What to review

| Area | Where |
| --- | --- |
| Homepage, kenteken finder, brands, expertise story, projects | `#/` |
| Catalog with filters, search, statuses, shareable selection | `#/mogelijkheden` |
| Retrofit detail with compatibility + technical panels | e.g. `#/mogelijkheden/audi-a3-8v-virtual-cockpit` |
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
  calibration, diagnostics), and a selection of 58 actual catalog entries (titles and short descriptions).
- **Photos:** taken from the approved design screens (hero, workshop projects, catalog images).
  Items without a real photo show a technical line drawing labelled "Illustratie".
- **Demo only:** all webshop products, prices, stock, shipping rates and the seeded orders.
  Products carry `isDemo: true`; the shop shows a "Testomgeving" notice.
- **Before/after slider:** built and working, but only shown when a project has real before *and* after photos
  (none exist yet). They can be added under Admin → Projecten.

### Please verify before going live
- Opening hours: cartronic.nl lists di–za **9:30–17:00**; new.cartronic.nl lists **09:00–17:00**. The prototype uses 9:30.
- Retrofit catalog: the online catalog lists hundreds of items; this prototype contains a representative subset.
- RDW type codes used to disambiguate generations (`MODELS[].types` in the data section) should be spot-checked;
  conflicts are flagged to the visitor instead of being trusted.

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
