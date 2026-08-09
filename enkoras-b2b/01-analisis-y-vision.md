# Análisis y Visión — Plataforma B2B de Proveedores

> **Documento fundacional del proyecto.** Aquí está el porqué, el qué, el para quién,
> el mercado, la competencia, los riesgos y la arquitectura conceptual completa.
> Nombre de trabajo del repo: `directorio-b2b` (el nombre comercial se decide al final,
> igual que se hizo con Tu Local).

**Fecha:** Agosto 2026
**Fundador:** Javier Calixto (desarrollo y producto)
**Socio:** promoción, networking y acceso a canales (grupos de WhatsApp empresariales, red de LinkedIn del sector industrial/aduanal de Tijuana)
**Proyecto base:** Tu Local (www.tu-local.com) — directorio de negocios locales de Baja California, ya en producción

---

## 1. Origen de la idea

La idea no nació de un brainstorm — nació de una **observación repetida de demanda real sin plataforma**.

Trabajando en la industria de cruce aduanal (brokerage) en Tijuana, y publicando en LinkedIn contenido sobre un nuevo sistema para el corredor Asia–México, el fundador y su socio notaron un patrón que se repetía todos los días en el feed de LinkedIn:

**Las empresas están usando LinkedIn como un marketplace B2B improvisado.** Dos tipos de publicaciones aparecen constantemente:

1. **Empresas que BUSCAN un proveedor.** Ejemplos reales observados en una sola sesión de navegación:
   - Una empresa buscando proveedor de **etiquetas térmicas** (no 100 ni 200 — un proveedor confiable de suministro continuo)
   - Otra buscando **hojas blancas** (papelería industrial)
   - Otra buscando transportista con **unidades disponibles** (rabones y tráileres) para cruzar mercancía
   - Otra buscando quien hiciera **entries aduanales**
   - Otra buscando **proveedor de alimentos** / servicio de comedor industrial para empresa
   - Una buscando distribuidor local con **existencia inmediata** de componentes Schneider Electric (guardamotor TeSys, contactores), con entrega el mismo día en Tijuana o Mexicali
   - Una buscando proveedor con **disponibilidad de 5,000 etiquetas de diferentes medidas por semana**

2. **Empresas que OFRECEN sus servicios.** Ejemplo real: un transportista publicando que tiene **10 troques disponibles al momento**. Otro (Cortez Transport) publicando sus condiciones comerciales buscando clientes de largo plazo.

El problema con ambos tipos de publicación: **se pierden**. LinkedIn es un feed cronológico social — la publicación del transportista con 10 unidades disponibles desaparece del feed en horas. La empresa que busca etiquetas térmicas recibe respuestas en comentarios y mensajes privados dispersos, sin estructura, sin historial, sin manera de comparar.

**La observación clave:** hay demanda diaria comprobable (decenas de publicaciones al día tan solo en la red de contactos de dos personas), y no existe una plataforma establecida para capturarla. LinkedIn, Indeed y Computrabajo son para empleo. Los directorios industriales existentes son estáticos. El hueco es real.

---

## 2. El problema

### 2.1 Para la empresa que busca proveedor (el comprador)

- **Encontrar un proveedor adecuado toma en promedio 5 semanas** (encuesta Inspectorio 2024 a ejecutivos de procurement).
- Cuando se necesita un proveedor **de emergencia** (una línea de producción parada, un embarque urgente), las opciones actuales fallan:
  - Google devuelve páginas de empresas con **formularios de contacto** que implican mandar correo y esperar días.
  - Los teléfonos publicados están **desactualizados** o son de operadora.
  - Muchos proveedores ni siquiera tienen página web.
  - Nadie publica su **disponibilidad actual** — puedes contactar a 5 transportistas y los 5 estar a capacidad llena.
- Consecuencias documentadas de la lentitud en supplier discovery:
  - 56% reporta **retrasos en proyectos**
  - 50% **excede presupuesto**
  - 43% no logra cubrir demanda
  - 41% **pierde negocios**
  - 40% sufre daño reputacional
