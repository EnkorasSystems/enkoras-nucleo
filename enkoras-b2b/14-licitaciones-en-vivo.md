# Licitaciones en vivo — subasta inversa (bloque 5)

> Evolución de Solicitudes (doc 09) a un proceso de compra formal y competitivo.
> Complementa a `02-roadmap.md` (esto expande el punto 4.4 "Cotizaciones estructuradas")
> y a `10-ruteo-demanda-oferta.md` (el ruteo existente es la base de la distribución).
> Forma de trabajo: **lineal — cada fase completa antes de la siguiente, nunca a medias.**

**Fecha:** Agosto 2026
**Origen:** Javi descubrió que Solicitudes ya era una licitación informal (RFQ) sin saberlo.
La decisión es llevarla a su forma completa: **subasta inversa en tiempo real**.

---

## 1. Qué es (el fundamento del mundo real)

Una **licitación** invierte la venta: el comprador publica lo que necesita y los
proveedores compiten por el contrato. Una **subasta inversa** es su forma dinámica:
los proveedores pujan **hacia abajo** en precio durante una ventana de tiempo, viendo
señales de la competencia. Lo usan CompraNet (gobierno MX), y las grandes privadas vía
Ariba/Coupa/Ivalua — plataformas donde una pyme mexicana normalmente **jamás entra**.
Ese es el hueco: subasta inversa accesible, sobre un directorio donde los proveedores
ya viven clasificados por rama y verificados.

Reglas de oro de las subastas inversas reales (todas aplican a nuestro diseño):

1. **El presupuesto del comprador es PRIVADO.** Publicarlo ancla los precios hacia
   arriba (todos ofertan "justo debajo del techo"). Se captura, pero solo lo ve el dueño.
2. **La competencia se alimenta con señales anónimas**: el proveedor ve la *mejor
   oferta actual* (sin nombre) o su posición ("vas 2°") — eso es lo que baja precios.
3. **Ventana de tiempo obligatoria.** Sin cierre no hay urgencia ni ofertas.
4. **Anti-sniping**: oferta en los últimos minutos → el reloj se extiende. Evita que
   todos esperen al último segundo.
5. **Adjudicación múltiple**: el comprador puede cubrir 100 ton con 50+50 de dos
   ganadores. En materiales es lo normal.
6. **Todo queda por escrito**: el registro del proceso y del acuerdo final (el "acta")
   es una de las razones por las que las empresas usan estos sistemas.

## 2. Caso de uso ancla

Asistente de un contratista de obra pública: necesita **40–100 toneladas de varilla
corrugada de cierta medida**, con presupuesto asignado que debe respetar, y urgencia.
Hoy tiene dos caminos en la plataforma; con este bloque tendrá tres:

- **Buscar y cotizar** (ya existe): busca proveedores de la rama, les escribe por
  chat/WhatsApp uno por uno, espera cotizaciones y compara a mano.
- **Solicitud** (ya existe, doc 09): publica la necesidad y espera a que le respondan.
- **Licitación en vivo** (nuevo): publica la necesidad estructurada, el sistema la
  distribuye SOLO a proveedores que pueden cumplirla, y en una ventana de tiempo los
  proveedores compiten ofertando cantidad + precio. Ella adjudica desde un tablero.

Solicitudes NO desaparece: queda como la vía tranquila. La licitación es la vía
urgente/competitiva. Misma sección, dos modos.

## 3. El flujo completo

### 3.1 Compradora — crear la licitación

Formulario estructurado (más campos que la solicitud simple):

- **Qué necesita** — título + descripción/especificación (medida, norma, calidad)
- **Cantidad + unidad de medida** — "100 toneladas", "5,000 piezas", "40 comidas/día"
  (catálogo de unidades: toneladas, kg, piezas, m², m³, litros, cajas, tarimas,
  servicios, horas… ampliable)
- **Presupuesto máximo (PRIVADO)** — lo que está dispuesta a pagar; el sistema lo usa
  para pintarle en verde las ofertas que le caben y alertar las que se pasan. Jamás
  se muestra a proveedores.
- **Ventana** — cuándo abre y cuándo cierra (pastillas: 2h / 6h / 24h / 72h / fecha
  exacta). Urgente = ventana corta.
- **Ubicación de entrega** — estado/ciudad opcionales ("Todo México" si da igual)
- **Límite de participantes** — opcional (ej. máximo 15 proveedores conectados)
- Clasificación: **mismo pipeline IA de solicitudes** (embedding + clasificación en
  dos pasos contra el árbol, falla suave) — cero código nuevo de IA

