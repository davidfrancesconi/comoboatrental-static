# Como Boat Rental — handoff per lo sviluppatore

Questo documento è il passaggio di consegne per lo sviluppatore
front-end che porterà avanti il sito **comoboatrental.com** per
Loris e Claudio. Copre solo gli aspetti tecnici:

1. **Cosa c'è nel sito** — ogni superficie SEO e di contenuto
   attualmente in produzione
2. **Come pubblicare il sito** — deploy su qualsiasi host statico
3. **Come modificare il sito** — workflow consigliato + caveat
4. **Cosa resta da fare** — task tecnici concreti
5. **Checklist di verifica**
6. **Contatti**

Per le attività **fuori dal sito** che servono a far rendere il
progetto (Google Business Profile, OTA come GetYourGuide e
Viator, sistema di prenotazione Bokun, partnership con concierge
degli hotel, stampa, foto da commissionare) leggi il documento
parallelo [`PER-LORIS.md`](./PER-LORIS.md). Quel documento è
scritto per Loris in italiano-discorsivo, non tecnico, e contiene
tutte le checklist di iscrizione alle piattaforme.

Per la spiegazione completa di **cosa fa il sito sul fronte SEO e
perché**, leggi [`SEO.md`](./SEO.md).

---

## 1. Cosa c'è nel sito

### Architettura

- **Sito statico puro.** 126 file `.html` + bundle `.css` + bundle
  `.js`. Nessun framework runtime, nessun backend, nessuna API key,
  nessun build step richiesto al deploy.
- **Routing per locale** sotto `/en/`, `/it/`, `/ru/`, `/ar/`.
  La root `index.html` reindirizza al locale del browser via JS
  (fallback su `/en/`). Ognuno tra `/en/`, `/it/`, `/ru/`, `/ar/`
  è un URL reale con i propri metadati, JSON-LD, hreflang e blocco
  Open Graph.
- **126 file HTML statici**:
  - 4 homepage (una per lingua)
  - 8 tour × 4 lingue = 32 pagine tour
  - 4 indici attrazioni + 13 attrazioni × 4 lingue = 56 pagine
    attrazioni
  - 4 esperienze × 4 lingue = 16 pagine esperienze
  - 4 pagine FAQ
  - 4 pagine recensioni
  - 4 pagine /about
  - 1 articolo blog in 2 lingue (EN, IT) = 2 pagine blog
  - `/sitemap.xml`, `/robots.txt`, `404.html`, `index.html` (root
    redirect)

### Tour offerti

| Slug | Durata | Prezzo da |
|---|---|---|
| `highlights-1h` | 1 ora | €220 |
| `cernobbio-2h` | 2 ore | €400 |
| `balbianello-nesso` | 3 ore | €480 |
| `top-villas-half-day` | 4 ore | €780 |
| `first-basin-5h` | 5 ore | €820 |
| `centre-lake-6h` | 6 ore | €950 |
| `full-day-8h` | 8 ore | €1.200 |
| `sunset-cruise` | 1.5 ore | €350 |

### Infrastruttura SEO

Tutto è già nei file HTML — il dev non deve ricostruire niente,
solo sapere dove trovare cosa quando deve aggiornare.

| Cosa | Dove vive |
|---|---|
| `<title>` e `<meta description>` per pagina | `<head>` di ogni file HTML |
| `<link rel="canonical">` | `<head>` di ogni pagina, lingua-aware |
| `<link rel="alternate" hreflang>` | `<head>` di ogni pagina, 4 alternates per pagina (en/it/ru/ar) + x-default |
| Open Graph + Twitter Card | `<head>` di ogni pagina |
| JSON-LD schema.org | `<script type="application/ld+json">` nell'`<head>` di ogni pagina |
| Sitemap | `/sitemap.xml` (statico, generato al build originale; ogni pagina ha 4 alternates hreflang per voce) |
| Robots | `/robots.txt` allow-all + puntatore al sitemap |
| Manifest PWA | `/manifest.webmanifest` |
| Favicon set | `/favicon.ico`, `/icon-32.png`, `/icon-192.png`, `/icon-512.png` |

### Tipi di schema coperti

