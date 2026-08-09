# Análisis y Visión — La Bolsa de Trabajo Invertida

> **Documento fundacional del proyecto.** El porqué, el qué, el para quién, los
> diferenciadores, la conexión con el ecosistema, el modelo de negocio y los riesgos.
> De este documento saldrán el scope y el roadmap cuando toque construir.
> **`bolsa-invertida` es nombre de trabajo** — el nombre comercial se decide antes del
> lanzamiento (mismo proceso que dio ENKORAS).

**Fecha:** Agosto 2026
**Fundadores:** Javier Calixto (producto y desarrollo) + socio (promoción, networking, canales)
**Horizonte:** proyecto de corto plazo — **antes de diciembre de 2026**, siguiente después de Enkoras B2B
**Proyecto hermano inseparable:** la página de expedientes profesionales (`pagina-cv/`) — su "mano derecha"

---

## 1. Origen de la idea

Nació de la misma fábrica que Enkoras: Javier y su socio, después de ver a Enkoras
avanzar tanto en unos días, se sentaron a pensar qué otras páginas construir, y esta
salió de observar **el defecto estructural del modelo de las bolsas de trabajo
actuales** (Indeed, Computrabajo, OCC y similares):

**Hoy la empresa publica su vacante y espera.** Le llegan cientos o miles de
postulantes por una sola posición, y el trabajo pesado — filtrar, descartar, leer CVs
genéricos uno por uno — lo hace la empresa. El que paga es el que más trabaja. Y del
otro lado, el candidato postula a ciegas: manda su PDF a un agujero negro junto con
otros 3,000, sin saber si alguien lo verá.

**La idea es voltear el modelo:** que el que busca trabajo publique su perfil una vez,
y que sea **la empresa la que entre a buscar** — con herramientas que le entreguen a
los candidatos correctos en minutos, no miles de PDFs que descartar.

## 2. El problema

### 2.1 Para la empresa que contrata
- Una vacante publicada = avalancha de postulantes, la mayoría sin el perfil requerido.
- Filtrar es manual, lento y caro: horas de reclutador leyendo CVs genéricos.
- Las herramientas de búsqueda de CVs que existen (las bases de Computrabajo/OCC,
  LinkedIn Recruiter) son features secundarias, caras, con buscadores booleanos
  pensados para reclutadores profesionales de empresas grandes.
- El CV tradicional en PDF no está hecho para decidir: sin estructura comparable, sin
  evidencia verificada, con formatos rotos.

### 2.2 Para el que busca trabajo
- Postular es un acto de fe: cientos de aplicaciones, casi ninguna respuesta.
- Su CV compite en una pila de miles con una plantilla genérica que el reclutador
  descarta con solo verla.
- No existe un lugar donde su perfil trabaje por él — donde ser **encontrado** en vez
  de estar postulando.
- El freelancer ni siquiera tiene un lugar natural: las bolsas son de empleo formal y
  las plataformas freelance son otro mundo aparte.

## 3. La solución

Una plataforma donde **la oferta de talento publica y la demanda busca**:

### 3.1 El candidato (el que publica)
- Publica su **expediente profesional** — creado exclusivamente en la página de CV del
  ecosistema (ver sección 6: exclusividad). No hay forma de subir un PDF propio.
- La plataforma lo categoriza por **áreas y especialidades** (clasificación IA sobre el
  expediente, con el mismo patrón de árbol/categorías de Enkoras; por categorías, tags,
  o ambas — por definir en el scope).
- Elige dónde publicarse: **empleo formal**, **freelancer**, o ambas.
- Publica su estado de disponibilidad ("buscando activamente", "abierto a ofertas") —
  con la mecánica de frescura/vigencia heredada de Enkoras, para que la base nunca se
  pudra con perfiles de gente que ya encontró trabajo.

