# Endpoints internos del sistema DS — capturados del tráfico real (vía 2)

> Análisis del network export que Javi capturó con su extensión de DevTools navegando
> **ds.imile.com** el 9-ago-2026 (308 requests). Es la **vía 2** del research de API
> (`03-research-api-imile.md`): inspeccionar los endpoints internos que la interfaz web
> consume, para conocer las formas del dato y evaluar integración directa.
> **Tokens y datos personales REDACTADOS** de este documento (el export original con el
> token vivo NO se sube al repo).

---

## Lo que se descubrió, en corto

1. **DS es una SPA (Vue) que habla con una API REST JSON** en `ds.imile.com` — misma
   estructura que tendría cualquier API moderna.
2. **La autenticación es un Bearer token** en el header `Authorization: Bearer <token>`
   (y también en la cookie `ACCESS_TOKEN`). El token es de sesión (opaco, UUID).
3. **Todas las respuestas usan el mismo sobre**:
   `{ status, resultCode, resultObject, message, traceId, requestToken }` — `resultObject`
   trae el dato. Estándar, fácil de consumir.
4. **El árbol COMPLETO de módulos del DS** viene en una sola llamada
   (`/permission/resource/auth/user-order-resource`) — es el mapa de todo lo que el
   sistema puede hacer (ver sección de módulos abajo).
5. Hay **8 subdominios** de iMile en juego, no solo ds.imile.com.

## Qué significa "DS" (y qué NO)

- **DS = Directly Operated Station** (estación de operación directa; 直营网点). Está literal
  en el Reglamento v1.3: *"CSP (Channel Service Partner) y DS (Directly Operated Station)"*.
  En jerga de paquetería, la **estación de entrega local** (la bodega de última milla). Se
  contrapone a CSP (estación operada por un socio de canal). La cuenta capturada opera la
  estación **TIJ-LAS HUERTAS.DS** (`ocType/stationType: DS`).
- **"Delivery Services" ≠ el DS del portal.** "Delivery Services" es el nombre de la EMPRESA
  (iMile Delivery Services LLC / MÉXICO) — también su marca de reclutamiento
  (imille-delivery-services.pandape.computrabajo.com). No es lo que significa el acrónimo DS.

## El login (ds-login.imile.com) — export de 14 req

- SPA aparte, proyecto **`ds-web-login`**. Portal INTERNO de empleado/proveedor (no público).
- Métodos de entrada: cuenta+contraseña, **QR**, **SSO Feishu/Lark**, **WeCom (企业微信)**,
  2FA por correo. Textos tipo "contacta a RH", "vincula tu WeCom".
- Confirma que **iMile es empresa de gestión china** (Lark/WeCom = ByteDance/Tencent) — por
  eso todo viene trilingüe con chino. Refuerza que el DS es portal operativo de personal, no
  API pública → el API oficial sigue siendo la vía limpia de integración.
- Sin hallazgos nuevos sobre restricciones de acceso ni API (es puro flujo de auth).

## Identidad de la cuenta capturada (la que compartió Fer)

Del endpoint `/ucenter/system/checkToken` (perfil del usuario logueado):

- **Usuario:** Jose Alberto Serrano Altamirano · `userCode: 2106128801` (la cuenta DS que dio Fer)
- **Estación:** `TIJ-LAS HUERTAS.DS` · `ocCode: S210744001` · `country: MEX` · `currency: MXN`
- **Tipo:** `userType: OWN` · `vendorCode: SRM2600773` · `orgId: 10` · `status: ACTIVE`
- Es un proveedor **DS** (Directly-operated Station) de Tijuana. Una sola estación en sus permisos.

## Autenticación (cómo se entra a la API)

```
Authorization: Bearer <TOKEN_DE_SESION_REDACTADO>
Cookie: ACCESS_TOKEN=Bearer%20<...>; UserInfo={...}; LANG=en_US
x-page-path: https://ds.imile.com/#/DSOperation/OperationManagement/arrivalScan
```

- El token es **de sesión** (se renueva al hacer login; no es una API key permanente).
  Para integrar de forma estable conviene: (a) las **API keys oficiales** que da iMile
  (ver doc 03 — la vía formal), o (b) automatizar el login para obtener el token.
- `/ucenter/system/checkToken` valida el token vigente.

## Subdominios / hosts observados

| Host | Rol aparente |
|---|---|
| **ds.imile.com** | El sistema DS (la API principal — 60 req) |
| ds-slave.imile.com | Réplica/lectura de DS |
| **ds-waybill.imile.com** | Servicio de guías (waybill) |
| fms.imile.com | FMS (Fleet/First-Mile management) |
| smart-logistics-slave.imile.com | Logística inteligente (rutas) |
| prism-mono.imile.com | Reporting/BI ("prism") |
| attendance-miniapp.imile.com | Asistencia de personal (no relevante) |
| fe-apm.imile-inc.com | Telemetría del front (ruido, ignorar) |

