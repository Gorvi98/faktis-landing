# Checkpoint — Landing Page Faktis

## Estado: diseño homologado en producción, deploy automático funcionando ✓

## Stack
- Astro 6.1.8 + Tailwind v4 (via @tailwindcss/vite) + TypeScript strict
- Build estático puro → /dist
- Deploy: Cloudflare Workers (assets estáticos vía `wrangler.jsonc`) — **no** Cloudflare Pages clásico
- Repo: github.com/Gorvi98/faktis-landing
- Producción: https://faktis.com.mx (proxy Cloudflare → Worker `faktis-landing`)

---

## 📅 Sesión: 2026-08-04 (Homologación de diseño + fix de deploy)

### ✅ Completado

**Homologación de diseño en las 3 páginas** (commit `f1158e6`)
- Conectadas `Metrics`, `HowItWorks` y `Security` al home — ya existían construidas (paridad con el import de Claude Design) pero nunca se habían montado en `index.astro`.
- Migrados `FeatureGrid`, `CTABanner`, `PricingCards`, `FAQ`, `DespachoSocio`, `ContactForm`, `Footer` y los hero cortos de `precios`/`contacto` del sistema ad-hoc (Tailwind con hex hardcodeado tipo `bg-[#0F172A]`) a las clases ya definidas en `landing.css`: `.sec`, `.fk-wrap`, `.sec-eyebrow`, `.page-hero`, `.cta-final`, `.partner`/`.partner-card`, `.fk-field`, `.footer`/`.footer-grid` — varias de esas clases estaban sin usar hasta esta sesión.
- Verificado: 0 clases `bg-[#...]`/`text-[#...]` hardcodeadas en el HTML final de las 3 páginas.

**Logo del nav agrandado** (commit `8008d22`)
- `.nav-logo img`: 40px → 56px alto (desktop), 32px → 42px (mobile).
- `--fk-nav-h`: 72px → 80px, para que `.page-hero` no quede tapado por el nav más alto.

**🔴 Fix crítico — el sitio llevaba meses sin actualizarse**

Dos problemas independientes, descubiertos en cadena al diagnosticar por qué `faktis.com.mx` no reflejaba el push:

1. **Proyecto mal configurado en Cloudflare** (commit `17060df`): el proyecto está creado como **Worker** (no Pages clásico), con `Deploy command: npx wrangler deploy`. Sin un `wrangler.jsonc` en el repo, cada build intentaba auto-instalar `@astrojs/cloudflare` vía `astro add cloudflare` (porque Wrangler asumía que necesitaba un adapter) y fallaba en el entorno no interactivo del build.
   → Fix: se agregó `wrangler.jsonc` en la raíz con `assets.directory: "./dist"` — le dice a Wrangler que esto son solo assets estáticos, sin necesidad de adapter ni Worker script.

2. **Integración GitHub↔Cloudflare rota**: el último deploy real, antes de hoy, era del **7 de mayo de 2026**. Ningún push desde entonces disparaba un build automático (el campo "Deploy command" no se podía vaciar porque es un proyecto Worker, no Pages). Se reconectó manualmente del lado de Cloudflare (Settings → gestión de GitHub); tras reconectar, Cloudflare pidió un push nuevo para arrancar el primer build — se disparó con un commit vacío (`5c3a51b`).

**Diagnóstico que sirvió (para la próxima vez que esto pase):**
- `curl -H "Cache-Control: no-cache" https://faktis.com.mx/` con `CF-Cache-Status: HIT` persistente incluso bypaseando cache = fuerte señal de que el problema NO es cache, sino que el recurso servido nunca se actualizó.
- Probar la URL nativa del Worker (`https://<worker>.<usuario>.workers.dev`) en paralelo al dominio custom aísla si el problema es el deploy en sí o el ruteo del dominio.
- "Deploy en verde" en el dashboard no confirma que sea el deploy más reciente — siempre confirmar la fecha/hash del commit del deployment más reciente en la lista.

### ⚙️ Estado al cerrar
- `faktis.com.mx` y `https://faktis-landing.r-gomez032.workers.dev/` sirven el HTML nuevo, confirmado con curl (cero rastros del diseño viejo).
- Deploy automático GitHub→Cloudflare funcionando de nuevo — pushes futuros a `main` deberían desplegarse solos.

---

## Archivos (estado actual)

```
faktis-landing/
├── public/assets/           faktis-logo.png, faktis-logo-horizontal.svg,
│                             faktis-logo-mark.svg, faktis-wordmark.svg,
│                             faktis-favicon.svg, faktis-icon.png, faktis-logo-onbg.png
├── src/
│   ├── components/
│   │   ├── Header.astro        Nav fijo, logo, links, CTA "Accede", menú móvil
│   │   ├── Hero.astro          Dark hero split + MockTable animada
│   │   ├── Metrics.astro       4 métricas con contador animado (IntersectionObserver)
│   │   ├── HowItWorks.astro    3 pasos con visual mono
│   │   ├── Security.astro      Break oscuro SAT — checklist + security-card
│   │   ├── FeatureGrid.astro   4 razones ("por qué Faktis")
│   │   ├── PricingCards.astro  Básico $499 / Pro $999
│   │   ├── DespachoSocio.astro Card dark 30% comisión (usa .partner/.partner-card)
│   │   ├── FAQ.astro           6 preguntas, acordeón details/summary
│   │   ├── ContactForm.astro   Form (usa .fk-field) + card WhatsApp
│   │   ├── CTABanner.astro     Break oscuro final (usa .cta-final)
│   │   ├── Footer.astro        4 columnas (usa .footer/.footer-grid)
│   │   └── MockTable.astro     Tabla animada del hero
│   ├── layouts/Layout.astro    HTML5, SEO, OG, Twitter Card, fuentes, banner construcción
│   ├── pages/
│   │   ├── index.astro         Hero → Metrics → HowItWorks → Security → FeatureGrid → CTA
│   │   ├── precios.astro       .page-hero + Pricing + Despacho + FAQ + CTA
│   │   └── contacto.astro      .page-hero + Form
│   └── styles/
│       ├── global.css          Tokens DS v1.2 (@theme Tailwind + :root vars)
│       └── landing.css         Estilos de sección, construidos sobre los tokens
├── wrangler.jsonc               Config Workers Static Assets (assets.directory → ./dist)
├── astro.config.mjs / tsconfig.json / package.json
```

## Comandos
```bash
npm run dev     → http://localhost:4321
npm run build   → /dist (build estático)
npx wrangler deploy --dry-run   → valida config sin desplegar
```

## Pendiente (próximas sesiones)
- [ ] og-image.png (1200×630) — FL-004
- [ ] Número WhatsApp real en ContactForm.astro (sigue `521XXXXXXXXXX`) — FL-002
- [ ] Conectar backend real al formulario de contacto — FL-003
- [ ] Página `/aviso-privacidad` (404 hoy) — FL-001, obligación LFPDPPP
- [ ] Analytics (Plausible o Cloudflare)
- [ ] QA visual manual en navegador de las 3 páginas homologadas (solo se verificó por HTML/build, no visualmente)
