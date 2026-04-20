# Checkpoint — Landing Page Faktis v1

## Estado: v1 completa, pusheada a GitHub ✓

## Stack
- Astro 6.1.8 + Tailwind v4 (via @tailwindcss/vite) + TypeScript strict
- Build estático puro → /dist
- Deploy target: Cloudflare Pages
- Repo: github.com/Gorvi98/faktis-landing

## Archivos

```
faktis-landing/
├── public/
│   ├── favicon.svg          SVG 32x32 cuadrado teal con F blanca
│   └── robots.txt           Allow all, sitemap pointer
├── src/
│   ├── components/
│   │   ├── Header.astro     Nav fijo, logo, 3 links, CTA "Entrar", menú móvil
│   │   ├── Footer.astro     4 columnas: brand, producto, empresa, social
│   │   ├── Hero.astro       Dark hero, H1, subhead, 2 CTAs, badge
│   │   ├── FeatureGrid.astro 4 features + sección "Cómo funciona" (3 pasos)
│   │   ├── PricingCards.astro Básico $499 / Pro $999 con features y badges
│   │   ├── DespachoSocio.astro Card dark 30% comisión recurrente
│   │   ├── FAQ.astro        6 preguntas con acordeón details/summary
│   │   ├── ContactForm.astro Form HTML + card WhatsApp + info contacto
│   │   └── CTABanner.astro  Banner teal final con CTA
│   ├── layouts/
│   │   └── Layout.astro     HTML5, SEO, OG, Twitter Card, fuentes, slot
│   ├── pages/
│   │   ├── index.astro      Home completo
│   │   ├── precios.astro    Hero + Pricing + Despacho + FAQ + CTA
│   │   └── contacto.astro   Hero + Form + WhatsApp
│   └── styles/
│       └── global.css       @theme tokens, variables CSS, utility classes
├── astro.config.mjs
├── tsconfig.json
└── package.json             name: "faktis-landing"
```

## Design system aplicado
- Color primario: `#1D9E75` (teal) · Warning: `#EF9F27` (amber)
- Tipografía: Inter + JetBrains Mono
- Tokens en `@theme {}` de Tailwind v4 → `bg-faktis-teal`, etc.
- Clases custom: `.fk-eyebrow`, `.fk-mono`, `.fk-card`, `.btn-primary`, `.btn-secondary`

## Comandos
```bash
npm run dev    → http://localhost:4321
npm run build  → /dist (build estático)
```

## Build status
- `npm run build` → 3 páginas, 0 errores, 0 warnings ✓
- `npm run dev`   → responde 200 en localhost:4321 ✓

## Historial de commits
| Hash      | Descripción |
|-----------|-------------|
| `c05019e` | chore: nombre paquete faktis-landing + checkpoint |
| `3fd5c85` | fix: btn secundario, h2 precios, banner en construcción |
| `dcb49b8` | feat: landing v1 con Astro + Tailwind |

## Fixes aplicados (sesión 1)
- `Hero.astro` — botón "Ver cómo funciona" con `color !important` para ser visible sobre fondo dark
- `PricingCards.astro` — H2 cambiado de texto duplicado a "Elige tu plan"
- `Layout.astro` — banner amber sticky "Sitio en construcción" encima del Header en todas las páginas
- `package.json` — name corregido a "faktis-landing"

## Pendiente (próximas sesiones)
- [ ] og-image.png (1200×630)
- [ ] Número WhatsApp real en ContactForm.astro
- [ ] Email real hola@faktis.com.mx
- [ ] Conectar backend real al formulario de contacto
- [ ] Configurar Cloudflare Pages + dominio faktis.com.mx
- [ ] Analytics (Plausible o Cloudflare)
- [ ] Página /aviso-privacidad
