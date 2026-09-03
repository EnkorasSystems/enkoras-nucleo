# AUDIT2 — Backend pre-lanzamiento (candados vivos, constancia 102, trial 60)

**Fecha:** 2-sep-2026 (noche, hora local; timestamps BD en UTC 3-sep) · **BD:** prod lkasoohmksrdgycheqpb via session pooler · **Método:** solo SELECT + pruebas transaccionales en `BEGIN…ROLLBACK` con fixtures `qa-audit2*@qa.dev` (rollback verificado: 0 usuarios qa residuales en ambas corridas). Funciones leídas de `pg_get_functiondef` (vivas), no de archivos. Cuentas y empresa reales intactas.

Estado base confirmado en vivo: `candados_prendidos() = true`, `app_config`: `candados_plan=true` (2026-09-03T01:02Z), `trial_dias=60` (T01:00Z), `trial_avisos_correo_dias=[5,3]`.

---

## HALLAZGOS

### S2-1 · Puerta trasera: cualquier miembro con llave 'anuncio' escribe `rfc` y `rfc_document_url` directo a la tabla — puede apuntar la constancia de OTRA empresa

- **Qué:** `authenticated` conserva grant `UPDATE` de columna sobre `companies.rfc` y `companies.rfc_document_url` (verificado en `information_schema.column_privileges`; el set completo de UPDATE es: address, city_id, description, email, employee_range, facebook_url, founded_year, giro, instagram_url, legal_name, linkedin_url, logo_url, name, phone, **rfc**, **rfc_document_url**, slug, state_id, tiktok_url, website_url, whatsapp, youtube_url). La policy que autoriza es `companies_update_own` que solo exige `es_miembro(id, 'anuncio')`.
- **Por qué importa:** rompe tres defensas a la vez:
  1. La separación de llaves de la 096 (el fix mu S2-2 movió la escritura a `guardar_datos_verificacion`, que exige la llave **'verificacion'** — pero el camino directo por PostgREST quedó abierto con solo 'anuncio').
  2. El anti-IDOR de la 099 (`guardar_datos_verificacion` valida `position(target_company||'/' in nueva_constancia) = 1`; el UPDATE directo no valida nada).
  3. Con candados vivos, **verificada = credencial para comprar** (`iniciarCheckoutPlan` exige `is_verified`); y con la 102, la constancia es "la evidencia antifraude". Un atacante puede someter verificación apuntando a la constancia de un tercero: el admin, al abrir el doc (liga firmada sobre `rfc_document_url`), vería el documento del tercero creyéndolo del solicitante.
- **Evidencia (prueba viva, BEGIN/ROLLBACK):** con jwt sintético de un miembro sentado con permisos `{"anuncio":true}` y `set local role authenticated`:
  ```
  update companies set rfc_document_url='<uuid-de-OTRA-empresa>/constancia-ajena.pdf',
                       rfc='XAXX010101000' where id='<mi-empresa>' returning id
  → 1 fila. Relectura: rfc_document_url = 'cccc0011-…-0011/constancia-ajena.pdf' (carpeta ajena).
  ```
- **Fix:** `revoke update (rfc, rfc_document_url) on public.companies from authenticated;` — la escritura legítima ya viaja completa por la RPC SECURITY DEFINER (`guardar_datos_verificacion`), verificado que la action `solicitarVerificacion` solo usa la RPC.

### S2-2 · La "evidencia antifraude" de la 102 la puede borrar el propio sospechoso (Storage)

- **Qué:** la 102 decide conservar `rfc_document_url` como evidencia, pero la policy viva `company_docs_owner_delete` sobre `storage.objects` permite a cualquier miembro con llave 'verificacion' **borrar** archivos de la carpeta de su empresa en `company-docs` — antes, durante y **después** de aprobar/rechazar. La columna queda apuntando a un objeto inexistente y el admin recibe liga firmada rota.
- **Evidencia:** policy viva: `company_docs_owner_delete DELETE {authenticated}: bucket_id='company-docs' AND foldername[1] IN (empresas donde es_miembro(c.id,'verificacion'))` — sin condición de estado de verificación. La vía se usa desde el cliente hoy: `components/mi-empresa/SeccionVerificacion.tsx:58` hace `storage.from("company-docs").remove([constancia])` al reemplazar.
- **Impacto:** el objetivo declarado de la 102 ("si un perfil resulta suplantado, el documento presentado es la evidencia") no se sostiene: verificarse y borrar el archivo son dos pasos del mismo usuario.
- **Fix sugerido:** quitar el DELETE del dueño (los nombres ya son únicos: `constancia-{Date.now()}`, el reemplazo puede simplemente subir otra y dejar la vieja), o mover el borrado de reemplazo a un server action con service role que se niegue cuando esa ruta sea la constancia de una verificación resuelta.

