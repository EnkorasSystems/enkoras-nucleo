# Roadmap — Plataforma B2B de Proveedores

> Plan de construcción por fases. Complementa a `01-analisis-y-vision.md` (el porqué y el qué)
> y a `03-arbol-categorias.md` (la taxonomía completa).
> Forma de trabajo: **lineal — bases sólidas, cada fase completa antes de pasar a la siguiente,
> nunca a medias, nunca mínimo viable a secas.**

**Fecha:** Agosto 2026
**Repo base:** `directorio-bc` (Tu Local) — se replica como punto de partida
**Repo nuevo:** `directorio-b2b` (nombre de trabajo; el nombre comercial se decide al final)

---

## Principios de construcción

1. **El diseño de Tu Local se conserva.** La estética (paleta, cards, wizard, lightbox, badges) ya fue refinada y aprobada. Se adapta al contexto B2B, no se rehace.
2. **Bilingüe desde la fundación.** La infraestructura i18n (ES/EN) se instala ANTES de cualquier pantalla nueva. Cada texto nuevo nace en dos idiomas. Hacerlo después significa tocar cada archivo dos veces.
3. **El esquema de datos se diseña completo desde el día 1.** Las 4 áreas estructurales (categorías-árbol, geografía, servicios+embeddings, módulos nuevos) se resuelven en la fundación aunque su UI llegue en fases posteriores. Migrar esquemas con datos en producción es 10x más caro que diseñarlos bien al inicio.
4. **La IA trabaja al escribir, no al leer.** Clasificación y embeddings ocurren una vez por publicación. La búsqueda es matemática en Postgres. Esto mantiene el costo de operación cerca de cero.
5. **Cada fase termina con la app desplegable y probada en local.** Commit y push solo con OK explícito tras probar.
6. **La BD de Tu Local no se toca.** Proyecto Supabase nuevo e independiente.

---

## FASE 0 — Fundación

**Objetivo:** el esqueleto correcto. Al terminar esta fase no hay features visibles nuevos, pero toda la estructura sobre la que se construye el resto ya existe y es la definitiva.

### 0.1 Replicación del repo
- Copiar `directorio-bc` → `directorio-b2b` (historia git nueva, repo limpio)
- Subir a GitHub (cuenta CrowSo)
- Limpiar lo que no aplica al B2B:
  - Categorías locales (comida, belleza...) y sus seeds
  - Tipos de servicio local (a domicilio / en el local / para recoger)
  - Búsqueda y filtros por código postal
  - Textos y branding "Tu Local" / "Baja California" como límite
- Proyecto Supabase nuevo (BD independiente); variables de entorno propias
- Proyecto Vercel nuevo

### 0.2 Infraestructura bilingüe (i18n)
- Instalar y configurar la librería de i18n (next-intl o equivalente para App Router)
- Estructura de archivos de traducción `es.json` / `en.json`
- Selector de idioma en el navbar (persistente por usuario)
- Migrar TODOS los textos existentes de las pantallas heredadas a los archivos de traducción
- Detección de idioma inicial por navegador con override manual

### 0.3 Esquema de datos definitivo
Migraciones nuevas que definen la estructura B2B:

- **`categories`** — rediseñada: árbol de 3 niveles con `parent_id` auto-referencial, `level` (1/2/3), `slug`, nombre en ES y EN
- **`company_categories`** — muchos-a-muchos empresa↔categoría (una empresa vive en varias ramas)
- **`services`** — catálogo por empresa: `company_id`, nombre, descripción, `embedding vector` (pgvector), orden
- **`states` / `cities`** — catálogo geográfico completo de México (32 estados, ciudades principales por estado), con `country` para el futuro USA
- **`companies`** (evolución de `businesses`) — giro de negocio, descripción, estado/ciudad, contacto completo, RFC (para verificación), campos bilingües donde aplique
- **`availability`** — disponibilidad: `company_id`, tipo (unidades/volumen/capacidad/cupo), cantidad, unidad, descripción, `updated_at`, `expires_at`
- **`conversations` / `messages`** — mensajería interna (estructura lista; UI en Fase 2)
- **`notifications`** — notificaciones (estructura lista; UI en Fase 2)
- **`requests`** (evolución de `posts`) — solicitudes de compradores con categorías asignadas y embedding
- Habilitar **pgvector** y los índices de similitud
- RLS completo para todas las tablas nuevas