- Las cadenas de email para un solo pedido escalan a **50+ mensajes**.
- 45% de los compradores B2B señala **datos incorrectos** como obstáculo principal.
- 36% de los profesionales de procurement señala los **procesos ineficientes** como su mayor desafío.

### 2.2 Para la empresa que ofrece servicios (el proveedor)

- No tiene dónde publicar su **capacidad disponible** de forma que dure y sea encontrable ("tengo 10 troques libres esta semana", "puedo surtir 5,000 etiquetas semanales").
- Su visibilidad depende de publicar repetidamente en redes sociales donde el contenido muere en horas.
- Los directorios estáticos existentes lo listan como una ficha muerta: nombre, teléfono, giro — sin catálogo vivo, sin disponibilidad, sin canal de contacto inmediato.
- Pierde ventas frente a competidores simplemente porque el comprador no supo que existía o no supo que tenía capacidad.

### 2.3 El contexto que multiplica el problema: nearshoring

- México superó a China como **mayor socio comercial de USA**: USD 505 mil millones en exportaciones en 2024.
- La industria maquiladora recibió **USD 9,050 millones** de inversión en activos fijos en 2024.
- Miles de empresas nuevas están llegando a Tijuana, Monterrey y Querétaro **sin redes de proveedores establecidas**. Necesitan encontrar proveedores locales completos: manufactura, ensamble, transporte, aduana, empaque, comedores, uniformes, todo.
- Baja California tiene **956 plantas maquiladoras** (621 en Tijuana — 65% del estado). Tijuana es el cluster de dispositivos médicos más grande de Norteamérica y el mayor polo aeroespacial de México.
- México nacional (IMMEX): **6,523 empresas registradas**, 3.17 millones de trabajadores, +USD 300 mil millones en exportaciones 2024.

La demanda de conexión proveedor-comprador está en su punto histórico más alto, y la infraestructura digital para atenderla no existe.

---

## 3. La solución

Una **plataforma de networking B2B** que conecta empresas que buscan proveedores con empresas que ofrecen productos y servicios, en México primero y USA después.

### 3.1 Los dos actores

**El Proveedor (empresa anunciante):**
- Crea el perfil de su empresa: giro de negocio, descripción, ubicación (estado/ciudad), datos de contacto.
- Registra su **catálogo de servicios**: cada servicio con nombre y descripción (una empresa puede ofrecer 5-6 servicios distintos: transporte + aduanas + almacén, por ejemplo).
- La IA clasifica automáticamente su perfil en el árbol de categorías (ver sección 9).
- Publica y mantiene su **disponibilidad actual**: "10 troques disponibles", "capacidad para 5,000 etiquetas/semana", "2 líneas de producción libres en marzo".
- Recibe **solicitudes** que coinciden con sus categorías y responde por el canal que el comprador prefiera.

**El Comprador (empresa que busca):**
- Busca proveedores por categoría, por texto libre interpretado por IA, por estado/ciudad.
- Ve **quién tiene disponibilidad ahora** — no quién existía hace 6 meses.
- Contacta por la **escalera de contacto**: correo electrónico → teléfono → WhatsApp → chat directo en la plataforma.
- Publica **solicitudes** ("busco proveedor de X con capacidad Y en ciudad Z") que llegan automáticamente solo a los proveedores relevantes.
- Ve reseñas y señales de confianza (verificación RFC, antigüedad, frescura de datos).

### 3.2 La escalera de contacto

El principio: **cero fricción para cerrar el contacto**. En orden de inmediatez:

1. **Correo electrónico** — el canal formal B2B
2. **Teléfono** — para urgencias
3. **WhatsApp** — el canal real donde se hacen negocios en México
4. **Chat directo en la plataforma** — deja historial, genera engagement recurrente, y es el dato que nos convierte en la fuente de verdad de la relación

### 3.3 Geografía

