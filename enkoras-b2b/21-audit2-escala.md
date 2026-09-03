# AUDIT 2 — Confiabilidad y escala pre-lanzamiento (directorio-b2b)

**Fecha:** 2/3-sep-2026 · **Auditor:** agente senior de confiabilidad
**Pregunta del CTO:** "que la página se me caiga con 10 users o menos, o por cualquier cosita"
**Métodos:** lectura completa del código · mediciones en vivo contra https://enkoras.com (curl -w, varias muestras) · prod solo-lectura con node+pg · E2E de negocio dentro de `BEGIN…ROLLBACK` con fixtures `qa-*` (residuos verificados: 0)

**Veredicto corto: no encontré NINGÚN punto de caída real con ≤10 usuarios (cero S1).** El catálogo es ISR de verdad, la BD tiene timeouts y candados, la IA falla suave en todos los flujos, el webhook de Stripe es idempotente y el E2E completo de negocio pasó al 100% contra prod. Lo que sí hay: **la hoja SEO nueva no cumple su propio contrato (S2)**, varios estados vacíos que se cachean en silencio tras un error transitorio (S3), y observabilidad prácticamente nula (S3).

---

## Mediciones en vivo (TTFB, 3-4 muestras por URL, 2-sep ~21:00 PT)

| Ruta | TTFB | Caché real (headers) | Notas |
|---|---|---|---|
| `/` | 0.24 – 0.78s | **HIT** (ISR 300) | HTML 394KB |
| `/categorias` | 0.17 – 0.44s | **HIT** (ISR 3600) | HTML 369KB |
| `/nosotros` | 0.12 – 0.40s | **HIT** (estática) | |
| `/planes` | 0.31 – 1.35s | MISS (dinámica: cookies) | anon: 0 queries BD |
| `/licitaciones` (tablón) | 0.32 – 0.69s | MISS (dinámica: cookies) | anon ≈4-5 queries |
| `/buscar` | 0.70 – 1.40s | MISS (dinámica) | sin q: 3-4 queries |
| `/buscar?q=…` | 0.23 – 0.93s | MISS | +3 queries + 1 embedding Gemini (LRU) |
| `/categoria/<slug>` | 0.30 – 0.55s | MISS (searchParams) | base amortiguada con `unstable_cache` 300s |
| `/empresa/<slug>` | 0.14s (HIT) / 1.25-1.35s (regen) | **HIT/STALE** (ISR 120) | ISR funciona |
| `/sitemap.xml` | — | **HIT** (cacheado, se regenera) | las lecturas de Google NO pegan a la BD |

Ningún TTFB patológico. Los totales largos de algunas muestras (6-12s) son peso de HTML + red local, no servidor.

---

## S1 — Caídas reales con pocos usuarios

**Ninguna encontrada.** Lo que lo sostiene (todo verificado, ver "Revisado limpio" abajo): catálogo ISR con headers reales de HIT; páginas dinámicas acotadas a pocas queries con `statement_timeout` 3s (anon) / 8s (authenticated) en la BD; la app no abre conexiones Postgres propias (todo va por PostgREST/HTTPS; `max_connections=60`, 17 en uso, ninguna de Vercel); IA con presupuesto de tiempo y circuit breaker; sin dependencia dura de Gemini/Realtime/pg_cron en ningún render.

---

## S2 — La hoja pública de licitación no cumple su contrato (SEO Bloque 1 cojo)

**Archivo:** `app/[locale]/(public)/licitaciones/[id]/page.tsx`

Tres incumplimientos medidos en vivo, del mismo archivo que declara lo contrario:

1. **El "ISR de 5 min" está muerto: la hoja es 100% dinámica.** `export const revalidate = 300` (línea 22) no aplica porque la ruta no tiene `generateStaticParams` ni llama `setRequestLocale` (usa `getLocale()`/`getTranslations()` directo, líneas 65-68 y 106). El propio repo documenta esta trampa en `empresa/[slug]/page.tsx:10-16` ("sin generateStaticParams… el revalidate de arriba es letra muerta") y ahí sí la corrigieron. **Medido:** 3 requests seguidos → `X-Vercel-Cache: MISS` + `Cache-Control: private, no-cache, no-store` los 3. Cada visita de Google/Bing a cada hoja = render + BD.
2. **Doble query por visita:** `cargarTender()` (línea 46) NO está envuelto en `React.cache()`, y lo llaman `generateMetadata` Y la página → 2 queries idénticas a `tenders` por request (compárese con `getCompanyBySlug`, que sí está `cache()`-wrapped en `lib/supabase/empresa.ts:66`).
3. **Las "tumbas dan 404" dan 200 (soft-404).** Medido: `GET /licitaciones/00000000-0000-4000-8000-000000000000` → **HTTP 200** con el body del not-found. Causa: `app/[locale]/(public)/licitaciones/[id]/loading.tsx` existe → el shell 200 se streamea antes de que `notFound()` (línea 104) pueda fijar el status. Mitiga: la metadata sí marca `noindex` para tumbas (líneas 70-71), así que el riesgo de indexado es bajo — pero el ciclo de vida diseñado (adjudicada/cancelada = 404 real) no ocurre.
4. **Bonus (descubrimiento):** ningún componente enlaza `/licitaciones/<id>` — grep en `app/ components/ lib/` solo lo encuentra en sitemap + IndexNow. Google solo puede descubrir las hojas por el sitemap; Bing además por IndexNow. La hoja está huérfana de links internos.

