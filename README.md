# Como Boat Rental — sito statico (HTML / CSS / JS)

Questa cartella contiene il sito **comoboatrental.com** in formato
statico puro: 126 file `.html`, i bundle `.css` e `.js`, le immagini
e gli asset. Niente backend, niente framework runtime, niente Node
da installare. Si carica su qualsiasi web server — Apache, Nginx,
Vercel, Netlify, Cloudflare Pages, GitHub Pages, FTP — e funziona.

---

## Da dove arriva questo sito

Questa cartella **non è scritta a mano**. È l'output di un build
generato da un sorgente Next.js (TypeScript + React) che vive in un
repository separato:

> **Sorgente canonico:** https://github.com/davidfrancesconi/comoboatrental
> (cartella `site/`)

Il sorgente:

- ha 4 file di contenuto (tour, attrazioni, FAQ, blog) da cui
  vengono generate automaticamente le 96 pagine in 4 lingue
- mantiene da sé tutti i meta-dati SEO (schema.org JSON-LD per
  ogni pagina, hreflang, sitemap, breadcrumbs, Open Graph)
- contiene un linkificatore che trasforma automaticamente i nomi
  delle attrazioni in cross-link interni + link autorevoli esterni
  (FAI, Villa Carlotta, Mandarin Oriental, ecc.)
- è documentato in italiano in `docs/HANDOFF.md`, `docs/SEO.md`,
  `docs/PER-LORIS.md`

**Per qualsiasi cambio strutturale (nuovo tour, nuova attrazione,
cambio prezzo, nuova lingua) il modo corretto è modificare il
sorgente e ri-eseguire il build**, non editare i file HTML uno per
uno in questa cartella. Vedi sezione "Workflow consigliato" sotto.

---

## Come spedire questo sito (deploy)

### Opzione A — qualsiasi host statico

Carica il contenuto di questa cartella nella root web del tuo host.
Esempio FTP:

```
ftp -e ftp.comoboatrental.com
cd /public_html
mput -r ./*
```

Esempio Vercel (CLI):

```
vercel --prod
```

Esempio Netlify (CLI):

```
netlify deploy --prod --dir .
```

Funziona ovunque perché è HTML puro.

### Opzione B — GitHub Pages

1. Push di questa repo su `gh-pages` o root del default branch
2. Settings → Pages → Source → quel branch
3. Custom domain → `comoboatrental.com`

---

## Struttura

```
/                            ← root del sito
├── index.html               ← redirect alla lingua del browser
├── 404.html                 ← pagina di errore
├── favicon.ico              ← favicon multi-resolution
├── icon-32.png, icon-192.png, icon-512.png
├── manifest.webmanifest     ← PWA manifest
├── robots.txt
├── sitemap.xml              ← sitemap di tutte le 96 pagine
├── en/                      ← versione inglese (96 ÷ 4 = 24 pagine)
│   ├── index.html           ← homepage
│   ├── about/index.html
│   ├── attractions/index.html
│   ├── attractions/bellagio/index.html
│   ├── attractions/villa-del-balbianello/index.html
│   ├── … (13 attrazioni totali)
│   ├── experiences/weddings/index.html
│   ├── experiences/photoshoots/index.html
│   ├── experiences/captains/index.html
│   ├── experiences/private-tours/index.html
│   ├── tours/highlights-1h/index.html
│   ├── tours/cernobbio-2h/index.html
│   ├── … (8 tour totali)
│   ├── faq/index.html
│   ├── reviews/index.html
│   └── blog/best-months-lake-como-by-boat/index.html
├── it/                      ← versione italiana, stessa struttura
├── ru/                      ← russo
├── ar/                      ← arabo (RTL)
├── images/                  ← tutte le foto + assets
│   ├── attractions/         ← 13 foto attrazioni
│   ├── experiences/         ← 4 foto esperienze
│   ├── instagram/           ← feed Instagram bakerato al build
│   └── …
├── _next/                   ← bundle JS + CSS compilati
│   ├── static/chunks/       ← codice JavaScript (Leaflet map,
│   │                         booking form, carousels, ecc.)
│   ├── static/css/          ← stylesheet compilato
│   └── …
└── __next.*.txt             ← payload RSC per hydration React
                              (utili solo se servi con un host
                              che parla con il router Next.js;
                              cancellabili se non ti serve)
```

---

## Cosa c'è dentro al sito (in breve, per riferimento SEO)

- **96 pagine indipendenti** in 4 lingue (EN / IT / RU / AR)
- **schema.org JSON-LD** su ogni pagina (LocalBusiness,
  TouristTrip, TouristAttraction, FAQPage, BreadcrumbList,
  AggregateRating 4.9/87, Review × 3, Product, Article)