- **Sin límite de zona.** A diferencia de Tu Local (hiperlocal, con código postal), aquí la unidad geográfica es **estado y ciudad**. Una empresa de Aguascalientes que necesita papel kraft puede encontrar y comparar proveedores de CDMX, Morelos o Monterrey — el mejor precio gana sin importar la distancia, porque las empresas cotizan y envían a nivel nacional.
- **Rollout:** Baja California → estados del nearshoring (Nuevo León, Querétaro) → resto de la república → USA.
- **Bilingüe ES/EN desde el día uno.** No es un feature de "fase final" — es primordial. El corredor México-USA es el mercado objetivo y los compradores americanos son los que más pagan.

---

## 4. Diferenciadores

| # | Diferenciador | Por qué nadie más lo tiene |
|---|---------------|---------------------------|
| 1 | **Disponibilidad en tiempo real** | Todos los directorios B2B en México son fichas estáticas. Ninguno responde "¿quién puede atenderme ESTA semana?" |
| 2 | **Contacto multicanal inmediato** | Los directorios dan un teléfono (a veces muerto). Aquí: email + tel + WhatsApp + chat, verificados |
| 3 | **Solicitudes con ruteo inteligente** | La demanda también publica. La solicitud llega solo a proveedores de la categoría correcta — nadie hace esto en México |
| 4 | **Búsqueda semántica con IA** | "Necesito quien imprima 5 mil etiquetas por semana" encuentra al proveedor correcto aunque sus palabras no coincidan |
| 5 | **Frescura como señal de ranking** | El proveedor que actualiza su disponibilidad sube; el abandonado baja. El directorio se mantiene vivo solo |
| 6 | **Bilingüe nativo** | ThomasNet no cubre México; los directorios mexicanos no hablan inglés. El corredor MX-USA queda sin puente |

---

## 5. El mercado (datos verificados, agosto 2026)

### 5.1 México

- B2B ecommerce México 2025: **USD 16.6 mil millones**
- Proyección 2034: **USD 133 mil millones**
- **CAGR 2026-2034: 26%** — el segmento de mayor crecimiento del comercio digital mexicano
- E-commerce general México 2025: 941,000 MDP (+19.2% vs 2024); México es #8 mundial en ventas online
- Dato revelador: los estudios de AMVO (la fuente estadística principal del e-commerce mexicano) **excluyen explícitamente el B2B** — el segmento ni siquiera se mide formalmente. No hay jugador dominante que lo represente.

### 5.2 Latinoamérica

- B2B ecommerce LATAM 2024: **USD 694 mil millones**
- Proyección 2033: **USD 4.77 billones (trillions)**
- CAGR 2025-2033: **23.9%**

### 5.3 USA

- Mercado B2B digital 2025: **USD 10.1 billones (trillions)** — el más grande del mundo
- Amazon Business proyecta USD 83.1 mil millones en 2025 solo en su vertical B2B
- Mercado maduro (CAGR ~2.8%) pero gigantesco — se entra por el nicho del corredor fronterizo MX-USA, no compitiendo de frente

### 5.4 El universo de empresas en México (DENUE/INEGI)

- **6,058,548 establecimientos** registrados (nov 2024)
- Distribución: Comercio 44% | Servicios 42% | Manufactura 11% | Resto 3%
- Manufactura aporta **21.4% del PIB** (7.2 billones MXN)
- Construcción creció 6.8% interanual en 2025; inversión en parques industriales proyectada en USD 5.83 mil millones para 2026

---

## 6. Competencia

### 6.1 Plataformas globales

| Plataforma | Origen | Enfoque | ¿Disponibilidad real? | Debilidad clave |
|---|---|---|---|---|
| ThomasNet | USA | Industrial/manufactura USA | No | Cobertura de México casi nula; sin contacto directo |
| Kompass | Europa | Directorio global B2B | No | Europa-céntrico; se usa como BD de prospección, no como canal activo |
| Alibaba | China | Comercio global Asia | No | Irrelevante para proveedores locales MX/frontera |
| Global Sources | Hong Kong | Manufactura Asia | No | Cobertura de México mínima |
| SoloStocks | España | Marketplace B2B productos | No | Catálogo estático; sin servicios; sin contacto inmediato |