## Endpoints de API observados (los reales, sin ruido)

Base: `https://ds.imile.com`. Sobre de respuesta común descrito arriba.

| Método | Endpoint | Para qué |
|---|---|---|
| POST | `/lm/express/ops/v1/biz/pending/arrival/searchArrive` | **Búsqueda de arribos pendientes** (el monitor de inventario/arrival) |
| GET | `/lm/express/base/v1/search/auth/permission/station` | Estaciones a las que el user tiene acceso (devolvió TIJ-LAS HUERTAS) |
| GET | `/lm/express/base/v1/search/auth/permission/country` | Países del user (MEX, con zona horaria, moneda) |
| GET | `/lm/express/ops/v1/biz/inbound/isBagNumberAllowed` | Regla de negocio (¿permite nº de bolsa?) |
| POST | `/lm/express/ops/biz/config/getHideWeightTabCountry` | Config de UI por país |
| POST | `/lm/express/ops/biz/config/supportScanReferenceNoCountries` | Config de escaneo por país |
| POST | `/saastms/biz/backToWarehouse/getNextWorkDay` | Siguiente día hábil (devolvió `2026-08-10`) |
| GET | `/permission/resource/auth/user-order-resource` | **Árbol completo de módulos/permisos** (el mapa del sistema) |
| POST | `/hermes/perm/privacy/auth/getUserAuthList` | Lista de autorizaciones de privacidad del user |
| POST | `/ucenter/system/checkToken` | Valida token + devuelve perfil del user |
| GET | `/hermes/configGlobal/getByGroupAndKey` | Config global (por group/key) |
| GET | `/resource/user/menu/favorites/ordered-menu` | Menú favoritos del user |
| POST | `/resource/user/menu/history/recently-list` | Historial reciente del user |
| GET | `/resource/navigation/preferences/user-preferences` | Preferencias de navegación |
| POST | `/translator/i18n/project/flat/config` | Traducciones i18n |
| POST | `/oa/user/payment/checkUserAccount` | Chequeo de cuenta de pago |

**Prefijos de la API** (namespaces): `/lm/express/...` (operación last-mile — el core),
`/saastms/...` (TMS), `/permission/...` y `/resource/...` (menús/permisos), `/hermes/...`
(config/privacidad), `/ucenter/...` (usuarios/token), `/translator/...` (i18n).
El namespace **`/lm/express/ops/...`** es el que trae la data operativa que nos interesa.

> Nota: esta captura fue navegando pocas pantallas (arribo/arrivalScan); hay muchísimos más
> endpoints — uno por cada módulo de la lista de abajo. Navegando cada módulo con la
> extensión se levanta el catálogo completo de la API.

## El MAPA COMPLETO del sistema DS (árbol de módulos del user)

Esto es lo más valioso para el scope: **todo lo que el DS puede hacer**, tal cual lo
declara el propio sistema para esta cuenta. Los ⭐ son los directamente útiles para
rastreabilidad.

### 📁 Operation (operación diaria de la estación)
- DS Operation Dashboard
- **Waybill Mgmt.**: ⭐ Waybill Query 2.0, Change Waybill, ⭐ Tracking, Label Reprint
- **Dispatch**: ⭐ DA OFD Monitor
- **Delivery**: ⭐ OFD Inventory, ⭐ Delivered List, Assign DA, Delivered Scan
- **Outbound**: Loading scan, Outbound Scan
- **Waybill management**: Waybill Input, Tracking, Label print, Shipping/Dispatch waybill query, ⭐ Waybill key timeline
- **Operation management**: Arrival (AWB), Pick up & Arrival, Offloading & Arrival, Arrival, Collect&Send, Offloading, Unlock Vehicle, Consolidate(Bag), Pending departure/loading, FM Consolidate, Rc Package Mgmt, **Back To DS Scan**, FM Loading
- **Dispatch management**: Return signing, Pick up, Assign DA, Return Dispatch, ⭐ Out for delivery, Delivered scan, Pre-allocation, Pending tasks, Assign Pickup DA
- **Inventory management**: Shelf/Pull-off/Inventory scan, Returned items Destroyed, PUDO Inventory
- **Scan management**: ⭐ Scan query
- **Intelligent Final Delivery**: ⭐ Route Planning, Route Config, ⭐ In-Transit Monitoring, Road network data
- **3PL Dispatch Management**: AWB Reprint (Vendor)
- **Return Management**: RTC Delivery Scan, RTC Pod

