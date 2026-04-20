# Checkpoint — Landing Page Faktis v1

## Estado: COMPLETO ✓

## Stack
- Astro 6.1.8 + Tailwind v4 (via @tailwindcss/vite) + TypeScript strict
- Build estático puro → /dist
- Deploy target: Cloudflare Pages

## Archivos creados

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
└── package.json
```

## Design system aplicado
- Color primario: `#1D9E75` (teal)
- Tipografía: Inter + JetBrains Mono
- Tokens en `@theme {}` de Tailwind v4 → clases `bg-faktis-teal`, etc.
- Clases custom: `.fk-eyebrow`, `.fk-mono`, `.fk-card`, `.btn-primary`, `.btn-secondary`

## Comandos
```bash
npm run dev    → http://localhost:4321
npm run build  → /dist (build estático)
```

## Build status
- `npm run build` → 3 páginas, 0 errores, 0 warnings ✓
- `npm run dev`   → responde 200 en localhost:4321 ✓

## Commit
dcb49b8 — feat: landing v1 con Astro + Tailwind

## Pendiente (próximas sesiones)
- [ ] og-image.png (1200×630)
- [ ] Conectar backend real al formulario de contacto
- [ ] Número WhatsApp real en ContactForm
- [ ] Email real hola@faktis.com.mx
- [ ] Configurar Cloudflare Pages + dominio faktis.com.mx
- [ ] Analytics (plausible o cloudflare)
- [ ] Página /aviso-privacidad