**ThomasNet en detalle** (el benchmark de negocio): USD 89.3 millones de revenue anual, 1.7 millones de usuarios activos mensuales, 500K+ proveedores, 75K categorías. Modelo freemium: compradores gratis, proveedores pagan suscripción por visibilidad y leads. Su existencia y revenue **prueban el modelo de negocio** — y su ausencia total en México prueba el hueco.

### 6.2 Plataformas mexicanas

| Plataforma | Qué es | ¿Disponibilidad real? | Limitación |
|---|---|---|---|
| RedeProveedores | Directorio industrial verificado, +5,000 empresas | No | Ficha estática; sin contacto inmediato |
| MarketB2B.mx | Marketplace B2B mayorista | No | Solo producto físico; sin servicios |
| Cosmos Online | Portal industrial B2B | No | Modelo basado en eventos, no búsqueda activa |
| México B2B | Comunidad digital de alianzas | No | Escala pequeña; sin catálogo ni disponibilidad |
| B2BMarketplace.mx | Cotizaciones electrónicas | No | Commodities; sin tiempo real |
| CAPIM | Big data proveedores-compradores (nearshoring) | Parcial | Enfoque gobierno/corporativo; sin autoservicio PyME |
| Tianguis Digital | Padrón proveedores gobierno CDMX | No | Solo compras públicas |

### 6.3 La brecha confirmada

**Ninguna plataforma en México ofrece:** disponibilidad en tiempo real + contacto inmediato multicanal + solicitudes con ruteo + búsqueda semántica. Todos son directorios estáticos o marketplaces de producto físico. El espacio del **servicio B2B vivo** está vacío.

### 6.4 El competidor real a futuro: la IA generalista

45% de los compradores B2B ya usa IA como método principal de research de proveedores. En 2-3 años, los LLMs competirán por este mismo dolor. **La defensa:** la disponibilidad en tiempo real es un dato que la IA generalista no puede inventar ni conocer — solo lo tiene quien lo captura. La plataforma debe convertirse en la fuente de verdad de ese dato. Si un LLM quiere responder "¿qué transportista tiene unidades libres hoy en Tijuana?", la única fuente será esta plataforma.

---

## 7. Los usuarios y sus casos de uso

### Caso 1 — Urgencia industrial
Una maquiladora de dispositivos médicos en Tijuana se queda sin etiquetas para una corrida de producción que arranca el lunes. Hoy: 20 llamadas, 15 no contestan, 3 no tienen material, pierde el fin de semana. Con la plataforma: busca "etiquetas adhesivas en rollo Tijuana", filtra por disponibilidad inmediata, ve 4 proveedores con stock actualizado hoy, les manda WhatsApp desde la ficha. Resuelto en una hora.

### Caso 2 — El transportista con capacidad ociosa
Un transportista tiene 10 unidades paradas esta semana. Hoy: publica en LinkedIn y grupos de WhatsApp, el post muere en horas. Con la plataforma: actualiza su disponibilidad ("10 rabones disponibles, zona Tijuana-Mexicali"), aparece arriba en los resultados de transporte por frescura, y recibe las solicitudes de carga que coinciden con su categoría.

### Caso 3 — Compra nacional a distancia
Una empresa de Aguascalientes necesita papel kraft; en su ciudad hay un solo proveedor y es caro. Busca en la plataforma, compara proveedores de CDMX, Morelos y Monterrey, contacta a tres, cierra con el mejor precio con envío incluido.

### Caso 4 — El comprador americano (fase USA)
Un fabricante de San Diego busca un proveedor de maquinado CNC en Tijuana para nearshoring. La plataforma en inglés le muestra proveedores verificados con RFC, reseñas, disponibilidad de capacidad y chat directo. El puente MX-USA que ThomasNet nunca construyó.

---

## 8. Features

### 8.1 Confirmados (núcleo del producto)