- **hreflang** wired correttamente fra le 4 lingue di ogni pagina
- **canonical** per pagina, lingua-aware
- **Open Graph + Twitter Card** per share su WhatsApp/Facebook/X
- **Sitemap** in `/sitemap.xml`
- **Robots** in `/robots.txt`
- **Linkificatore automatico** che nei testi delle 13 pagine
  attrazione + delle 8 pagine tour:
  - linka i nomi delle altre attrazioni alle loro pagine
    (cross-link interno)
  - linka le entità autorevoli (FAI per Villa del Balbianello,
    villacarlotta.it, villamonastero.eu, mandarinoriental.com,
    serenohotels.com, villadeste.com, grandhoteltremezzo.com,
    passalacqua.it, fondoambiente.it, navigazionelaghi.it,
    castellodivezio.it, giardinidivillamelzi.it)
  - sulle pagine attrazione, il riferimento all'attrazione stessa
    nel corpo del testo linka al sito ufficiale (es. sulla pagina
    Balbianello, "Villa del Balbianello" → FAI)
- **Mappa Leaflet interattiva** sulla home con barca animata
  (CartoDB Voyager tiles, niente API key)
- **Carousel orizzontali** con frecce + dots: tour sulla home,
  flotta, gallery foto barca, tours-che-includono-questa-attrazione
  sulle pagine attrazione
- **BookingForm** con dropdown tour completo (8 SKU + custom) e
  azione `mailto:` precompilata
- **Pannelli "Useful links"** in sidebar di ogni pagina attrazione
  con link a Maps + sito ufficiale + Wikipedia + Navigazione Laghi

---

## Workflow consigliato per i cambiamenti

### Cambio piccolo (testo, prezzo, link)

Se devi cambiare un singolo testo:

1. Apri la pagina interessata in `<locale>/<percorso>/index.html`
2. Modifica il testo direttamente nel file
3. Carica il file modificato sul server

**ATTENZIONE:** ogni testo del sito appare di solito in più punti.
Esempio: il prezzo del tour 1-ora ("from €220") appare in:

- `<en|it|ru|ar>/index.html` (card nella home × 4 lingue)
- `<en|it|ru|ar>/tours/highlights-1h/index.html` (pagina dettaglio × 4)
- `<en|it|ru|ar>/attractions/<varie>/index.html` (card "Tours that
  visit this attraction" × ogni attrazione che lo include × 4 lingue)
- JSON-LD `TouristTrip > offers > price` su tutti i precedenti

Cambio prezzo in un singolo file → il sito diventa incoerente. Per
qualsiasi cambio strutturale (prezzo, contatto, nuovo tour, …) usa
il workflow B sotto.

### Cambio strutturale (nuovo tour, nuova attrazione, nuova lingua, prezzo, telefono, …)

1. `git clone https://github.com/davidfrancesconi/comoboatrental.git`
2. `cd comoboatrental/site`
3. `bun install` (o `npm install`)
4. Modifica il file di contenuto rilevante:
   - `app/content/tours.ts` per i tour
   - `app/content/attractions.ts` per le attrazioni
   - `app/content/faq.ts` per le FAQ
   - `app/seo.ts` per costanti (telefono, email, prezzo range, ecc.)
   - `app/translations.ts` per il resto del copy
5. `bun run build`
6. Copia il contenuto di `out/` in questa repo (`comoboatrental-static`)
7. Commit + push qui

Lo step 4-5 ci mette **meno tempo di editare 96 file HTML a mano**,
e il sito resta coerente.

---

## Cose che si rompono se modifichi solo l'HTML

| Cosa | Quando si rompe |
|---|---|
| `<link rel="alternate" hreflang>` | Cambi una pagina ma non l'equivalente nelle altre 3 lingue → Google ti penalizza |
| `JSON-LD AggregateRating` (le stelline) | Cambi le recensioni in `/reviews` ma non nei 95 JSON-LD inline → Google smette di mostrare le 4.9 stelle |
| `JSON-LD TouristTrip > itinerary` | Aggiungi tappa al tour ma non aggiorni i 4 JSON-LD del tour × 4 lingue → Google ignora il rich result |
| `sitemap.xml` | Aggiungi pagina nuova ma non la metti nel sitemap → Google non la trova |
| Linkificatore | Aggiungi un'attrazione nuova ma le altre 12 non la linkano automaticamente nel body → niente PageRank interno |
| Open Graph image | Cambi l'immagine in un solo file ma non negli altri share contexts → preview WhatsApp inconsistente |

Tutte queste cose sono **già fatte e mantenute automaticamente** dal
sorgente. Per non perderle: usa il sorgente.

---

## Contatti

- **Loris / Claudio:** info@comoboatrental.it · +39 340 6487574
- **Sorgente / build:** David Francesconi
- **Repo sorgente:** https://github.com/davidfrancesconi/comoboatrental

---

*Generato dal sorgente Next.js il 2026-05-15.*
