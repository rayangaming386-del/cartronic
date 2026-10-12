# CarTronic — website

Eén zelfstandig bestand: **`index.html`** (alle pagina's, teksten, foto's en Beheer). Het werkt op een webserver met
nette adressen (`/producten`, `/storingen/…`) en ook lokaal (dubbelklikken; dan met `#/…`-adressen). De root van deze
repository is direct te hosten (Vercel/Netlify); `release/` bevat de volledige oplever-zip met broncode.

## Pagina's

| Pagina | Adres |
| --- | --- |
| Home: hero, merken, voertuigzoeker (eerst kenteken, daarna merk/model), categorieën, diensten, werkwijze, beveiliging (klasse 1–5), portfolio, beoordelingen | `/` |
| Producten (catalogus met filters, zoeken, statuses, deelbare selectie) | `/producten`, `/producten/<slug>` |
| Merken en modellen (elk model is een eigen link naar de gefilterde producten) | `/merken`, `/merken/<merk>` |
| Diensten (menu met submenu) | `/diensten`, `/diensten/<slug>` |
| Storingen & oplossingen (filter op merk, eigen pagina per storing met WhatsApp/contact) | `/storingen`, `/storingen/<slug>` |
| Beveiliging: klasse 1–5, peilsystemen, SCM/CCV/VbV, origineel alarm | `/beveiliging` |
| Portfolio (projecten + “Meer van ons werk”) | `/portfolio`, `/portfolio/<slug>` |
| Video's (YouTube incl. Shorts, TikTok, Instagram, Facebook; laden pas na een klik) | `/videos` |
| Over ons, Contact, Beoordelingen, Berichten & kennis, Productinformatie, Privacy, Mijn auto | `/over`, `/contact`, … |
| Webshop (staat standaard **uit**, de code is aanwezig) | `/webshop` |
| Beheer (niet gelinkt, niet in sitemap of zoeken, `noindex`) | `/admin` — alleen door het adres in te typen |

**Oude adressen sturen door** naar de nieuwe: `/mogelijkheden…` → `/producten…`, `/projecten…` → `/portfolio…`,
`/expertise` → `/diensten`, ook met `#/` ervoor en met de filters erachter. Op de server gebeurt dat met permanente
redirects (301) in `_redirects` (Netlify) en `vercel.json` (Vercel); in de pagina zelf nog een keer, zodat oude links
in teksten en bladwijzers altijd werken. Canonieke adressen en de sitemap gebruiken alleen de nieuwe adressen.

## Beheer

Wijzigingen ziet u direct in uw eigen browser; bezoekers zien ze na **publiceren en uploaden** (zie onder).

| Scherm | Wat u er beheert |
| --- | --- |
| Publiceren | Status, **Download website (zip)**, back-up exporteren/importeren, concept weggooien, standaardinhoud herstellen, opslagmeter |
| Bedrijfsgegevens | Naam, adres, telefoon, WhatsApp, e-mail, openingstijden, profielen, KvK en btw (in de footer zodra ingevuld) |
| Teksten | Alle teksten van alle pagina's (ook de nieuwe), doorzoekbaar, met **vet**, links en regels |
| Weergave | Onderdelen van de homepage aan/uit (ook het blok Beveiliging), afsluitende balk, “Recent bekeken”, webshop aan/uit, mededelingsbalk |
| SEO | Website-adres, titels, omschrijvingen, verificatiecodes, zoekmachines tegenhouden (standaard uit), alle pagina's met Google-voorbeeld |
| Producten (catalogus) | Alle 327 producten: bewerken, verbergen, dupliceren, toevoegen |
| Categorieën & merken | Categorieteksten en merkintroducties |
| **Merken & modellen** | Modellen toevoegen, bewerken, volgorde, verbergen en verwijderen: naam, slug, bouwjaren, merk, RDW-zoekwoorden, typecodes en uitzonderingen (met uitleg). Verwijderen van een model dat in gebruik is, geeft een waarschuwing en biedt “Verbergen” aan, zodat er nooit kapotte pagina's ontstaan. Nieuwe modellen verschijnen in de zoeker, merkpagina's, filters, het zoeken en de sitemap. |
| Diensten | Toevoegen, bewerken, volgorde, verwijderen |
| **Storingen** | Storingen toevoegen, bewerken, volgorde, verbergen, verwijderen: titel, merk, modellen, symptomen, oorzaak, oplossing, foto's (upload, vervangen, volgorde, verwijderen, verplichte beschrijving), illustratie, gerelateerde dienst/product, Google-titel; plus de koppen en introductie van de pagina |
| **Beveiliging** | Alle teksten, de klassen (toevoegen, volgorde, verbergen, verwijderen), peilsystemen, het verzekeraarsblok, keurmerken, foto's, knoppen en het blok op de homepage |
| Portfolio | Projecten met foto's, voertuig, tekst en koppelingen; aan/uit per project |
| **Meer van ons werk** | Kop en introductie, kaarten toevoegen/bewerken/verwijderen/volgorde, titel, tekst, links, foto (upload, vervangen, verwijderen) |
| **Video's** | Video's toevoegen via een geplakte link (platform wordt herkend, duidelijke fout bij een ongeldige of verkorte link), titel, beschrijving, eigen voorbeeldafbeelding, uitgelicht, zichtbaar, volgorde; koppen en “Volg ons”-rij |
| Berichten & kennis, Beoordelingen | Zoals voorheen |
| Webshopproducten, Shopcategorieën, Bestellingen, Acties, Voorraad, Instellingen | De webshop (standaard uitgeschakeld) |
| **Account & wachtwoord** | Wachtwoord en gebruikersnaam wijzigen |

