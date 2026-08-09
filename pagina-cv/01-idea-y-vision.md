# Análisis y Visión — Página de Expedientes Profesionales

> **Documento fundacional del proyecto.** El porqué, el qué, el flujo completo, las dos
> features estrella, la integración con la bolsa, el modelo de negocio y los riesgos.
> De este documento saldrán el scope y el roadmap cuando toque construir.
> **`pagina-cv` es nombre de trabajo** — el nombre comercial se decide antes del
> lanzamiento (mismo proceso que dio ENKORAS).

**Fecha:** Agosto 2026
**Fundadores:** Javier Calixto (producto y desarrollo) + socio (promoción, networking, canales)
**Horizonte:** proyecto de corto plazo — **antes de diciembre de 2026**
**Proyecto hermano inseparable:** la bolsa de trabajo invertida (`bolsa-invertida/`) — esta página es su fábrica de contenido
**Referencia viva del producto:** el artifact "David Alejandro García — Expediente profesional" (agosto 2026) — el primer expediente real compilado, y la prueba de que el formato funciona

---

## 1. Origen de la idea

Dos experiencias se juntaron:

**La primera: el infierno de hacer un currículum — vivido en carne propia** (por Javier
y por su socio). El camino de hoy es este:

1. **La ruta "hazlo tú":** buscas una plantilla genérica de Excel/Word o la descargas de
   Google. Conseguirla ya es difícil; al abrirla, todo está descuadrado, con secciones
   que ni ocupas y textos de relleno. Terminas pasando **horas** peleándote con el
   formato en vez de con el contenido.
2. **La ruta "te lo hacemos":** páginas que te hacen el CV "con IA" (o ni eso — puros
   formularios). De todos modos **tú escribes todo, tú mueves todo, tú editas todo**...
   y al darle "descargar": sorpresa, **$300 pesos o una suscripción**. El paywall llega
   *después* de que ya invertiste tu esfuerzo — el peor momento posible. Pagas de mala
   gana o te vas; en ambos casos no vuelves jamás. Cero valor retenido, cero retención.
3. **El remate:** aun pagando, lo que sale es una plantilla tan genérica que el
   reclutador la descarta con solo verla — el mismo formato que los otros 3,000.

**La segunda: el artifact de David.** Al compilar con IA el expediente profesional de
David Alejandro García (13+ años de operaciones en manufactura, Tijuana–San Diego) quedó
claro que con la data correcta se puede generar algo de **otra categoría**: no un CV —
un **expediente profesional interactivo**, con métricas duras en el hero, evidencia
numérica por empresa, trayectoria cronológica verificada con detalle expandible,
portafolio de lo construido, fortalezas, formación y certificaciones. Un documento que
argumenta con datos, diseñado para que quien lo abre decida en 30 segundos.

La pregunta que parió el producto: **¿y si hacer tu currículum fuera exactamente esto —
subes tus datos y la IA te compila TU expediente?**

## 2. El problema

- Hacer un buen CV es un infierno de horas y el resultado es genérico.
- Las páginas de CV existentes cobran por un producto malo, en el momento equivocado
  (paywall sorpresa al final), sin dar ninguna razón para regresar.
- El PDF tradicional no está hecho para competir: sin evidencia estructurada, sin
  interactividad, indistinguible del resto de la pila.

## 3. La solución: el expediente profesional

El usuario no "llena una plantilla" — **alimenta su expediente y la IA lo compila**,
al nivel del de David. El formato de referencia (secciones del expediente):

1. **Hero con métricas duras** — años de experiencia, empresas/puestos, cifras pico
2. **Resumen** — redactado por la IA con todo lo capturado
3. **Resultados/Evidencia** — logros numéricos por empresa
4. **Trayectoria** — historial cronológico con detalle expandible por puesto
5. **Lo construido/logrado** — proyectos, con estado
6. **Competencias clave**
7. **Formación** — educación, certificaciones, reconocimientos
8. **Contacto**

## 4. El flujo de creación (definido)

1. **Foto** — se queda: es la del perfil/expediente.
2. **Documentos académicos (opcional)** — certificados, títulos: la IA los lee, extrae
   los datos de estudios, **y los archivos se eliminan después del análisis**
   (retención mínima — mismo principio que la constancia RFC en Enkoras).
