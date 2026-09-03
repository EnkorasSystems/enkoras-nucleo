# AUDIT2 — Servidor / Seguridad pre-lanzamiento (directorio-b2b)

**Fecha:** 2026-09-02 · **Auditor:** agente (solo lectura) · **Alcance:** server actions, hoja pública de licitación, cachés, Stripe vivo, auth, headers/proxy/secretos.
**Método:** lectura completa de los 12 archivos de `app/actions/*.ts` + rutas API + páginas señaladas; BD viva con `node+pg` (solo SELECT: grants, policies, `pg_get_functiondef`, `app_config`); GETs reales a https://enkoras.com (cero POSTs, cero logins).
**Estado de prod al auditar:** `candados_plan=true` (monetización ENCENDIDA), `trial_dias=60`, 0 tenders, 1 company, 2 filas en account_plans (verificado en BD viva).

---

## HALLAZGOS

### S2-1 · Cupón 100% en checkout: el pago llega como `no_payment_required` y el webhook NUNCA activa el plan
- **Evidencia:** `app/api/stripe/webhook/route.ts:55` — `if (session.payment_status && session.payment_status !== "paid") break`. El guard S3-3 se escribió para OXXO/SPEI (`unpaid` → llega después `async_payment_succeeded`). Pero con `allow_promotion_codes: true` (`app/actions/stripe.ts:113`) un cupón del 100% produce un checkout con total $0, y Stripe lo entrega con `payment_status = "no_payment_required"` — no es `paid`, así que el `break` lo descarta, y para ese caso **jamás llega** un `async_payment_succeeded`.
- El respaldo tampoco salva: el webhook no maneja `customer.subscription.created` (`route.ts:153` solo `updated`/`deleted`). Una suscripción $0 que nace directa en `active` puede no emitir ningún `updated` hasta el siguiente ciclo → el cliente queda suscrito en Stripe y **sin plan en la plataforma** (falla cerrada: no regala plan, pero rompe el flujo de cupones que hoy está habilitado).
- **Fix sugerido:** aceptar también `no_payment_required` en la condición de `route.ts:55`. Es seguro: el propio handler ya consulta la suscripción real (`route.ts:63-69`, `planCuentaSegunSuscripcion`) y una sub muerta aterriza `presencia`, nunca regalo. Opcional: manejar `customer.subscription.created` con la misma lógica de `updated`.

### S2-2 · `eliminarCuenta` no cancela la suscripción v2: la tarjeta sigue cobrando a una cuenta borrada
- **Evidencia:** `app/actions/cuenta.ts:31-51` — solo recorre `companies.stripe_subscription_id` (el flujo **v1 por-empresa, retirado con cero suscripciones vivas**). La suscripción viva del catálogo v2 vive en `account_plans.stripe_subscription_id` (`supabase/migrations/087_plan_en_la_cuenta.sql:35`) y **ningún** camino de borrado la toca (grep de `account_plans` en `app/` y `lib/`: solo webhook, checkout, portal y páginas de lectura).
- Al borrar el usuario (`cuenta.ts:54`, `deleteUser`), la fila de `account_plans` cae en cascada (`087:29` — `references auth.users(id) on delete cascade`), así que después ni siquiera el webhook puede degradar o adoptar nada: `customer.subscription.updated` no matchea fila (`route.ts:166-171`) y el update por cuenta pega en 0 filas. **Stripe sigue cobrando cada mes a un cliente que ya no existe**, sin plan que degradar y sin rastro en la BD.
- **Fix sugerido:** en `eliminarCuenta`, antes del paso 2, leer `account_plans.stripe_subscription_id` (service role) y cancelarla con la misma falla dura que hoy tiene el paso Stripe v1.

### S3-1 · Open redirect post-login con `?next=/\evil.com` (login, registro y confirm)
- **Evidencia:** el candado del cliente es `rawNext.startsWith("/") && !rawNext.startsWith("//")` en `app/[locale]/auth/login/page.tsx:19` (se usa en `router.push(next)` :76), `register/page.tsx:20` (`router.push(next)` :78) y `confirm/page.tsx:34` (`router.replace(next)` :50). Ese check NO cubre `/\` — el parser WHATWG trata `\` como `/`:
  ```
  node> new URL("/\\evil.com", "https://enkoras.com").href  →  "https://evil.com/"
  node> "/\\evil.com".startsWith("/") && !"/\\evil.com".startsWith("//")  →  true
  ```
  (probado; también pasa `/\/evil.com`). El router de Next resuelve el href contra `location` y una URL de otro origen dispara navegación dura → víctima que entra por `https://enkoras.com/auth/login?next=/\evil.com` con correo/contraseña aterriza **en el dominio del atacante ya "logueada"** (phishing de segunda pantalla). El camino Google es inmune: `app/auth/callback/route.ts:12-18` valida con `new URL(rawNext, origin)` y compara `u.origin === origin` (correcto).
