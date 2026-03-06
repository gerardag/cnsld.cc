# Anàlisi i pla de refactorització del CSS

Fitxer afectat: `themes/smol/static/css/style.css` (459 línies)

---

## Problemes identificats

### 1. Duplicació entre `.archive-main` i `.single-post, .page`

Aquestes tres classes comparteixen exactament els mateixos estils:

| Propietat | `.single-post, .page` | `.archive-main` |
|---|---|---|
| `section padding-block` | `calc(var(--spacing) * 8)` | `calc(var(--spacing) * 8)` |
| `header padding-inline` | `calc(var(--spacing) * 8)` | `calc(var(--spacing) * 8)` |
| `h1 font-size` | `3rem` | `3rem` |
| `h1 letter-spacing` | `-0.05em` | `-0.05em` |
| `h1 line-height` | `1` | `1` |
| `h1 margin-top` | `calc(var(--spacing) * 8)` | `calc(var(--spacing) * 8)` |
| `max-width` | `48rem` | `48rem` |

L'única diferència: `.archive-main h1` té `margin-bottom: calc(var(--spacing) * 10)` que els altres no.

**Solució**: Afegir `.archive-main` al selector existent. La diferència del `margin-bottom` queda com a override mínim.

---

### 2. Padding duplicat al header i footer

```css
/* Línies 77-78 */
.site-header > div {
  padding-inline: calc(.25rem * 4);
  padding-block: calc(.25rem * 3);
}

/* Línies 286-287 — idèntic */
.site-footer > div {
  padding-inline: calc(.25rem * 4);
  padding-block: calc(.25rem * 3);
}
```

**Solució**: Combinar en un sol selector:
```css
.site-header > div,
.site-footer > div {
  padding-inline: calc(.25rem * 4);
  padding-block: calc(.25rem * 3);
  max-width: 72rem;
  width: 100%;
}
```

---

### 3. Anidació de 4 nivells a `.post-content`

Dins de `.single-post, .page` hi ha `.post-content` amb regles que arriben a 4 nivells:

```
.single-post, .page
  └── .post-content          (nivell 2)
        └── figure           (nivell 3)
              └── img        (nivell 4)
```

**Solució**: Treure `.post-content` fora del bloc pare i usar selector directe `.post-content`:
```css
.post-content { ... }
.post-content h2 { ... }
.post-content figure img { ... }
.post-content blockquote { ... }
```

---

### 4. Inconsistència de breakpoints

- `.single-post, .page` → media query a `768px`
- `.archive-layout` → media query a `640px`

Hauria de ser un sol breakpoint per a tota la pàgina.

---

### 5. `a:hover` redundant

```css
/* Línia 127 */
a { text-decoration: none; }

/* Línia 131 — redundant, ja és none */
a:hover { text-decoration: none; }
```

---

### 6. Especificitat innecessària a les icones del toggle

```css
#theme-toggle .icon-sun { display: none; }
html.dark #theme-toggle .icon-sun { display: block; }
```

El selector `#theme-toggle` és innecessari si `.icon-sun` i `.icon-moon` s'usen únicament dins del toggle.

---

## Pla d'implementació (per fases)

### Fase A — Eliminació de duplicació (baix risc)
1. Afegir `.archive-main` al selector `.single-post, .page`
2. Eliminar les regles duplicades de `.archive-main` (section, header, h1 base, max-width)
3. Mantenir únicament el `margin-bottom` del h1 com a override
4. Consolidar el padding del header i footer en un sol selector

### Fase B — Desanidació de `.post-content` (risc moderat)
1. Extreure `.post-content` fora de `.single-post, .page`
2. Usar selectors directes per a `h2`, `figure img`, `blockquote`, etc.
3. Verificar que no hi ha col·lisions amb altres `.post-content` al markup

### Fase C — Neteja de redundàncies (baix risc)
1. Eliminar `a:hover { text-decoration: none }` redundant
2. Unificar breakpoints a `768px`
3. Simplificar selectors d'icones del toggle

---

## Resultat esperat

| Mètrica | Actual | Objectiu |
|---|---|---|
| Línies totals | 459 | ~400 (-13%) |
| Nivells màxims d'anidació | 4 | 2 |
| Duplicacions | 3 blocs | 0 |
| Breakpoints | 2 (640px, 768px) | 1 (768px) |

---

## Notes importants

- **Cap canvi visual**: Totes les fases són refactoritzacions pures.
- **Ordre recomanat**: A → C → B (la fase B és la de més risc per possible impacte en especificitat)
- **Verificació**: Comprovar totes les pàgines en tema clar i fosc — home, post individual, arxiu, pàgina de tag, pàgina "now"