### 0.4 Seed del árbol de categorías
- Sembrar el árbol completo desde `03-arbol-categorias.md`: 14 sectores → subcategorías → sub-subcategorías
- Nombres en ES y EN para cada nodo
- Slugs únicos y jerárquicos

### 0.5 Catálogo geográfico
- Seed de los 32 estados de México con sus ciudades principales
- Estructura lista para agregar estados de USA en Fase 4

**Criterio de salida de Fase 0:** la app corre en local, bilingüe, con BD nueva poblada (categorías + geografía), sin rastro del contexto "directorio local BC", y con el esquema completo listo para las fases siguientes. Typecheck y lint limpios.

---

## FASE 1 — Directorio B2B núcleo

**Objetivo:** el lado de la oferta completo. Una empresa puede registrarse, publicar su catálogo y su disponibilidad, y ser encontrada.

### 1.1 Registro de empresa (wizard adaptado)
- Paso 1: datos de la empresa — nombre, giro de negocio (texto libre), descripción, RFC (opcional al inicio, requerido para verificación)
- Paso 2: ubicación — estado → ciudad (selects del catálogo)
- Paso 3: **servicios** — inputs dinámicos: la empresa agrega N servicios, cada uno con nombre + descripción ("agregar otro servicio")
- Paso 4: contacto — email, teléfono, WhatsApp, sitio web, redes
- Paso 5: fotos — logo + instalaciones/trabajo (hereda el sistema de fotos con portada)
- Al enviar: **llamada de clasificación IA** (giro + servicios + descripciones → categorías de los 3 niveles) + generación de embeddings por servicio
- La empresa ve las categorías asignadas y puede ajustarlas (la IA propone, el humano confirma)

### 1.2 Perfil público de empresa (rediseño de la ficha)
- Header: nombre, giro, ciudad/estado, badge de verificación, rating
- **Catálogo de servicios** — cada servicio con nombre y descripción
- **Bloque de disponibilidad** con indicador de frescura ("actualizado hace 2 horas" verde / "hace 3 semanas" gris)
- Categorías del árbol como chips navegables
- Escalera de contacto: email → teléfono → WhatsApp → (chat en Fase 2)
- Reseñas (heredado)
- Tracking de eventos de contacto (heredado)

### 1.3 Cards de empresa (rediseño)
- Logo/foto, nombre, giro, ciudad
- Chips de categorías principales
- **Señal de disponibilidad** (punto verde + resumen si está fresca)
- Badge de verificado
- Rating

### 1.4 Sección de categorías (rediseño)
- Navegación del árbol: 14 sectores → subcategorías → sub-subcategorías
- Página por categoría con las empresas de esa rama (incluye descendientes: ver "Transporte Terrestre" muestra también FTL, refrigerada, etc.)
- Contador de empresas por rama

### 1.5 Búsqueda híbrida
- Barra de búsqueda en lenguaje natural
- Pipeline: embedding de la query → query híbrida en Postgres (filtro de categorías + full-text + similitud pgvector + peso por frescura de disponibilidad)
- Filtros: estado, ciudad, con disponibilidad activa, verificados
- Página de resultados con las cards nuevas

### 1.6 Módulo de disponibilidad (el diferenciador)
- En el dashboard de la empresa: crear/actualizar disponibilidad en 2 clics
- Tipos flexibles según giro: unidades ("10 troques"), volumen ("5,000 etiquetas/semana"), capacidad ("2 líneas libres"), cupo ("40 comidas diarias")
- Expiración automática configurable (24h / 72h / 1 semana) — al expirar desaparece del perfil y baja el ranking
- Recordatorio por notificación antes de expirar

