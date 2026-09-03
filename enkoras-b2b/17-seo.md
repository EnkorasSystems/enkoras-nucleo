# 17 · SEO y GEO de toda la plataforma

Fecha: 2026-09-02 · Orden de Javi: el análisis y el plan no se centran solo en licitaciones — es el SEO **completo de toda la plataforma** — y además: *"estaría muy bien que el SEO [tenga] los benchmarks que ayuden a que salgamos en búsqueda de IA"*.

Fuentes completas: [18-seo-research.md](18-seo-research.md) (research 2026 con fuentes verificadas) y [19-seo-auditoria-2sep.md](19-seo-auditoria-2sep.md) (auditoría interna verificada EN VIVO contra enkoras.com).

---

## El diagnóstico en una línea

El motor SEO estaba **sano pero vacío**: canonical/hreflang/JSON-LD/robots impecables, pero 1 sola empresa activa, 517 categorías cascarón, y las licitaciones —el contenido más fresco de la casa— eran invisibles para Google (vivían tras `?sel=` con canónica al tablón). El hueco de contenido lo llena el semillero comercial; los huecos de código se cerraron todos el 2-sep.

---

## Lo que se construyó (4 bloques, 2-sep-2026)

### Bloque 1 — La hoja pública de licitación

- **`/licitaciones/[id]`** (`app/[locale]/(public)/licitaciones/[id]/page.tsx`): URL permanente e indexable por licitación, ISR 5 min, con el **ciclo de vida oficial de Google para listados que expiran** (patrón job postings):
  - ABIERTA → `index, follow, unavailable_after: <cierre ISO>` — se auto-desindexa sola al cerrar, sin trabajo manual.
  - CERRADA / DESIERTA → `noindex, follow` (pueden revivir con "extender").
  - ADJUDICADA / CANCELADA → **404** (tumba, también para Google).
- BreadcrumbList JSON-LD + miga visible, datos duros (cantidad, cierre TZ Tijuana, ubicación, cupo), card de la convocante → `/empresa/[slug]`, CTAs a la sala real (`/licitaciones?sel=`) y al tablón. i18n namespace `HojaBid` (unidades reusadas de `TendersPage`).
- Sitemap: `/licitaciones` + una entrada por licitación **abierta** con lastmod real.

### Bloque 2 — Higiene técnica

| Hueco (auditoría) | Arreglo |
|---|---|
| `/auth/*` emitía `index, follow` | `app/[locale]/auth/layout.tsx` nuevo: `noindex` para TODA la sección |
| `/solicitudes` era 200 + meta-refresh | `redirects()` en `next.config.ts`: **308 reales** desde el edge (ES y `/en`, nueva → nueva, resto → tablón cotización) |
| Sitemap solo listaba URLs ES | Cada ruta emite **dos entradas** (ES y `/en`), ambas con el set completo de alternates (guía hreflang de Google) |
| `changefreq: daily` inventado, 1 solo lastmod | changefreq/priority eliminados (Google los ignora y eran mentira); lastmod **solo cuando sale de la BD** (empresas, licitaciones) |
| Descriptions débiles | Legales con description propia; `/planes` con **precios en el snippet** ($649/$1,190, keyword transaccional); empresa corta en frontera de palabra (`descripcionSeo()` en `lib/seo.ts`, compartido) |
| 517 categorías cascarón = thin content masivo | Categoría vacía → **noindex automático** + fuera del sitemap (semántica de RAMA por prefijos de code); revive sola al llegar su primera empresa. La que tiene inventario lleva **cifra real** en la description ("N proveedores") |
| `/categoria/[slug]` 100% dinámica (SSR por cada hit del crawler) | `unstable_cache` en el camino sin filtros (el del crawler): rama 5 min, total 1 h. La ruta sigue dinámica por los filtros, pero la BD ya no paga cada rastreo |

### Bloque 3 — GEO: los benchmarks para salir en búsqueda de IA

Lo que los estudios 2025-2026 dicen que correlaciona con ser citado por ChatGPT/Perplexity/Gemini (fuentes en el research §4), y cómo quedó cada palanca:

