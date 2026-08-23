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

## FASE 5 — Plan Maestro: asientos, tokens y monetización v2

> Origen: **Plan Maestro Enkoras** (documento de socios, ago-2026). Decisión ya
> tomada por los socios y confirmada como construible por el CTO. El orden de
> los bloques es el acordado; Stripe cierra a propósito — se cablea el cobro
> cuando ya existe todo lo que se cobra.

### 5.1 Búsqueda experta con IA (sección nueva)
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

### 5.2 Multiusuario — asientos y roles
- La empresa renta N asientos y los asigna por correo (invitaciones).
- Al aceptar, cada persona elige rol: **Reclutador** (libera tokens de
  búsqueda) o **Comprador** (solo publica solicitudes/licitaciones).
- Técnica: tabla de miembros (empresa + persona + rol) + RLS por membresía en
  lugar de `owner_id` único. Convive con el multiempresa actual (1 correo →
  hasta 5 empresas).

### 5.3 Sistema de tokens
- Compra de paquetes, saldo por empresa, consumo por búsqueda experta,
  historial de movimientos.
- Precio del token = costo directo ÷ (1 − margen objetivo). **Antes de fijar
  precio: instrumentar el costo real** (llamadas Gemini + cómputo del
  matching) — hoy no hay dato de uso real suficiente.

### 5.4 Plan Free con vencimiento
- El Free actual es indefinido → se convierte en trial de captación.
- El Plan Maestro propone 3 meses; **contrapropuesta de Javi: 1.5–2 meses
  máximo** (3 se siente excesivo). Pendiente de acordarse con socios; se
  evaluará de igual forma.

### 5.5 Candado de pago a Licitaciones en vivo
- El módulo (ya construido y en producción) pasa a ser **complemento de pago**:
  solo se accede con suscripción pagada. Se implementa directo — hoy no hay
  clientes reales registrados (aún sin marketing), así que no hay nadie a
  quien migrar ni transiciones que cuidar.

### 5.6 Unificación Solicitudes ↔ Licitaciones
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

### 5.7 Catálogo de planes v2 (el nuevo menú comercial)
Estructura de trabajo **entendida y aceptada por Javi (22-ago-2026)** — boceto
visual en el artifact "Planes Enkoras v2". Los planes se nombran por **lo que
eres** (explorar / vender / comprar-y-vender), no por tamaño:

| Plan | Para quién | Qué incluye |
|---|---|---|
| **Explorador** — $0, trial 2 meses | Conocer la plataforma | Home, categorías, búsqueda, contactar, 1 empresa. Al vencer: plan o pausa |
| **Proveedor** — $—/mes | El que **vende** | Hasta 5 empresas, prioridad en búsquedas, responder solicitudes, **ofertar** en licitaciones EN VIVO |
| **Empresa completa** — $—/asiento/mes | El que **compra** (o ambos) | Todo lo anterior + hasta 10 empresas, publicar sobres cerrados, **convocar** EN VIVO, asientos con roles, **mesada de tokens/mes** |
| **Recargas de tokens** — consumible | Búsqueda experta | La mesada del plan se reinicia cada mes; el paquete extra se suma al saldo. Nunca "ilimitado" |

Reglas del catálogo:
- **Ofertar vive en Proveedor (barato) y convocar solo en Empresa completa**:
  el que convoca recibe el ahorro y paga fuerte; el que oferta da la liquidez
  que hace bajar los precios y paga suave.
- El cliente "solo quiero IA y licitaciones" NO necesita plan especial: es
  Empresa completa con 1 asiento y 1 empresa.

- Ningún precio se publica hasta tener los números del CTO (costo por
  búsqueda y desglose del $950).
- Los cambios de plan se hacen directo, sin planes de transición: no hay
  clientes reales registrados todavía (el marketing no ha arrancado).
- La suscripción de profesionistas ($800) es del lado Talento — no es de esta
  plataforma; se lista solo para que el catálogo global cuadre.

### 5.8 Stripe v2 (el cierre)
- Webhooks nuevos, precios por asiento, venta de paquetes de tokens, cargo del
  complemento de licitaciones, trial con vencimiento — la configuración
  completa, al final, cuando 5.1–5.7 ya existen.
- CFDI 4.0 desglosado por asiento y por complemento (requisito legal del Plan
  Maestro).

**Datos que produce el CTO para calibrar precios (previo a 5.3/5.7):**
tasa de conversión real Free→Premium de las empresas registradas · costo real
por búsqueda con IA · desglose del precio de trabajo de $950 MXN (cuánto es
asiento base, cuánto licitaciones).

**Criterio de salida de Fase 5:** una empresa puede comprar asientos,
invitar a su equipo con roles, gastar tokens en la búsqueda experta, publicar
necesidades en ambos modos, y el complemento de licitaciones se cobra —
todo facturado por Stripe con CFDI desglosado.

---

## Resumen visual

| Fase | Nombre | Qué entrega | Depende de |
|------|--------|-------------|------------|
| 0 | Fundación | Repo replicado y limpio, i18n ES/EN, esquema definitivo, árbol sembrado, geografía nacional | — |
| 1 | Directorio núcleo | Registro multi-servicio + IA, perfiles y cards B2B, búsqueda híbrida, disponibilidad, verificación RFC | Fase 0 |
| 2 | Conexión | Solicitudes con ruteo, mensajería realtime, notificaciones, alertas | Fase 1 |
| 3 | Monetización y lanzamiento | Planes, destacados, SEO, lanzamiento BC con test 2-3 meses | Fase 2 |
| 4 | Expansión | Estados MX, USA, apps móviles, features de profundidad | Fase 3 + datos del test |
| 5 | Plan Maestro | Búsqueda experta IA, asientos+roles, tokens, trial con vencimiento, paywall licitaciones, unificación solicitudes/licitaciones, Stripe v2 | Fase 3 + Plan Maestro |

---

## Riesgos por fase y su control

- **Fase 0:** el riesgo es subestimar el esquema. Control: diseñar TODAS las tablas (incluso las de Fase 2) antes de la primera pantalla.
- **Fase 1:** el riesgo es que la clasificación IA falle en giros ambiguos. Control: la IA propone y el humano confirma — las correcciones manuales generan el dataset para mejorar el prompt.
- **Fase 2:** el riesgo es construir mensajería compleja de más. Control: chat 1-a-1 simple; nada de grupos, archivos ni features de Slack.
- **Fase 3:** el riesgo es lanzar sin masa mínima. Control: no se anuncia públicamente hasta tener el seed de proveedores invitados con perfiles completos (el playbook de mensajes directos ya está probado con Tu Local).
- **Fase 4:** el riesgo es expandir sin datos. Control: cada estado nuevo se abre solo cuando las métricas del anterior lo justifican.
- **Fase 5:** el riesgo es fijar precio de token sin dato real de costo. Control: instrumentar consumo de IA durante el piloto antes de anunciar precio.