**Impacto:** no tumba nada (con 3s de statement_timeout anon, un crawler agresivo pega queries de a 2 por hoja y ya), pero la superficie SEO recién construida trabaja al 20% de su diseño y le pasa costo de BD a cada rastreo. Fix chico: `generateStaticParams(){return []}` + `setRequestLocale` + envolver `cargarTender` en `cache()`; el soft-404 se resuelve quitando el `loading.tsx` de esa ruta (la hoja es liviana) o aceptando el 200-noindex.

**Nota hermana (S3):** el mismo patrón soft-404 por `loading.tsx` aplica a `/empresa/<slug>` y `/categoria/<slug>` inexistentes — medido: `GET /empresa/esta-no-existe-qa` → **200** con body 404 (el catch-all `/pagina-que-no-existe` sí da 404 real, medido).

---

## S3 — Errores transitorios que se cachean como "vacío" y nadie lo ve

El patrón dominante de lecturas es `const { data } = await anon…` — un error de PostgREST (timeout de 3s, blip de Supabase, deploy de PostgREST) produce `data=null` y **se renderiza como estado vacío legítimo**, que además se CACHEA:

| Superficie | Qué pasa con un error transitorio | Cuánto dura | Evidencia |
|---|---|---|---|
| Home (ISR 300) | "Actividad reciente" y sectores se pintan vacíos y ESA página se vuelve la cacheada | 5 min | `lib/supabase/queries.ts:125-173` (sin manejo de error) + `page.tsx:14` |
| `/categorias` (ISR 3600) | árbol vacío cacheado | **1 hora** | `queries.ts:45-72` |
| `generateMetadata` de categoría | `totalDeRama` devuelve **0** ante error del RPC y `unstable_cache` cachea ese 0 → la categoría se marca `robots: noindex` | **1 hora** | `app/[locale]/(public)/categoria/[slug]/page.tsx:45,50-59` |
| Listado base de categoría | `itemsDeRama` cachea lista vacía ("categoría sin empresas") | 5 min | mismo archivo, líneas 98-129 |
| Sitemap | fail-open documentado: sin conteo lista el árbol completo; sin categorías/empresas simplemente las omite | hasta la próxima regeneración | `app/sitemap.ts:65-69` |

Con 1 empresa y tráfico bajo esto es cosmético; el problema es que **es invisible**: nada lo loguea (ver siguiente). Un guard barato ("si la query dio error, throw para que ISR conserve la versión anterior") mataría la clase entera de bug.

## S3 — Observabilidad: si se cae a las 3am, se enteran por un cliente

Estado real, sin adornos:

- **No hay Sentry, ni log drain, ni alerta alguna.** `package.json` no trae ningún SDK de monitoreo; el grep de `sentry|axiom|datadog|posthog|logflare` da cero en `app/ lib/ components/`.
- Los únicos `console.error/warn` de todo el runtime son 3 en el webhook de Stripe (eventos envenenados/cortesía que pagó) y 2 en `lib/ia/gemini.ts` (cayó a modelo de respaldo / modelo 404). Viven en los logs de Vercel, que nadie mira de madrugada y expiran.
- **Los errores de server actions mueren en el `{ ok:false, error }` que ve el usuario** — no queda rastro del lado del equipo. Los `catch {}` de IA/IndexNow/ruteo son silencios deliberados (correctos como UX) sin contraparte de registro.
- Señales accidentales que sí existen: Stripe avisa por correo si el webhook falla sostenido (los 500 están bien usados como cola de reintento); GA4 muestra caída de tráfico; `cron.job_run_details` guarda el historial de pg_cron (hoy: 576/576 corridas OK en 48h, 0 fallos en 7 días — verificado en prod).
- Los 2 crons de Vercel devuelven JSON con `errores[]`… que nadie lee.