3. **Historial laboral** — por cada empresa: periodo, nombre de la empresa, puesto y
   breve descripción de qué hacía. Todas las empresas de su trayectoria.
4. **Logros / lo construido** — qué ha logrado, construido o tiene en proceso (la
   sección "Lo que ha construido" del expediente de David).
5. **Competencias clave** — input libre, **con explicación de qué se puede escribir
   ahí** (el usuario nunca se queda en blanco).
6. **Educación y certificaciones** — pre-llenada con lo extraído de los documentos, o
   manual: carreras, licenciaturas, certificaciones, reconocimientos.
7. **Resultados** — cifras y métricas (tipo "Resultados de piso").
8. **Resumen** — la IA lo redacta con todo lo anterior + lo que el usuario agregue en
   ese input.
9. **CV en PDF existente (opcional)** — se puede subir: la IA también extrae su
   información para alimentar el perfil con más datos. (Aquí sí; en la bolsa jamás.)
10. **Generación** — el usuario **elige un diseño** del catálogo y la IA compila el
    expediente: corrige ortografía, adapta el registro a algo profesional — **sin
    mentir, sin inventar, sin borrar: adapta**. Regla dura: la IA no alucina.
11. **Clasificación** — con el expediente generado, la IA le asigna las categorías (o
    tags, o ambas — por definir) **que son las mismas de la bolsa de trabajo**.
12. **Corrección** — un chat donde el usuario señala "esto no es correcto" y se ajusta.
    La IA propone, el humano confirma (patrón del wizard de Enkoras).

**El efecto secundario estratégico:** la persona entra queriendo un CV bonito y sale
con un expediente clasificado, con embeddings, listo para ser encontrado en la bolsa —
la fábrica de contenido del marketplace funciona sola.

## 5. Feature estrella #2: práctica de entrevistas por voz

Lo que existe en internet es genérico: bancos de las mismas preguntas de siempre, texto
plano que escribes, o dictado por voz pero igual de genérico. Aquí es diferente —
**la entrevista te conoce**:

- **Modo 1 — vacante agendada (ecosistema):** si una empresa ya te agendó entrevista
  por la bolsa, la vacante se **precarga automáticamente**. No pegas nada.
- **Modo 2 — vacante externa (híbrido a propósito):** buscaste una vacante por tu
  cuenta (Indeed, donde sea) mientras esperas que te encuentren; copias la descripción
  del anuncio, la pegas, y listo. **Siempre puedes usar ambos modos** — la práctica no
  está amarrada a la bolsa.

El flujo: la IA estudia **tu expediente + la vacante** y:
1. Primero te dice **qué tan adecuado eres para esa vacante** según tu perfil (el fit
   score — el mismo motor de matching de la bolsa, reutilizado).
2. Genera la secuencia de preguntas **nacida de ese cruce** (tu historial real contra
   los requisitos reales).
3. La entrevista es **por voz**: la IA habla contigo, pregunta, tú contestas hablando.
4. Al final: **resultado de cómo te fue y qué debes mejorar.**

Mucha gente ya paga por esto (coaching de entrevistas, plataformas de mock interviews,
prep de LinkedIn Premium) — y todas las opciones son genéricas. Además, la práctica le
da a la página **retención recurrente**: el CV se hace una vez; a entrevistas vuelves
cada vez que apliques a algo.

## 6. Exportar a PDF

El expediente vive como página interactiva (en la página de CV y en la bolsa), pero se
puede **descargar en PDF**: se pierde la interactividad, pero sirve para procesos y
entrevistas externas. Generosidad estratégica: el producto te sirve aunque no vivas en
nuestro ecosistema — "eso nos hace bien a nosotros".

## 7. Integración con la bolsa invertida (reglas ya decididas)

- Mismo correo = **expediente precargado en la bolsa**; allá solo eliges publicarte
  (formal / freelancer / ambas).
- Redirección cruzada en ambos sentidos (detalle en
  `bolsa-invertida/01-idea-y-vision.md`, sección 6).
- **Exclusividad:** la bolsa solo acepta expedientes creados aquí. Esta página es la
  única puerta de entrada al marketplace — el funnel es estructural.