Foto's worden in de browser verkleind tot maximaal 1600 px en opgeslagen als WebP (of JPEG). Is de opslag van de
browser vol, dan wordt niets opgeslagen en ziet u een duidelijke melding.

## Inloggen en wachtwoord

- De inloggegevens staan **niet** in deze README, de code, de tests of de gepubliceerde bestanden. De website bewaart
  alleen een gezouten hash van het wachtwoord (PBKDF2-SHA-256, 600.000 iteraties, via WebCrypto) en vergelijkt
  daartegen.
- Na 5 mislukte pogingen moet u steeds langer wachten (30 seconden, oplopend tot 15 minuten). Na 30 minuten zonder
  activiteit wordt u automatisch uitgelogd.
- **Wachtwoord wijzigen:** Beheer → Account & wachtwoord. Vul het huidige wachtwoord in en het nieuwe twee keer
  (minimaal 10 tekens, anders dan het huidige); de gebruikersnaam kunt u daar ook wijzigen.
  *Het nieuwe wachtwoord geldt in deze browser direct; op andere apparaten pas nadat u de website opnieuw hebt
  gepubliceerd en geüpload.* De hash reist mee in de publicatie.
- Een back-up bevat geen inloggegevens. Een back-up terugzetten, het concept weggooien of de standaardinhoud
  herstellen zet nooit stilletjes een oud wachtwoord terug.
- **Eerlijk gezegd:** dit is een controle in de browser, geen serverbeveiliging. Iemand met technische kennis kan in
  zijn eigen browser Beheer openen, maar kan daarmee niets aan de website van bezoekers veranderen: dat kan alleen
  wie de website publiceert en uploadt (toegang tot GitHub/Vercel of Netlify). Voor een webshop met echte betalingen
  is server-side authenticatie nodig.
- Een nieuwe standaard-hash maken (bijvoorbeeld voor een nieuwe installatie): `CT_ADMIN_USER=… CT_ADMIN_PASSWORD=…
  node qa/make-auth.cjs` en de uitvoer in `DEFAULT_AUTH` (src/03-domain.js) zetten. Het wachtwoord staat daarbij
  alleen in de omgevingsvariabele.

## Publiceren

Beheer → Publiceren → **Download website (zip)** maakt in de browser de complete website met alle inhoud:
`index.html`, `404.html`, `_redirects` (Netlify, met de 301-redirects), `vercel.json` (Vercel, met dezelfde
redirects), `robots.txt`, `sitemap.xml`, `og-image.jpg` en `LEESMIJ.txt` (stappen in het Nederlands).

- **Netlify:** pak de zip uit en sleep de map `cartronic-website` naar Deploys.
- **Vercel (deze repository):** upload de inhoud van `cartronic-website` naar de root van de repository (GitHub → Add
  file → Upload files → Commit). Vercel zet de nieuwe versie automatisch online. In Vercel: *Add New → Project →
  Import* deze repository, Framework Preset *Other*, geen build command, output = root. `release/` wordt door
  `.vercelignore` niet gepubliceerd.

Zoekmachines staan standaard **aan** (`index, follow`); `/admin` is uitgesloten in `robots.txt` en `noindex`.

## Nog aan te leveren / te controleren door CarTronic