- Matiz: con locale EN el prefijo de next-intl probablemente lo neutraliza; con ES (default, sin prefijo) el push viaja tal cual.
- **Fix sugerido:** replicar en las 3 páginas la validación del callback (resolver con `new URL(raw, location.origin)` y comparar origin), o rechazar cualquier `next` que contenga `\`.

### S3-2 · `guardarFotosEmpresa` (wizard) no valida las URLs → puede tumbar las páginas públicas compartidas
- **Evidencia:** `app/actions/empresa.ts:326-344` acepta `logoUrl`/`fotoUrls` arbitrarios y los escribe directo (RLS limita a empresa propia, pero NO valida el contenido). Sus hermanas del panel SÍ validan que la URL sea del bucket propio: `mi-empresa.ts:333-335` (logo) y `mi-empresa.ts:385-386` (fotos) — con el comentario explícito "sin esto entraba un hotlink/tracker externo".
- Impacto real: los logos se pintan con `next/image` (`components/search/ResultadoCard.tsx:106-113`) y `next.config.ts:13-25` solo permite `*.supabase.co` y `lh3.googleusercontent.com`. Un `logo_url` con host externo hace que `<Image>` **lance error en el render del server** → la card rota revienta la página completa que la lista (home "actividad", /buscar, /categoria). Es decir: cualquier dueño de empresa, llamando esta action a mano con una URL externa, degrada páginas públicas para todos hasta que un admin lo banea. (Si en algún punto se pintara con `<img>` nativo, además sería tracker de IPs de visitantes.)
- **Fix sugerido:** copiar el candado de base-de-bucket de `mi-empresa.ts` a `guardarFotosEmpresa` (mismas 3 líneas).

### S3-3 · Las "tumbas" de la hoja pública responden HTTP 200 (soft-404), y CERRADA/DESIERTA son 404 aunque el diseño dice noindex-vivo
- **Evidencia en vivo (curl):**
  ```
  GET /licitaciones/00000000-0000-4000-8000-000000000000 → HTTP 200
    X-Matched-Path: /[locale]/licitaciones/[id]
    body: <meta name="robots" content="noindex"> (marcador de notFound streameado)
  GET /licitaciones/notauuid → 200 · GET /empresa/no-existe-xyz → 200 · GET /categoria/no-existe-xyz → 200
  ```
  `notFound()` tras el flush del shell (metadata en streaming) deja el status en 200 en TODAS las páginas públicas — la "tumba también para Google" (`app/[locale]/(public)/licitaciones/[id]/page.tsx:17,104`) es en realidad un soft-404 con noindex. Google la desindexa igual (por el noindex), pero no es el 404 diseñado, y cada URL basura cuesta un render completo (el `UUID_RE` de `page.tsx:24,47` sí evita el viaje a BD para no-UUIDs — verificado).
- **La divergencia mayor:** la hoja consulta con el cliente `anon` (`page.tsx:48`) y la policy pública de tenders es `status='open' AND ends_at>now() AND company activa` (verificado en BD viva: `tenders_select_public`). Una licitación **closed/deserted es invisible para `anon`** → `cargarTender` da null → tumba. El estado del código para "CERRADA sigue viva con noindex,follow" (`page.tsx:87-89` rama else, y el label `estadoCerrada` :148) es **código muerto**: nadie puede llegar ahí. Quien guardó el link de una licitación cerrada (que "puede revivir" con `extenderLicitacion`) ve un not-found en vez de la hoja diseñada.
- Consistencia con sitemap: OK (solo lista `status='open'`, `app/sitemap.ts:44-48`); ISR viejo: tope 300 s + `revalidarSeccion("/licitaciones")` en adjudicar/cancelar/extender — OK.
- **Fix sugerido:** decidir si CERRADA debe verse; si sí, leerla con una vía que vea closed/deserted (p. ej. RPC pública acotada a esos estados sin budget) — o actualizar el comentario/metadata y aceptar la tumba total.

### S3-4 · Baneo de empresa: sigue visible hasta 5 min en /categoria y hasta 1 h en el snippet/noindex (unstable_cache no se purga)
- **Evidencia:** `admin.ts:44-54` (`toggleCompanyActive`) llama `revalidarCatalogo()` = `revalidatePath(...)` (`lib/revalidar.ts`). Pero `revalidatePath` **no invalida entradas de `unstable_cache`** (solo TTL o `revalidateTag`, y aquí no se pasan tags): `itemsDeRama` (`app/[locale]/(public)/categoria/[slug]/page.tsx:98-129`, revalidate 300) sigue sirviendo la lista con la baneada hasta 5 min, y `totalDeRama` (`page.tsx:50-59`, revalidate 3600) mantiene la cifra del snippet y el veredicto index/noindex hasta 1 h. El sitemap también la lista hasta su regeneración (~5 min: en vivo `X-Vercel-Cache: HIT, Age: 290`). Choca con la intención escrita: "Un baneo debe sacar a la empresa del catálogo AL INSTANTE" (`admin.ts:52`).
- **Fix sugerido:** pasar `tags: ["categoria"]` a ambos `unstable_cache` y añadir `revalidateTag("categoria")` a `revalidarCatalogo()`.

### S4-1 · `.or()` con texto del admin sin sanear (inyección de filtro PostgREST, admin-gated)
- `app/actions/admin.ts:161` (con **service role**), `:203`, `:234` — `q` se interpola crudo en `.or(\`name.ilike.%${q}%...\`)`; con comas/paréntesis un admin puede alterar el árbol de filtros. La búsqueda pública SÍ sanea (`lib/busqueda/hibrida.ts:33-35` quita `%,()`). Solo invocable tras `exigirAdmin` → higiene, no exposición; conviene reutilizar `terminoSeguro` también aquí.

### S4-2 · Ping IndexNow auto-spameable con licitación propia (sin abuso con ids arbitrarios)
- `avisarIndexNow` NO es server action (sin `"use server"` en `lib/indexnow.ts`) y todas sus llamadas van tras mutación autorizada (`licitaciones.ts:141,387,431,450`; `empresa.ts:209`) — no se puede pingear un id ajeno/arbitrario. Pero `extenderLicitacion` open→open no tiene rate limit de UPDATE (el `ritmo_por_persona` de la 071 es solo `before insert`), así que un dueño puede extender en bucle y disparar 2 pings por clic. Impacto: reputación de la llave ante Bing; acotado a URLs propias del dominio.

### S4-3 · `enviarInvitacionPorCorreo` sin enfriamiento: correo saliente ilimitado a direcciones arbitrarias
- `app/actions/equipo.ts:165-219` — cualquier dueño con liga vigente puede mandar el correo (Resend, remitente de la casa) a cualquier dirección, sin límite de frecuencia. Vector de spam/reputación del dominio de correo. Sugerencia: enfriamiento por liga (como `ENFRIAMIENTO_RECLASIFICAR_MS` de `empresa.ts:247-248`).

### S4-4 · `?n=` infla la caché de categoría (97 variantes por rama)
- `categoria/[slug]/page.tsx:147` — `limite` acepta CUALQUIER entero 24–120 (no solo los pasos de 24), y es parte de la clave de `itemsDeRama` → hasta ~97 entradas × 518 categorías de caché de datos inflables por un crawler hostil. Acotado (no crece sin tope) pero pagas storage/writes de data cache. Sugerencia: redondear `n` al múltiplo de `PASO`.

### S4-5 · Doble checkout en dos pestañas: la sub desplazada se cancela SIN reembolso del cobro ya hecho
- `webhook/route.ts:81-121` — la red anti-doble-cobro evita el cobro **recurrente** doble (cancela la desplazada), pero si el usuario completó dos checkouts, el primer mes de la desplazada ya se cobró y `subscriptions.cancel` no lo reembolsa. Caso de soporte manual, raro (exige completar dos checkouts). Documentarlo para soporte o cancelar con `prorate`/refund.

### S4-6 · Sin CSP (conocido y diferido — se confirma en vivo)
- `next.config.ts:40` lo declara ("La CSP se deja para un fix dedicado"); curl a `https://enkoras.com/` confirma que no viaja `Content-Security-Policy`. Los 5 headers prometidos SÍ están (ver LIMPIO). Sigue pendiente el fix dedicado (GA + Stripe).

### S4-7 · Enumeración de cuentas en registro (dependiente de configuración de Supabase)
- `register/page.tsx:68` muestra "alreadyRegistered" si el signUp devuelve ese error. Con confirmación de correo ACTIVADA Supabase ofusca el caso (devuelve éxito falso) y no hay enumeración; si algún día se apaga la confirmación, esta rama se vuelve un oráculo. El reset NO filtra (siempre "enviado", `reset/page.tsx:26-37`). Informativo.

---

## LO REVISADO QUE SALIÓ LIMPIO

**Server actions (los 12 archivos leídos completos: admin, chat, cuenta, disponibilidad, empresa, equipo, licitaciones, mi-empresa, resenas, semillero, stripe, verificacion):**
- Todos los exports exigen sesión y delegan la autorización a RLS/RPC con guard interno; el patrón "0 filas = sin-permiso" (RLS silenciosa) está aplicado en updates/deletes (`mi-empresa.ts:67,104,154,310,354,424`; `disponibilidad.ts:104,118`; `equipo.ts:82,99,116`; `licitaciones.ts:428,448,544,653`).
- `admin.ts`: las RPC llevan `is_admin()` adentro (verificado con `pg_get_functiondef` en BD viva: `admin_set_company_verification`, `admin_toggle_company_active`, `set_user_role`) — aunque `anon` tiene EXECUTE, el guard interno bloquea; `exigirAdmin` cubre las que van por RLS directa.
- `stripe.ts` (checkout): candados verificados (`candados_prendidos` en BD viva y `app_config.candados_plan=true`), `verificacion-requerida` con service role, cortesía intocable, upgrade/downgrade por `subscriptions.update` con prorrateo sobre la MISMA suscripción (anti doble-cobro S1 previo, sano), cliente Stripe reutilizado.
- Webhook: firma SIEMPRE verificada (400 sin firma / firma inválida), 500-para-reintento cuando la BD falla, idempotente ante eventos repetidos, metadata envenenada (23505) → 200+grito, cortesía jamás pisada, `past_due` mantiene el plan (gracia), estados desconocidos → `presencia` (falla cerrada). Salvo S2-1/S4-5 arriba, la máquina de estados está bien armada.
- `avisarIndexNow`: solo tras mutación autorizada; llave IndexNow es pública por diseño y se sirve correcta en `/473579fd07d0278b2b8a3af9084972bc.txt` (curl 200, contenido = llave).
- `licitaciones.ts`: `eliminarAdjunto` confirma el delete ANTES de tocar Storage (anti-IDOR destructivo, :537-546); `guardarPlantillaDesdeLicitacion` exige `es_miembro('publicar')` antes de leer `budget_max` con service (:617-622); `abrirAdjunto` anónimo es deliberado y la RLS de `tender_attachments` (verificada en vivo) espeja la visibilidad del tender; plantillas totalmente constriñidas en BD (checks verificados en vivo).
- `chat.ts`: `miLadoEnConversacion` resuelve el lado en servidor; rate limit POR PERSONA vivo en BD (098: trigger `rate_limit_guard('created_by','60','20')` sobre messages, con `created_by` SIN grant de INSERT → default `auth.uid()` inviolable) + candado anti-monólogo en la action.
- `semillero.ts`: CSPRNG de ~80 bits para passwords, doble validación de que la empresa es del equipo, rollback si falla la transferencia.
- `cuenta-limpieza` / storage: orden Stripe→usuario→storage correcto (v1); el hueco es el v2 (S2-2).

**Hoja pública de licitación:**
- `budget_max` NO puede salir: el SELECT de la hoja no lo pide y — verificado en BD viva — la columna no tiene grant de SELECT ni para `anon` ni para `authenticated` (solo INSERT); la única vía es service role tras validar convocante (sala: `(sala)/page.tsx:363,383-385`; plantilla: `licitaciones.ts:621`). También verificado: `embedding` y `created_by` fuera del SELECT de anon.
- `UUID_RE` (page.tsx:24) previene el viaje a BD con basura — real.
- `jsonLdSeguro` escapa `< > & U+2028/29` (XSS en JSON-LD cubierto); breadcrumb espejo del JSON-LD.
- `unavailable_after` con ISO en abiertas; alternates/canonical vía `alternatesSeo`.

**Caché:**
- Clave de `unstable_cache` con el objeto `category`: sin colisión posible (el id viaja en la serialización de args; categorías distintas → claves distintas), crecimiento acotado (salvo el matiz S4-4).
- `unstable_cache` no usa cookies/headers adentro (patrón correcto); `itemsDeRama` congela el reloj de disponibilidad solo 5 min.
- Sitemap: CACHEADO (curl x2: `X-Vercel-Cache: HIT`, `Age: 290`, 0.08 s) — no pega a BD en cada hit; lee `company_categories` como anon y esa tabla tiene SELECT público con policy `true` (verificado): solo expone vínculos categoría-empresa, nada sensible; fail-open documentado si el conteo llega vacío.

**Auth:**
- Trial por Google-crea-cuenta: nace en la BD con el trigger `trg_trial_alta` al estrenar empresa (088), idéntico para cualquier vía de signup; `on conflict do nothing` (una sola vez); equipo Enkoras exento. `trial_dias=60` vivo.
- `app/auth/callback/route.ts`: validación de `next` correcta (origin-check, inmune a `//` y `/\`).
- Reset: sin enumeración; confirm: token_hash canjeado solo con clic humano (anti-SafeLinks); update-password exige sesión de recuperación.
- Líneas de aceptación de términos presentes en registro (`register:182-195`) Y en login cubriendo "Google crea cuenta" (`login:171-184`) — commits de hoy 52b7f17/4d581b0.

**Headers / proxy / secretos:**
- Headers en vivo (curl a `/`): `X-Frame-Options: SAMEORIGIN`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `HSTS max-age=63072000; includeSubDomains; preload`, `Permissions-Policy` — los 5 de `next.config.ts` aplicando en TODA ruta (probado también en robots.txt y estáticos).
- Redirects 308 de `/solicitudes/*` funcionando en vivo (curl: 308 → `/licitaciones?modo=cotizacion`).
- `proxy.ts`: `/mi-empresa` sin sesión → 307 a `/auth/login?next=%2Fmi-empresa` (vivo); el `next` que arma el proxy es el pathname propio (no inyectable); la exclusión del matcher por punto (`.*\..*`) no expone nada (no hay rutas privadas con punto; `/mi-empresa/x.txt` → 404) y las páginas conservan su segunda línea (admin layout verifica rol en BD, `(admin)/layout.tsx:8-17`; acciones admin re-verifican por RPC/exigirAdmin).
- robots.txt en vivo: privadas bloqueadas en ES y /en, grupo de bots IA repite los disallows (correcto: el grupo con UA propio sustituye al `*`).
- Secretos: cero usos de `NEXT_PUBLIC_` fuera de los 5 legítimos (URL, anon key, site URL, GA, Stripe publishable); `serviceClient` solo en código de servidor (actions/route handlers/páginas server; el uso en `(sala)/page.tsx` es server component y solo tras `soyConvocante`); ningún client component importa la service key; `account_plans` con RLS de fila propia y columnas `stripe_*` fuera del grant (verificado en vivo); columnas `rfc`, `rfc_document_url`, `email/phone/whatsapp` de companies fuera del grant de `anon` (verificado en vivo).
- Crons (`/api/cron/trials`, `/api/cron/backfill`): gated por `CRON_SECRET` Bearer; avisos de trial con reclamo transaccional (dedupe).
- Constancia PDF (`/api/constancia/[tenderId]`): exige sesión, estado `awarded`, y autoriza convocante o dueña de ESA oferta con doble check (RLS + explícito :77-79); `private, no-store`.
- Buckets: `company-docs`/`tender-quotes`/`tender-specs` privados, `company-photos` público (por diseño); policies de storage por carpeta con `es_miembro` + candados de claim/sellado (leídas completas en BD viva, coherentes con las actions).
- Navbar: counts de `companies`/`account_members` con la sesión del propio usuario (RLS), solo costo, cero exposición.
- Búsqueda pública: término saneado antes del `.or()` (`hibrida.ts:33-35`); ids de estado/ciudad forzados a UUID (`uuidOVacio`) en categoría y tablón.

**Nota de verificación pendiente de dato real:** no hay tenders en prod (demos borradas), así que el header ISR de una hoja VIVA y su `unavailable_after` en HTML no se pudieron observar en vivo — el código es correcto; conviene un curl a la primera licitación real que se publique.