### S3-1 · Efecto lateral de la 102: re-solicitud tras rechazo con la MISMA constancia rechazada, un clic, sin límite

- **Qué:** antes de la 102, rechazar borraba `rfc_document_url` → re-solicitar obligaba a subir un documento nuevo. Hoy `submit_company_verification` solo exige `rfc is not null and rfc_document_url is not null` y solo bloquea cuando `status='approved'` — tras un rechazo, el mismo doc y RFC re-someten a `pending` inmediatamente, sin cambiar un byte y sin rate limit en la RPC.
- **Evidencia (prueba viva):** empresa qa con `verification_status='rejected'` y doc poblado → `select submit_company_verification(id)` como el dueño (authenticated) → OK; relectura: `{"verification_status":"pending","rfc_document_url":"<mismo doc>"}`.
- **Impacto:** ping-pong contra el equipo de verificación (el rechazo pierde fuerza como señal); el panel admin puede llenarse de pendientes idénticos.
- **Fix sugerido:** en la RPC, si el último estado fue `rejected`, exigir que (rfc, rfc_document_url) difieran de los del rechazo (guardar un hash/copia al rechazar), o al menos un `rate_limit`/cooldown.

### S4-1 · EXECUTE de `anon` sobre RPCs administrativas y de cuenta (los guards internos aguantan, el grant sobra)

- `anon` puede ejecutar `admin_set_company_verification`, `admin_toggle_company_active`, `set_user_role`, `get_profiles_admin`, `submit_company_verification`, `become_empresa` (evidencia: `proacl` con `anon=X/postgres`). Verificado en vivo que los guards internos los frenan (`is_admin()` → "Solo un administrador…", `es_miembro`/`auth.uid()` → excepción), pero es superficie gratuita: un descuido futuro en el guard queda expuesto a tráfico sin sesión. Fix: `revoke execute … from anon` (y de `authenticated` en las 2 `admin_*` no cambia nada funcional: el panel llama con sesión admin).

### S4-2 · Grants de tabla de `anon` con INSERT/UPDATE/DELETE que la RLS deja en cero

- `anon` tiene INSERT/UPDATE/DELETE de tabla sobre `availability`, `categories`, `cities`, `states`, `requests`, `request_categories` y UPDATE/DELETE sobre `company_events`. Verificado en vivo que la RLS lo deja en nada (insert de availability muere en el trigger de plan y en with_check; `delete from requests` → 0 filas). El único write intencional de anon es `company_events` INSERT (`company_events_insert_any`, métricas públicas, con `amortiguar_eventos` y `event_type` acotado por check a view/phone_click/whatsapp_click/email_click/website_click/chat_started). Higiene: revocar los write-grants no usados de anon.

### S4-3 · Índices duplicados (doble mantenimiento por cada write)

- `idx_companies_owner` ≡ `idx_companies_owner_id` (ambos `btree(owner_id)`, ambos usados por el planner: 29,085 y 11,953 scans) — y además existe `idx_companies_owner_created (owner_id, created_at)` que cubre el mismo prefijo: sobran los DOS de una columna.
- `idx_company_categories_category` ≡ `idx_company_categories_category_id` (ambos `btree(category_id)`; 845/776 scans). Sobra uno.

### S4-4 · Columnas huérfanas v1 en `companies` (+ índices muertos)

- `companies.plan` (check `'free'/'premium'`, valor único vivo: `free`), `companies.stripe_customer_id`, `companies.stripe_subscription_id`: cero referencias en `app/`, `lib/`, `components/` (grep) — el plan v2 vive en `account_plans`. Sus índices: `companies_stripe_customer_idx` (0 scans), `companies_stripe_subscription_idx` (1 scan) e `idx_companies_activas_prom (plan DESC, rating_avg DESC) WHERE is_active` que ordena por la columna muerta (sus 1,402 scans son por el predicado `is_active`, no por el orden). Además `companies.plan` es SELECTable por anon — inofensivo pero ruido. Candidatos a drop en una migración de limpieza post-lanzamiento (no urgente).

### S4-5 · Decisión a confirmar: contraofertas y aceptaciones NO pasan por el candado de plan

- El gate `trg_plan_ofertar` lleva `WHEN (kind='bid' AND target_bid_id IS NULL)`: solo la oferta INICIAL exige plan proveedor. Verificado en vivo: convocante en `presencia` (trial vencido) emitió `counter_buyer`, y proveedor en `presencia` aceptó (`kind='bid'` con `target_bid_id`) — ambos pasaron. No hay bypass de entrada (para negociar hubo que ofertar con plan), y "la negociación empezada se puede terminar" es defendible — pero significa que un plan vencido aún puede cerrar acuerdos nuevos en licitaciones donde ya ofertó. Dejarlo escrito como decisión, o extender el gate.