| Wat | Waar in Beheer |
| --- | --- |
| Openingstijden: op cartronic.nl staat 9:30–17:00, op new.cartronic.nl 09:00–17:00. De website toont nu 9:30. | Bedrijfsgegevens → Openingstijden |
| “Ruim 14 jaar ervaring” (eigen formulering van CarTronic; registers noemen augustus 2005 als start) | Teksten → Over ons → Opmerking onderaan |
| KvK- en btw-nummer (alleen invullen als ze kloppen) | Bedrijfsgegevens |
| Prijzen in de catalogus (o.a. Golf 7 Active Info Display, App Connect, Discover/CarPlay, Audi Smartphone Interface) | Producten (catalogus) |
| Echte storingen met foto's: de vier storingen zijn algemene voorbeelden op basis van de diensten van CarTronic, zonder foto's | Storingen |
| Beveiliging: welke klassen, systemen (alarm, volgsysteem, peilsysteem) en certificaten CarTronic zelf levert en inbouwt; eventueel foto's van een inbouw. Er staan bewust geen merken, prijzen of certificaatnummers | Beveiliging |
| Video's: twee echte Facebook-video's staan erin; meer video's (YouTube, TikTok, Instagram) en eigen voorbeeldafbeeldingen | Video's |
| Links en profielen voor “Volg ons” (Instagram, YouTube, TikTok …) en het aanzetten van die rij | Bedrijfsgegevens → Andere profielen; Video's → Volg ons |
| Foto's bij “Meer van ons werk” | Meer van ons werk |
| RDW-typecodes van modellen steekproefsgewijs controleren | Merken & modellen |
| Webshop: producten en prijzen zijn voorbeeldgegevens en betalingen zijn gesimuleerd; laat de webshop uit tot er echte producten en een betaalkoppeling (Mollie, server-side) zijn | Weergave → Webshop |
| Website-adres (nu https://www.cartronic.nl) controleren vóór publicatie | SEO |
| Na de eerste keer inloggen: eigen wachtwoord instellen en de website publiceren | Account & wachtwoord, Publiceren |

## Architectuur

| Onderdeel | Nu | Productie |
| --- | --- | --- |
| Opslag (`Repo`, `Content`) | Concept in de browser, publicatie in `index.html` | CMS/API + database |
| Kenteken | RDW Open Data vanuit de browser | Eigen proxy (caching, rate limiting) |
| Betalingen | Gesimuleerd (webshop staat uit) | Server-side Mollie, eerst testmodus |
| Inloggen (`Auth`) | PBKDF2-hash in de browser, vergrendeling, automatisch uitloggen | Serversessies, 2FA, autorisatie per wijziging |
| Statistieken | Niets wordt verstuurd | Toestemmingsbewust endpoint |

Broncode: `src/` (JavaScript-modules en CSS) wordt door `node build.mjs` tot één `index.html` samengevoegd.

## SEO

Per pagina titel, omschrijving, canonical, Open Graph en `robots`; structured data (`AutoRepair`, `Service`,
`BreadcrumbList`, `Article`). De sitemap bevat alle indexeerbare pagina's met de nieuwe adressen (ook storingen,
beveiliging, video's, portfolio en per model een productenpagina) en geen verborgen inhoud.

## Tests

Playwright-suites in `qa/`. Beheer-tests lezen het wachtwoord uit een omgevingsvariabele (nooit uit een bestand):

```
CT_ADMIN_PASSWORD=… sh qa/run-all.sh index.html     # alle suites
CT_ADMIN_PASSWORD=… CT_URL=file://$PWD/index.html node qa/launch.cjs
```

`qa/launch.cjs` test de lanceringsupdate: redirects en canonicals, menu en submenu, modellinks (ook met het
toetsenbord), kenteken eerst, storingen (CRUD, foto upload/vervangen/volgorde, alt-tekst, verbergen), beveiliging,
video's (geen verzoeken vóór een klik, youtube-nocookie), modellen (toevoegen, verbergen, verwijderen met
waarschuwing, RDW-herkenning), Meer van ons werk, inloggen (fout wachtwoord, vergrendeling, inactiviteit),
wachtwoord wijzigen (alle foutgevallen, back-up en reset houden het nieuwe wachtwoord), de volledige
publicatie-ronde (zip, 301-redirects, sitemap, robots, nieuw wachtwoord op een ander apparaat pas na publiceren)
en controleert dat het wachtwoord in geen enkel bestand voorkomt.
