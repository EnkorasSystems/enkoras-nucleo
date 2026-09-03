# Auditoría SEO — directorio-b2b (enkoras.com)

Fecha: 2026-09-02 · Repo: `C:\Users\enriq\Documents\directorio-b2b` · Verificado EN VIVO contra https://enkoras.com (solo GET públicos)

---

## 0. Infraestructura (qué hay y dónde vive)

| Pieza | Archivo | Estado |
|---|---|---|
| Canonical + hreflang | `lib/seo.ts:8-16` (`alternatesSeo`) | ✓ ES raíz, EN `/en`, x-default→ES. Uniforme en todas las páginas indexables |
| Serializador JSON-LD | `lib/seo.ts:28-35` (`jsonLdSeguro`) | ✓ escapa `<>&` y U+2028/29 (XSS-safe) |
| Sitemap | `app/sitemap.ts` | ✓ estáticas + categorías (limit 1000) + empresas activas (limit 5000). **En vivo: 526 URLs** = 7 estáticas + 518 categorías + **1 empresa**; 1052 `xhtml:link` (hreflang es/en recíproco en las 526); **1 solo `lastmod`** (la empresa). Falta `/licitaciones` |
| Robots | `app/robots.ts` | ✓ vivo = código: disallow admin/mi-empresa/auth/api/registro/mensajes/cuenta/guardados/planes/gracias + variantes `/en`; referencia al sitemap |
| JSON-LD Organization + WebSite/SearchAction | `app/[locale]/layout.tsx:80-104` | ✓ en TODAS las páginas; SearchAction apunta a `/buscar?q=` (coincide con el param real). Verificado en vivo: emite válido |
| JSON-LD LocalBusiness | `app/[locale]/(public)/empresa/[slug]/page.tsx:59-84` | ✓ address/MX, logo, sameAs, aggregateRating condicional (solo con reseñas). En vivo: objeto válido. Sin `@id`, sin `telephone` para anónimos (el dato está gated tras login — coherente), sin geo/priceRange |
| JSON-LD BreadcrumbList | `app/[locale]/(public)/categoria/[slug]/page.tsx:141-161` | ✓ árbol completo con prefijo de locale. En vivo: 5 ListItem válidos |
| OG card estática | `app/opengraph-image.png` + declarada en `app/[locale]/layout.tsx:53` | ✓ 200 en vivo (50 KB) |
| OG dinámica por empresa | `app/[locale]/(public)/empresa/[slug]/opengraph-image.tsx` | ✓ revalidate 3600, determinista (tile con inicial, no logo remoto). En vivo la URL emitida lleva prefijo `/es/…/opengraph-image-…` → **200 OK, sin redirect** |
| ISR | home `revalidate=300` (`(public)/page.tsx:14`), `/categorias` 3600 (`categorias/page.tsx:6`), `/empresa/[slug]` 120 + `generateStaticParams []` (`empresa/[slug]/page.tsx:8-16`) | ✓ |
| Purga on-demand | `lib/revalidar.ts` | ✓ quirúrgica (home, categorias, perfil en ambos idiomas) |
| i18n | `i18n/routing.ts` (`localePrefix: "as-needed"`), proxy en `proxy.ts` (next-intl + candado de rutas privadas → 307 a login) | ✓ |
| GA4 | `app/[locale]/layout.tsx:119-121` condicional a env | ✓ |
| Verificación GSC | — | No hay meta ni archivo en el código (si está verificado, es por DNS; no comprobable desde aquí) |

---

## 1. Tabla sección × estado

✓ = bien · ◐ = parcial · ✗ = falta · n/a = no aplica (noindex/redirect)

