# Términos legales de iMile — lectura para la cuestión del acceso por API

> Lectura de los tres documentos legales públicos de iMile (Términos de Uso, Aviso Legal,
> Política de Privacidad) para responder: **¿los términos prohíben acceder por código/API
> a los datos que el cliente ya ve en su sistema?** 9-ago-2026.
> Textos completos guardados en `legales-imile/` (extraídos de la API de contenido de
> iMile — son documentos públicos).

---

## TL;DR

1. **Los "Términos de Uso" y la "Política de Privacidad" en es_MX son en realidad los de
   iMile ESPAÑA (RGPD, Madrid, ley española)** — el sitio mexicano sirve la versión
   europea traducida. Solo el **Aviso Legal** identifica a la entidad mexicana (IMILE
   DELIVERY SERVICES MÉXICO, S. de R.L. de C.V., RFC IDS210629V91).
2. **Estos términos rigen el SITIO WEB público** (imile.com) — *"se aplican a usted
   personalmente y a su uso del sitio web de iMile"*. **NO rigen la plataforma operativa
   DS/PCS** a la que el cliente entra con credenciales; eso lo rige su **contrato de
   proveedor** (que no hemos visto, pero cuyo Reglamento v1.3 **no menciona APIs** — ver
   doc 02).
3. Hay una cláusula **anti-ingeniería-inversa y anti-duplicación**, pero (a) apunta al
   *software y al contenido del sitio web*, no a que un proveedor jale su propio dato
   operativo, y (b) trae un **carve-out explícito**: la prohibición aplica *"salvo que
   haya obtenido una licencia pertinente mediante otro acuerdo entre usted e iMile"*.
   **El API oficial que iMile vende ES ese "otro acuerdo".**
4. **En ningún documento aparecen las palabras** "acceso automatizado", "scraping",
   "robots", "bots" ni "crawlers".

## Las cláusulas que sí importan (texto literal — Términos de Uso, jul-2026)

**Alcance (a qué aplican):**
> *"Estas Condiciones de uso … se aplican a usted personalmente y a su uso del sitio web
> de iMile … en https://www.imile.com/ES-es (…el Sitio web)."*

→ El objeto es el **sitio web de marketing**, no el sistema DS/PCS.

**Contenido del Software (ingeniería inversa) — CON su carve-out:**
> *"iMile o sus licenciantes poseen … todos los derechos … sobre todo el software de este
> sitio web … API, SDK, documentación … (el «Contenido del Software»). **Salvo que haya
> obtenido una licencia pertinente mediante otro acuerdo entre usted e iMile**, ninguna
> disposición de estos Términos le otorga derecho o licencia … y usted no podrá realizar
> ingeniería inversa, descompilar, desensamblar, dividir, adaptar, implantar ni implementar
> obras derivadas del Contenido del Software."*

→ Es sobre **descompilar SU software**. Y explícitamente NO aplica si tienes una licencia
por otro acuerdo (= el API oficial).

**Derechos de autor (duplicación de contenido del sitio):**
> *"Sin el consentimiento previo por escrito de iMile … ningún contenido del sitio web
> podrá ser reproducido, modificado, … desensamblado, sometido a ingeniería inversa,
> descompilado, … cargado en otros servidores mediante el método de 'duplicación',
> almacenado en un sistema de recuperación de información o utilizado de cualquier otra
> forma…"*

→ Es sobre **duplicar el contenido del sitio web público** (textos, imágenes) — no sobre
que un proveedor consulte su propia operación.

**Corte de acceso (discrecional):**
> *"iMile podrá, a su entera discreción y sin previo aviso, cancelar su acceso al sitio web …
> si iMile determina que usted ha infringido estos Términos…"*

→ Riesgo real pero acotado al **sitio web**; iMile puede cortar acceso si considera que
violaste sus términos. Nada de multas aquí (eso vive en el contrato de proveedor).

## Aviso Legal (entidad mexicana — texto completo, es corto)

