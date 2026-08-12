# ALTAM — Análisis completo del scope (board de Plaky)

> Lectura completa de las 65 tarjetas del board **ALTAM** (Plaky, space "Enkoras
> Projects" 229278, board 285660), con TODOS sus comentarios. Scope preparado por
> **David (COO)** el 2026-08-11 para Javi (CTO). Este documento es mi análisis de CTO
> sobre ese scope. 9-ago-2026.

---

## 0. Qué es ALTAM y cómo se relaciona con lo que ya investigamos

**ALTAM = el cliente de Enkoras** (la empresa para la que construimos). ALTAM es un
**operador de reparto de última milla**: recibe paquetes de una bodega ("el cliente de
ALTAM"), los segrega por ruta en su almacén, los asigna a choferes y los entrega. El
sistema a construir es su **plataforma de escaneo, reparto, tracking y flotilla**.

**Observación clave (a confirmar con Fer/David):** el módulo estrella "Integración API
con Cliente" describe un cliente upstream que **entrega un manifiesto matutino con rutas,
tracking numbers y geolocalización ligados a la etiqueta/label** — exactamente el tipo de
sistema que investigamos en iMile (labels con código de estación/ruta, tracking, geo).
Es muy probable que **el "cliente de ALTAM" sea iMile (o un operador equivalente), y que
toda nuestra investigación del API de iMile ALIMENTE directamente el Módulo 2.1**. No lo
afirmo como hecho — lo dejo señalado para que el equipo lo confirme, porque cambia de
dónde sale el manifiesto.

**Estructura del board:** 65 tarjetas =
- **1** tarjeta maestra (el reporte de definición completo de David, grupo 01).
- **26** tarjetas de *definición* (una por sub-feature) — cada una con su **Background**
  en comentarios (el porqué de la decisión).
- **38** tarjetas de *subtarea técnica* — tituladas `[Backend]` / `[App Móvil]` /
  `[Panel Web]`, sin comentarios (son el desglose de implementación por capa).

**Lo más importante que revela la estructura:** el sistema tiene **tres componentes**
(esto define el stack): un **Backend/API**, una **App Móvil del chofer**, y un **Panel Web
de supervisor/gerencia**.

---

## 1. El flujo operativo actual (as-is) y sus problemas

**Cómo opera ALTAM hoy:**
1. Una bodega (el cliente de ALTAM) le entrega los paquetes.
2. En su almacén hacen **segregación visual**: leen la ruta impresa en la etiqueta y
   colocan cada paquete en un **gaylord** (contenedor) identificado con esa ruta. Manejan
   **+10 rutas al día**.
3. Los choferes llegan; el **líder asigna ruta de memoria** según la experiencia del
   chofer. **No hay catálogo de choferes.**
4. El chofer **escanea uno por uno** con su celular cada paquete de su gaylord.
5. Va por su auto, carga la cajuela, sale a ruta. **Tarda en promedio 45 min** desde que
   llega hasta que sale.
6. En ruta a veces no puede entregar (nadie salió, dirección no encontrada, inaccesible).
7. Al final del día solo ven un **"logrado/no logrado"** del % de reparto (meta **>85%**).

**Los 7 problemas que el sistema debe resolver:**
1. Las incidencias de no-entrega **no se actualizan en tiempo real**.
2. **Se pierde la visibilidad del chofer en el mapa** (el ícono deja de moverse) mientras
   los paquetes se siguen marcando entregados — **no hay forma de verificar** si es falla
   de conexión o si el chofer no está donde dice. ← *el dolor central.*
3. **No hay check-in/check-out de descansos** (avisan por mensaje informal).
4. El escaneo se registra pero **no se muestra en tiempo real** a nadie que pueda actuar.
5. **No hay dashboard** de performance ni de incidencias.
6. **No hay certeza** de que las rutas asignadas sean las más eficientes.
7. **No se mide combustible ni kilometraje** por unidad.

---

## 2. Los 9 módulos to-be (con sus decisiones de diseño)

### 2.1 Integración API con Cliente  *(grupo 06 — la columna vertebral)*
Integración **bidireccional** con el sistema del cliente upstream, desde cero:
- **Entrante:** manifiesto matutino con rutas + tracking numbers + geolocalización (el
  cliente confirmó que la etiqueta ya trae ruta y geo ligadas al tracking en su sistema).
- **Saliente:** al asignar ruta se informa a qué chofer quedó cada tracking; luego
  entregas e incidencias.
- **Capas:** `[BE]` modelo de manifiesto · `[BE]` endpoint de ingestión · `[BE]` servicio
  de envío de estatus (cola de eventos salientes).
- **Pendiente con cliente:** contrato/documentación del API, formato exacto del
  manifiesto, frecuencia del estatus (tiempo real vs. batch).
- **Nota mía:** este módulo **depende del cliente** y es el que más riesgo externo tiene.
  Es también donde encaja toda la investigación de iMile.

### 2.2 Recepción y Segregación — Sistema de Bins  *(grupo 09)*
Digitaliza la segregación visual. Se numera un **bin/ruta** por gaylord: se escanea el
bin (asocia bin↔ruta) y luego cada paquete al llegar, **verificando contra el manifiesto**
(correcto / faltante / sobrante) como control de calidad. Con el manifiesto matutino se ve
**saturación de rutas desde temprano**.
- **Capas:** `[BE]` modelo bin↔ruta · `[Móvil]` alta/escaneo de bin · `[BE]` verificación
  contra manifiesto · `[Móvil]` escaneo con feedback ok/error · `[Web]` vista de saturación.

### 2.3 Catálogo de Choferes y Unidades  *(grupo 05)*
Catálogo formal (perfil, login, historial) **aunque la asignación de ruta la siga haciendo
el líder a mano** — no se automatiza esa decisión. El catálogo existe para poder **ligar**
GPS, fotos de combustible, incidencias y desempeño a cada chofer/unidad de forma
persistente. Unidades identificadas con **placas/número económico existente** (sin crear
numeración nueva).
- **Capas:** `[BE]` modelo chofer + auth/login · `[Web]` CRUD choferes · `[BE]` modelo
  unidad · `[Web]` CRUD unidades.

### 2.4 Asignación de Ruta y Carga  *(grupo 04)*
Elimina la redundancia del escaneo paquete-por-paquete: como los paquetes ya quedaron
asociados a gaylord/ruta en la segregación, el chofer **escanea UNA vez la etiqueta del
gaylord** y el sistema **auto-asigna todos los tracking numbers** de ese gaylord a él +
dispara el estatus al cliente por API. Ataca directo los **45 min** actuales, midiendo
**checkpoints** (llegada / escaneo / salida) como KPI.
- **Capas:** `[Móvil]` escaneo único de gaylord · `[BE]` auto-asignación + trigger API ·
  `[BE]` timestamps por checkpoint · `[Web]` reporte de tiempos por chofer.

### 2.5 Reparto y Captura de Incidencias  *(grupo 03)*
**Catálogo de incidencias extensible.** El chofer captura **foto al entregar** y **foto +
nota + motivo** cuando no puede entregar. Paquete **se reintenta al día siguiente**.
Problema puntual resuelto: si un paquete se segregó en la ruta equivocada, al día
siguiente lo reintentan en la misma ruta mal → se agrega **pantalla de supervisor para
corregir la ruta antes del reintento**.
- **Capas:** `[BE]` catálogo configurable · `[Móvil]` foto de entrega · `[Móvil]` foto +
  nota de no-entrega · `[BE]` job de reprogramación · `[Web]` corrección de ruta · `[BE]`
  reasignación a otra ruta/gaylord.

### 2.6 Tracking en Tiempo Real y Verificación  *(grupo 10 — el dolor central)*
Resuelve el problema #2. **GPS desde el celular del chofer** (no dispositivo dedicado).
La **confirmación de entrega (foto) captura la geolocalización** y la compara contra la
**dirección esperada** → así se verifica de verdad que estuvo ahí. Estatus formal de **"en
descanso/comida"**. Explícito en el scope: **todo vive dentro de la app nueva — reemplaza
cualquier plataforma de tracking externa, sin integración con terceros para esto.**
- **Capas:** `[Móvil]` GPS en background · `[BE]` ingestión de ubicación · `[Móvil]` geo
  al momento de la foto · `[BE]` comparación geo vs. dirección · `[Móvil]` botón descanso ·
  `[Web]` reflejo en mapa/dashboard.
- **Pendiente con Javi:** radio de tolerancia GPS válido y frecuencia de reporte.

### 2.7 Optimización y Predicción de Rutas  *(grupo 02)*
Como el manifiesto llega temprano por API, el sistema **predice rutas sobrecargadas** al
inicio del día (para pedir más choferes a tiempo) y **recomienda ruta óptima**. Las
direcciones ya vienen **geolocalizadas** ligadas al label → **no hay que geocodificar**.
- **Capas:** `[BE]` predicción de sobrecarga · `[BE]` integración con motor de ruteo
  (*pendiente elegir cuál*) · `[Web]` vista de ruta sugerida.
- **Pendiente con Javi:** qué motor de ruteo (Google Directions/Distance Matrix vs. propio)
  y criterios de "sobrecarga".

### 2.8 Consumo de Combustible y Mantenimiento  *(grupo 08)*
Foto de **tanque y odómetro por unidad** (no por chofer) al inicio y fin del día. El
sistema **lee las fotos con OCR/visión**, calcula **eficiencia de combustible** y
**calendariza mantenimientos por kilometraje**. Objetivo de fondo declarado: detectar
mantenimientos **y también usos indebidos de la unidad** fuera de la ruta.
- **Capas:** `[Móvil]` captura de fotos · `[BE]` OCR/visión · `[BE]` cálculo de eficiencia ·
  `[BE]` job de próximo mantenimiento por km · `[Web]` vista de mantenimientos próximos.
- **Pendiente con Javi:** umbral de "eficiente" (km/litro o costo/km) y reglas de
  mantenimiento.

### 2.9 Dashboard de Resultados  *(grupo 07)*
Reemplaza el "logrado/no logrado" por un **dashboard en tiempo real** (no corte diario)
para supervisor y gerencia:
- **KPIs de reparto:** entregas vs. meta 85%, tiempo de almacén por chofer, incidencias.
- **KPIs de flotilla:** combustible, mantenimientos próximos, rutas sobrecargadas.
- **Capas:** `[Web]` vista de reparto · `[Web]` vista de flotilla.

---

## 3. La arquitectura implícita (lo que el scope ya decidió sin decirlo)

El desglose por capa deja claro que hay que construir **tres piezas**:

| Componente | Para quién | Qué hace |
|---|---|---|
| **Backend / API** | interno | modelos, endpoints, jobs, colas, OCR, ruteo, la integración con el cliente. El cerebro. |
| **App Móvil (chofer)** | choferes | escaneo (bin, gaylord, paquete), fotos (entrega, no-entrega, tanque/odómetro), GPS en background, botón descanso. |
| **Panel Web (supervisor/gerencia)** | oficina | CRUDs, dashboards en tiempo real, corrección de rutas, saturación, reportes. |

Esto es lo que más pesa para decidir el stack (ver §6).

---

## 4. Los pendientes abiertos (del propio scope)

**Con el CLIENTE (bloquean el módulo 2.1 y dependientes):**
- Contrato/documentación formal del API del cliente.
- Formato exacto del manifiesto matutino.
- Frecuencia del estatus saliente (tiempo real vs. batch).

**Con JAVI / equipo técnico:**
- Radio de tolerancia GPS y frecuencia de reporte (módulo 2.6).
- Motor de ruteo y criterio de "sobrecarga" (módulo 2.7).
- Umbral de "eficiente" en combustible y reglas de mantenimiento (módulo 2.8).

---

## 5. La pregunta que David le hace a Javi (⚠️ requiere respuesta)

> **"¿Cuánto costará solventar esta app en servidores, cuentas y servicios de
> infraestructura?"** — necesitan **costo de arranque** (una vez) y **costo mensual
> recurrente** para validar el modelo de negocio ANTES de comprometerse con el cliente.

Rubros que David pide considerar: hosting backend (+ staging), base de datos,
almacenamiento de fotos, OCR/visión, motor de ruteo/mapas, push/mensajería, cuentas de
desarrollador (Apple/Google Play), dominio + SSL, monitoreo/logs/backups, y cualquier otro
servicio de terceros.

*(Mi propuesta de respuesta a esta pregunta va aparte — es la siguiente tarea.)*

---

## 6. Mi lectura como CTO (lo que veo del scope)

**Lo bueno:** el scope está **excepcionalmente bien hecho** — David separó el *qué/por qué*
(definición con background) del *cómo* (subtareas por capa). Cada decisión trae su
justificación operativa. Rara vez llega un scope así de limpio; se puede construir sobre
esto sin adivinar.

**El tamaño real:** esto **no es "un sistema"** — son **tres** (backend + app móvil +
panel web) con 9 módulos funcionales. Es un producto de logística completo, no un MVP de
fin de semana. El "entregar en ~1 mes" que se había hablado hay que **recalibrarlo**: en un
mes se puede entregar un **núcleo que enamore** (segregación + escaneo de gaylord +
tracking GPS + dashboard básico + la integración de manifiesto), pero los 9 módulos
completos con OCR y optimización de rutas es más. Vale la pena proponer un **corte por
fases** (MVP que enamora → resto).

**Los 3 focos de riesgo técnico:**
1. **App móvil nativa vs. PWA.** GPS en background + cámara + escaneo de código de barras
   apuntan a **app instalable**. Con GPS en *background* real, una PWA se queda corta
   (iOS restringe el background). Probable **app nativa/Expo** → implica cuentas de
   Apple/Google Play (dato para el costeo). Es el mayor cambio vs. la fórmula web de Enkoras.
2. **Motor de ruteo / "optimización".** "Ruta óptima" y "predicción de sobrecarga" pueden
   volverse un pozo sin fondo (VRP es problema de doctorado). Hay que acotar con David a
   algo entregable: orden de paradas + heurística de carga por ruta, no un optimizador
   perfecto. Ya viene marcado como pendiente — bien.
3. **OCR de tanque/odómetro.** Leer un odómetro de foto es viable con visión (Gemini/Cloud
   Vision), pero la exactitud del tanque es dudosa. Proponer capturar el número a mano con
   la foto como respaldo, al menos en v1.

**La convergencia con nuestra investigación:** el módulo 2.1 (manifiesto entrante + estatus
saliente por API) es **literalmente** lo que estuvimos investigando de iMile — endpoints,
formatos, el debate del API oficial vs. exports. Si el "cliente de ALTAM" es iMile (o su
ecosistema), **ya tenemos meses de ventaja** en ese módulo. Confirmarlo es la pregunta #1.

**Recomendación de stack (a ratificar):**
- **Backend + Panel Web:** la fórmula probada de Enkoras — **Next.js + Supabase** (Postgres
  con PostGIS para geo, Auth/RLS por rol, Realtime para el mapa/dashboard en vivo, Storage
  para las fotos). Multi-tenant desde el día 0.
- **App Móvil:** **React Native / Expo** (comparte lenguaje y modelo mental con el web;
  soporta cámara, escaneo de barcode, GPS background, push). Es la pieza genuinamente nueva.
- **OCR/visión:** Gemini (ya lo dominamos de Enkoras).
- **Ruteo:** Google Maps Platform para v1 (sin reinventar).

---

## 7. Referencia — mapa de tarjetas del board

- **01 Resumen** (1): [7095139] reporte maestro de David.
- **02 Optimización y Predicción de Rutas** (5): 2 def + 3 tech.
- **03 Reparto y Captura de Incidencias** (11): 5 def + 6 tech.
- **04 Asignación de Ruta y Carga** (6): 2 def + 4 tech.
- **05 Catálogo de Choferes y Unidades** (6): 2 def + 4 tech.
- **06 Integración API con Cliente** (6): 3 def + 3 tech.
- **07 Dashboard de Resultados** (4): 2 def + 2 tech.
- **08 Consumo de Combustible y Mantenimiento** (9): 4 def + 5 tech.
- **09 Recepción y Segregación / Bins** (8): 3 def + 5 tech.
- **10 Tracking en Tiempo Real y Verificación** (9): 3 def + 6 tech.

Total: **65 cards** (1 maestra + 26 definición + 38 subtareas técnicas).