### 1.7 Verificación RFC
- La empresa sube su RFC y constancia de situación fiscal
- Cola de revisión en el panel admin (aprobar/rechazar)
- Badge de verificado en perfil y cards

### 1.8 Dashboard de empresa (mi-empresa)
- Herencia del mi-negocio: editar perfil, fotos, estadísticas de contacto
- Nuevo: gestión de servicios, gestión de disponibilidad, estado de verificación

**Criterio de salida de Fase 1:** flujo completo probado — registrar empresa con 5 servicios, clasificación IA correcta, aparecer en búsqueda semántica y por árbol, publicar disponibilidad, ser contactada por WhatsApp/tel/email. Ambos idiomas.

---

## FASE 2 — Conexión

**Objetivo:** el lado de la demanda y el ciclo completo de comunicación dentro de la plataforma.

### 2.1 Tablero de solicitudes
- Cualquier empresa registrada publica una solicitud: título, descripción, categoría sugerida por IA (ajustable), ciudad/estado, vigencia
- La IA clasifica la solicitud en el árbol y genera su embedding
- Página de solicitudes activas (el "feed" B2B): filtrable por categoría y ubicación
- **Ruteo:** notificación automática SOLO a las empresas cuyas categorías coinciden con la solicitud
- Los proveedores responden vía la escalera de contacto o chat
- La solicitud expira en su vigencia; el autor puede cerrarla marcando "resuelta"

### 2.2 Mensajería interna
- Chat 1-a-1 empresa↔empresa con Supabase Realtime
- Iniciable desde: perfil de empresa, solicitud, o card
- Bandeja de conversaciones con no-leídos
- Indicadores de estado (enviado/visto), timestamps

### 2.3 Notificaciones
- Campanita en navbar con contador
- Centro de notificaciones: mensajes nuevos, solicitudes que coinciden, disponibilidad por expirar, verificación aprobada, alertas de búsqueda
- Realtime en la app; email digest para no-conectados (resumen diario, no spam)

### 2.4 Alertas de búsqueda
- Guardar una búsqueda ("transporte refrigerado en Tijuana")
- Notificación cuando aparece un proveedor nuevo o con disponibilidad fresca que coincide
- Gestión de alertas en el dashboard

**Criterio de salida de Fase 2:** ciclo completo demanda→oferta probado — empresa A publica solicitud de etiquetas, empresa B (proveedora de etiquetas) recibe la notificación, responde por chat, y la conversación fluye en tiempo real. Ambos idiomas.

---

## FASE 3 — Monetización y lanzamiento

**Objetivo:** las líneas de ingreso activas y el lanzamiento real en Baja California.

### 3.1 Planes de proveedor
- Adaptar la infraestructura Stripe heredada
- Plan gratuito: perfil completo, servicios, disponibilidad, responder solicitudes
- Plan premium: prioridad en resultados, badge destacado, estadísticas avanzadas de contactos, más slots de disponibilidad simultánea
- Precios en MXN y USD

### 3.2 Destacados y publicidad
- Posiciones destacadas por categoría/ciudad (inventario limitado — escasez = precio)
- Espacios publicitarios en páginas de categoría y resultados
- Gestión desde el panel admin

### 3.3 Endurecimiento pre-lanzamiento
- SEO completo: sitemap dinámico, OG images por empresa, metadata bilingüe, JSON-LD (Organization/LocalBusiness)
- Google Search Console + Analytics
- Páginas legales adaptadas al B2B (términos, privacidad LFPDPPP, nosotros)
- QA del flujo completo en ambos idiomas
- Performance (Core Web Vitals)

### 3.4 Lanzamiento BC (test de 2-3 meses)
- Seed inicial: proveedores clave de la red del socio invitados directamente (transporte, aduanas, empaque, servicios industriales de Tijuana)
- Distribución por los canales del socio: grupos de WhatsApp empresariales + LinkedIn industrial
- Métricas del test: empresas registradas, % con disponibilidad activa y frecuencia de actualización, solicitudes publicadas, contactos generados (clicks + chats), retención semanal
- Iteración semanal sobre feedback real