> *IMILE DELIVERY SERVICES MÉXICO, S. DE R.L. DE C.V. — Calle Laguna de Términos 221,
> Torre B, Piso 3, Col. Granada, C.P. 11520 (CDMX). RFC: IDS210629V91. Capital Social:
> 1,506,548,158 MXN. Correo: peter.huang[AT]imile.me*

→ Solo identificación corporativa. Sin restricciones de uso. **Dato útil:** el correo de
un contacto directo de iMile México (peter.huang@imile.me) — posible vía para tramitar el
API oficial.

## Política de Privacidad (es_MX = versión España/RGPD)

- *"Esta Política de Privacidad no constituye un contrato y no crea derechos ni
  obligaciones contractuales."* — es informativa.
- Rige el tratamiento de datos personales en el **sitio web y las apps**; el §2.5 "Uso
  automatizado de datos" es sobre **cookies**, no sobre acceso por API.
- Reconoce que iMile comparte datos con *"subcontratistas de entrega de última milla,
  proveedores externos y conductores"* y usa APIs de terceros (Google Maps) — o sea, el
  ecosistema ya es de integraciones.
- Nada aquí prohíbe a un proveedor acceder a **su propio** dato operativo.

## Respuesta a la pregunta (honesta y matizada)

**¿Prohíben estos documentos nuestro enfoque? No de forma clara — y por tres razones:**

1. **Alcance equivocado:** estos términos son del **sitio web público**, no del sistema
   DS/PCS que el cliente usa con sus credenciales. Lo que gobierna el DS es el **contrato
   de proveedor**, y su Reglamento v1.3 **no dice nada de APIs**.
2. **La única cláusula "dura" (ingeniería inversa) trae carve-out:** no aplica si hay
   *"una licencia … mediante otro acuerdo"*. El **API oficial** que iMile vende (doc 03)
   es precisamente ese acuerdo → convierte cualquier duda en integración autorizada.
3. **Cero mención de scraping/acceso automatizado/bots.** La supuesta prohibición que le
   dijeron al cliente **no existe en ninguno de estos textos.**

**El matiz honesto (el "pero"):** el lenguaje anti-ingeniería-inversa es amplio, y un
abogado muy conservador *podría* argumentar que decodificar el API interno del DS es
"ingeniería inversa" del software de iMile. No es la lectura natural (esa cláusula es para
que no descompiles su código ni dupliques su sitio, no para que dejes de leer tu propia
operación), pero es la razón #1 por la que **el API oficial es la jugada limpia**: elimina
el argumento de raíz, porque pasa a ser un acceso *licenciado por otro acuerdo* — la
excepción que los propios Términos contemplan.

## Recomendación

- **Perseguir el API oficial** (doc 03) como meta — es la vía bendecida y el propio
  carve-out de los Términos. Contacto posible: peter.huang@imile.me (del Aviso Legal) o el
  ejecutivo de cuenta del cliente.
- **Mientras tanto, desarrollar** contra los endpoints internos (doc 04) con
  comportamiento humano (ritmo razonable, sin martillar) es de bajo riesgo: es tu propia
  cuenta, tu propio dato, para trabajo del cliente — no hay acceso a terceros ni a datos
  ajenos.
- **Pedir al cliente su contrato de proveedor con iMile** (el documento marco que firmó)
  y los términos específicos del sistema DS si existen — es donde viviría, si acaso, una
  cláusula de acceso; los términos del sitio web no lo son.

## Cómo se obtuvieron estos textos (nota técnica)

Las páginas legales de imile.com son SPAs (Next.js) que cargan el cuerpo legal desde una
API de contenido: `POST https://www.imile.com/flash/admin/privacy/agreement/list` con body
`{"type":"Terms of Use" | "Privacy Policy" | "Legal Notice"}` y header `lang`. Devuelve
versiones por idioma/país (es_MX entre ellas). Documento público — guardado íntegro en
`legales-imile/`.

---

*Ver también: doc 02 (el Reglamento del cliente no menciona API), doc 03 (iMile vende el
API — el carve-out), doc 04 (endpoints internos capturados).*