1. **Árbol de categorías B2B de 3 niveles** — 14 sectores → 94 subcategorías → 410 sub-subcategorías (documento `03-arbol-categorias.md`). Basado en SCIAN/INEGI, DENUE, ThomasNet y Kompass. Un proveedor puede pertenecer a múltiples categorías.
2. **Catálogo de servicios por empresa** — cada empresa registra N servicios (nombre + descripción). El perfil muestra el catálogo completo.
3. **Disponibilidad en tiempo real** — el proveedor publica y actualiza su capacidad actual. Formato flexible por giro (unidades, volumen, capacidad, cupo). Con indicador de frescura visible ("actualizado hace 2 horas" / "hace 3 semanas") y decaimiento en el ranking si no se actualiza.
4. **Escalera de contacto** — email, teléfono, WhatsApp, chat interno. Con tracking de eventos de contacto (ya existe la infraestructura en el repo base).
5. **Mensajería interna** — chat empresa-a-empresa con Supabase Realtime. Historial, notificaciones, y razón para volver a la plataforma.
6. **Notificaciones** — campanita + centro de notificaciones: solicitudes que coinciden, mensajes nuevos, alertas de búsqueda.
7. **Bilingüe ES/EN** — infraestructura i18n desde la fundación.
8. **Cobertura nacional por estado/ciudad** — catálogo geográfico completo de México (después USA).
9. **Rediseño de categorías y cards** — la sección de categorías y las cards de empresa se rediseñan para el contexto B2B (mostrar giro, servicios, disponibilidad, ubicación, verificación).
10. **Monetización** — destacados, prioridad en búsquedas, espacios publicitarios, renta por anuncio (ver sección 11).

### 8.2 Propuestos y aceptados

11. **Tablero de solicitudes (la demanda también publica)** — una empresa publica lo que necesita; la IA la clasifica en el árbol; la solicitud llega SOLO a los proveedores cuyas categorías coinciden. Resuelve el arranque en frío: le da razón de estar a los dos lados del mercado desde el día uno. Reutiliza la infraestructura del feed de Tu Local.
12. **Verificación de empresa con RFC** — badge de verificado real (RFC + datos fiscales), no cosmético. La confianza es la moneda del B2B: sin ella, esto es un Google Maps filtrado.
13. **Frescura de disponibilidad con expiración automática** — la disponibilidad caduca sola si no se refresca; el perfil baja en resultados. Ataca directamente el riesgo #1 del modelo (proveedores que no actualizan).
14. **Alertas de búsqueda guardada** — el comprador guarda "transporte refrigerado Tijuana" y recibe notificación cuando aparece un proveedor con disponibilidad. Retención automática.
15. **IA de clasificación y búsqueda** — ver sección 9.

### 8.3 Contemplados a futuro (no en el arranque)

- Apps nativas iOS/Android (tras 2-3 meses de validación web)
- Cotizaciones estructuradas (RFQ formal con comparativa)
- Historial de relación comercial y proveedores frecuentes
- Métricas avanzadas para proveedores (funnel de contactos)
- Integración con WhatsApp Business API para actualizar disponibilidad por mensaje
- Expansión LATAM y global

---

## 9. Arquitectura de IA

El principio rector: **la IA trabaja una vez al publicar (barato), y la búsqueda opera con matemáticas al leer (gratis y rápido)**. Nunca se llama a un LLM por cada resultado de búsqueda.

### 9.1 Al registrar/publicar una empresa

La empresa captura: **giro de negocio + N servicios (nombre + descripción de cada uno)**.

Con esos 3 datos, una llamada única a la IA:
1. Asigna las categorías del árbol en los 3 niveles (puede asignar varias: un transportista cae en FTL + refrigerada + HAZMAT a la vez)
2. Se genera el **embedding** de cada servicio (vector semántico) y se guarda en la BD

**Por qué embeddings y no keywords:** las keywords solo matchean palabras exactas — "etiquetas de última milla" jamás encontraría al proveedor cuyo servicio dice "impresión de labels para paquetería". Los embeddings matchean por **significado**: ambos textos quedan cerca en el espacio vectorial y el match ocurre. Supabase trae **pgvector** integrado — cero infraestructura adicional, es una columna más.

### 9.2 Al buscar