1. **Bots de IA bienvenidos con nombre** (`app/robots.ts`): GPTBot, OAI-SearchBot (el de ChatGPT Search — es OTRO bot que GPTBot), ChatGPT-User, ClaudeBot/Claude-SearchBot/Claude-User, PerplexityBot/Perplexity-User, Google-Extended, meta-externalagent, Applebot-Extended — todos con allow explícito y los mismos disallows de lo privado (un grupo con UA propio sustituye al `*`).
2. **IndexNow → Bing** (`lib/indexnow.ts` + llave pública en `public/<llave>.txt`): el índice de Bing alimenta a ChatGPT Search y Copilot. Ping automático (fire-and-forget con `after()`, jamás bloquea la acción) en los 4 momentos de vida de una URL: **publicar** licitación, **adjudicar**, **cancelar**, **extender/revivir** — y al **registrar empresa** (perfil nuevo). Solo en producción (BASE https).
3. **Cifras citables**: las IAs citan páginas con datos únicos y verificables — la description de categoría ahora lleva el conteo REAL de proveedores de la rama; `/planes` lleva los precios (los directorios mexicanos los esconden — dato citable diferenciador).
4. **Entidad de marca legible**: Organization + WebSite + SearchAction ya existían en el layout raíz (verificados en vivo); se les añadió la `description` citable de la entidad ("Directorio B2B de México: …"). LocalBusiness por perfil ya existía.
5. **La palanca #1 es de negocio, no de código**: menciones de marca fuera del sitio (YouTube 0.737 de correlación, menciones web 0.664 — Ahrefs 75k marcas) pesan más que todo lo anterior. Va al plan comercial, no al repo.

### Bloque 4 — Este documento

---

## Lo que NO se hace (y por qué — para que nadie lo "descubra" en 6 meses)

| Tentación | Veredicto | Razón (fuente en el research) |
|---|---|---|
| **FAQPage / HowTo schema** | ❌ jamás | Muertos en Google (FAQPage sin rich results desde may-2026; HowTo desde 2023). Esfuerzo cero retorno |
| **Indexing API de Google** para licitaciones | ❌ prohibido | Restringida oficialmente a JobPosting/BroadcastEvent; Mueller: los spammers la abusan así y puede dejar de procesarse sin aviso |
| **llms.txt** | ❌ no invertir | Google no lo soporta ni planea (Illyes jul-2025); los logs muestran que los bots de IA ni siquiera lo piden. Moda, no estándar |
| **Categoría × ciudad programático** | ⏳ solo con inventario | El producto cartesiano 518×ciudades vacío es el patrón exacto de scaled content abuse (caso documentado: 42k páginas, 8 meses de castigo). Se construye cuando haya ≥3-5 listados reales por combinación — misma regla que ya aplica el noindex de categorías vacías |
| **hreflang es-MX** | ❌ innecesario | `es` a secas es válido oficialmente; -MX estrecharía sin haber segunda variante de español que desambiguar |
| **Product/Offer para disponibilidad** | ❌ no encaja | Offer exige precio para las experiencias ricas; la disponibilidad B2B no publica precio. Si algún día hay catálogo con precios, se revisa |
| **Google Business Profile del directorio** | ❌ no elegible | GBP exige contacto en persona; una plataforma web no califica. Los PROVEEDORES listados sí — jugada de producto futura: ayudarles con su GBP genera menciones de marca (palanca GEO #5) |
| **Sitemaps ping / rel=next-prev / URL parameters tool** | ❌ muertos | Retirados por Google (2023 / 2019 / 2022) |

## Pendientes que NO son de código

- **Search Console**: verificar propiedad (no hay meta ni archivo en el repo — si está verificado es por DNS) y dar de alta el sitemap. Con Bing Webmaster Tools igual (IndexNow ya empuja, pero el panel da visibilidad).
- **Contenido**: el semillero comercial es EL plan SEO — cada empresa real enciende sus categorías (noindex se levanta solo) y cada licitación real publica una hoja fresca.
- **Menciones de marca fuera del sitio** (YouTube, foros, directorios mexicanos): palanca GEO principal según los datos; es plan comercial/socios.