### S4-6 · FKs sin índice de cobertura (frías hoy, anotar para escala)

- 20 FKs sin índice (query sobre `pg_constraint`/`pg_index`). Las únicas tibias: `bids.target_bid_id` (lookups de validación de procedencia en `validate_bid_insert`), `tender_quotes.company_id` (barrido de `cuenta-limpieza`), `saved_companies.company_id` y `reviews.user_id` (cascadas al borrar empresa/usuario). El resto son catálogos (city/state) o auditoría. A 4-7k cuentas no duele; revisar si crecen bids por tender.

---

## VERIFICADO Y LIMPIO

**1. Candados en vivo (pruebas transaccionales, 56+6 pasos OK, rollback verificado):**
- Flag prendido de verdad: `candados_prendidos()=true`; y **checkout apagable**: `iniciarCheckoutPlan` re-verifica el flag antes de vender.
- Trial nace de **60 días exactos** al estrenar la primera empresa (`nacer_trial` lee `trial_dias` en vivo; fila `plan='presencia', cortesia=false, trial_avisos=[]`), **una sola vez** (`on conflict do nothing` — verificado que una 2ª empresa no revive un trial vencido), y **abre el alcance**: con trial vivo `plan_de_cuenta='empresa_completa'` y el tender abre.
- Caída a presencia al vencer: inmediata y sin cron (el CASE de `plan_de_cuenta` compara `trial_ends_at > now()`).
- Gates con flag prendido, todos verificados en vivo con la excepción `plan-requerido:*`: presencia NO convoca / NO publica disponibilidad / NO renueva disponibilidad / NO oferta / NO adjunta cotización; proveedor SÍ responde pero NO convoca; revivir tender re-gatea (`trg_plan_revivir`).
- **Ninguna función vieja quedó con el flag-apagado:** las 7 lectoras de `candados_prendidos` (candado_de, categoria_ranking, destacadas_de, exigir_plan, mi_plan_estado, sillas_extra_de, tope_empresas_de) + los 6 triggers de gate delegan en `exigir_plan`; `trials_por_avisar` duerme sola si el flag se apaga; `destacadas_de`/`categoria_ranking` solo premian pago con flag prendido. `exigir_plan`/`empresa_alcanza`/`plan_de_*` NO son ejecutables por authenticated (verificado: permission denied) — el gate no es opinión del cliente.
- Topes de empresas **1/2/4** y sillas **3/5/7** (2/4/6 extras + dueño) verificados en vivo plan por plan, con **congelamiento al degradar** (lo existente se queda, lo nuevo se bloquea; 6 sillas sobrevivieron el downgrade y la 7ª murió). `valida_silla_066` vivo: dueño de empresas no se sienta (`cuenta-duena`), nadie ocupa dos sillas (`cuenta-ocupada`). `crear_invitacion`/`aceptar_invitacion` cuentan sillas + ligas pendientes bajo el mismo advisory `sillas:` y re-validan 066 al aceptar.
- Semillero: `nacer_trial` y `validate_companies_tope` eximen admin/operador — verificado en vivo que una empresa de operador no crea fila en `account_plans`.
- `mi_plan_estado`/`candado_de` como authenticated devuelven el estado coherente (prendidos, plan efectivo, topes, trial_ends_at).

**2. Constancia conservada (102):**
- La def VIVA de `admin_set_company_verification` ya no toca `rfc_document_url` (solo status + is_verified) y `app/actions/admin.ts` ya no borra el archivo — aplicada de verdad.
- El admin sigue viendo el doc: `company_docs_owner_read` incluye `is_admin()`, y el panel lee la columna por service role (`admin.ts:152-157`).
- `guardar_datos_verificacion` (099) intacta y coherente: llave 'verificacion' + anti-IDOR de ruta + RFC ≥12 — es la única vía de escritura de la app (la action no hace update directo). *(El agujero es el grant de columna, no la RPC — ver S2-1.)*
- `lib/cuenta-limpieza.ts` sigue barriendo `company-docs` (+ company-photos, tender-specs, tender-quotes) por empresa; las constancias viven planas en `{companyId}/constancia-*.ext`, compatibles con su `list(id)`. `semillero.ts` también limpia ambos buckets al borrar demos.
- Flujo re-solicitud tras rechazo: FUNCIONA (pending de nuevo) — el efecto lateral es que funciona *demasiado* fácil (S3-1).