**Criterio de salida de Fase 3:** plataforma en producción con dominio propio, primeras decenas de empresas reales activas, primeras solicitudes reales fluyendo, y datos del test para decidir la expansión.

---

## FASE 4 — Expansión

**Objetivo:** crecer geográficamente y salir del navegador.

### 4.1 Expansión nacional
- Apertura por estados según demanda del test: Nuevo León (Monterrey) → Querétaro → CDMX/Edomex → resto
- La estructura geográfica ya lo soporta desde Fase 0 — la expansión es comercial, no técnica
- Seed de proveedores por estado (mismo playbook: invitación directa + canales locales)

### 4.2 Mercado USA
- Activar estados de USA en el catálogo geográfico (California primero — corredor Tijuana-San Diego)
- La plataforma ya es bilingüe desde Fase 0 — sin re-arquitectura
- Enfoque: compradores americanos buscando proveedores mexicanos (nearshoring)
- Cumplimiento legal USA (términos, privacidad CCPA)

### 4.3 Apps móviles iOS/Android
- Tras validar la web (2-3 meses de datos)
- Prioridad de la app: el lado proveedor — actualizar disponibilidad y responder chats desde el celular en 2 taps
- Push notifications nativas (solicitudes y mensajes)

### 4.4 Features de profundidad (según datos del test)
- Cotizaciones estructuradas → evolucionó a **Licitaciones en vivo / subasta inversa** (spec completa en `14-licitaciones-en-vivo.md`)
- Actualización de disponibilidad vía WhatsApp Business API
- Perfiles multi-usuario por empresa (equipos)
- Historial de relación comercial

**Criterio de salida de Fase 4:** plataforma operando en múltiples estados + corredor fronterizo, apps publicadas en las stores, y el ciclo de monetización validado con ingresos recurrentes.

---

## FASE 5 — Plan Maestro: monetización v2 (re-alcance 28-ago-2026)

> Origen: **Plan Maestro Enkoras** (documento de socios, ago-2026), con el
> **re-alcance de Javi del 28-ago-2026**: Enkoras arranca del lado
> PROVEEDORES — la búsqueda experta con IA y el sistema de tokens se
> POSPONEN a una v2 futura, cuando ya haya clientes reales. El orden de
> abajo es el de CONSTRUCCIÓN (lo estructural primero); Stripe cierra a
> propósito — se cablea el cobro cuando ya existe todo lo que se cobra.

### Decisiones del re-alcance (28-ago-2026)
- **POSPUESTO a v2 futura (con clientes):** búsqueda experta IA y tokens
  (specs conservados al final de la fase — no se pierden, solo esperan).
- **Asientos/multiusuario: SE QUEDA** en la tanda cercana.
- **Límites nuevos por cuenta:** Proveedor **2 empresas** · Empresa completa
  **4 empresas** (la principal + 3). (El boceto original decía 5 y 10.)
- **El premium POR EMPRESA muere:** hoy cada empresa paga su propio $499;
  en v2 el plan es **de la CUENTA** y sus beneficios cubren a sus empresas
  (acotadas por el límite del plan). Por eso el precio sube — es justo.
- **Nuevo nivel "Presencia"** (propuesta del CTO, 28-ago, para validar en la
  mesa de socios): al vencer el trial del Explorador la cuenta NO desaparece
  — cae a un piso gratis permanente donde su empresa sigue **visible y
  contactable** en el directorio pero **no puede operar** (ni responder
  solicitudes, ni ofertar, ni convocar, ni publicar disponibilidad). Tres
  razones: (1) el catálogo nunca se vacía — un directorio vale por estar
  lleno (compradores + SEO + oferentes para las subastas); (2) revive el
  diferenciador de visibilidad — vuelve a existir una masa "normal" sobre la
  que la prioridad de los pagadores significa algo; (3) es el mejor vendedor:
  al de Presencia le llega una solicitud que coincide y no puede responderla
  → "súbete a Proveedor" en el momento de máxima intención.
