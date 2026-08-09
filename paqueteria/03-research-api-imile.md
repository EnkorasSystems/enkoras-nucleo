# Research — El API de iMile: qué existe, de dónde salen las keys y cómo pedirlo

> Investigación web del 9-ago-2026, a raíz de la afirmación del cliente de que "el
> contrato no permite conectarse por API" (sin poder citar en qué documento). Complementa
> a `02-hallazgos-documentos-proveedores.md` (donde ya se verificó que **ninguno de los
> 16 documentos del cliente menciona ni prohíbe APIs**).
> **Regla vigente: ausencia de mención ≠ prohibición. Nada se cierra sin evidencia
> explícita o decisión de socios.**

---

## Resumen en 4 líneas

1. **iMile NO tiene portal público de desarrolladores** (no hay "regístrate y saca tu key").
2. **El API SÍ existe** — múltiples integradores comerciales ya están conectados a él.
3. **Las keys y la documentación las entrega el equipo de iMile bajo solicitud directa.**
4. **iMile VENDE oficialmente la integración por API en su página — con México en cobertura.**

---

## 1. No existe portal público de developers

- `open.imile.com` → el dominio **no resuelve** (no existe).
- En `imile.com` no hay sección de developers, docs de API ni generación de keys.
- No hay specs OpenAPI ni colecciones Postman públicas de iMile.

Es decir: no es autoservicio. Pero eso solo describe el *cómo se tramita*, no una prohibición.

## 2. El API existe — evidencia de integradores conectados

Empresas que ya operan contra el API de iMile y lo documentan públicamente:

| Integrador | Qué muestra | URL |
|---|---|---|
| **ClickPost** | Lista las APIs de iMile que consume: **generación de guías/etiquetas (manifest), tracking, cancelación, EDD (fecha estimada), NDR actions (intentos fallidos), webhooks de retornos**. "You enter your iMile credentials in the dashboard" | https://www.clickpost.ai/carrier-integration/imile-api |
| **Unicommerce** (ERP) | **"API credentials to be provided by the iMile team"** — la frase clave: las credenciales las da el equipo de iMile | https://support.unicommerce.com/index.php/knowledge-base/integration-with-imile/ |
| **AfterShip** | API de tracking y shipping de iMile (RESTful + webhooks) | https://www.aftership.com/carriers/imile/api |
| **TrackingMore** | API de rastreo de guías iMile; menciona docs vía GitHub/Postman (compartidas al integrar) | https://www.trackingmore.com/imile-tracking-api |
| **Track123** | Agregador de tracking con iMile, con ejemplos curl | https://www.track123.com/carriers/imile/api |
| **KD100** | Tracking + shipping API de iMile | https://www.kd100.com/carriers/imile |
| **Módulo Odoo** (Vraja) | Integración de envíos iMile para el ERP Odoo ("usa tus credenciales de test o live") | https://apps.odoo.com/apps/modules/16.0/imile_shipping_integration |

## 3. La evidencia OFICIAL — iMile vende la integración por API (México incluido)

**El hallazgo más fuerte** (URLs aportadas por Javi, 9-ago): las páginas oficiales de iMile
del producto **"Modelo de entrega personalizado"** (servicio de valor añadido):

- 🇲🇽 https://www.imile.com/MX-es/logistics-solutions/customized-delivery-model
- 🇧🇷 https://www.imile.com/BR-en/logistics-solutions/customized-delivery-model/

Textos literales de iMile:

> *"Conecte múltiples productos iMile a múltiples o únicas API para una solución"*
> (versión MX-es)

> *"offers API integrated customized Delivery model"* · *"Connect multiple iMile Products
> to multiple or single API for seamless business delivery solution"* (versión BR-en)

- Cobertura declarada: Emiratos, Arabia Saudita, **México**, Omán, Kuwait, Qatar, Bahrein, Australia, Jordania.
- Vía de contratación en la propia página: **"Solicitar una cotización"** / formulario de contacto / WeChat.

**Implicación:** la integración por API no solo no está prohibida — **es una oferta
comercial pública de iMile, disponible en México**. La historia de "el contrato lo
prohíbe" queda contradicha por el material comercial de la propia empresa.

## 4. De dónde salen las keys (respuesta directa)

**No hay botón en DS ni en PCS ni en ningún panel.** El proceso es humano:

1. Se solicita al **contacto/equipo de iMile** (ejecutivo de cuenta, o el formulario de
   cotización del "customized delivery model").
2. iMile entrega **credenciales** (test y productivas, según el módulo de Odoo) y **su
   documentación** (circula vía GitHub/Postman compartido al integrar).

Esto explica el caso del **proveedor de Guadalajara**: no violó nada ni hizo magia —
pidió sus credenciales al equipo de iMile y se las dieron.

## 5. Matiz honesto

Las APIs documentadas por los integradores son del lado **merchant/seller** (crear guías,
rastrear, cancelar). Lo que nuestro sistema necesita es principalmente data del lado
**proveedor/estación (DS)**. Si el API cubre también ese lado, lo confirmará el equipo de
iMile al responder la solicitud. (Y aunque solo fuera merchant-side, el tracking por AWB
ya alimenta rastreabilidad.)

## 6. Los tres caminos abiertos (ninguno se cierra hasta el scope)

| # | Camino | Estado |
|---|---|---|
| 1 | **Pedir credenciales al equipo de iMile** — la vía formal y la que desbloquea todo | Pendiente: hacer la pregunta |
| 2 | **Inspeccionar los endpoints internos de DS/PCS** con el user de desarrollo de Fer (las webs son SPAs que consumen JSON — ver qué llama la interfaz enseña las formas del dato) | Posible al llegar el user |
| 3 | **Agregadores de terceros** (TrackingMore/AfterShip/Track123/KD100) para tracking por AWB | Opción puntual |

Y como **piso garantizado** siguen los exports nativos que iMile mismo enseña a usar
(bases de tickets + Excel de guías a pago — ver doc 02).

## 7. Siguiente paso recomendado

Que el cliente (o Fer) haga a su contacto de iMile **la pregunta con munición oficial**:

> *"En su página ofrecen el 'Modelo de entrega personalizado' con integración por API
> para México ([link MX](https://www.imile.com/MX-es/logistics-solutions/customized-delivery-model)).
> Quiero integrarme por API — sé que otros proveedores ya tienen credenciales.
> ¿Con quién lo tramito?"*

Una petición respaldada por el material comercial de la propia iMile es muy difícil de
negar, y su respuesta define la arquitectura de ingesta.

---

*Investigación: búsqueda web 9-ago-2026 + verificación directa de dominios + páginas
oficiales de iMile aportadas por Javi. Ver también: `02-hallazgos-documentos-proveedores.md`
(hallazgo 1: nada en los documentos del cliente prohíbe el API).*