**3. Trial 60 — avisos y dinero:**
- `trials_por_avisar` verificada en vivo: 3.5 días → marca 5; 1.5 días → marca 3 (la más urgente, no las dos); trial de 60 días ausente; `reclamar_avisos_trial` cobra bajo advisory `avisos-trial` y deja `trial_avisos=[5]` / `[3,5]` (cobra la de 5 de paso — si el cron durmió no manda doble); segunda corrida = 0 (dedupe). Cron en Vercel (`/api/cron/trials`, 16:00 UTC diario, Bearer CRON_SECRET); campana in-app siempre, correo falla-suave sin recobrar.
- Webhook v2 (`app/api/stripe/webhook/route.ts`): firma siempre; solo `payment_status='paid'` (asíncronos esperan su evento); consulta el estado REAL de la sub antes de activar; **toda** escritura lleva `.eq('cortesia', false)` — las 2 cortesías vivas (`plan=empresa_completa, cortesia=true, trial null`, únicos 2 registros en `account_plans` hoy) son intocables por Stripe, y si una cortesía paga, los ids quedan rastreables y grita en logs sin pisar el plan.
- Anti-doble-cobro de punta a punta: checkout se niega con sub viva (upgrade = update de la MISMA sub con prorrateo); el webhook cancela la sub desplazada (red S1) y limpia la liga al morir la sub; degradación por respaldo solo con `stripe_subscription_id is null`; eventos envenenados (23505) → 200 + grito, no reintento eterno. El pago **sustituye** al trial (`trial_ends_at: null`), y comprar exige empresa verificada + flag prendido + no-cortesía.
- `account_plans` blindada: RLS solo SELECT propio/de-miembro; authenticated NO ve `stripe_customer_id`, `stripe_subscription_id` ni `trial_avisos` (column grants: solo cortesia/created_at/cuenta/plan/trial_ends_at/updated_at); sin policies de escritura (todo por definer/service); unique en customer y subscription.

**4. Integridad general:**
- **SECURITY DEFINER sin search_path: cero** (barrido completo de `pg_proc.proconfig` en public).
- **Cron `cron.job`:** 6 jobs activos (barrer-licitaciones */5, purgas, stats, avisar-disponibilidades) — 48h de `job_run_details` sin un solo fallo; los 2 crons de Vercel (backfill, trials) declarados en `vercel.json`.
- **Locks:** familia `bid:` compartida por `validate_bid_insert`, `validate_quote_insert`, `validate_quote_update`, `adjudicar_licitacion` y `barrer_licitaciones` (misma llave `hashtext('bid:'||tender_id)`, verificado en prosrc vivo — cero zombis posibles entre ofertar/adjudicar/barrer); `sillas:` en tope+invitaciones (orden de locks documentado y consistente en aceptar); `empresas:`/`disp:`/`avisos-trial` en sus topes.
- **Regresión 086 NO volvió:** `adjudicar_licitacion` vivo conserva la normalización `counter_supplier` y el lock `bid:`.
- **Triggers en orden alfabético correcto:** los gates de plan disparan tras las validaciones pero dentro del mismo statement (un abort revierte también el `superseded` de `validate_bid_insert`); `trg_trial_alta`/`trg_trial_entrega` en AFTER; renovación de disponibilidad gateada con `WHEN (new.expires_at > old.expires_at)`; el tope de availability exime service-role igual que `rate_limit_guard`.
- **RLS:** habilitada en las 32 tablas de public; `app_config` sin policies NI grants (verificado en vivo: permission denied para authenticated — el flag no es legible ni editable por clientes); notifications/messages solo `read_at` por column-grant; profiles/team_invites/claims correctos.
- **Storage:** 4 buckets, solo company-photos público; policies por llave (verificacion/anuncio/ofertar/publicar) con claims y quotes selladas respetadas.
- **Realtime:** publicación con las 11 tablas esperadas.
- **Datos (estados imposibles): todo en cero** — bids activas en tenders muertos: 0; awarded sin ganadora: 0; accepted en abiertas: 0; `is_verified` vs status coherente; approved sin doc/RFC: 0; miembros-dueños y doble silla: 0; cuentas sobre tope: 0; >3 disponibilidades vigentes: 0; servicios sin embedding: 0.
- **Índices calientes presentes:** hnsw en `services.embedding` (buscarHibrido), trigram en name/description/giro (companies y services), `idx_company_categories_category` + `idx_availability_company/expires` (categoria_ranking), `idx_requests_abiertas`/`idx_requests_status` (tablón), `idx_tenders_abiertas` parcial (barrer), `idx_notifications_dedupe` (anti-dup de avisos), `uq_bids_activa_por_empresa` (una activa por empresa), unique `tender_quotes(tender_id, company_id)`, `uq_companies_rfc` case/trim-insensitive.
- `handle_new_user` crea profiles rol 'usuario' (verificado en vivo); `registrar_empresa` corre como caller (RLS) y sin `returning *` (compatible con columnas revocadas).

**No re-reportado (decisiones aceptadas):** cortesías de 2 cuentas, tests solo-en-serie, los 2 hallazgos QA aceptados, llaves Gemini compartidas con tests.