| Sección | title | desc | canonical | hreflang | OG | JSON-LD propio | Index | Sitemap |
|---|---|---|---|---|---|---|---|---|
| `/` (home) | ✓ | ✓ | ✓ | ✓ | ✓ estática | ✓ Org+WebSite | ✓ | ✓ |
| `/buscar` | ✓ | ✓ | ✓ (limpia sin `?q`/`?sel`, verificado vivo) | ✓ | ✓ heredada | ✗ | ✓ | ✗ (a propósito, `app/sitemap.ts:29`) |
| `/categorias` | ✓ | ✓ ("504 categorías, 14 sectores" — correcto: 94+410; +14 sectores = las 518 del sitemap) | ✓ | ✓ | ✓ heredada | ✗ | ✓ (h1 `sr-only`) | ✓ |
| `/categoria/[slug]` (niveles 1/2/3) | ✓ nombre | ◐ plantilla genérica única (`seoDesc` messages/es.json:118) | ✓ limpia con `?sel/?q/?estado` | ✓ | ✓ heredada | ✓ BreadcrumbList · ✗ ItemList | ✓ | ✓ (518) |
| `/empresa/[slug]` | ◐ largo: "Nombre — giro \| Enkoras" >100 chars en vivo | ◐ `slice(0,160)` corta a media palabra (`empresa/[slug]/page.tsx:29`, confirmado vivo) | ✓ | ✓ | ✓ dinámica propia | ✓ LocalBusiness | ✓ | ✓ (1 en vivo) |
| `/licitaciones` | ✓ | ✓ | ✓ (con `?sel`/`?modo` canonicaliza al tablón — verificado vivo) | ✓ | ✓ heredada | ✗ | ✓ | ✗ **falta** |
| `/planes` | ✓ | ◐ usa el subtitle, sin precios/keywords | ✓ | ✓ | ✓ heredada | ✗ (FAQ/Offer posibles) | ✓ | ✓ |
| `/nosotros` | ✓ | ✓ | ✓ | ✓ | ✓ heredada | ✗ | ✓ | ✓ |
| `/publica` | ✓ | ✓ | ✓ | ✓ | ✓ heredada | ✗ | ✓ | ✓ |
| `/privacidad` | ✓ | ✗ hereda la genérica del sitio (verificado vivo) | ✓ | ✓ | ✓ heredada | n/a | ✓ | ✓ |
| `/terminos` | ✓ | ✗ ídem (`terminos/page.tsx:6-12` no define description) | ✓ | ✓ | ✓ heredada | n/a | ✓ | ✓ |
| `/planes/gracias` | genérico | — | ✗ | ✗ | — | — | ✓ `noindex,nofollow` (`gracias/page.tsx:13-15`) + robots.txt | ✗ ✓ |
| `/auth/*` (login/register/reset/confirm) | ✗ genérico heredado | ✗ genérica | ✗ | ✗ | — | — | **✗ emite `index, follow`** heredado del layout (verificado vivo); solo robots.txt lo tapa | ✗ ✓ |
| `/registro` | n/a | | | | | | 307 → login (proxy) ✓ | ✗ |
| `/solicitudes` (+ `/nueva`, `/editar/[id]`) | genérico | genérica | ✗ | ✗ | — | — | **◐ HTTP 200 + meta-refresh 0s** (no 308) con `index, follow` — ver hueco #4 | ✗ |
| `/equipo/invitacion/[token]` | ✓ (preview chat, `invitacion/[token]/page.tsx:10-13`) | ✓ | ✗ | ✗ | — | — | ◐ sin noindex, pero anon → 307 a login; riesgo bajo | ✗ |
| `/licitaciones/nueva`, `/mensajes` | n/a | | | | | | tras auth (proxy 307) ✓ | ✗ |
| 404 catchall (`[...rest]/page.tsx`) | | | | | | | ✓ HTTP 404 real en vivo | |

---

## 2. Huecos por impacto (con evidencia)

### 🔴 1. El motor SEO real está vacío: 1 sola empresa activa en producción
- Evidencia viva: sitemap con **1** URL `/empresa/*` (`proveedora-de-pinturas-y-acabados`), `/buscar` y la home listan esa misma única empresa. La query del sitemap (`app/sitemap.ts:36-40`, `is_active=true`) es correcta — es la BD la que tiene 1 activa (demos borradas).
- Impacto: las 518 páginas de categoría son cascarones sin resultados (517 sin ninguna empresa) — thin content masivo a ojos de Google; los perfiles, que son la superficie SEO diseñada, casi no existen. No es bug de código: es el hueco de contenido que domina todo lo demás.
- Nota: la memoria decía "~536 URLs"; hoy son 526 — la diferencia son las empresas demo que ya no están.