### 3.2 Distribución (ruteo)

El ruteo existente de solicitudes (docs 10/13) aplica tal cual: notificación SOLO a
empresas cuyas categorías coinciden con la licitación (+ filtro geográfico si aplica).
Tipo de notificación nuevo: `tender_invite`. La campana y el email digest ya existen.

### 3.3 Proveedor — la sala en vivo

Al entrar a la licitación el proveedor ve:

- La necesidad completa (título, especificación, cantidad, unidad, entrega, reloj)
- **La mejor oferta actual como precio unitario, ANÓNIMA** — nunca quién
- **Su posición** si ya ofertó ("vas 2° de 6")
- Cuántos proveedores hay conectados (presencia)

Y oferta: **cantidad que puede cubrir + precio total por esa cantidad** (el sistema
calcula y compara por precio unitario). Ofertas parciales bienvenidas: "cubro 50 ton
a $30,000". Puede **mejorar su oferta** las veces que quiera mientras la ventana viva
(cada mejora es una fila nueva — el historial se conserva solo).

### 3.4 Compradora — el tablero en vivo

Ella ve TODO con nombre: cada oferta con empresa (perfil linkeable, verificada,
rating), cantidad, precio total, precio unitario, hora; quiénes están conectados; y
sus ofertas ordenables (por unitario, por cobertura, por rating). Con su presupuesto
privado sobrepuesto: verde = le cabe, rojo = se pasa.

- **Contraoferta dirigida**: selecciona una oferta y propone otro precio a ESE
  proveedor ("¿le haces a $28,000 las 50 ton?"). El proveedor acepta, rechaza o
  re-contraoferta. Todo dentro de la sala, todo registrado.
- **Elige la que más le convenga** — no está obligada a la más barata (rating,
  cobertura, verificación y ubicación pesan). Como en las licitaciones reales:
  el criterio es del comprador.

### 3.5 Cierre y adjudicación

- La ventana cierra (con anti-sniping: oferta en los últimos 2 min → +5 min).
- La compradora **adjudica 1..N ofertas** hasta cubrir su cantidad (múltiple/parcial).
- Automático al adjudicar:
  - Mensaje a ganadores ("tu oferta fue aceptada") y a no ganadores (cortés, sin
    revelar el precio ganador)
  - **El acta**: resumen escrito del acuerdo (quién, qué, cuánto, a qué precio
    unitario y total, cuándo se acordó, con el historial de contraofertas) — visible
    para ambas partes, ancla un hilo de chat entre ellas
- El cierre fino (entrega, factura, pago) ocurre FUERA de la plataforma, por
  chat/llamada — igual que hoy. El acta es la constancia de lo acordado, no un
  contrato: nosotros no somos parte del trato (va en términos legales).
- Licitación sin ofertas al cierre = **desierta** (estado propio; puede re-publicarse
  con un clic ajustando ventana o especificación).

## 4. Modelo de datos (diseño completo desde el día 1)

Convención inglesa de la BD existente (`requests`, `availability`, `conversations`):

- **`tenders`** — company_id, title, description, quantity numeric, unit text,
  budget_max numeric **(RLS: solo owner)**, currency (MXN default), state_id/city_id
  opcionales, max_participants int null, starts_at, ends_at, status
  (`draft` → `open` → `closed` → `awarded` | `deserted` | `cancelled`),
  embedding vector, timestamps. Constraint: ends_at > starts_at.
- **`tender_categories`** — igual que request_categories (el ruteo cuelga de aquí).
- **`bids`** — tender_id, company_id, quantity, total_price, unit_price generada,
  kind (`bid` | `counter_buyer` | `counter_supplier`), target_bid_id null (para
  contraofertas dirigidas), status (`active` | `superseded` | `accepted` |
  `rejected`), created_at. **Inmutables**: mejorar = fila nueva que marca la
  anterior `superseded` → historial y acta gratis.
- **`tender_events`** — log inmutable de la sala: published, joined, bid, counter,
  accepted, closed, extended (anti-sniping). El acta se genera de aquí.
- **RLS fino** (la parte delicada):
  - `budget_max` visible solo para el dueño (columna con grant restringido, mismo
    patrón que rfc/stripe del doc de seguridad)
  - Proveedor ve: la licitación, SUS bids, y la mejor oferta unitaria vía función
    `security definer` que devuelve SOLO el número (jamás company_id ajeno)
  - Compradora (owner) ve todos los bids completos
- **Realtime**: `bids` y `tenders` a la publicación `supabase_realtime` (mismo
  mecanismo que availability, migración 011); presencia de conectados con Supabase
  Presence channels.