- `LocalBusiness` + `TravelAgency` (con founders, geo, orari, sameAs)
- `WebSite`
- `AggregateRating` (4.9/87, su ogni pagina)
- `Review` × 3 testimonianze (su /reviews + homepage)
- `Product` × 2 barche con `Offer` + `UnitPriceSpecification`
- `TouristTrip` × 8 tour con `ItemList` di itinerario e `Offer`
- `FAQPage` (homepage abbreviata, `/faq` completa)
- `BreadcrumbList` (ogni pagina)
- `TouristAttraction` / `Place` × 13 attrazioni con `geo` +
  `sameAs` (Wikipedia + sito ufficiale)
- `Article` (blog)

### Dati di contenuto

Tutti i contenuti sono ripetuti direttamente nei file HTML in 4
lingue. Le costanti del business (telefono, email, indirizzo,
prezzi base, rating) appaiono in:

- Nav in cima a ogni pagina (WhatsApp pill)
- Booking form in fondo alla home (telefono + email + indirizzo)
- Footer di ogni pagina interna (WhatsApp + email + telefono)
- JSON-LD `LocalBusiness` (`telephone`, `email`, `address`,
  `geo`, `openingHours`) — embedded in `<script>` di ogni pagina
- Costanti tour (prezzo, durata) appaiono nelle card tour delle
  homepage × 4 lingue + nelle pagine dettaglio tour × 4 lingue +
  nelle card "Tours that visit this attraction" sulle pagine
  attrazione che le includono × 4 lingue + nei JSON-LD
  `TouristTrip > offers > price` inline su quelle stesse pagine

Le 4 lingue sono ottimizzate sul mercato di ricerca corrispondente:

- **Italiano**: scritto a mano per "noleggio barche como"
  (~3.600/mese), "tour barca lago di como" (~1.200/mese), "gita
  in barca bellagio" (~800/mese), "noleggio barca villa
  balbianello" (~450/mese)
- **Inglese**: scritto a mano per il mercato inglese globale —
  "lake como boat rental", "lake como boat tour", "private boat
  tour bellagio", "villa balbianello boat tour", "lake como
  sunset cruise"
- **Russo**: mirato sul mercato russo del lusso che ancora
  raggiunge il Lago di Como passando per Dubai e Istanbul —
  "Аренда лодки на озере Комо", "Прогулка на лодке Комо",
  "Вилла Бальбьянелло Casino Royale", "VIP чартер озеро Комо"
- **Arabo**: focus sul mercato Gulf (Emirati, Arabia Saudita,
  Kuwait — il segmento ad altissimo margine per il Lago di Como)
  — "تأجير قارب بحيرة كومو", "جولة بحرية بحيرة كومو", "فيلا
  جورج كلوني", "شهر العسل بحيرة كومو". Layout RTL completo

Il copy russo e arabo è competente a livello SEO (keyword giuste,
struttura giusta, lunghezza giusta) ma non scritto da madrelingua.
Vedi sezione "Cosa resta da fare, G" sotto.

### Funzionalità interattive (già nei bundle JS)

Tutto JS lato client già compilato in `_next/static/chunks/`:

- **Mappa Lago di Como** sulla home — Leaflet con tile CartoDB
  Voyager (nessuna API key, attribution control nascosta perché
  decorativa), barca animata in orbita lungo l'itinerario
- **Carousel orizzontali** con frecce + dot indicators:
  - Tour sulla home (8 card scrollabili con auto-advance mobile)
  - Flotta sulla home (2 card barche)
  - Gallery foto per ogni barca (multi-slide con dots in-frame)
  - "Tours that include this attraction" sulle pagine attrazione
- **Booking form** con dropdown tour completo (8 SKU + opzione
  custom) e azione `mailto:` precompilata che apre il client
  email dell'utente con tutti i campi
- **Newsletter / lead capture** "send me a sample itinerary" —
  singolo campo email + mailto
- **FAQ accordion** nativo `<details>`
- **Lingua switcher**, banner WhatsApp flottante, nav mobile
  con hamburger menu

### Link autorevoli outbound

I testi delle pagine attrazione e tour linkano automaticamente a:

- **FAI** (`fondoambiente.it`) per Villa del Balbianello
- **villacarlotta.it** per Villa Carlotta
- **villamonastero.eu** per Villa Monastero (Varenna)
- **castellodivezio.it** per il Castello di Vezio
- **giardinidivillamelzi.it** per Villa Melzi
- **navigazionelaghi.it** per i traghetti pubblici
- **funicolarecomo.it** per la funicolare di Como
- **cattedraledicomo.it** per il Duomo
- **mandarinoriental.com/lake-como** — Mandarin Oriental
- **grandhoteltremezzo.com** — Grand Hotel Tremezzo
- **villaserbelloni.com** — Grand Hotel Villa Serbelloni
- **passalacqua.it** — Villa Passalacqua
- **serenohotels.com** — Il Sereno
- **villadeste.com** — Villa d'Este
- Wikipedia URL per ogni landmark, in `sameAs` del JSON-LD