### 🔴 2. Las licitaciones no existen para Google
- `/licitaciones` **no está en el sitemap** (`app/sitemap.ts:43-61` no lo incluye) pese a ser indexable con metadata propia (contra lo que se creía, SÍ tiene title/desc/canonical/hreflang: `licitaciones/(sala)/page.tsx:95-102`, verificado vivo).
- Cada licitación vive tras `?sel=<uuid>` y la canónica siempre apunta al tablón pelón (verificado: `/licitaciones?modo=cotizacion` → canonical `/licitaciones`). **No hay URL canónica individual por licitación** → cero landing pages, cero long-tail ("licitación de tornillería en Tijuana"), contenido huérfano sin internal linking indexable.
- Además el tablón entero es dinámico por sesión (usa `createClient` + `auth.getUser`, `page.tsx:109-114`): lo que ve el crawler es la vista anónima "todas", sin JSON-LD.

### 🟠 3. `/auth/*` emite `index, follow` y solo lo tapa robots.txt
- Los pages de auth son client components sin metadata (`app/[locale]/auth/login/page.tsx:1`), heredan el robots `index:true` del layout raíz (`app/[locale]/layout.tsx:56-60`). Verificado vivo: `/auth/login` → `robots: index, follow`, sin canonical.
- `Disallow: /auth` impide el crawl pero **no** la indexación URL-only (y de paso impide que Google vea un futuro noindex). Falta un `robots: { index: false }` en un layout de `app/[locale]/auth/`.

### 🟠 4. Los redirects de `/solicitudes` son 200 + meta-refresh, no 308
- Código: `permanentRedirect("/licitaciones?modo=cotizacion")` (`solicitudes/page.tsx:14`, ídem `/nueva` y `/editar/[id]`).
- En vivo: **HTTP 200**, cuerpo con `<meta http-equiv="refresh" content="0;url=/licitaciones?modo=cotizacion">`, `robots index,follow` y title genérico — el prerender estático convierte el `permanentRedirect` en meta-refresh. Google suele tratar refresh-0 como 301, pero los enlaces viejos (correos, chats, notificaciones) no transfieren señal como redirect HTTP y las 3 rutas quedan como páginas 200 indexables de contenido vacío. Un redirect en `proxy.ts` (o `redirects()` en `next.config.ts`) daría el 308 real.

### 🟠 5. El sitemap solo lista las URLs ES
- Las 526 entradas `<loc>` son ES; las versiones `/en` solo aparecen como `xhtml:link alternate`. La guía de Google pide **una entrada `<url>` por cada versión de idioma**, cada una con el set completo de alternates — hoy las ~526 URLs `/en` no están declaradas como entradas (Google las descubre igual por hreflang del head, pero la señal es más débil y no hay lastmod/priority para ellas).

### 🟡 6. Descriptions débiles o heredadas
- `/privacidad` y `/terminos` no definen description (`privacidad/page.tsx:6-12`, `terminos/page.tsx:6-12`) → heredan la genérica del sitio (confirmado vivo). Snippet engañoso en SERP.
- `/empresa/[slug]`: `description.slice(0,160)` corta a media palabra (vivo: "…en el mercado de alta").
- `/categoria/[slug]`: una sola plantilla para 518 páginas ("X: proveedores con catálogo…") — sin conteo, sin ciudades, sin diferenciación.
- `/planes`: la description es el subtitle sin "precios/planes/$649" — desperdicia la keyword transaccional.

### 🟡 7. lastmod/changefreq poco honestos
- Solo 1 `lastmod` en todo el sitemap (empresas con `updated_at`; hoy 1). Las 518 categorías declaran `changefreq: daily` (`app/sitemap.ts:52`) cuando 517 no cambian nunca; las estáticas sin lastmod. Señal de frescura devaluada.