- **Notificaciones**: tipos nuevos `tender_invite`, `tender_bid`, `tender_counter`,
  `tender_awarded`, `tender_lost`, `tender_closing` (por expirar sin que respondas).

## 5. UX/UI

- La sección **Solicitudes se renombra "Licitaciones"** (EN: "Tenders") con dos
  modos visibles: **Cotizaciones** (las solicitudes de siempre) y **En vivo**.
  El split view actual se conserva; la sala en vivo es la pantalla nueva.
- La sala se construye con el design system completo (cursos A/B/C): cajitas
  panel-vidrio, tablero con motion (ofertas entrando animadas, reordenamiento con
  layout animations, el reloj con urgencia visual en los últimos minutos), estados
  vacíos ilustrados. Es la pantalla más "viva" de la plataforma — debe sentirse
  como tal sin romper la sobriedad B2B.
- Bilingüe desde el primer commit (regla de la casa).
- El formulario de crear licitación hereda el patrón del wizard (pasos
  direccionales, validación con shake, AvisoIA).

## 6. Reglas de negocio — decididas vs. abiertas

**Decididas (por cómo funcionan las subastas inversas reales):**
- Presupuesto NUNCA público
- Señal competitiva = mejor precio unitario anónimo + posición propia
- Ventana obligatoria con anti-sniping
- Ofertas parciales y adjudicación múltiple desde v1
- El comprador elige libremente (no gana automático el más barato)
- Acta automática de todo acuerdo

**Abiertas (se deciden con los socios, no por ausencia):**
- ¿Solo empresas **verificadas** pueden ofertar? (recomendado: sí — mata ofertas
  fantasma y hace la verificación más valiosa) ¿O verificadas para ofertar y
  cualquiera para mirar?
- Monetización del módulo: ¿licitar gratis y ofertar premium? ¿N ofertas gratis/mes?
  ¿el módulo entero como gancho gratuito para crecer liquidez primero? (si se cobra:
  cadencias cortas, jamás anuales — regla de la casa)
- ¿Invitación restringida (la compradora elige a dedo quiénes participan) en v1 o
  en profundidad?
- Nombre comercial del feature ("Licitaciones en vivo", "Subasta inversa", otro)

## 7. Roadmap del bloque (lineal)

### L0 — Esquema y candados
Migraciones completas (tenders, tender_categories, bids, tender_events), RLS fino
(presupuesto privado, mejor-oferta anónima), realtime en la publicación, tipos de
notificación. Tests de RLS (el patrón de candados de siempre). Sin UI.

### L1 — Licitación con cierre (sin "vivo" todavía)
Crear (form estructurado + IA), ruteo con `tender_invite`, sala donde el proveedor
oferta y ve la mejor oferta anónima **al recargar**, tablero de la compradora,
adjudicación simple + múltiple, acta + mensajes automáticos, estados completos
(desierta, cancelada), Mis licitaciones en /mi-empresa. **Al terminar L1 el feature
ya es usable de punta a punta** — lo que falta es la adrenalina.