Plus sidebar "Useful links" su ogni pagina attrazione con 3-5
link curati (Wikipedia, sito ufficiale, Google Maps, ente
turismo).

---

## 2. Come pubblicare il sito

### Su qualsiasi host statico

Carica il contenuto della cartella nella root web dell'host.

```bash
# Esempio FTP
ftp -e ftp.comoboatrental.com
cd /public_html
mput -r ./*
```

### Su Vercel

```bash
vercel --prod
```

Nessun build step, nessuna configurazione: Vercel rileva
l'`index.html` in root e serve i file come sono. Push su
GitHub → Vercel auto-deploya.

### Su Netlify

```bash
netlify deploy --prod --dir .
```

### Su GitHub Pages

Settings → Pages → Source → branch `main`, cartella `/` (root).
Aggiungi un `CNAME` con il dominio se serve.

### Su Cloudflare Pages, S3, qualsiasi altro

Funziona ovunque perché è HTML puro. Punta l'host a servire la
cartella come root web.

---

## 3. Come modificare il sito

I file sono HTML puro: si aprono con qualsiasi editor di testo
(VS Code, Sublime, Notepad++, anche TextEdit). Niente da
installare in locale per modificare.

### Caveat fondamentale: ogni dato del business appare in più posti

Esempio: il prezzo del tour 1-ora ("from €220") appare in:

- 4 homepage (card tour × 4 lingue)
- 4 pagine `tours/highlights-1h/` (×4 lingue)
- Le card "Tours that visit this attraction" sulle pagine
  attrazione che includono `highlights-1h` (× 4 lingue × N
  attrazioni)
- JSON-LD `TouristTrip > offers > price` inline su tutte le
  precedenti
- `sitemap.xml` (URL, non prezzo, ma sempre da tenere coerente)
- L'attributo `<meta property="og:description">` se menziona il
  prezzo

Se modifichi il prezzo in 1 file e non negli altri, il sito
diventa incoerente: la home dice €220, la pagina tour dice €240,
Google si confonde, i visitatori vedono numeri diversi.

### Workflow consigliato per i cambiamenti

**Per un cambio singolo (un testo, un link in una pagina specifica)**:

1. Apri il file in `<locale>/<percorso>/index.html`
2. Cerca il testo, modificalo
3. Carica il file modificato sul server (o commit + push se usi
   Vercel/Netlify con GitHub integration)

**Per un cambio di dato ricorrente (prezzo, telefono, indirizzo,
email, rating)**:

1. Cerca tutte le occorrenze del valore vecchio:
   ```bash
   grep -rn "€220" .
   grep -rn "+39 340 6487574" .
   ```
2. Sostituiscile in tutti i file trovati:
   ```bash
   find . -name "*.html" -exec sed -i '' 's/€220/€240/g' {} \;
   ```
   (`sed -i ''` su macOS; su Linux è `sed -i`)
3. Cerca anche le occorrenze in `sitemap.xml` se cambi una URL
4. Verifica che il JSON-LD inline (`<script
   type="application/ld+json">`) sia stato aggiornato — il
   `grep` lo prende perché il prezzo appare letteralmente come
   stringa anche lì

**Per aggiungere una pagina nuova**:

1. Duplica la cartella di una pagina esistente con struttura
   simile (es. duplica `en/tours/highlights-1h/` per un nuovo
   tour)
2. Modifica il contenuto, i metadati, il JSON-LD, gli `og:`
3. Aggiungi entries equivalenti in `it/`, `ru/`, `ar/` (4
   lingue per pagina)
4. Aggiungi le entries nel `sitemap.xml` (con `<xhtml:link
   rel="alternate" hreflang>` per le 4 lingue)
5. Aggiorna i `<link rel="alternate" hreflang>` nelle pagine
   esistenti dello stesso tipo se serve

**Per aggiornare le foto Instagram**:

I 6 post Instagram bakerati nella home sono in
`images/instagram/`. Per aggiornarli, sostituisci le immagini
mantenendo gli stessi filename e ricarica.

---

## 4. Cosa resta da fare

Nessuno di questi è bloccante — il sito parte e si posiziona
competitivo così com'è. Affronta nell'ordine di impatto-per-ora.