1. El texto de búsqueda se convierte en embedding (operación barata y rápida, no es una llamada de chat)
2. **Búsqueda híbrida** en una sola query de Postgres:
   - Filtro por categorías (si el usuario navegó el árbol)
   - Full-text search (ya existe en el repo base, migración 003)
   - Similitud vectorial (pgvector)
   - La frescura de disponibilidad pesa en el ranking
3. Resultados ordenados por relevancia real

### 9.3 Al publicar una solicitud

1. La IA lee la solicitud y la clasifica en el árbol (misma operación que un anuncio)
2. Notificación automática **solo a los proveedores cuyas categorías coinciden**
3. El embedding de la solicitud sirve de respaldo para matches que la categoría no captura

### 9.4 Costos de operación

- Clasificación: una llamada por publicación (modelo económico tipo Haiku es suficiente)
- Embeddings: fracciones de centavo por texto, una vez por servicio y una por búsqueda
- Búsqueda: matemática pura en Postgres — sin costo de IA por búsqueda ejecutada

---

## 10. Base técnica — herencia de Tu Local

### 10.1 Lo que se reutiliza (~70% del repo)

- **Stack completo:** Next.js 16 (App Router) + Supabase (Postgres, Auth, Storage, Realtime) + Tailwind 4 + Stripe. Moderno, probado en producción, escala sin cambios.
- **Auth y roles** (usuario, dueño de negocio, admin)
- **Panel admin completo** (negocios, reseñas, usuarios, reportes, dashboard con gráficas)
- **Feed con posts, likes, guardados, reportes** → se transforma en el tablero de solicitudes
- **Planes y pagos con Stripe** → base de la monetización
- **Analytics de eventos** (vistas de perfil, clicks de contacto) → ya trackea phone_click, whatsapp_click, etc.
- **Sistema de fotos con Storage + portada seleccionable**
- **Reseñas con rating**
- **Todo el design system** — la estética refinada de Tu Local (cards, wizard de registro, lightbox, badges, paleta naranja) se conserva y se adapta

### 10.2 Los 4 cambios estructurales

1. **Categorías:** la tabla actual es plana (`id, name, slug, icon`) y cada negocio tiene UNA categoría. Se rediseña a: árbol de 3 niveles con auto-referencia + relación muchos-a-muchos empresa↔categoría + tabla de servicios por empresa (nombre, descripción, embedding).
2. **Geografía:** hoy es texto libre + zip code de BC. Se rediseña a: catálogo de estados y ciudades de toda la república (con soporte futuro para estados de USA).
3. **i18n:** hoy no existe — todos los textos están quemados en los componentes. La infraestructura bilingüe se instala ANTES de construir pantallas nuevas (hacerlo después = tocar cada archivo dos veces).
4. **Módulos nuevos:** disponibilidad (tabla + UI + expiración), mensajería (conversaciones + mensajes + Realtime), notificaciones (tabla + campanita + Realtime), embeddings (pgvector).

### 10.3 Lo que se elimina del repo base

- Categorías de negocio local (comida, belleza, etc.) → reemplazadas por el árbol B2B
- Tipos de servicio local (a domicilio / en el local / para recoger) → reemplazados por conceptos B2B
- Búsqueda por código postal → reemplazada por estado/ciudad
- Referencias a "Baja California" como límite geográfico

---

## 11. Modelo de negocio

### 11.1 Principio

**Freemium para masa crítica, pago por visibilidad y prioridad.** El registro y el perfil básico son gratis SIEMPRE — sin proveedores no hay compradores y sin compradores no hay proveedores. Se cobra por destacar, no por existir.

### 11.2 Líneas de ingreso (en orden de implementación)

1. **Plan premium de proveedor** (suscripción mensual): prioridad en resultados de búsqueda, badge destacado, perfil enriquecido, estadísticas de contactos, más espacios de disponibilidad activa.
2. **Destacados** ("renta por anuncio"): posiciones premium en la página de categoría o en resultados — inventario limitado por categoría/ciudad, lo que crea escasez y precio.
3. **Espacios publicitarios** dentro de la plataforma (banners de categoría, patrocinio de sección).
4. **Futuro — lado comprador enterprise:** alertas avanzadas, listas curadas, historial de proveedores, múltiples usuarios por empresa. (Los compradores corporativos son quienes más pagan por eficiencia — benchmark del reporte de viabilidad.)