Mínimo digno pre-lanzamiento: un log drain o Sentry gratis + una alerta de uptime externa (la página ya es pública). No lo instalo aquí; lo reporto.

## S3 — Cuota de Gemini quemable por anónimos vía /buscar

- Cada `q` nueva en `/buscar` (y en búsqueda scoped de categoría) paga **1 embedding** (`lib/busqueda/hibrida.ts:190-207`). El LRU de 300 términos es **por instancia** de serverless (se evapora en cada cold start) y no hay rate limit de red — el propio JSON-LD `SearchAction` del layout (`app/[locale]/layout.tsx:99-107`) anuncia `?q={search_term_string}` a los crawlers.
- **Degradación verificada en código: suave.** `buscarSemantica` hace catch → `[]` → la búsqueda sigue por texto (`hibrida.ts:209-239`); el breaker por familia evita colgar funciones (`gemini.ts:71-81,123-125`). Nada revienta.
- El costo real de agotarla: ese día la búsqueda pierde la capa semántica y el backfill de embeddings falla (reintenta mañana). Con las 5 llaves × cuota por modelo hay colchón, pero es la "cosita" más fácil de gastar desde afuera. Si molesta: cachear el embedding de consulta en BD (el vector de un texto no cambia) o rate-limit por IP.

## S3 — Ruteo de invitaciones sin red de reintento (caso parcial)

`publicarLicitacion` (`app/actions/licitaciones.ts:109-137`): embedding, clasificación y `route_tender_notifications` son 3 try/catch **independientes**. Si el embedding SÍ entró pero clasificación o ruteo fallaron (blip de BD/Gemini-chat), el backfill diario **no lo repara**: filtra por `embedding is null` (`app/api/cron/backfill/route.ts:80-84`). Resultado: licitación viva y visible pero sin invitaciones "Para ti" ni categorías, en silencio. Mismo patrón en `requests`. Es exactamente el hueco que el barrido dice cubrir ("re-rutear solo crea los avisos que faltaron") pero solo lo cubre para la mitad de los fallos.

## S3 — Duración de actions vs. presupuestos de Gemini

Ninguna page/action define `maxDuration` (solo los 2 crons, 60s). Publicar empresa/licitación puede gastar hasta 60s de embedding (`LIMITE_DOCUMENTO.totalMs`, `gemini.ts:60`) + 2 llamadas de clasificación — con Gemini degradado eso rebasa el límite default del plan de Vercel y la plataforma mata la función **después** del insert (el tender/empresa ya existe: `licitaciones.ts:88-107` inserta antes de clasificar). El usuario ve error y puede duplicar. Raro con Gemini sano (falla rápido por diseño), pero es la combinación exacta "Gemini saturado + usuario publicando". Los crons: el backfill hace hasta ~40 embeddings secuenciales con presupuesto de 60s cada uno dentro de un `maxDuration=60` — si Gemini está lento, el cron muere a medias (el progreso por fila sí queda; retoma al día siguiente).

---

## S4 — Deudas de escala (hoy inofensivas, apuntadas con evidencia)

1. **El tablón logueado cuesta ~17-20 queries por click.** `app/[locale]/(public)/licitaciones/(sala)/page.tsx` (1,418 líneas) resuelve por request: empresas mías, invitaciones, mis bids, modos, plantillas, lista, 2 contadores, detalle, categorías, adjuntos, tablero/pulso, cotizaciones, empresa activa, llaves, candado, equipo×2, autores, claims×2. Cada navegación (`?sel=`, pestañas) es un SSR completo. Anónimo: solo 4-5. Con 10 usuarios sobra; a cientos, este archivo es el primer cuello.
2. **supabase-js sin timeout de red.** Los fetch a PostgREST no llevan AbortSignal; el tope real es el `statement_timeout` del lado BD (3s/8s, verificado en prod) — un cuelgue de RED (no de BD) retendría la función hasta el timeout de Vercel. Baja probabilidad, cero manejo.
3. **Regeneración del sitemap = 4 queries, una de ellas `company_categories` con `limit 20000` y join** (`app/sitemap.ts:50-54`). Hoy son 12 filas; a 5k empresas será la query más gorda del sistema corriendo en cada regeneración. Acotada y cacheada — solo vigilarla.
4. **Realtime publica `tenders` con TODAS las columnas** (incl. `budget_max` y `embedding` — verificado en `pg_publication_tables`). Hoy NO fuga: verifiqué en prod que ni `anon` ni `authenticated` tienen grant de SELECT sobre esas columnas y walrus filtra por grant. Es una protección de una sola capa: un `grant select` futuro las pondría en el aire de cada suscriptor del tablón.
5. **Peso de HTML:** home 394KB / categorías 369KB (medido). Con ISR no toca servidor, pero en móvil mexicano promedio son 3-8s de descarga. Los WebGL/animaciones (ogl, motion) son client-side y no afectan disponibilidad del servidor.
6. **`avisar-disponibilidades` (pg_cron cada 30 min) y `barrer-licitaciones` (cada 5)**: si pg_cron muere, no hay caída — la BD re-valida la ventana al ofertar (`095:46: now() >= ends_at` rechaza tardías) y las vencidas solo se ven "abiertas" en Mías hasta el próximo barrido. Los avisos de cierre/trial se saltan y no se recuperan (dedupe por marca). Estado hoy: 6 jobs activos, 0 fallos en 7 días.