### A. Ottimizzazione immagini (penalità Lighthouse)

Tutte le foto sono JPG. Nessuna ha varianti AVIF/WebP, non tutti
i tag `<img>` portano width/height (la maggior parte sì, alcuni
no). Il Cumulative Layout Shift è mediocre.

La fix pulita:

1. Per ogni JPG in `images/`, genera fratelli `.avif` e `.webp`:
   ```bash
   for f in images/*.jpg images/**/*.jpg; do
     cwebp -q 82 "$f" -o "${f%.jpg}.webp"
     avifenc --min 30 --max 35 "$f" "${f%.jpg}.avif"
   done
   ```
   (`cwebp` da Homebrew, `avifenc` da `libavif`)

2. Sostituisci i tag `<img src=…>` con blocchi `<picture>`:
   ```html
   <picture>
     <source type="image/avif" srcset="/images/balbianello.avif">
     <source type="image/webp" srcset="/images/balbianello.webp">
     <img src="/images/balbianello.jpg" alt="…" width="1200" height="800">
   </picture>
   ```

Tempo stimato: 2-3 ore. I punteggi Lighthouse Performance e CLS
salgono di 5-15 punti.

### B. Vera immagine OG cover

`images/hero-sunset.jpg` è 1586×2410 (taglio verticale). I
metadati Open Graph dichiarano 1200×630 (default OG). Oggi
funziona ma il crop non è perfetto.

Genera `images/og-cover.jpg` come 1200×630 vero e proprio, con
wordmark Como Boat Rental + tagline impressi (idealmente per
lingua — 4 versioni). Aggiorna i `<meta property="og:image">` in
tutte le pagine.

### C. Widget di prenotazione sulle pagine tour

Loris si iscriverà a **Bokun** (sistema di prenotazione di
proprietà di Tripadvisor — vedi `PER-LORIS.md` priorità 2.4).
Quando lo fa, ogni pagina tour può ricevere un calendario di
disponibilità embeddato sopra il blocco FAQ.

Bokun fornisce uno snippet iframe del tipo:

```html
<div data-bokun-channel-id="…" data-bokun-product-id="…"></div>
<script src="https://widgets.bokun.io/assets/javascripts/apps/build/BokunWidgetsLoader.js?bookingChannelUUID=…"></script>
```

Inseriscilo in ognuna delle 32 pagine `<locale>/tours/<slug>/`
(8 tour × 4 lingue) tra il blocco CTA e il blocco FAQ. Considera
anche:

- Mostrare uno stato di "loading" mentre l'iframe Bokun si
  carica
- Garantire che l'altezza dell'iframe non causi CLS (`height`
  fisso o reservato)
- Locale-aware: Bokun supporta locale switch via parametro nello
  snippet

### D. Form di prenotazione → email automatica (oggi mailto)

Il booking form sulla home (`<section id="contact">`) ha azione
`mailto:` precompilata. Apre il client email dell'utente. Per
upgradare a un flow che arrivi direttamente in inbox senza
passare dal client utente, tre opzioni in ordine di semplicità:

1. **Formspree** (formspree.io, free 50 submissions/mese) —
   sostituisci l'action `mailto:…` con
   `action="https://formspree.io/f/XXX" method="POST"`, togli il
   JS preventDefault. Email automatica a info@. 5 minuti di
   setup.
2. **Web3Forms** o **Netlify Forms** — simili a Formspree.
3. **Vercel serverless function** o **Cloudflare Worker** —
   crea un endpoint `/api/booking` che riceve il POST e manda
   email via Resend / SendGrid. Più lavoro ma controllo totale.

Lo stesso pattern vale per il Newsletter / lead-capture "send me
a sample itinerary": submit `mailto:` precompilato verso
`info@comoboatrental.it`. Sostituibile con Formspree o
Mailchimp/Brevo per costruire una mailing list vera.

### E. Pass nativo russo + arabo

Il copy RU e AR è keyword-targeted e della giusta lunghezza
(200-400 parole per pagina, parità con EN/IT). Quello che manca
è il 10-15% finale di rifinitura che solo un madrelingua porta:
cadenza naturale, idioma, scelte di registro.

**Per il russo** un madrelingua dovrebbe:

- Affilare i titoli homepage per cadenza
- Aggiustare il registro delle FAQ — il fraseggio attuale è
  corretto ma leggermente formale per il mercato del lusso