- **3.2 del roadmap viejo (destacados/publicidad vendidos): SUPERSEDIDO** por
  el catálogo v2 — la prioridad en búsquedas vive DENTRO del plan Proveedor,
  no como producto aparte. Posible revival futuro como add-on entre pagadores
  (posiciones limitadas por categoría/ciudad), no en esta tanda.
- **Disponibilidad = el corazón, JAMÁS palanca de cobro (decidido 28-ago):**
  la disponibilidad es lo que convierte esto en una app viva y no una sección
  amarilla — el dato que le da confianza al comprador ("vende material +
  fabrica a la medida + cuadrilla libre" → handshake) no se recorta por plan.
  Política: **tope de CALIDAD de 3 activas por empresa** (curaduría, no
  monetización — hoy no existe tope alguno, verificado en código) · igual
  para TODOS los que operan (Explorador en trial incluido — que pruebe el
  corazón de la app) · Presencia en 0 solo porque Presencia no opera. En las
  **cards** de resultados: las disponibilidades ROTAN solas (fade suave ~4s,
  relojes desfasados entre cards para no marear, pausa al hover, respeta
  reduced-motion) con el chip "+N" siempre visible; el panel de detalle sigue
  mostrando todas.
- **Promesa vieja del premium jamás construida** (estadísticas avanzadas de
  contacto): sigue como posible palanca de upgrade a decidir en 5.E. El
  límite de disponibilidades DEJA de ser palanca (ver punto anterior).
- **Decisión de ranking pendiente (para 5.E):** en búsqueda el orden es
  mérito (disponibilidad +0.15 > verificada +0.10 > premium +0.08, y la
  relevancia domina todo); pero en el listado de CATEGORÍA el orden actual es
  "premium primero siempre, luego rating" — la disponibilidad ahí no ordena.
  Decidir si la categoría se alinea con la filosofía "el activo le gana al
  pagador dormido".

### 5.0 Quick wins de disponibilidad (independientes — no bloquean nada)
- **Tope de 3 activas por empresa**: migración chiquita (constraint/trigger —
  el candado vive en BD, como siempre) + mensaje claro en mi-empresa al
  llegar al tope ("desactiva o edita una para publicar otra").
- **Carrusel en las cards de resultados**: las disponibilidades rotan solas
  (fade ~4s, relojes desfasados entre cards, pausa al hover, respeta
  reduced-motion), chip "+N" siempre visible.
- Pendientes menores heredados (ninguno bloquea): 2.4 alertas de búsqueda
  guardada (sin UI) · email digest de notificaciones (el correo transaccional
  llega con 5.D) · alta en Google Search Console (trámite, no código) ·
  sincronizar la copia docs/02-roadmap.md del repo directorio-b2b.

### Bloque M — Mi Empresa se vuelve dashboard (intercalado antes de 5.A, decidido 28-ago)
> El panel quedó desactualizado: columna centrada de formularios, sin
> estadísticas (company_events jamás se instrumentó — la app ni escribe ni
> lee eventos), sin licitaciones ni reseñas, /cuenta en otro layout. Se
> moderniza a dashboard real ANTES de construir asientos encima (5.A vivirá
> aquí). Vara: nivel asombroso, nada mediocre.

- **M1 — El grifo de datos**: instrumentar company_events desde el perfil
  público (view al montar con dedupe por sesión + clicks tel/WA/email/web/
  chat; el dueño no se cuenta a sí mismo). Sin UI — el reloj de datos
  empieza a correr de inmediato. La tabla, RLS y retención ya existen.
- **M2 — El cascarón**: sidebar fijo izquierdo full-height (selector
  multi-empresa arriba, navegación AGRUPADA: Inicio / Actividad / Perfil
  público / Plan / Mi cuenta, "ver mi perfil" abajo, indicador deslizante
  con resorte) + contenido a TODO el ancho (los formularios conservan ancho
  legible; lo que crece es el lienzo). Integra /cuenta al mismo shell
  (persona ≠ empresa como grupos — crítico para 5.A asientos; el usuario
  SIN empresa conserva /cuenta funcional). Pase visual completo: bordes en
  cards de sección, transición al cambiar sección (nada en seco), popover
  animado, fallbacks naranjas, next/image, jerarquía de botones. Móvil:
  drawer. Las 9 secciones migran tal cual.
- **M3 — El Resumen** (nueva sección default): stat tiles (vistas, clicks
  con desglose, conversaciones, rating) + gráfica 7/30/90 días + "estado
  del negocio" (verificación, plan, disponibilidades x/3, categorías,
  completitud) + actividad reciente + accesos rápidos + card de
  licitaciones con números y link (no se duplica la sala).
