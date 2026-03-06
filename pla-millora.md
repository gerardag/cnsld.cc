# Pla de millora global del blog cnsld.cc

## Context

Anàlisi complet del blog Hugo per trobar punts de millora en performance, SEO, accessibilitat, optimització de recursos i neteja de codi. El blog usa Hugo 0.147.2, tema "smol" personalitzat, i es desplega via GitHub Actions (`--minify`) a GitHub Pages amb Cloudflare.

---

## PRIORITAT ALTA — Impacte directe en performance i SEO

### 1. CSS: Cache busting i Hugo Pipes
**Problema:** `baseof.html:20` usa `?v={{ now.Unix }}` que invalida la caché del navegador en cada build, encara que el CSS no canviï. El CSS viu a `static/` i no passa per Hugo Pipes.

**Acció:**
1. Moure `themes/smol/static/css/style.css` → `themes/smol/assets/css/style.css`
2. A `baseof.html`, substituir la línia del CSS per Hugo Pipes amb fingerprint i SRI

**Fitxers:** `baseof.html`, moure `style.css`

---

### 2. Imatges: WebP automàtic, width/height, lazy loading
**Problema:** `render-image.html` serveix imatges tal qual sense width/height (causa CLS), sense `loading="lazy"`, i sense conversió a WebP. Hi ha 2 JPGs (189KB) i 1 PNG (64KB) que es beneficiarien de WebP.

**Acció:** Reescriure `render-image.html` per usar Hugo image processing:
- Carregar la imatge com a page resource
- Generar versió WebP amb `$img.Process "webp"`
- Usar `<picture>` amb `<source>` WebP i fallback original
- Afegir `width` i `height` reals
- Afegir `loading="lazy"` i `decoding="async"`
- Fallback per imatges externes (no page resources)

**Fitxers:** `themes/smol/layouts/_default/_markup/render-image.html`

---

### 3. Favicon.ico de 270KB
**Problema:** `assets/favicon.ico` pesa 270KB — hauria de ser <5KB. Ja existeix el SVG com a favicon principal.

**Acció:** Regenerar el favicon.ico a 48x48px (<5KB) o eliminar-lo i confiar només en el SVG.

**Fitxers:** `assets/favicon.ico`, potencialment `themes/smol/layouts/partials/favicon.html`

---

### 4. Meta description estàtica
**Problema:** `baseof.html:10` usa sempre `Site.Params.description` — totes les pàgines tenen la mateixa description. Google veu duplicats.

**Acció:** Prioritzar: description del post → summary truncat → description del site.

**Fitxers:** `themes/smol/layouts/_default/baseof.html`

---

### 5. `<time>` sense atribut `datetime`
**Problema:** `time.html` genera `<time>` sense `datetime`, essencial per SEO i accessibilitat.

**Acció:** Afegir `datetime="{{ .Date.Format "2006-01-02" }}"` al tag `<time>`.

**Fitxers:** `themes/smol/layouts/partials/time.html`

---

### 6. Meta tags OG/Twitter incomplets
**Problema:** `social.html` no té `twitter:card`, `twitter:title`, `twitter:description`, `article:tag` ni `article:modified_time`.

**Acció:** Afegir els meta tags que falten al final de `social.html`.

**Fitxers:** `themes/smol/layouts/partials/social.html`

---

## PRIORITAT MITJANA — Accessibilitat i qualitat

### 7. Logo `<img>` amb `width="auto"`
**Problema:** `header.html:5` té `width="auto"` i `height="50px"` — valors invàlids per atributs HTML (han de ser números sense unitat).

**Acció:** Canviar a `height="50"` i treure `width="auto"`.

**Fitxers:** `themes/smol/layouts/partials/header.html`

---

### 8. SVGs del toggle sense `aria-hidden="true"`
**Problema:** Els SVGs del sol i la lluna a `header.html` són decoratius però els lectors de pantalla intenten interpretar-los.

**Acció:** Afegir `aria-hidden="true"` a ambdós `<svg>`.

**Fitxers:** `themes/smol/layouts/partials/header.html`

---

### 9. Navegació sense `<ul>`
**Problema:** Els links del menú a `header.html` estan solts dins `<nav>`, sense estructura de llista.

**Acció:** Embolcallar amb `<ul>/<li>`. Adaptar CSS per `list-style: none; display: flex;`.

**Fitxers:** `themes/smol/layouts/partials/header.html`, `style.css`

---

### 10. TZ incorrecte a GitHub Actions
**Problema:** `.github/workflows/hugo.yaml:37` usa `TZ: America/Los_Angeles`. El blog és en català.

**Acció:** Canviar a `TZ: Europe/Madrid`.

**Fitxers:** `.github/workflows/hugo.yaml`

---

## PRIORITAT BAIXA — Neteja

### 11. Eliminar `X-UA-Compatible`
`baseof.html:6` — meta tag obsolet d'IE.

### 12. Eliminar `cnsld.webp` no usat
`assets/images/cnsld.webp` (17KB) — no referenciat enlloc.

### 13. Eliminar `sidebar.html` no usat
`themes/smol/layouts/partials/sidebar.html` — només referenciat en un comentari a `single.html:47`.

### 14. Eliminar `pagination.html` no usat
`themes/smol/layouts/partials/pagination.html` — no cridat, text en anglès.

---

## Ordre d'implementació recomanat

| Pas | Items | Fitxer principal | Benefici |
|-----|-------|-----------------|----------|
| 1 | 1 (Hugo Pipes) | baseof.html + moure CSS | Caché eficient + SRI |
| 2 | 2 (render-image) | render-image.html | WebP auto + CLS + lazy |
| 3 | 4, 5, 11 (meta/SEO) | baseof.html, time.html | SEO |
| 4 | 6 (OG/Twitter) | social.html | Social sharing |
| 5 | 3 (favicon) | favicon.ico | -265KB |
| 6 | 7, 8, 9 (header) | header.html + CSS | Accessibilitat |
| 7 | 10 (TZ) | hugo.yaml | Dates correctes |
| 8 | 12-14 (neteja) | Eliminar fitxers | Higiene |

---

## Verificació

Després de cada pas:
1. `hugo server` — comprovar que no hi ha errors de build
2. Verificar tema clar i fosc: home, post individual (amb imatges), arxiu, pàgina de tag
3. Inspeccionar el `<head>` generat per validar meta tags
4. Lighthouse per confirmar millores en Performance i SEO
