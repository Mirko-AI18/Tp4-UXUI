# AGENTS.md — Landing Page Coffea (HTML + CSS)

Fuente de verdad: `docs/PRD.md` (TP4_PRD_Landing_Coffea.pdf). Ante dudas, leer el PRD antes de decidir. No inventes valores: si falta un dato, dejalo marcado con `TODO:` y avisame.

## Stack (estricto)
- Solo HTML5 semántico + CSS3 (custom properties, Flexbox, Grid, scroll-snap, @layer).
- PROHIBIDO: JavaScript (ni un `<script>`), Tailwind, Sass, bundlers, frameworks, CMS.
- Sitio estático, un solo `index.html`. Deploy en Netlify (sin build command, publish = ".").

## Estructura de archivos
```
coffea-landing/
├── index.html
├── 404.html            (mismo header y footer que la landing)
├── netlify.toml
├── README.md
├── css/
│   ├── main.css        (@import de capas, en orden)
│   ├── tokens.css      (custom properties)
│   ├── base.css        (reset moderno, tipografía base, :focus-visible)
│   ├── layout.css      (.container, franjas)
│   ├── utilities.css   (.visually-hidden, .skip-link)
│   └── components/ header, hero, categories, product-card, carousel,
│                   promo, testimonial-card, newsletter, footer
└── assets/ img/ · icons/ · fonts/ (opcional)
```
Orden de capas: `@layer tokens, base, layout, components, utilities;` — sin `!important` (única excepción: el bloque `prefers-reduced-motion`).

## Convenciones CSS
- BEM: `.product-card`, `.product-card__title`, `.product-card--featured`.
- CERO valores sueltos: todo color, fuente, radio y espaciado sale de un token de `tokens.css`.
- Mobile-first: estilos base para 360 px y `@media (min-width: …)` hacia arriba.
- `rem` para tipografía y espacios; `clamp()` para títulos fluidos (ej. `--fs-hero: clamp(2.5rem, 1.5rem + 3.5vw, 4.5rem)`).
- Flexbox/Grid para layout; `position: absolute` solo para: header sobre hero, corazón sobre imagen, imágenes decorativas.
- Breakpoints: base 360–767 · md ≥768 · lg ≥1024 · xl ≥1440 (contenedor 1444 px centrado).
- Contenedor: base 100% − 2×16 · md 100% − 2×32 · lg 100% − 2×48 · xl 1444 px.

## Design tokens
Colores: espresso #30261C · maroon #2A0000 · sand #E2D9C8 · sand-20 rgba(226,217,200,.2) · sand-40 rgba(226,217,200,.4) · bg #F1F0EE · muted #544A3E · border-soft rgba(48,38,28,.1) · text-soft rgba(48,38,28,.7) · white #FFFFFF · star (TODO: extraer del SVG).

Fuentes (Google Fonts, `font-display: swap`, solo estos pesos):
- `--font-display`: Playfair Display 500/600/700
- `--font-body`: Outfit 400/500/600
- `--font-ui`: Poppins 400/500/600
- `--font-accent`: Reggae One 400 (solo "Come and Join")

Escala (desktop): hero 72/82 bold · h2 32 semibold uppercase tracking 1.28px · cta 40/50 semibold · card-title 38 semibold · logo 32 bold lowercase · footer-h 24 medium uppercase · lg 20 tracking 4px uppercase · md 18 · base 16 · sm 14 · xs 12/20.

Forma: radius-pill 100px · radius-card 18px · radius-img 12px · radius-btn 8px · container 1444px · space: 4, 8, 12, 16, 24, 28, 40, 56 px.

## HTML semántico
```html
<body>
  <a class="skip-link" href="#main">Saltar al contenido</a>
  <header class="header">…</header>
  <main id="main">
    <section class="hero" aria-labelledby="hero-title">…</section>
    <section class="categories" aria-label="Categorías">…</section>
    <section class="products" aria-labelledby="coffee-title">…</section>
    <section class="products" aria-labelledby="dessert-title">…</section>
    <section class="promo">…</section>
    <section class="testimonials" aria-labelledby="testimonials-title">…</section>
    <section class="newsletter">…</section>
  </main>
  <footer class="footer">…</footer>
</body>
```
- Un único `<h1>` (hero). `<h2>` por sección. `<h3>` para nombres de producto y columnas del footer.
- Card = `<article>` con `<h3>`; precio con `<data value="200">`.
- Testimonio = `<figure>` + `<blockquote>` + `<figcaption>`; rating con `aria-label="4 de 5 estrellas"`.
- Newsletter: `<form action="#">`, `<label>` visualmente oculto, `input type="email" required autocomplete="email"`.
- Categorías: `<ul>` de `<a>`; íconos con `alt=""`.
- Footer: columnas como `<nav>` o `<ul>` con encabezado; redes con `aria-label`; área táctil ≥ 44×44.

