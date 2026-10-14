# Coffea: landing page (HTML + CSS)

Landing estática de una página, sin JavaScript. TP4, Diseño UX-UI 2026.

## Estructura
- `index.html`, `404.html`, `netlify.toml`
- `css/`: `main.css` importa las capas (`tokens → base → layout → components → utilities`)
- `css/components/`: un archivo por componente (BEM)
- `assets/`: `img/` (placeholders SVG), `icons/` (SVG monocromos, se tiñen con `mask`)

## Convenciones
BEM, mobile-first, `rem` y `clamp()`, cero valores sueltos de color/fuente/radio (todo en `tokens.css`).
Breakpoints: md 48rem, lg 64rem, xl 90rem.

## Levantar en local
Abrir la carpeta en VS Code y usar la extensión **Live Server** sobre `index.html`
(los íconos usan `mask`, por lo que no funcionan abriendo el archivo con `file://`).

## Pendiente
- Reemplazar placeholders de `assets/img` por las fotos exportadas del Figma (AVIF/WebP + JPG con `<picture>`, ≤ 1,5 MB en total).
- Extraer `--color-star` del SVG de estrellas del Figma.
- Confirmar idioma (`lang`), moneda ("Rs.") y licencia de las fotos de stock.
- Con caché `immutable`, renombrar con versión los CSS/assets modificados (p. ej. `main.v2.css`).

## Deploy (Netlify)
1. Subir el repo a GitHub/GitLab.
2. En Netlify: *Add new site → Import from Git*. Sin build command, publish directory `.`
3. `main` publica a producción; cada Pull Request genera un Deploy Preview.
4. Medir Lighthouse sobre la URL de producción.