- **M4 — Reorganización fina**: Ubicación se parte (Ubicación / Contacto y
  redes) · RFC en UN solo lugar (Verificación; en Datos solo lectura si
  aprobado) · nueva sección Reseñas (monitorear las recibidas) · pulido
  interno de cada sección al estándar actual. Plan v1 se migra congelado
  (lo rehace 5.E).

### 5.A Multiusuario — asientos y roles *(antes 5.2)*
- La cuenta renta N asientos y los asigna por correo (invitaciones).
- Roles: con los tokens pospuestos, el rol "Reclutador" pierde su razón
  original — los roles se redefinen al diseñar (p. ej. **Admin** /
  **Comprador**); queda abierto hasta el diseño detallado.
- Técnica: tabla de miembros (empresa + persona + rol) + RLS por membresía en
  lugar de `owner_id` único. Convive con el multiempresa (los límites nuevos:
  2 por cuenta en Proveedor, 4 en Empresa completa).
- Es la pieza MÁS estructural (toca la seguridad de toda la app) — por eso va
  primero: todo lo demás se monta encima.

### 5.B Unificación Solicitudes ↔ Licitaciones *(antes 5.6)*
- Se construye ANTES del paywall para candar el flujo ya unificado.
- (El detalle del flujo, en su sección original más abajo — sin cambios.)

### 5.C Candado de pago a Licitaciones en vivo *(antes 5.5)*
- El módulo (ya construido y en producción) pasa a ser **complemento de pago**:
  ofertar EN VIVO desde Proveedor; convocar solo en Empresa completa. Se
  implementa directo — hoy no hay clientes reales registrados (aún sin
  marketing), así que no hay nadie a quien migrar ni transiciones que cuidar.

### 5.D Trial con vencimiento → cae a Presencia *(antes 5.4)*
- El Free actual es indefinido → se convierte en trial de captación
  (Explorador). El Plan Maestro propone 3 meses; **contrapropuesta de Javi:
  1.5–2 meses máximo**. Pendiente de acordarse con socios.
- Al vencer NO se apaga la cuenta: cae al nivel **Presencia** (ver decisiones
  arriba) — visible sin operar.
- Requiere **correo transaccional** (avisos de "tu prueba vence en X días"):
  hoy la app no envía ningún email; Resend ya está contratado (lo usa Supabase
  Auth) — falta cablearlo en la app.

### Detalle de 5.B — Unificación Solicitudes ↔ Licitaciones
- Un solo flujo "**Publica tu necesidad**" con dos modos:
  - **Normal / sobres cerrados** (hoy: solicitud) — cada proveedor manda UNA
    oferta a ciegas: sin pulso, sin ver a los demás, sin mejorarla, sin
    negociación estructurada. Cotizar y esperar. Incluido en la suscripción.
  - **EN VIVO** (hoy: licitación) — reloj, pulso anónimo con posición,
    mejorar oferta, anti-sniping, contraofertas, presupuesto privado. Es el
    complemento premium (5.5).