## Medidas de referencia (Figma 1920 px, tolerancia ≤ 4 px)
- Header: absoluto sobre hero, a 55 px del borde superior, alineado al contenedor; botón buscar 61 px circular blanco.
- Hero: 1000 px (min-height 100vh con tope 1000), texto 557 px de ancho a 238 px izq / 294 px arriba. Eyebrow → 40px → H1 → 56px → párrafo (máx 431px) → 56px → botón (padding 16×40).
- Categorías: franja 220 px, fila de 845 px, íconos 82 px.
- Carruseles: 4 cards 366×570, gap 4, centrado en 1476; flechas 71 px a 80 px de cada borde; imagen 334×364.
- Card: fondo sand-20, borde 1.5px sand, radio 18, padding 16, gap 16. Botón Order Now maroon, radio 8, padding 10×26.
- CTA: franja 335 px; título máx 372 px en 2 líneas; botón 258×51.
- Testimonios: central 441×235, laterales 417×207, fondo sand-40, borde 2px border-soft, padding 28.
- Newsletter: franja 320 px; input 364×44; gap 12; imagen derecha espejada con `transform: scaleX(-1)`.
- Footer: 358 px, fondo espresso, separación vertical entre links 42 px.

## Interacciones SOLO con CSS
- Carrusel: `overflow-x:auto; scroll-snap-type:x mandatory; scroll-snap-align:start; scrollbar-width:none`. Flechas = anclas `<a href="#coffee-5">` + `scroll-behavior:smooth`.
- Paginador testimonios: 3 anclas `<a href="#t-1">`.
- Menú mobile: `<details>/<summary>` estilizado como hamburguesa.
- Favorito: `<input type="checkbox">` + `<label>` con `:checked` (corazón relleno).
- Hover botones: `translateY(-2px)` + color, `transition: 200ms ease`. Hover cards solo dentro de `@media (hover:hover)`.
- Foco: `:focus-visible` outline 2px espresso, offset 3px (blanco sobre fondos oscuros). Obligatorio en todo interactivo.
- Newsletter: validación nativa + `:user-invalid`.
- Incluir `@media (prefers-reduced-motion: reduce)` que anule transitions, animations y scroll-behavior.

## Accesibilidad y performance
- WCAG 2.2 AA: contraste 4.5:1 (3:1 texto grande). Overlay oscuro en el hero si hace falta.
- `alt` descriptivo en fotos de producto; `alt=""` en decorativas. Botones solo-ícono con `aria-label`.
- Imágenes con `<picture>` (AVIF/WebP + JPG), `width`/`height` explícitos, `srcset/sizes`.
- Hero: `fetchpriority="high"`, sin lazy. Resto: `loading="lazy" decoding="async"`.
- `preconnect` a Google Fonts. Íconos repetidos (corazón, estrella, chevron) como un solo SVG reutilizado.
- Sin scroll horizontal entre 360 y 1920 px. Áreas táctiles ≥ 44×44 en base y md.

## Correcciones de copy (typos del Figma)
"ger"→"get" · "in get"→"and get" · "Out Products"→"Our Products" · "Out story"→"Our story" · "INFOTNATION"→"INFORMATION" · "Plons & pricing"→"Plans & pricing" · "prodcts"→"products". Marca: "Coffea" en header y footer. Moneda del diseño: "Rs." (pendiente confirmar). Dessert: usar un solo título.

## SEO / entrega
- `<title>` y `<meta name="description">` únicos, Open Graph básico, favicon. `lang` según idioma final (TODO: confirmar).
- `netlify.toml`: publish ".", headers de seguridad (nosniff, X-Frame-Options DENY, Referrer-Policy) y caché immutable en /assets/* y /css/*.
- Metas: Lighthouse Perf ≥90 (mobile), A11y/BP/SEO ≥95, 0 errores W3C HTML y CSS, peso ≤ 1,5 MB.

## Cómo trabajar (reglas para el agente)
1. Antes de escribir código, mostrame un plan corto (Artifact) y esperá mi OK en cada fase.
2. Trabajá de a una fase por vez (PRD sección 11). No adelantes fases.
3. Al terminar, verificá en el navegador a 1920, 1024, 768 y 360 px y adjuntá capturas.
4. Si falta un asset, usá un placeholder (`assets/img/placeholder-*.svg`) y listalo; no descargues fotos de stock.
5. Antes de cerrar cada fase, revisá: ningún valor suelto fuera de tokens, 0 JS, foco visible, sin scroll horizontal.
