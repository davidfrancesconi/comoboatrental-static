# Como Boat Rental — sito web

Sito di marketing per **comoboatrental.com**: 126 pagine HTML
statiche in quattro lingue (inglese, italiano, russo, arabo),
con i bundle CSS / JS associati, le immagini e tutti gli asset.

Tecnologie: solo `.html`, `.css` e `.js`. Niente backend, niente
database, niente API key, niente Node da installare in produzione.
Si carica su qualsiasi web server e funziona.

> 📚 **Documentazione completa in [`docs/`](./docs/):**
>
> - 📈 [`docs/SEO.md`](./docs/SEO.md) — cosa fa il sito per Google
>   e perché, spiegato in italiano semplice
> - 📋 [`docs/PER-LORIS.md`](./docs/PER-LORIS.md) — checklist
>   operativa off-site per Loris (Google Business Profile,
>   Tripadvisor, Bokun, partnership hotel, foto)
> - 🛠 [`docs/HANDOFF.md`](./docs/HANDOFF.md) — handoff tecnico per
>   lo sviluppatore (struttura, deploy, modifiche, task residui)

---

## In due righe

- **122 pagine di contenuto** in 4 lingue + 4 file di supporto
  (404, redirect root, helper) = **126 file `.html` totali**
- Tutto ottimizzato per Google: hreflang, canonical, schema.org
  JSON-LD, sitemap, Open Graph, Twitter Card
- Mappa interattiva del Lago di Como, carousel, form di
  prenotazione `mailto:` e gallery foto per ogni barca

### Le 122 pagine di contenuto

| Tipo | Quante |
|---|---|
| Homepage × 4 lingue | 4 |
| Indici attrazioni × 4 lingue | 4 |
| Pagine attrazione (13 × 4 lingue) | 52 |
| Pagine esperienza (4 × 4 lingue) | 16 |
| Pagine tour (8 × 4 lingue) | 32 |
| FAQ × 4 lingue | 4 |
| Recensioni × 4 lingue | 4 |
| /about × 4 lingue | 4 |
| Blog (EN + IT) | 2 |
| **Totale** | **122** |

---

## Come pubblicare il sito

### Su qualsiasi host statico

Carica il contenuto di questa cartella nella root web del tuo host.

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
l'`index.html` in root e serve i file come sono.

### Su Netlify

```bash
netlify deploy --prod --dir .
```

### Su GitHub Pages

Settings → Pages → Source → branch `main`, cartella `/` (root).
Aggiungi `CNAME` con il dominio se serve.

Funziona ovunque perché è HTML puro.

---

## Struttura

```
/                          ← root del sito
├── index.html             ← redirect alla lingua del browser
├── 404.html               ← pagina di errore
├── favicon.ico            ← favicon multi-risoluzione
├── icon-32.png            ← favicon PNG 32×32
├── icon-192.png           ← icona Android / PWA
├── icon-512.png           ← icona Android / PWA
├── manifest.webmanifest   ← PWA manifest
├── robots.txt
├── sitemap.xml            ← sitemap di tutte le pagine
├── en/                    ← versione inglese
│   ├── index.html         ← homepage
│   ├── about/
│   ├── attractions/
│   │   ├── index.html     ← indice attrazioni
│   │   ├── como/
│   │   ├── bellagio/
│   │   ├── villa-del-balbianello/
│   │   ├── varenna/
│   │   ├── villa-carlotta/
│   │   ├── cernobbio/
│   │   ├── moltrasio-laglio/
│   │   ├── nesso/
│   │   ├── isola-comacina/
│   │   ├── villa-la-cassinella/
│   │   ├── blevio-torno/
│   │   ├── menaggio/
│   │   └── lecco/
│   ├── experiences/
│   │   ├── weddings/
│   │   ├── photoshoots/
│   │   ├── captains/
│   │   └── private-tours/
│   ├── tours/
│   │   ├── highlights-1h/
│   │   ├── cernobbio-2h/
│   │   ├── balbianello-nesso/
│   │   ├── top-villas-half-day/
│   │   ├── first-basin-5h/
│   │   ├── centre-lake-6h/
│   │   ├── full-day-8h/
│   │   └── sunset-cruise/
│   ├── faq/
│   ├── reviews/
│   └── blog/
├── it/                    ← versione italiana (stessa struttura)
├── ru/                    ← versione russa (stessa struttura)
├── ar/                    ← versione araba — RTL
├── images/                ← tutte le foto e gli asset
│   ├── attractions/       ← 13 foto attrazioni
│   ├── experiences/       ← 4 foto esperienze
│   ├── instagram/         ← griglia Instagram
│   └── logo-flag*.png
└── _next/                 ← bundle CSS + JS compilati
    ├── static/chunks/
    ├── static/css/
    └── static/media/
```