- **Regla de oro (decidida 22-ago-2026):** todo lo que huela a competencia en
  tiempo real vive SOLO en EN VIVO. La línea de pago no es "rápido vs lento"
  — es "cotizaciones a ciegas vs subasta donde pelean por tu contrato". El
  valor premium es la guerra de precios, no el reloj: el reloj solo la
  concentra.
- El modo normal se re-monta sobre la infraestructura de licitaciones (94
  candados, ciclo de vida con cron) — se retira el código paralelo de
  solicitudes. Migrar solicitudes existentes y no romper el ruteo
  `request_match`.

### 5.E Catálogo de planes v2 — CUATRO niveles *(antes 5.7)*
Estructura original **entendida y aceptada por Javi (22-ago-2026)**, ajustada
con el re-alcance del 28-ago: límites 2/4, sin tokens ni IA por ahora, y el
nivel Presencia nuevo. Boceto visual en el artifact "Planes Enkoras v2". Los
planes se nombran por **lo que eres**, no por tamaño:

| Plan | Para quién | Qué incluye |
|---|---|---|
| **Presencia** — $0 permanente | A donde CAE el trial vencido | Su empresa sigue visible y contactable en el directorio. NO opera: ni responder solicitudes, ni ofertar, ni convocar, ni publicar disponibilidad |
| **Explorador** — $0, trial 1.5–2 meses | Conocer la plataforma | Home, categorías, búsqueda, contactar, 1 empresa. Al vencer: elige plan o cae a Presencia |
| **Proveedor** — $—/mes | El que **vende** | Hasta **2 empresas**, prioridad en búsquedas, responder solicitudes, **ofertar** en licitaciones EN VIVO |
| **Empresa completa** — $—/asiento/mes | El que **compra** (o ambos) | Todo lo anterior + hasta **4 empresas** (principal + 3), publicar sobres cerrados, **convocar** EN VIVO, asientos con roles |

Reglas del catálogo:
- **La verificación es requisito de los planes de operación** (confirmado por
  Javi 28-ago, venía de la mesa de socios pero no estaba escrito): para
  contratar Proveedor o Empresa completa la empresa debe estar verificada.
  La verificación deja de ser cosmética — es la credencial que abre las
  funciones de operar. El candado se cablea en 5.E (y la sección Verificación
  del panel ya comunica este rol desde ahora).
- **Ofertar vive en Proveedor (barato) y convocar solo en Empresa completa**:
  el que convoca recibe el ahorro y paga fuerte; el que oferta da la liquidez
  que hace bajar los precios y paga suave.
- El plan es **por CUENTA** (muere el premium por-empresa del v1): los
  beneficios cubren a las empresas de la cuenta, acotadas por su límite.
- Aquí se deciden también: las palancas de upgrade heredadas del roadmap
  viejo (estadísticas pro · límite de disponibilidades por nivel) y la regla
  de ranking del listado de categoría (ver decisiones del re-alcance).
- Ningún precio se publica hasta tener el desglose del $950.
- Los cambios de plan se hacen directo, sin planes de transición: no hay
  clientes reales registrados todavía (el marketing no ha arrancado).
- La suscripción de profesionistas ($800) es del lado Talento — no es de esta
  plataforma; se lista solo para que el catálogo global cuadre.

### 5.F Stripe v2 (el cierre) *(antes 5.8)*
- Webhooks nuevos, precios por asiento, cargo del complemento de licitaciones,
  trial con vencimiento y caída a Presencia — la configuración completa, al
  final, cuando 5.A–5.E ya existen.
- CFDI 4.0 desglosado por asiento y por complemento (requisito legal del Plan
  Maestro).

### ⏳ POSPUESTO a v2 futura (cuando ya haya clientes) — specs conservados

**Búsqueda experta con IA** *(antes 5.1)*
- Sección propia, separada de `/buscar` — el layout de dos columnas de search
  no se toca: es identidad.