### 3.2 La empresa (la que busca)
- Escribe lo que necesita en lenguaje natural ("ingeniero industrial con validación de
  procesos y manejo de 100+ operadores") o selecciona una o varias categorías.
- El motor — funciona sin IA por categorías/filtros; con IA (búsqueda semántica) como
  plus — le regresa **en tiempo real los perfiles que cumplen, ordenados por niveles de
  cercanía a la vacante** (tipo rating de fit). Ya no descarta miles: revisa los más
  cercanos y elige.
- Sobre su shortlist, actúa sin salir de la plataforma:
  - **Correo a todos los elegidos con un clic**
  - **Agendar citas y entrevistas ahí mismo** (agenda integrada, tipo Meet)
  - **Chat interno** con el candidato, o los medios de contacto del expediente

## 4. Diferenciadores

| # | Diferenciador | Por qué nadie lo tiene así |
|---|---|---|
| 1 | **El modelo invertido como EL producto** | En Computrabajo/OCC/Indeed la búsqueda de CVs existe pero es la feature secundaria escondida detrás de las vacantes; aquí es el corazón |
| 2 | **Shortlist rankeada por cercanía** | Los buscadores de CVs son booleanos (palabras exactas); aquí el matching es semántico y entrega niveles de fit, no una lista plana |
| 3 | **Calidad uniforme del contenido** | El 100% de los perfiles son expedientes de fábrica (estructurados, con evidencia, mismo formato) — jamás un PDF roto. Ningún incumbente puede prometer eso |
| 4 | **Ciclo completo en la plataforma** | Buscar → shortlist → correo 1-clic → agendar entrevista → chat. Los demás terminan en "aquí está el teléfono" |
| 5 | **Frescura como señal** | Disponibilidad del candidato con vigencia que expira — la base se mantiene viva sola (el foso de Enkoras aplicado a talento) |
| 6 | **Dualidad formal + freelancer** | Un solo perfil, dos mercados; nadie junta ambos mundos |

## 5. Herencia técnica de Enkoras (el mapeo es casi 1:1)

| Enkoras B2B (en producción) | Bolsa invertida |
|---|---|
| Empresa con catálogo de servicios | Candidato con expediente profesional |
| Clasificación IA en árbol de categorías | Categorización por áreas/especialidades |
| Búsqueda híbrida (semántica + texto + ranking con bonos) | La búsqueda de la empresa con niveles de fit |
| Solicitud de compra ruteada a proveedores | Búsqueda de vacante matcheada a candidatos |
| Disponibilidad en tiempo real con vigencia | "Disponible ya / abierto a ofertas" con vigencia |
| Verificación RFC con retención mínima | Verificación de historial/certificados (definir en scope) |
| Chat 1-a-1, campana Realtime, notificaciones | Igual |
| Split view estilo Indeed, i18n ES/EN, Stripe | Igual |

**Lo genuinamente nuevo a construir:** el módulo de **agenda/citas de entrevistas**
(calendario, invitaciones, recordatorios) y el **correo masivo con un clic**. Todo lo
demás es adaptación de arquitectura probada en producción.

## 6. La conexión con el ecosistema (reglas ya decididas)

1. **Exclusividad:** la bolsa **SOLO acepta expedientes creados en la página de CV**.
   No existe "subir mi CV" aquí. Razón doble: (a) estamos dando algo de valor y no
   queremos que el candidato quede mal cuando las empresas lo busquen; (b) control de
   calidad total del marketplace — la experiencia de búsqueda de la empresa nunca se
   degrada.
2. **Una identidad:** cuenta con el mismo correo = el expediente se **precarga**. En la
   bolsa no se crea nada; solo se elige dónde publicarse.
3. **Redirección cruzada:** la página de CV te presenta la bolsa; la bolsa, si no
   tienes expediente, te ofrece exportarlo desde la página de CV o crearlo en ese
   momento (re-login con las mismas credenciales, directo al wizard).
4. **Entrevistas conectadas:** si una empresa te agenda entrevista por la bolsa, esa
   vacante se **precarga automáticamente** en la práctica de entrevistas de la página
   de CV.

## 7. Modelo de negocio

- **El candidato nunca paga en la bolsa** (paga en la página de CV por su expediente y
  sus prácticas — ver `pagina-cv/01-idea-y-vision.md`). Liquidez sagrada, lección de
  Enkoras.
- **La empresa es la que paga** por buscar/contactar (suscripción, paquetes de
  contactos, o por asiento — modelo exacto por definir en el scope; cortesías al
  arranque, mismo playbook de Enkoras: llenar primero, vender con datos de uso después).
- **Ventaja estructural:** el ecosistema factura desde el día uno por el lado del
  candidato (expediente + entrevistas) sin necesitar que la bolsa tenga liquidez — el
  clásico problema del huevo y la gallina de todo marketplace queda amortiguado.

## 8. Go-to-market (el target ya está — solo falta construir)

1. **Universidades** — estudiantes que necesitan trabajo mientras estudian: se les
   ofrece el expediente gratis/accesible y la bolsa los pone en el radar de las
   empresas. El lado talento se llena por valor propio, no por promesa.
2. **Expos de fábricas y empresas en Tijuana** — presencia directa donde están los dos
   lados; cada stand promociona el ecosistema completo (incluida Enkoras B2B).
3. **Red del socio** — grupos de WhatsApp empresariales y LinkedIn industrial para
   traer a las empresas que buscan talento operativo/técnico.
4. **Voz en voz** — estudiantes y empresas pasando la voz; cada expediente compartido
   es publicidad de la plataforma.

## 9. Riesgos y mitigaciones

| # | Riesgo | Mitigación |
|---|---|---|
| 1 | **Privacidad** — un perfil profesional es dato personal (LFPDPPP en serio); el empleado que busca discretamente no quiere que su empresa actual lo vea | Diseñar desde el esquema: visibilidad controlada (ocultar mi perfil a empresas que yo elija, estilo "Open to Work" privado de LinkedIn); consentimiento explícito; retención mínima de documentos (patrón ya probado en Enkoras) |
| 2 | **Perfiles rancios** — la base de CVs muerta es el cáncer de los incumbentes | Frescura con vigencia que expira sola + peso en el ranking (el foso de Enkoras) |
| 3 | **Hábito del reclutador** — publicar vacante es cero esfuerzo; buscar es esfuerzo activo | El pitch ataca el dolor exacto: "deja de filtrar 3,000 postulantes — te damos los 15 más cercanos"; la shortlist rankeada hace que buscar cueste MENOS que filtrar |
| 4 | **Liquidez de dos lados** | El lado candidato se llena solo vía la página de CV (valor standalone); las empresas llegan por expos/red del socio; ingreso del día uno no depende de la bolsa |
| 5 | **Incumbentes con bases de CVs gigantes** | No se compite por volumen: se compite por calidad del match y del contenido (expedientes uniformes + fit semántico + frescura), y por región (BC primero, donde está la distribución física) |

## 10. Visión

**Corto plazo:** el lugar donde las empresas de Baja California encuentran al candidato
correcto en minutos — y donde publicar tu expediente significa ser encontrado, no
postular al vacío.

**Mediano plazo:** el estándar regional del talento verificado — formal y freelance —
con el ciclo completo dentro: encontrar, contactar, agendar, entrevistar.

**Largo plazo:** el ecosistema Enkoras completo — empresas que se encuentran entre sí
(B2B), y empresas que encuentran a su gente (talento) — sobre la misma
infraestructura, la misma marca de confianza y el mismo dato vivo que nadie más captura.
