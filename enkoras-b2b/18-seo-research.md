# Research SEO 2026 — Marketplace B2B / Directorio industrial (enkoras.com)

Fecha del research: 2026-09-02. Método: búsquedas web reales + verificación directa en docs oficiales de Google (developers.google.com) vía fetch. Cada afirmación importante lleva fuente. Lo que no pude confirmar está marcado **NO CONFIRMADO**.

---

## 1. Contenido efímero e indexación (licitaciones que abren y cierran en días)

### Lo que dice Google HOY (patrón job postings, el análogo más cercano)

Verificado directamente en la doc oficial de JobPosting ([developers.google.com/search/docs/appearance/structured-data/job-posting](https://developers.google.com/search/docs/appearance/structured-data/job-posting)):

- **El listado individual debe vivir en su propia URL "hoja"**: "Put structured data on the most detailed leaf page possible. Don't add structured data to pages intended to present a list of jobs." Google indexa el detalle, no la lista.
- **Al expirar, Google da exactamente 3 opciones**: (1) `validThrough` poblado y en el pasado, (2) eliminar la página con **404 o 410**, (3) quitar el structured data de la página. `noindex` NO aparece como método oficial en esa doc.
- Dejar listados expirados como si estuvieran activos puede provocar **acción manual** ([Search Engine Land, 2018](https://searchengineland.com/google-may-issue-manual-actions-over-job-schema-on-expired-job-listings-296376)) — aplica al schema JobPosting, pero el principio (no engañar con contenido muerto) es general.
- El consenso operativo de job boards: **410 > 404** para expiración deliberada (deindexado más rápido, días a semanas); dejar la página en 200 con "esta vacante expiró" = riesgo de **soft-404** + crawl desperdiciado + mala UX ([SEJ, guía de expired content](https://www.searchenginejournal.com/the-essential-guide-to-managing-expired-content/318417/); [Botify](https://www.botify.com/blog/expired-content-seo)).

### unavailable_after — VIGENTE

La directiva robots `unavailable_after` (meta tag o header `X-Robots-Tag`) **sigue soportada** por Google (solo web search): tras la fecha indicada la página se trata como noindexada y sale de resultados ~24h después; acepta RFC 822/850/ISO 8601 ([doc oficial robots meta tag](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag)). Es el mecanismo perfecto para contenido con fecha de cierre conocida: la licitación se auto-desindexa sin que tengas que hacer nada al cierre.

### Indexing API — NO usable para licitaciones

La Indexing API de Google está **oficialmente restringida a páginas con JobPosting o BroadcastEvent** ([doc oficial de cuota](https://developers.google.com/search/apis/indexing-api/v3/quota-pricing)). John Mueller (mayo 2025, Bluesky): "We see a lot of spammers misuse the Indexing API like this, so I'd recommend just sticking to the documented & supported use-cases"; Gary Illyes añadió que puede dejar de procesar formatos no soportados sin aviso ([ppc.land](https://ppc.land/google-clarifies-indexing-api-quota-and-usage-in-recent-documentation-update/)). Una licitación B2B no es JobPosting → no usarla. El sustituto para frescura: sitemap con `lastmod` honesto (tema 5) + IndexNow para Bing.

### ¿Qué hacen los grandes?

- **Job boards (Indeed y similares)**: páginas individuales indexables con JobPosting mientras están vivas + Indexing API (permitida en su caso) + 404/410 al expirar; el consenso del sector es retirar lo expirado, no dejarlo en 200 ([Job Boardly, guía 2026](https://www.jobboardly.com/blog/job-board-seo-vs-general-website-seo); [gouconnect sobre 410](https://support.gouconnect.com/articles/expired-job-posts-how-http-410-gone-works-and-why-its-best-for-accessibility-and-seo)). Detalle interno exacto de Indeed: **NO CONFIRMADO** (no publican su playbook).
- **Alibaba RFQ**: su "RFQ market" vive tras login en seller central; el SEO de Alibaba se apoya en producto/categoría/proveedor, no en las RFQs individuales ([seller.alibaba.com/rfq](https://seller.alibaba.com/rfq)). Que ninguna RFQ individual esté indexada: **NO CONFIRMADO al 100%**, pero no hay evidencia de que las usen como activo SEO.
- **OCC Mundial**: **NO CONFIRMADO** (no encontré análisis serio de su manejo de expirados).

### ¿Vale la pena indexar cada licitación?

Con ciclos de vida de días, el valor SEO por licitación individual es bajo (Google puede tardar más en indexar que lo que dura abierta). El activo evergreen es **el índice del tablón** (y sus vistas por categoría). Pero las URLs individuales SÍ importan por tres razones: (a) sin URL propia no hay nada que compartir/enlazar (hoy `?sel=` no acumula señales); (b) el índice necesita enlaces internos a algo rastreable para que Google entienda que hay actividad fresca (señal de sitio vivo); (c) las respuestas de IA citan páginas concretas, no estados de JS.

**Recomendación (1 línea):** sacar cada licitación de `?sel=` a una URL propia (`/licitaciones/{slug}`), indexable mientras está abierta con `unavailable_after` = fecha de cierre, y al cerrar servir 410 (o página de "cerrada" con `noindex` si quieres conservar UX, nunca 200 indexable), con el índice del tablón como página SEO permanente.

---

## 2. Datos estructurados 2026: qué vive y qué murió

### DEPRECADO (no gastar esfuerzo)

| Tipo | Estado | Fuente |
|---|---|---|
| **FAQPage** | **MUERTO del todo**: dejó de aparecer el 7-may-2026; en jun-2026 Google quitó el filtro de apariencia, el reporte y el soporte en Rich Results Test; ago-2026 muere el soporte en la API de Search Console. (Ya venía restringido a gob/salud desde ago-2023.) El markup no penaliza, pero no rinde nada en Google. | [Search Engine Journal, 10-may-2026](https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/) — verificado por fetch |
| **HowTo** | Deprecado desde sep-2023 | [SEJ/histórico](https://www.thehoth.com/blog/google-faq-rich-results-deprecated/) |
| **7 tipos retirados jun-2025**: Book Actions*, Course Info, Claim Review, Estimated Salary, Learning Video, Special Announcement, Vehicle Listing | Retirados; reportes eliminados desde 8-sep-2025. (*Book Actions luego revertido parcialmente.) | [Search Engine Roundtable](https://www.seroundtable.com/google-drops-support-structured-data-types-40386.html); [WebFX](https://www.webfx.com/blog/seo/google-schema-updates/); changelog oficial: [developers.google.com/search/updates](https://developers.google.com/search/updates) |
| rel=next/prev | Muerto desde mar-2019 (tema 3) | [Search Engine Land](https://searchengineland.com/pagination-seo-what-you-need-to-know-453707) |

### VIGENTE y aplicable al directorio

- **Organization / LocalBusiness** — vivos y entre los que siguen rindiendo ([elementera](https://www.elementera.com/blog/google-has-removed-faq-rich-results); doc oficial). Para perfiles públicos de proveedor: `LocalBusiness` con dirección/teléfono/geo en cada perfil es legítimo y es LA señal de entidad local (tema 6).
- **BreadcrumbList** — vivo; clave con árbol de 3 niveles y 518 categorías.
- **Product**: dos niveles ([doc oficial](https://developers.google.com/search/docs/appearance/structured-data/product)): *merchant listings* (requiere página donde se puede COMPRAR, con precio → no aplica a un directorio que no vende) y *product snippets* (más amplio, páginas con info de producto sin compra directa → [doc](https://developers.google.com/search/docs/appearance/structured-data/product-snippet)). Para "disponibilidad B2B" sin precio público, Product/Offer **no encaja bien**: Offer pide price para las experiencias ricas. Veredicto: no forzarlo; si algún día los proveedores publican productos con precio, ahí sí.
- **ItemList** en páginas de categoría/listado — válido y recomendado como higiene semántica, pero los carruseles rich solo existen para Recipe/Course/Movie/Restaurant, y el nuevo "Carousels (Beta)" (sep-2025) es **solo EEA** (viajes/local/shopping) ([doc oficial carousel](https://developers.google.com/search/docs/appearance/structured-data/carousel); [carousels-beta](https://developers.google.com/search/docs/appearance/structured-data/carousels-beta)). En MX/US no esperes rich result de ItemList; su valor es de comprensión de entidades (incl. para LLMs).
- **JobPosting**: NO aplica a licitaciones (es exclusivamente empleo). **Análogo para RFQs: NO EXISTE** — schema.org tiene el tipo `Demand`, pero Google no tiene ningún rich result ni doc para él (**NO CONFIRMADO** que aporte nada; costo bajo si se añade por semántica, beneficio no demostrado).

**Recomendación (1 línea):** consolidar Organization (sitio) + LocalBusiness (perfil de proveedor) + BreadcrumbList (todo el árbol) + ItemList (categorías); borrar FAQPage/HowTo de la hoja de ruta y NO inventar schema para licitaciones.

---

## 3. SEO programático para directorios (categoría × ciudad, paginación, facetas)

### Categoría × ciudad: cuándo sí y cuándo es castigo

- Google intensificó en 2024-2025 la aplicación de **scaled content abuse / doorway pages** (spam policies de mar-2024 + updates de 2025); el patrón castigado es el de páginas por ciudad con el nombre intercambiado y cuerpo idéntico — caso documentado: sitio legal con 42,000 páginas de ciudad clonadas, 8 meses de recuperación ([getpassionfruit](https://www.getpassionfruit.com/blog/programmatic-seo-traffic-cliff-guide); [eastondev](https://eastondev.com/blog/en/posts/media/20260326-programmatic-seo-guide-2025/)).
- Lo que SÍ funciona: "unique data at scale matched to intent" — cada página combinada debe tener **inventario real y datos propios** (proveedores reales de esa categoría en esa ciudad, conteos, disponibilidad) ([seomatic](https://seomatic.ai/blog/programmatic-seo-best-practices); [guptadeepak](https://guptadeepak.com/the-programmatic-seo-paradox-why-your-fear-of-creating-thousands-of-pages-is-both-valid-and-obsolete/)). Google evalúa el sitio de forma holística: un volumen grande de páginas vacías arrastra TODO el dominio hacia abajo.
- Regla práctica del sector: crear/indexar la landing "categoría en ciudad" solo cuando hay un mínimo de listados reales (p. ej. ≥3-5); las combinaciones vacías → 404 real o no generarlas. Con 518 categorías × N ciudades, generar el producto cartesiano completo sería el error clásico.

### Paginación — rel=next/prev DEPRECADO

Google dejó de usar `rel=next/prev` (anunciado mar-2019; ya no lo usaba desde antes) ([Search Engine Land](https://searchengineland.com/pagination-seo-what-you-need-to-know-453707); [doc oficial de paginación](https://developers.google.com/search/docs/specialty/ecommerce/pagination-and-incremental-page-loading)). Práctica vigente: enlaces `<a href>` normales entre páginas, **cada página con canonical a sí misma** (no canonicalizar la página 2 a la 1), no noindexar paginadas, no bloquearlas por robots.txt; enlazar primera/última/intermedias para que el crawler salte profundo ([journeyfurther](https://www.journeyfurther.com/articles/how-does-google-handle-pagination-links-in-2025)).

### Facetas e index bloat — doc oficial NUEVA (dic-2024)

Google publicó doc dedicada: [Managing crawling of faceted navigation URLs](https://developers.google.com/crawling/docs/faceted-navigation) + blog [Crawling December](https://developers.google.com/search/blog/2024/12/crawling-december-faceted-nav):
- Decidir explícitamente qué facetas merecen índice; las que no → impedir crawleo (robots.txt) o consolidar con `rel=canonical` a la vista base.
- Separador estándar `&`; orden de filtros consistente en la URL; **combinaciones sin resultados → 404 real**.
- El riesgo citado: overcrawling de duplicados que retrasa el descubrimiento de páginas nuevas (relevante con ISR y 518 categorías).
- Nota: la vieja herramienta de "URL parameters" de Search Console ya no existe (retirada en 2022); canonical + robots.txt son las herramientas.

**Recomendación (1 línea):** landing categoría×ciudad solo con inventario real mínimo (resto ni se genera), paginación con links normales y canonical propio por página, y parámetros de estado (`?sel=`, `?modo=`) canonicalizados a la URL limpia o excluidos de crawleo.

---

## 4. AI search / GEO

### Posición oficial de Google — verificada por fetch

Doc oficial "AI features" ([developers.google.com/search/docs/appearance/ai-features](https://developers.google.com/search/docs/appearance/ai-features)): **"There are no additional requirements to appear in AI Overviews or AI Mode"** y **"You don't need to create new machine readable files, AI text files, or markup to appear in these features"**. SEO normal + contenido útil + enlaces internos. Los controles existentes (`nosnippet`, `max-snippet`, `noindex`) aplican también a features de IA.

### llms.txt — MODA, no estándar

- Gary Illyes (Search Central Live APAC, 23-jul-2025): Google no lo soporta ni planea hacerlo. John Mueller (abr-2025): comparable al meta keywords; los logs muestran que los servicios de IA **ni siquiera piden el archivo** ([SEJ](https://www.searchenginejournal.com/google-says-llms-txt-is-purely-speculative-for-now/577576/); [análisis Spriestersbach](https://medium.com/@kaispriestersbach/the-llms-txt-is-dead-more-precisely-a-dud-ab7bee4f469c)). La guía de optimización IA de Google (may-2026) lo lista entre lo que se puede ignorar ([getpassionfruit](https://www.getpassionfruit.com/blog/should-i-create-an-llms.txt-file-google-s-2026-guidance-explained)). OpenAI/Anthropic/Meta tampoco lo han adoptado en producción (Q1-2026). Veredicto: no invertir (costo casi cero si un día se quiere, pero cero evidencia de retorno).

### Acceso de crawlers IA — lo que sí es real

- **AI Overviews sale del índice normal de Googlebot**; bloquear `Google-Extended` NO afecta AI Overviews (solo entrenamiento de Gemini) ([Search Engine Land](https://searchengineland.com/google-extended-crawler-432636); [menra](https://www.menra.ai/guides/ai-overviews-crawler-guide)).
- Para ChatGPT search hay que permitir **OAI-SearchBot** (GPTBot es entrenamiento, bot distinto); PerplexityBot y ClaudeBot son directivas independientes — bloquear uno no afecta a los demás ([aicrawlercheck](https://aicrawlercheck.com/blog/google-extended-vs-googlebot); [trakkr](https://trakkr.ai/bots/google-extended)). Revisar que el robots.txt de enkoras.com no los bloquee.

### Qué correlaciona con ser citado (estudios 2025-2026)

- **Ahrefs (75,000 marcas, Brand Radar)**: los predictores más fuertes de visibilidad en IA son señales de marca fuera del sitio — menciones en YouTube (correlación 0.737), menciones de marca en la web (0.664), anchors de marca (0.527); los backlinks solos correlacionan mucho menos (0.218) ([ahrefs.com/blog/ai-brand-visibility-correlations](https://ahrefs.com/blog/ai-brand-visibility-correlations/)).
- **Ahrefs (15,000 queries)**: solo ~12% de las URLs citadas por IA coinciden con el top-10 orgánico de Google — la IA cita fuentes que no rankean página 1 ([vía tryprofound](https://www.tryprofound.com/blog/ai-platform-citation-patterns)).
- **Sesgos por plataforma** ([OtterlyAI, 1M+ citas, 2026](https://otterly.ai/blog/the-ai-citations-report-2026/); [tryprofound, 680M citas ago-2024→jun-2025](https://www.tryprofound.com/blog/ai-platform-citation-patterns)): ChatGPT sobre-cita Wikipedia/contenido enciclopédico; Perplexity sobre-cita Reddit/foros; AI Overviews favorece YouTube/multimodal. Solo ~11% de dominios son citados por ChatGPT Y Perplexity a la vez.
- **Semrush "ghost citations"**: 62% de las citas de IA no producen mención de marca — ser citado ≠ ser nombrado ([semrush.com](https://www.semrush.com/blog/the-ghost-citations-study/)); Semrush AI Visibility Index 2026 analizó 126M prompts ([nota de prensa](https://www.semrush.com/news/463141-semrush-releases-expanded-2026-ai-visibility-index-analyzing-126-million-ai-search-prompts/)).
- Traducción práctica para un directorio: las páginas que ganan citas son las que tienen **datos únicos y afirmaciones verificables** (conteos de proveedores por categoría/ciudad, definiciones claras, comparativas) — exactamente lo que un directorio con 518 categorías puede generar con datos reales; y la presencia de marca fuera del sitio (menciones, YouTube, foros) pesa más que el link building clásico.

**Recomendación (1 línea):** nada de llms.txt; permitir OAI-SearchBot/PerplexityBot/ClaudeBot en robots.txt, convertir las páginas de categoría en "la fuente de datos" citable (cifras reales de proveedores por ciudad) y trabajar menciones de marca fuera del sitio (incl. YouTube) como palanca GEO principal.

---

## 5. Técnico Next.js / ISR

### Core Web Vitals — VIGENTES sin cambios de umbral

LCP ≤ 2.5s, **INP ≤ 200ms** (reemplazó a FID en mar-2024), CLS ≤ 0.1, medidos al percentil 75 de datos reales (CrUX); INP es el vital que más sitios fallan (~43% según [corewebvitals.io](https://www.corewebvitals.io/core-web-vitals) — cifra de fuente secundaria). Con App Router: cuidado con hidratación pesada y handlers de la búsqueda/tablón (INP se mide en TODAS las interacciones, no solo la primera).

### Sitemaps — lastmod honesto, lo demás ignorado

- Google **usa `lastmod` solo si es "consistently and verifiably accurate"**, e **ignora `priority` y `changefreq`** — blog oficial de la muerte del sitemaps ping (jun-2023): [developers.google.com/search/blog/2023/06/sitemaps-lastmod-ping](https://developers.google.com/search/blog/2023/06/sitemaps-lastmod-ping). Mentir en lastmod (p. ej. poner "hoy" en todo por un rebuild de ISR) hace que Google desconfíe del campo entero. El endpoint de ping ya no existe: sitemap referenciado en robots.txt/Search Console y punto.
- Límites 50,000 URLs / 50MB por archivo; en Next.js App Router: `sitemap.ts` + `generateSitemaps()` para particionar (por ejemplo un sitemap de categorías, uno de perfiles, uno del tablón), con revalidate propio ([nextjs.org/docs — generate-sitemaps](https://nextjs.org/docs/app/api-reference/functions/generate-sitemaps); [sitemap file convention](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/sitemap) — soporta `alternates` localizados por URL).

### IndexNow — Google NO, Bing SÍ

Google probó IndexNow en 2022 y decidió no adoptarlo; **a 2026 sigue sin soportarlo**. Sí lo usan Bing, Yandex, Naver, Seznam, Yep; Bing reporta que para dic-2025 el 22% de las URLs clickeadas en sus resultados venían de IndexNow ([indexbolt](https://www.indexbolt.com/glossary/indexnow); [pressonify](https://pressonify.ai/blog/indexnow-instant-indexing-press-releases-2026) — cifra reportada por Bing, fuente secundaria). Para contenido que vive días (licitaciones), un ping IndexNow al abrir/cerrar es barato y da frescura en Bing (y el ecosistema que bebe del índice de Bing; el peso exacto de Bing en ChatGPT search hoy: **NO CONFIRMADO**).

### hreflang — "es" a secas es válido

Doc oficial ([developers.google.com/search/docs/specialty/international/localized-versions](https://developers.google.com/search/docs/specialty/international/localized-versions), verificada por fetch): el código de solo-idioma es explícitamente válido ("you can specify a language code by itself"); **la bidireccionalidad es obligatoria** ("If two pages don't both point to each other, the tags will be ignored"); x-default es recomendado, no obligatorio. Para enkoras: con UN solo español que sirve a todo hispanohablante, usar `es` (no `es-MX`, que estrecharía sin necesidad y hoy no hay segunda variante de español que desambiguar) + `en` para /en + `x-default` apuntando al español raíz (el mercado primario). Si algún día hay es-MX vs es-AR u otro, se re-etiqueta. La geo-relevancia México se gana con contenido/entidades (tema 6), no con el sufijo -MX. Nota de la industria: si hubiera versiones por país, es-MX ganaría para usuarios mexicanos por cercanía geográfica ([better-i18n](https://better-i18n.com/en/blog/hreflang-html-spanish/)) — no es el caso hoy.

### Canónicas con parámetros (?sel=, ?modo=)

Parámetros que solo cambian estado de UI y no el contenido principal → canonical a la URL limpia y/o impedir su crawleo, per la doc de facetas ([developers.google.com/crawling/docs/faceted-navigation](https://developers.google.com/crawling/docs/faceted-navigation)). Pero ojo: canonical es una *sugerencia* de consolidación, no un sustituto de URLs de verdad — para el tablón, la solución de fondo es rutas reales por licitación (tema 1), no canonicalizar `?sel=`.

**Recomendación (1 línea):** particionar el sitemap con `generateSitemaps()` (categorías/perfiles/licitaciones) con `lastmod` que salga de la fecha real de modificación en BD (jamás del build), añadir IndexNow para Bing en el ciclo abrir/cerrar de licitaciones, hreflang `es`/`en`/x-default→es bidireccional, y canonical limpio en toda URL con `?sel=`/`?modo=`.

---

## 6. Lo local México

### Google Business Profile — el directorio NO es elegible

Política oficial ([support.google.com/business/answer/3038177](https://support.google.com/business/answer/3038177) y [13763036](https://support.google.com/business/answer/13763036)): GBP exige **contacto en persona con clientes** (local físico o servicio a domicilio); los negocios 100% online, marcas y plataformas no son elegibles. Enkoras-directorio como producto web no puede tener GBP (Enkoras-empresa solo si tuviera oficina con atención presencial real — y sería para la marca, no ayuda a rankear las páginas de categoría). **Los proveedores listados SÍ son elegibles cada uno** — ahí hay una jugada de producto: los perfiles del directorio complementan (no sustituyen) el GBP de cada proveedor.

### Cómo se gana "proveedores de X en [ciudad]" sin map pack

Para un directorio, la batalla es el **orgánico local**, no el map pack (ese es de los proveedores). Señales que citan las guías mexicanas y de local B2B ([upwego.mx](https://upwego.mx/guia-de-seo-local-b2b/); [chingonesdigitales](https://chingonesdigitales.com/ultimas-noticias/seo-local-para-negocios-en-mexico-la-guia-definitiva-2025/); [abcdigital.mx](https://www.abcdigital.mx/agencia/que-es-seo-local-en-mexico-y-como-debes-realizarlo/); [consultorseomexico](https://consultorseomexico.mx/seo/seo-local-en-negocios-industriales/)):
- Búsqueda B2B con intención geográfica explícita o implícita → landing por ciudad con inventario real (tema 3) + mención natural de ciudad/estado/región en title, H1 y contenido.
- **LocalBusiness schema con NAP (nombre-dirección-teléfono) real en cada perfil de proveedor** — el directorio se vuelve corpus de entidades locales; consistencia NAP con lo que aparece en otros directorios.
- El ecosistema de directorios mexicano sigue pesando como señal (Sección Amarilla, INEGI/DENUE, CANACO, Coparmex, Directorio Industrial, Tuugo) — tanto para citations de los proveedores como para el propio link profile de enkoras.com.
- Mercados urbanos prioritarios citados: CDMX, Monterrey, Guadalajara, Querétaro (+ Tijuana para el corredor fronterizo).
- Español mexicano nativo en el copy (vocabulario local: "tarimas", no "palets"), precios/contexto en MXN cuando aplique — coherente con servir `es` como idioma raíz.

**Recomendación (1 línea):** no perseguir GBP para el directorio; ganar el orgánico local con landings ciudad-con-inventario + LocalBusiness/NAP impecable en cada perfil, y considerar como feature que Enkoras ayude a sus proveedores con su presencia local (GBP + citations), lo que de paso genera menciones de marca (tema 4).

---

## Advertencias de confiabilidad

- Cifras de estudios (Ahrefs 0.737, Bing 22%, 43% fallan INP, 62% ghost citations) vienen de los publicadores citados; son sus mediciones, no verdades de Google.
- "OCC Mundial" y el detalle interno de Indeed: NO CONFIRMADO.
- El tipo schema.org `Demand` para RFQs: existe en el vocabulario, pero sin soporte documentado de Google — NO CONFIRMADO que aporte.
- Peso actual de Bing dentro de ChatGPT search: NO CONFIRMADO a 2026.