### L2 — El VIVO
Realtime en la sala: ofertas aterrizando en vivo con motion, presencia ("6
conectados"), posición actualizándose, reloj con anti-sniping y extensión visible,
notificaciones instantáneas. La sala se convierte en el espectáculo.

### L3 — Negociación
Contraoferta dirigida compradora↔proveedor con su hilo dentro de la sala, re-ofertas,
acta enriquecida con el historial de negociación completo.

### L4 — Profundidad (post-datos)
Adjuntos de especificación (ficha técnica PDF/imagen), plantillas de licitación
recurrente, invitación restringida, re-publicar desierta con un clic, métricas del
módulo en admin, decisión de monetización con datos.

**Criterio de salida del bloque:** ciclo real probado — compradora publica varilla
con presupuesto privado, 3+ proveedores rutados ofertan en vivo, contraoferta, cierre
con anti-sniping, adjudicación dividida, acta en ambos chats. Ambos idiomas. Candados
probados: presupuesto invisible para proveedores, ofertas ajenas invisibles entre sí.

## 8. Riesgos y su control

- **Liquidez** (el riesgo #1): una licitación en vivo con 0 ofertas se siente muerta.
  Control: L1 funciona sin realtime (ventanas largas, sin sensación de "sala vacía");
  antes de ofrecer "en vivo" el sistema valida que la rama tenga un mínimo de
  proveedores activos y sugiere ventana según la densidad de la rama; una desierta
  se re-publica en un clic.
- **Ofertas fantasma / confianza**: proveedores que ofertan y desaparecen. Control:
  ofertar solo verificadas (propuesto), y el historial de adjudicaciones cumplidas
  vive en el perfil a futuro.
- **Complejidad realtime**: el vivo es lo más difícil técnicamente. Control: va en L2,
  separado — L1 entrega valor completo sin él; el mecanismo (publicación + Presence)
  ya está probado en availability y chat.
- **Legal**: no somos parte del contrato. Control: el acta se llama constancia, los
  términos lo dicen explícito, y el pago/entrega quedan fuera de plataforma.
- **Canibalizar Solicitudes**: control: son la misma sección con dos modos — la
  solicitud simple sigue siendo el camino sin fricción, la licitación es para cuando
  hay cantidad, presupuesto y urgencia definidos.

---

## 9. Decisiones del cierre del spec (22-ago-2026, Javi)

Lo pendiente de L4 quedó decidido así — este es el orden de cierre del spec
viejo, previo a arrancar la Fase 5 del roadmap:

1. **Solo verificadas ofertan — SÍ.** ✅ CONSTRUIDO (26-ago, migración 039 + aviso con CTA en la sala + candado en suite). La verificación la otorga Enkoras y es
   un plus del producto. A futuro será **requisito para comprar el plan
   Proveedor y el de Empresa completa** (Explorador no la pide). Mientras los
   planes no existan, el candado interino es directo: ofertar exige empresa
   verificada (BD + UI).
2. **Adjuntos de la convocante — SÍ.** Al crear la licitación se pueden subir
   planos, fichas, fotos — para que el proveedor vea "bien bien" qué se
   requiere antes de ofertar.
3. **Plantillas — SÍ.** Guardar una licitación como plantilla reutilizable,
   editable al momento de republicar.
4. ~~Invitación restringida~~ — **CANCELADA hasta nuevo aviso** (26-ago-2026: feature no necesaria por el momento). El diseño sobre Guardados queda anotado abajo por si revive.
   Diseño original: **sobre Guardados.** El filtro de empresas ya
   existe: `saved_companies` (migración 019, página /guardados). La licitación
   privada elige destinatarios desde tus guardados (el "follow" estilo
   Computrabajo ya es nuestro Guardar empresa). Conecta a futuro con la
   búsqueda experta IA (5.1): la shortlist desemboca en "invitar a licitación".
5. **Métricas de admin — al final.** Solo para admin; espera a que haya uso
   real.

---

## 10. Contrato de notificaciones (26-ago-2026, acordado con Javi)

Toda notificación nueva del módulo se valida contra esta matriz. Regla de
oro: **la campana es para lo que pasa cuando NO estás viendo** — acciones de
otros y del reloj, jamás confirmaciones de tus propias acciones (eso es
feedback de UI). Y los sombreros no se mezclan: la convocante JAMÁS recibe
notificaciones de proveedor de su propia licitación (candado 037), aunque
sus contraofertas estén activas.

| Evento | Convocante | Ofertantes | Categoría sin ofertar |
|---|---|---|---|
| Se crea la bid | — | — | ✅ invitación (ruteo por categorías, dedupe) |
| Alguien oferta | ✅ "nueva oferta" | — | — |
| Contraoferta | ✅ si va dirigida a ella | ✅ si va dirigida a él | — |
| Por cerrar (≤1h) | — | ✅ una sola vez (dedupe) | — |
| Cerró por tiempo | ✅ "cerró con N — a adjudicar" | ✅ "espera su decisión" | — |
| Revivió | — | ✅ "tu oferta sigue en la mesa" | — |
| Cancelada | — | ✅ "tu oferta queda sin efecto" | — |
| Desierta | ✅ "reábrela con más tiempo" | — | — |
| Adjudicada | — | ✅ ganó (con chat) / ✅ no ganó (sin revelar precio) | — |

Decisiones:
- **Cierre y revivir: SOLO a ofertantes.** El invitado que no entró no tiene
  nada en juego — avisarle es ruido, y el ruido mata campanas.
- **Sin confirmaciones de acciones propias** (extender, cancelar): la UI las
  confirma en el momento. La excepción correcta es el RELOJ: "tu bid cerró"
  sí llega, porque lo hizo el tiempo, no tú.
- **Futuro (asientos, Fase 5):** cuando un compañero de equipo actúe (ej. el
  comprador cancela), los DEMÁS asientos sí se notifican — lo hizo otro.
- **Opción anotada, no construida:** al revivir, reinvitar a los de la
  categoría que no ofertaron ("segunda oportunidad") — tiene lógica de
  negocio; se decidirá cuando haya uso real.