- Verificare le translitterazioni dei toponimi (Bellagio →
  Беладжо, Cernobbio → Черноббио sono le convenzioni usate)

**Per l'arabo** un copywriter Gulf-market dovrebbe:

- Affilare il registro dialettale — il copy attuale è in arabo
  standard moderno (MSA), che va bene per la SEO ma è leggermente
  rigido per i viaggiatori Gulf abituati a marketing con
  influenze khaleeji
- Decidere se aggiungere un framing più forte di viaggio
  multi-generazionale / familiare nella pagina del charter
  giornaliero
- Verificare le translitterazioni (Bellagio → بيلاجيو,
  Balbianello → بالبيانيلو sono le convenzioni usate)

Modifica i file HTML in `ru/` e `ar/` direttamente. Mantieni
sincronizzati i `<title>`, `<meta description>`, `<h1>` con le
modifiche al body.

### F. Performance & accessibilità

Vincite veloci rimaste:

- Aggiungi una media query `@media (prefers-reduced-motion:
  reduce)` per disabilitare la barca animata sulla mappa Leaflet
  e i fade-in delle reveal animations
- Sostituisci gli accordion FAQ `<details>` con bottoni
  `aria-expanded` propri per accessibilità migliore con screen
  reader (richiede JS dedicato)
- Audita i tag `<img>` che mancano width/height — il browser ne
  ricava le proporzioni evitando layout shift
- Aggiungi polyfill `inert` per il menu mobile quando chiuso

### G. Cose differite esplicitamente

- **Footer sitemap-style** con link a tutte le 13 attrazioni + 8
  tour. Saltato perché il `sitemap.xml` copre già il crawl
  depth per Google, e per gli umani la nav + i cross-link dalla
  home bastano. Se vuoi un footer-mega, è ~30 minuti di lavoro
  (aggiungi un `<footer>` esteso a tutte le 126 pagine — usa
  `sed` per propagarlo)
- **Cookie banner**: il sito non usa cookie e nessun analytics.
  Se aggiungi analytics (Plausible, Simple Analytics, GA4),
  verifica se serve un banner per la regione di deploy

---

## 5. Checklist di verifica

Prima di promuovere qualsiasi modifica in produzione:

1. **Schema check**: incolla la URL homepage in
   [search.google.com/test/rich-results](https://search.google.com/test/rich-results)
   — deve mostrare LocalBusiness, AggregateRating, FAQPage,
   TouristTrip
2. **Lighthouse mobile**: gira da Chrome DevTools sulla
   homepage. Target: Performance ≥ 90, Accessibility ≥ 95,
   SEO = 100
3. **Sitemap**: visita `/sitemap.xml` e verifica che le nuove
   pagine compaiano
4. **hreflang**: incolla qualsiasi URL pagina interna in
   [aleydasolis.com/english/international-seo-tools/hreflang-tags-generator](https://www.aleydasolis.com/english/international-seo-tools/hreflang-tags-generator/)
   — deve validare
5. **OG previews**: incolla ogni homepage di lingua in
   [opengraph.xyz](https://www.opengraph.xyz/) — verifica che
   immagine, titolo e descrizione si renderizzino
6. **Test mobile**: apri il sito su un telefono, gira nella
   nav, vai su una pagina tour, conferma che la mappa Leaflet,
   il carousel e il booking form funzionino
7. **RTL**: apri `/ar/` da telefono e verifica che il layout
   destra-a-sinistra funzioni
8. **Search Console** (post-launch): submitta `/sitemap.xml`,
   richiedi indicizzazione per le 4 homepage di lingua e per le
   8 pagine tour principali

---

## 6. Contatti

### Costanti di contatto del business

- **Email**: info@comoboatrental.it
- **Telefono primario**: +39 340 6487574
- **Telefono secondario**: +39 348 0689769
- **WhatsApp**: https://wa.me/393406487574
- **Indirizzo**: Lungolago Viale Geno, 10, 22100 Como (IT)
- **Geo**: 45.808 N, 9.085 E
- **Instagram**: https://www.instagram.com/comoboatrental
- **Founders**: Loris e Claudio

Tutte queste costanti appaiono ripetute ovunque nel sito. Per
aggiornarle: `grep -rn` + `sed` come descritto nella sezione 3.

### Per domande sul contenuto

WhatsApp di Loris e Claudio sono in homepage. Loro gestiscono il
business, non il sito web — per domande sui contenuti sono loro
l'autorità.