### 🟡 8. `/categoria/[slug]` es 100% dinámica
- Sin `export const revalidate` y consume `searchParams` (`categoria/[slug]/page.tsx:22-25`) → cada hit del crawler a las 518 URLs es SSR completo contra Supabase (varias queries). El resto del catálogo sí tiene ISR. TTFB y presupuesto de crawl pagan la cuenta.

### ⚪ 9. Menores
- Título de empresa: `Nombre — giro | Enkoras` supera 100 chars en vivo (template del layout suma "| Enkoras" a un title ya compuesto).
- Cosmético: canonical de home `https://enkoras.com` (sin slash) vs sitemap `https://enkoras.com/`.
- `/planes/gracias`: noindex ✓ pero también `Disallow` en robots.txt — Google no puede leer el noindex (irrelevante en la práctica para una thank-you).
- `/equipo/invitacion/[token]`: metadata para preview de chat ✓, sin noindex — mitigado porque anon redirige a login.

---

## 3. Lo que está sano (una línea cada uno)

- `alternatesSeo` emite canonical + hreflang es/en/x-default **recíproco y consistente en las 11 superficies indexables** — verificado vivo en `/`, `/en`, categoría, empresa, `/en/empresa`, licitaciones, `/en/licitaciones`, buscar, planes, nosotros, privacidad.
- Las URLs con query (`?sel`, `?modo`, `?q`, `?estado`) canonicalizan a la URL limpia — sin contenido duplicado indexable (verificado vivo).
- robots.txt vivo = `app/robots.ts`, con sitemap referenciado.
- JSON-LD válido y con serialización XSS-safe (`jsonLdSeguro`); Organization+WebSite+SearchAction en todo el sitio, LocalBusiness en perfil, BreadcrumbList en categoría — todos verificados emitiendo bien en vivo.
- OG dinámica por empresa funciona (URL con prefijo `/es/` responde 200 directo, sin redirect que mate a WhatsApp/FB); card estática de marca en el resto.
- `/planes/gracias` noindex ✓; `/registro`, `/mensajes`, `/licitaciones/nueva` tras 307 a login vía proxy ✓; catchall devuelve 404 real ✓.
- "504 categorías" de la metadata es correcto (94 nivel 2 + 410 nivel 3; las 518 del sitemap suman los 14 sectores).
- Naming EN respetado: `/en/licitaciones` titula "Bids" ✓.
- ISR bien pensado donde existe (home 300s, categorias 3600s, empresa 120s con purga quirúrgica en `lib/revalidar.ts`).
- x-default → ES (mercado principal) — decisión correcta.

---

## 4. Inventario de oportunidades (solo inventario — la decisión viene del research del otro agente)

**Páginas indexables que NO existen y podrían:**
1. **Licitación individual pública** `/licitaciones/[id]` — canónica indexable por licitación abierta (título, cantidad, ubicación, ventana de cierre) + entrada en sitemap. Hoy todo vive tras `?sel=`.
2. **Categoría × estado** (`/categoria/[slug]/baja-california` o similar) — el filtro ya existe como `?estado=<uuid>` (ilegible e inindexable); URLs legibles capturarían "proveedores de X en Y", el query B2B por excelencia.
3. **Landing por sector** (nivel 1) con contenido editorial — hoy los 14 sectores son listados secos.
4. **`/licitaciones` en el sitemap** (existe y es indexable; es una línea en `app/sitemap.ts`).
5. `/buscar` con canónica indexable ya existe pero está fuera del sitemap — decisión deliberada, revisable.

**Structured data que falta donde YA hay contenido:**
6. **ItemList** en `/categoria/[slug]` (las empresas listadas) y en `/categorias` (el árbol).
7. **Product/Offer u OfferCatalog** en el perfil de empresa para la disponibilidad vigente (tabla `availability` con title/expires_at ya renderizada).
8. **FAQPage** en `/planes` (los precios públicos son diferenciador declarado).
9. **LocalBusiness enriquecido**: `@id`, `geo`, `priceRange`, y `telephone` para el crawler (hoy gated tras login — decisión de producto, solo inventariado).
10. **BreadcrumbList también en `/empresa/[slug]`** (hoy solo en categoría).
11. Sitemap: entradas `<url>` para las versiones `/en` + lastmod en categorías/estáticas.