Ogni pagina è una `index.html` dentro la sua cartella, così l'URL
finale è pulito senza l'estensione `.html`.

---

## Cosa c'è nel sito

### Contenuti

- **8 tour** con pagina dettaglio dedicata: 1h / 2h / 3h / 4h / 5h /
  6h / 8h / sunset cruise. Ogni pagina ha itinerario, prezzi,
  cosa è incluso, FAQ specifiche
- **13 destinazioni** del lago con pagina dettaglio: Como, Bellagio,
  Villa del Balbianello, Varenna, Villa Carlotta, Cernobbio,
  Moltrasio / Laglio, Nesso, Isola Comacina, Villa La Cassinella,
  Blevio / Torno, Menaggio, Lecco
- **4 esperienze**: matrimoni, servizi fotografici, skipper, tour
  privati
- **FAQ** con 12 domande / risposte
- **Recensioni** Google (4.9 / 87)
- **Pagina /about** founder
- **Blog** seed con un articolo

### Lingue

- **EN** — mercato globale
- **IT** — mercato locale + Italia turistica
- **RU** — mercato del lusso russo
- **AR** — mercato Gulf, con layout RTL completo

### SEO già configurato

- `<title>` e `<meta description>` per pagina, per lingua
- `<link rel="canonical">` per pagina
- `<link rel="alternate" hreflang>` fra le 4 lingue di ogni pagina
- Open Graph + Twitter Card meta tag
- **schema.org JSON-LD** in `<script>` inline su ogni pagina:
  - `LocalBusiness` + `TravelAgency`
  - `AggregateRating` 4.9 / 87
  - `Review` × 3 testimonianze
  - `Product` × 2 barche con `Offer`
  - `TouristTrip` × 8 tour con itinerario completo
  - `FAQPage`
  - `BreadcrumbList`
  - `TouristAttraction` / `Place` × 13 destinazioni con `geo`
  - `Article` per il blog
- `sitemap.xml` con tutte le URL e gli alternates hreflang
- `robots.txt` allow-all + puntatore al sitemap

### Funzionalità interattive (JS)

- **Mappa Lago di Como** sulla home — Leaflet con tile CartoDB
  Voyager (nessuna API key), barca animata in orbita lungo
  l'itinerario
- **Carousel orizzontali** con frecce + dot indicators:
  - Tour sulla home
  - Flotta sulla home
  - Gallery foto per ogni barca
  - "Tours that include this attraction" sulle pagine attrazione
- **Form di prenotazione** con dropdown tour completo + opzione
  custom. Submit apre il client email dell'utente con tutti i campi
  precompilati (`mailto:` action)
- **Lead capture** "send me a sample itinerary" con singolo campo
  email + mailto
- **FAQ accordion** nativo `<details>`
- **Newsletter, accordion, lingua switcher, banner WhatsApp
  flottante** — tutto JS lato client già compilato in `_next/`

### Link autorevoli outbound

I testi delle pagine attrazione e dei tour linkano automaticamente:

- **FAI** (`fondoambiente.it`) per Villa del Balbianello
- **villacarlotta.it** per Villa Carlotta
- **villamonastero.eu** per Villa Monastero (Varenna)
- **castellodivezio.it** per il Castello di Vezio
- **giardinidivillamelzi.it** per Villa Melzi
- **navigazionelaghi.it** per i traghetti pubblici
- **funicolarecomo.it** per la funicolare
- **cattedraledicomo.it** per il Duomo
- Hotel partner: Mandarin Oriental, Grand Hotel Tremezzo,
  Grand Hotel Villa Serbelloni, Villa Passalacqua, Il Sereno,
  Villa d'Este

---

## Come modificare i contenuti

I file sono HTML puro: puoi aprirli con qualsiasi editor di testo.