- Interacción conversacional: el comprador **describe un requerimiento
  completo** ("envases PET grado alimenticio, certificado FDA, entrega en
  Tijuana, 50k/mes"); la IA hace 1-2 preguntas de aclaración si le falta algo.
- Entrega un **dictamen, no un feed**: shortlist curada de 30-40 empresas que
  SÍ cumplen, con la evidencia de por qué cumple cada condición, y acciones
  directas (contactar, guardar, invitar a licitación).
- Momento de confirmación antes de gastar: "Esto usará 1 token (te quedan N)".
- Puente desde search: consulta larguísima tipo requerimiento → sugerencia
  discreta "¿quieres que la IA te lo resuelva? (1 token)".

**Sistema de tokens** *(antes 5.3)*
- Compra de paquetes, saldo por cuenta, consumo por búsqueda experta,
  historial de movimientos. La mesada mensual vuelve al plan Empresa completa
  cuando esto reviva.
- Precio del token = costo directo ÷ (1 − margen objetivo). **Antes de fijar
  precio: instrumentar el costo real** (llamadas Gemini + cómputo del
  matching).

**Datos que produce el CTO para calibrar precios (previo a 5.E/5.F):**
tasa de conversión real Free→Premium de las empresas registradas · desglose
del precio de trabajo de $950 MXN (cuánto es asiento base, cuánto
licitaciones) · costo real por búsqueda IA (solo cuando reviva la v2 de IA).

**Criterio de salida de Fase 5:** una cuenta puede comprar asientos e invitar
a su equipo con roles, publicar necesidades en ambos modos (sobres cerrados /
EN VIVO), el complemento de licitaciones se cobra, el trial vence y cae a
Presencia sin vaciar el directorio — todo facturado por Stripe con CFDI
desglosado.

---

## Resumen visual

| Fase | Nombre | Qué entrega | Depende de |
|------|--------|-------------|------------|
| 0 | Fundación | Repo replicado y limpio, i18n ES/EN, esquema definitivo, árbol sembrado, geografía nacional | — |
| 1 | Directorio núcleo | Registro multi-servicio + IA, perfiles y cards B2B, búsqueda híbrida, disponibilidad, verificación RFC | Fase 0 |
| 2 | Conexión | Solicitudes con ruteo, mensajería realtime, notificaciones, alertas | Fase 1 |
| 3 | Monetización y lanzamiento | Planes, destacados, SEO, lanzamiento BC con test 2-3 meses | Fase 2 |
| 4 | Expansión | Estados MX, USA, apps móviles, features de profundidad | Fase 3 + datos del test |
| 5 | Plan Maestro (re-alcance 28-ago) | Asientos+roles → unificación solicitudes/licitaciones → paywall EN VIVO → trial que cae a Presencia → planes v2 de 4 niveles (límites 2/4) → Stripe v2. IA y tokens pospuestos a v2 con clientes | Fase 3 + Plan Maestro |

---

## Riesgos por fase y su control

- **Fase 0:** el riesgo es subestimar el esquema. Control: diseñar TODAS las tablas (incluso las de Fase 2) antes de la primera pantalla.
- **Fase 1:** el riesgo es que la clasificación IA falle en giros ambiguos. Control: la IA propone y el humano confirma — las correcciones manuales generan el dataset para mejorar el prompt.
- **Fase 2:** el riesgo es construir mensajería compleja de más. Control: chat 1-a-1 simple; nada de grupos, archivos ni features de Slack.
- **Fase 3:** el riesgo es lanzar sin masa mínima. Control: no se anuncia públicamente hasta tener el seed de proveedores invitados con perfiles completos (el playbook de mensajes directos ya está probado con Tu Local).
- **Fase 4:** el riesgo es expandir sin datos. Control: cada estado nuevo se abre solo cuando las métricas del anterior lo justifican.
- **Fase 5:** dos riesgos. (a) El acantilado del trial: si el vencimiento borrara empresas, cada trial vencido vaciaría el catálogo — control: el nivel Presencia (visible sin operar), el directorio nunca se vacía. (b) Fijar precios sin datos — control: nada se publica sin el desglose del $950; y cuando reviva la IA (v2), instrumentar el costo por búsqueda antes de anunciar precio de token.