### 📁 Monitor (⭐⭐ el corazón para un sistema de rastreabilidad)
- **KPI**: ⭐ D0 Arrival Rate
- ⭐ **Inventory Workbench**
- **Inventory**: ⭐ In-Hub Inventory, ⭐ In Transit Inventory, ⭐ OFD Inventory
- **Operation Monitor**: ⭐ Arrival Monitor, ⭐ Discrepancy Monitor, ⭐ **Inventory Monitor** (el link de Fer), ⭐ Dispatch Monitor, Delivered Preview, ⭐ **Timeliness Report**, Return Center SLA, ⭐ Pending Task, ⭐ **Control Tower**, ⭐ **Overdue without delivered** (backlog), Collection schedule, ⭐ Prediction of arrived pieces, Station Operation Monitoring, ⭐ **Alert Report**, ⭐ Real Time Inventory Monitor, Error Consolidate Report, ⭐ DA Monitor
- ⭐ **Driver Monitoring (Map)** ← el mapa de repartidores en tiempo real YA EXISTE en DS
- ⭐ DS Task Dashboard, ⭐ Task To Be Delivery, Arrival Monitor New

### 📁 Service (tickets y servicio)
- **Return management**: Received/Initiated Return
- **Service quality**: ⭐ Problem management, Order interception, My Ticket (deprecated), Pending tasks, Ticket Query (deprecated)
- **Headless Piece**: Headless, Claim Headless Pieces
- ⭐ **Ticket management**: Ticket Workbench, Ticket Aggregation Query, Complaint Ticket, Arbitration Ticket, Normal Ticket, Create Ticket

### 📁 Order management
- LM Task Allocation
- **FM Data Monitoring**: ⭐ **SHEIN Feedback Data**, FM Packet Scan List
- LM order management (MEX)

### 📁 Basic config
- Authority Mgmt: Organizational/Staff/Driver/Role Management
- Basic Config: Station Coverage Area, Agent Coverage Area, Warehouse Management

### 📁 Finance (existe en DS, pero fuera de scope por decisión)
- Station Finance: Account Management, BillManagement (nation flow, State Bill)

### 📁 TransportManage (flota y rutas)
- basic info: ⭐ Vehicle management, Trunk/Branch Driver
- Route Planner / Schedule Manage (LM/FM, Trunk/Branch)
- Branch Dispatch Management
- **Transport Monitor**: LM/FM Branch Monitor Report, Segment Report, ⭐ **Sent But Not Received Data**

---

## Implicaciones para el proyecto (grandes)

1. **La integración directa por API es TÉCNICAMENTE VIABLE.** El DS es una API REST JSON
   normal con sobre estándar y auth por Bearer. Independientemente de si iMile da keys
   oficiales (doc 03), los endpoints existen y responden. **La vía 2 quedó confirmada.**
2. **El "mapa de repartidores en tiempo real" NO hay que construirlo — iMile ya lo tiene**
   (`Driver Monitoring (Map)` + `In-Transit Monitoring` + `Real Time Inventory Monitor`).
   Cambia la conversación del lunes: el valor de nuestro sistema no es re-hacer el mapa,
   es **consolidar/cruzar/alertar** sobre datos que hoy viven dispersos en 20+ pantallas.
3. **El dolor real que resuelve nuestro sistema se ve clarísimo aquí**: el DS tiene
   MUCHÍSIMAS pantallas de monitoreo separadas (Arrival, Inventory, Dispatch, Discrepancy,
   Timeliness, Backlog, Alert, Control Tower...). El cliente tiene que entrar a cada una.
   **Nuestro producto = una capa que unifica lo que importa + alertas + el cruce con
   penalizaciones/dinero** que el DS no da junto.
4. **SHEIN Feedback Data** confirma el vínculo con Shein a nivel dato (relevante para el
   proyecto 2 de la escalera).
5. **La ingesta puede arrancar por estos endpoints** (con el token de sesión) mientras se
   tramitan las keys oficiales — sin depender de exports manuales. Diseño por adaptadores:
   un adaptador "DS-web-API" además del "exports" y el futuro "API oficial".

## Siguiente paso técnico (cuando llegue el user de desarrollo de Fer)

Navegar con la extensión, módulo por módulo del árbol de arriba (empezando por
**Inventory Monitor, Tracking, Waybill Query, Timeliness Report, Control Tower**), para
capturar sus endpoints y las formas exactas de sus respuestas (request body + columnas del
resultObject). Con eso se arma el contrato de datos real del sistema — la base del modelo
de datos de nuestro producto.

---

*Fuente: network-export-308req.json (extensión DevTools de Javi, ds.imile.com, 9-ago-2026).
El archivo original con token vivo y datos personales se mantiene FUERA del repo. Ver
también docs 02 (documentos del cliente) y 03 (research del API oficial).*