---

## E2E de negocio contra PROD (BEGIN…ROLLBACK, fixtures qa-*, script `a2-e2e.js`)

Todo dentro de una transacción; rollback incondicional; residuos verificados = 0. Simulación de sesión con `set local role authenticated` + `request.jwt.claims`.

| Paso | Resultado |
|---|---|
| Alta 2 cuentas qa en `auth.users` → trigger crea `profiles` | ✅ 2/2 |
| `registrar_empresa` (la RPC real del wizard, atómica) | ✅ empresa creada |
| **Trial de nacimiento**: fila en `account_plans` | ✅ **60 días exactos**, plan efectivo `empresa_completa` |
| Falla suave de clasificación IA (empresa viva con 0 categorías) | ✅ tolerado (elección manual después) |
| Trial vencido → plan efectivo | ✅ `presencia` |
| **Convocar SIN plan con candados ON** | ✅ **RECHAZADO**: `plan-requerido:empresa_completa` (trigger 089) |
| Cortesía sintética `empresa_completa` → convocar | ✅ tender `open`, folio L-#### asignado |
| Alta proveedora qa + verificación | ✅ |
| **Ofertar SIN plan proveedor** | ✅ **RECHAZADO**: `plan-requerido:proveedor` |
| Cortesía `proveedor` → ofertar | ✅ bid activa, `unit_price` calculado por la BD |
| Self-bid de la convocante | ✅ rechazado ("Solo las empresas verificadas pueden ofertar" — la muralla antes de la muralla) |
| Ofertar MÁS cantidad de la pedida | ✅ rechazado ("No puede ofertarse más cantidad de la solicitada") |
| `adjudicar_licitacion` (RPC blindada, advisory lock `bid:*`) | ✅ tender `awarded`, bid `accepted`, 2 notificaciones |
| Constancia: lo que lee `/api/constancia` visible para el ganador vía RLS | ✅ folio + accepted visibles (el PDF es determinista de esos datos) |

**Candados de plan: encendidos y funcionando en prod** (`app_config`: `candados_plan=true`, `trial_dias=60`, avisos `[5,3]`).

---

## Cascadas de fallo — resumen por dependencia

| Se cae… | Degrada (suave) | Revienta |
|---|---|---|
| **Gemini (cuota/503)** | búsqueda→texto; alta de empresa/licitación sin vector ni categorías (backfill repara vectores); breaker evita cuelgues | nada. Único hueco: ruteo no reintentado si el vector sí entró (S3 arriba) |
| **Stripe webhook tarde/repetido** | — (updates idempotentes; 500=cola de reintento; asíncronos OXXO/SPEI esperan `paid`; anti-doble-cobro cancela la desplazada; cortesía jamás pisada; envenenados 200+log) | nada |
| **Supabase blip en action** | el usuario ve error y reintenta; `registrar_empresa`/`adjudicar` son atómicas; storage se limpia en fallo (`adjuntarCotizacion`, `adjuntarEspecificacion`) | nada a medias en dinero/estado. Sí: vacíos cacheados en lecturas (S3) |
| **pg_cron caído** | cierres/avisos tardíos; la BD re-valida ventanas al escribir | nada |
| **Realtime caído** | tablón/sala dejan de auto-refrescarse (el pulso del proveedor sigue por sondeo de 6s); badges viejos | nada |

---

## Revisado LIMPIO (verificado, sin hallazgo)