### 11.3 Benchmark

ThomasNet factura **USD 89.3M/año** con este mismo modelo (freemium proveedor + suscripción por visibilidad) en un mercado más maduro. México está una década atrás en digitalización B2B — la ventana está abierta.

### 11.4 Realismo de adopción

El mercado mexicano tiene resistencia a pagar por software B2B. La curva de adopción de pago se estima en 12-18 meses. La estrategia: crecer masa crítica gratis, monetizar cuando el canal demuestre que genera contactos reales (el proveedor paga cuando ve que le llegan clientes).

---

## 12. Riesgos y mitigaciones

| # | Riesgo | Mitigación |
|---|--------|------------|
| 1 | **Huevo y gallina** — sin proveedores no hay compradores y viceversa | El tablero de solicitudes le da valor a ambos lados desde el día 1. Arranque geográfico concentrado (BC) donde el socio tiene red directa (grupos de WhatsApp empresariales, LinkedIn industrial). Seed manual de proveedores clave invitados |
| 2 | **Proveedores que no actualizan su disponibilidad** — muere el diferenciador | UX de actualización en 2 clics desde el celular; expiración automática con decaimiento en ranking; notificaciones de recordatorio; futuro: actualizar por WhatsApp |
| 3 | **IA generalista comiéndose el discovery** | Ser la fuente de verdad del dato que la IA no tiene: disponibilidad en tiempo real. Ese dato solo existe si alguien lo captura |
| 4 | **Monetización lenta en México** | Costos de operación mínimos (stack serverless, IA solo al publicar); el proyecto no depende de levantar capital; runway personal |
| 5 | **Confianza / proveedores fantasma** | Verificación RFC desde el inicio; badge de verificado real; reseñas; señales de actividad visibles |
| 6 | **Entrada al mercado USA** | No se compite de frente con ThomasNet: se entra por el nicho del corredor fronterizo MX-USA (compradores americanos buscando proveedores mexicanos), que ThomasNet no cubre. Bilingüe nativo desde día 1 lo hace posible sin re-arquitectura |

---

## 13. Visión

**Corto plazo:** el lugar donde las empresas de México encuentran proveedores con disponibilidad real y los contactan en minutos, no semanas.

**Mediano plazo:** el puente B2B del corredor México-USA — el dato de "quién puede atenderme ahora" del nearshoring.

**Largo plazo:** la meta (no el sueño): plataforma multi-país. Buscar un día un proveedor y encontrar opciones en China, España, USA o Japón — conectar empresas entre países con la misma facilidad que hoy se busca un restaurante. Pero primero caminar: México. Luego correr: USA. Luego volar.

---

## 14. Fuentes

- IMARC Group — Mexico B2B E-Commerce Market Size and Forecast to 2034
- Straits Research — Latin America B2B eCommerce Market
- Capital One Shopping — B2B eCommerce Statistics 2025
- INEGI — SCIAN 2023, DENUE noviembre 2024 / mayo 2025
- ThomasNet — Browse Categories / TopBubbleIndex — ThomasNet Business Model
- Kompass Global B2B Directory
- Inspectorio 2024 — Supplier Discovery Survey / Veridion — Supplier Discovery Challenges
- B2BE — Procurement Challenges 2024
- Trax Tech — 66% of B2B Buyers Now Use AI For Supplier Research
- Incomex — Tijuana concentra el 65% de la industria maquiladora de BC
- TACNA — Manufacturing in Tijuana
- Forbes México — Inversión maquiladora por nearshoring 2024
- Biz Latin Hub — México superó a China como socio comercial de USA
- AMVO — Estudio sobre Venta Online en México 2025
- Mondu — Monetization of B2B Marketplaces
- Mexico Industry — Manufactura y construcción 33.7% del PIB