## 8. Modelo de negocio

**Se cobra: (1) la creación del expediente y (2) las prácticas de entrevistas.**

- **Principio de diseño innegociable** (nacido de nuestra propia crítica a la
  competencia): **jamás el paywall sorpresa**. Precio visible desde antes de empezar,
  nunca revelado después del esfuerzo. A evaluar en el scope: preview del expediente
  generado (con marca de agua / sin publicar ni descargar) antes de pagar — que el
  usuario pague viendo lo que recibe.
- **De-risking del ecosistema:** este ingreso **no depende de la bolsa ni de tener
  empresas registradas**. Alguien busca "práctica de entrevistas" o "hacer currículum",
  nos encuentra, se registra y consume. Facturación desde el día uno mientras la bolsa
  acumula inventario.
- El candidato paga aquí; en la bolsa nunca (allá paga la empresa).

## 9. Go-to-market

1. **Universidades** — el mismo target de la bolsa: estudiantes que necesitan su primer
   CV decente y trabajo mientras estudian.
2. **Expos de fábricas/empresas en Tijuana** — presencia física; cada stand promociona
   el ecosistema completo.
3. **Búsqueda de intención** — "hacer currículum", "practicar entrevista de trabajo":
   tráfico que llega solo, por dolor, sin marketing. SEO desde el día uno (playbook
   Enkoras: sitemap, hreflang, JSON-LD).
4. **Growth loop del expediente** — cada expediente compartido (WhatsApp, LinkedIn,
   entrevistas) es publicidad del producto, con la marca visible. El contenido del
   usuario recluta usuarios.
5. **Voz en voz** — estudiantes y empresas pasando la voz.

## 10. Herencia técnica

- **Del wizard de Enkoras:** captura por pasos con borrador, subida de archivos,
  clasificación IA con falla suave, "IA propone / humano confirma".
- **De la arquitectura madre:** la IA trabaja al escribir (compilación + clasificación
  + embeddings una vez), Gemini con rotación de keys, RLS como autoridad, retención
  mínima de documentos, i18n ES/EN, Stripe.
- **Lo genuinamente nuevo:** el motor de render de expedientes (catálogo de diseños +
  generación de la página interactiva + export PDF), el chat de corrección, y la
  **entrevista por voz en tiempo real** (el módulo técnico más nuevo del ecosistema —
  dimensionarlo bien en el scope).

## 11. Riesgos y mitigaciones

| # | Riesgo | Mitigación |
|---|---|---|
| 1 | **Alucinación de la IA** — inventar logros o títulos destruiría la confianza (y al candidato) | Regla dura de producto: adaptar, nunca inventar ni borrar; chat de corrección; el usuario confirma antes de publicar |
| 2 | **Datos personales** (LFPDPPP) — CVs, títulos, foto | Retención mínima de documentos (se analizan y se eliminan); consentimiento explícito; legales desde el día uno (playbook Enkoras) |
| 3 | **Expectativa de gratis** — hay generadores de CV gratuitos (Canva, plantillas) | No competimos en la categoría "plantilla": el expediente es otra categoría de objeto (interactivo, con evidencia, conectado a la bolsa y a entrevistas). El gratis no hace esto |
| 4 | **Calidad de la entrevista por voz** — si la voz falla o se siente robótica, mata la feature estrella | Prototipar y probar ANTES de prometerla en el lanzamiento; degradación elegante a modo texto si hace falta |
| 5 | **El cobro en el momento equivocado** — repetir el pecado de la competencia | Principio de la sección 8: precio antes del esfuerzo, preview antes del pago |

## 12. Visión

**Corto plazo:** el lugar donde hacer tu currículum deja de ser un infierno — subes tus
datos y sales con un expediente profesional que ningún reclutador descarta, y con una
entrevista practicada.

**Mediano plazo:** la capa de identidad profesional verificada del ecosistema Enkoras —
cada expediente alimenta la bolsa, cada entrevista agendada regresa a practicarse aquí.

**Largo plazo:** que "mándame tu expediente" sustituya a "mándame tu CV" — primero en
Baja California, después donde el ecosistema crezca.