### Cambio di un testo singolo

Apri il file in `<locale>/<percorso>/index.html`, cerca il testo,
modificalo, ricarica il file sul server.

### Cambio di un dato ricorrente (prezzo, telefono, indirizzo, …)

Ogni dato del sito appare di solito in più punti. Esempio: il prezzo
di un tour appare sia nella card della home sia nella pagina
dettaglio del tour sia nelle card "Tours that visit this attraction"
sulle pagine attrazione che lo includono, sia nei JSON-LD
`TouristTrip > offers > price` inline su quelle stesse pagine. Per
modificare un prezzo in modo coerente:

1. Cerca il valore vecchio in tutta la cartella:
   ```bash
   grep -rn "€220" .
   ```
2. Sostituiscilo nei file trovati:
   ```bash
   find . -name "*.html" -exec sed -i '' 's/€220/€240/g' {} \;
   ```
3. Verifica che il `sitemap.xml` e i JSON-LD siano coerenti

### Aggiungere una pagina

Duplica la cartella di una pagina esistente con struttura simile,
modifica il contenuto, aggiungi l'entry nel `sitemap.xml`.

### Aggiornare il feed Instagram

I 6 post Instagram bakerati nella home sono in `images/instagram/`
con il manifest implicito dentro l'HTML della home. Per aggiornarli
sostituisci le immagini con gli stessi filename e ridepoya.

---

## Performance & accessibilità

- Lighthouse mobile: ~90 Performance / 100 SEO
- Tutte le immagini hanno `loading="lazy"` e attributi `width` /
  `height` per evitare layout shift
- HTML semantico (`<nav>`, `<main>`, `<article>`, `<section>`)
- Contrast ratios WCAG AA
- Supporto RTL completo per la versione araba

### Miglioramenti possibili sulle immagini

Le foto sono JPG. Convertire in AVIF / WebP con un fallback `<picture>`
riduce ulteriormente il peso pagina del 30-50%. Sharp o `cwebp` da
linea di comando bastano. Non bloccante per il deploy.

---

## Costanti di contatto

Tutte le informazioni di contatto sono ripetute nelle pagine HTML.
Posizioni principali:

- **Nav** in cima a ogni pagina: WhatsApp pill
- **Booking form** in fondo alla home: telefono + email + indirizzo
- **Footer** di ogni pagina interna: WhatsApp + email + telefono
- **JSON-LD `LocalBusiness`** in `<script>` di ogni pagina: tutti
  i campi (`telephone`, `email`, `address`, `geo`, `openingHours`)

Per cambiare il telefono o l'email: `grep -rn "+39 340 6487574" .`
e sostituire ovunque.

---

## Tile della mappa

La mappa Leaflet usa tile **CartoDB Voyager** via il CDN pubblico
`basemaps.cartocdn.com`. Niente API key, niente costo. I tile sono
servirti da CartoDB sotto licenza ODbL (OpenStreetMap contributors);
nel codice della home la `attributionControl` di Leaflet è
nascosta perché la mappa è decorativa. Riattivala se in futuro
mostri la mappa con un contesto editoriale che richiede attribution.

---

## Foto

Tutte le immagini vivono in `images/`:

- `images/attractions/` — 13 foto destinazione (CC BY-SA da
  Wikimedia Commons; credits in `images/attractions/CREDITS.md`)
- `images/experiences/` — 4 foto esperienze
- `images/instagram/` — griglia Instagram
- `images/logo-flag*.png` — burgee + logo wordmark
- `images/hero-*.jpg`, `images/balbianello.jpg`, `images/bellagio.jpg`,
  `images/luxury-cruise.jpg`, ecc. — assorted

Le foto più importanti — gli eroi dei 4 tour + 25 foto per Google
Business Profile — sono in arrivo dal cliente. Quando arrivano,
sostituisci i file mantenendo lo stesso nome o aggiorna gli `src`
nei file HTML che le referenziano.

---

## Contatti

- **Como Boat Rental**: info@comoboatrental.it · +39 340 6487574
- **Indirizzo**: Lungolago Viale Geno 10, 22100 Como (IT)
- **WhatsApp**: https://wa.me/393406487574
- **Instagram**: https://www.instagram.com/comoboatrental