- **ISR real donde importa:** `/` (300), `/categorias` (3600), `/nosotros` (estática), `/empresa/[slug]` (120, con `generateStaticParams` vacío + `setRequestLocale` correctos) — confirmado con `X-Vercel-Cache: HIT/STALE` en vivo. `revalidarCatalogo` purga quirúrgico (`lib/revalidar.ts`), ya no el árbol entero.
- **El (public) layout NO lee cookies** (`app/[locale]/(public)/layout.tsx:6-11`) — la trampa que mata ISR está documentada y evitada; navbar hidrata sesión en cliente.
- **Conexiones BD:** la app entera va por PostgREST/Auth HTTPS; cero clientes pg en runtime de Vercel; `anon` singleton de módulo; `max_connections=60` con 17 usadas (internos de Supabase). El sitemap/ISR/actions no abren nada propio.
- **Timeouts de BD en prod:** `anon statement_timeout=3s`, `authenticated=8s`, `authenticator lock_timeout=8s` — un query pesado de un anónimo no puede retener nada más de 3s.
- **Advisory locks `bid:*`:** `pg_advisory_xact_lock` (se liberan solos al terminar la transacción), serializan por tender, con `lock_timeout=8s` de tope; adjudicar toma el mismo lock (084/086/099) — la carrera ofertar-vs-adjudicar está cerrada y el E2E la atravesó.
- **Embeddings 1536 fuera de los payloads:** todos los selects vivos son de columnas explícitas; los `select("*")` restantes son categorías/ciudades/fotos/availability (sin vector). El panel documenta el porqué (`mi-empresa/page.tsx:26-28`). PostgREST además NIEGA los `*` sobre tablas con columnas restringidas (grants por columna).
- **Columnas sensibles en prod:** `tenders.budget_max`/`embedding` y `companies.rfc/email/phone/whatsapp/stripe_*` sin grant de SELECT para anon (verificado en `information_schema.column_privileges`); Realtime filtra por esos mismos grants.
- **Gemini (lib/ia/gemini.ts):** modelos pinneados (nada de `-latest`), cascada 2 ejes modelo×llave, cuota diaria detectada por `quotaId` y no por `retryDelay`, castigos con TTL, breaker POR FAMILIA de 60s, presupuesto de consulta 8s (la búsqueda no espera), documento 60s. Es de lo mejor cuidado del repo.
- **IndexNow:** `after()` + `AbortSignal.timeout(4000)` + catch — jamás bloquea ni tumba la acción que publicó; en dev guarda silencio (`lib/indexnow.ts`).
- **Sitemap:** cacheado en el edge (HIT verificado; se regenera — Last-Modified avanza) y su contenido HOY coincide 1:1 con prod (1 empresa activa, 0 tenders abiertos, solo categorías con inventario). El fail-open está razonado y documentado.
- **Webhook Stripe:** firma SIEMPRE, sin firma 400; toda la matriz de eventos tardíos/duplicados/envenenados tratada con carriles explícitos (`app/api/stripe/webhook/route.ts` — las notas S1-S4 de la auditoría anterior están implementadas).
- **Checkout:** solo dueño + verificada + candados prendidos; upgrade = update de la MISMA suscripción con prorrateo (no segunda suscripción); portal solo con customer.
- **Crons Vercel:** autenticados con `CRON_SECRET`; `reclamar_avisos_trial` cobra la lista bajo advisory lock (dos crones simultáneos no duplican); "solo 2 correos" respetado incluso ante fallo.
- **pg_cron en prod:** 6 jobs activos; `barrer-licitaciones` 576 corridas OK/48h; cero fallos en `job_run_details` en 7 días.
- **proxy.ts:** i18n + refresh de sesión + candado de rutas privadas; sin queries; matcher excluye estáticos y og-images (con el porqué documentado).
- **Anti-DoS puntual:** UUID validado antes de tocar BD en la hoja (`[id]/page.tsx:24,47`) y en filtros de búsqueda (`uuidOVacio`); término de búsqueda sanitizado para el `.or()` de PostgREST (`hibrida.ts:33-35`); techo de "ver más" en 120; catch-all `[...rest]` da 404 real (medido).
- **RLS habilitada** en las 9 tablas calientes (verificado `relrowsecurity` en prod) y las actions verifican filas afectadas tras cada write (patrón "RLS silenciosa" aplicado consistentemente).
- **`/planes` para anónimo no toca BD** (getUser sin cookie no viaja a la red) — su TTFB alto ocasional es cold start, no queries.
- **E2E completo** (tabla arriba): 15/15 pasos con el resultado esperado, rollback limpio, prod intacta (empresa "Proveedora de Pinturas y Acabados" y cuentas protegidas: ni leídas ni tocadas por los fixtures).

### Scripts de esta auditoría (en el scratchpad)
`a2-lectura.js` (estado de prod solo-lectura) · `a2-grants.js` (grants/timeouts/RLS) · `a2-e2e.js` (E2E con rollback) · `timing.sh`/`timing2.sh` + `timings*.txt` (mediciones crudas)
