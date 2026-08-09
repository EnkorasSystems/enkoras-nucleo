# ENKORAS B2B — Contexto maestro

> Documento que amarra todo lo que existe de la plataforma: qué es, cómo nació, qué se
> construyó, cómo funciona y dónde está parada. Los documentos 01–13 y el
> PITCH-COMERCIAL (copiados del repo de producción `directorio-b2b`) son el detalle;
> este es el mapa. **Compilado: 8 de agosto de 2026.**

---

## Qué es

**ENKORAS** (*/en-ko-ras/* — "Ecosistema donde las empresas transforman conexiones en
oportunidades") es la plataforma de networking B2B industrial de México, **en producción
en [enkoras.com](https://enkoras.com)**. Proveedores publican su catálogo de servicios y —
el diferenciador que nadie más tiene — su **disponibilidad en tiempo real** con vigencia
que expira sola. Compradores encuentran proveedores con búsqueda semántica IA o publican
solicitudes de compra que se rutean automáticamente solo a los proveedores de las
categorías coincidentes. Bilingüe ES/EN desde el día uno. Sin comisiones por transacción:
Enkoras conecta, no intermedia.

## Cómo nació

De una **observación repetida de demanda real sin plataforma**, no de un brainstorm.
Trabajando en la industria del cruce aduanal en Tijuana y publicando en LinkedIn,
Javier y su socio notaron el patrón diario: las empresas usan LinkedIn como marketplace
B2B improvisado — unas buscando proveedor (etiquetas térmicas, transportistas con
unidades libres, entries aduanales, componentes con entrega inmediata), otras ofreciendo
capacidad ("10 troques disponibles al momento"). Y esas publicaciones **mueren en horas**
en un feed cronológico, sin estructura, sin comparación, sin historial.

El contexto lo multiplica el nearshoring: México superó a China como socio comercial #1
de USA, Baja California tiene 956 plantas maquiladoras (621 en Tijuana), y miles de
empresas nuevas llegan sin redes de proveedores. Los directorios B2B mexicanos son fichas
estáticas; ThomasNet (USD 89.3M/año con este modelo en USA) no cubre México. El hueco es
real y el modelo de negocio está probado por el benchmark.

La base técnica vino de **Tu Local** (tu-local.com), el directorio B2C de Baja California
de Javier, ya en producción: ~70% del código se heredó (stack, auth, admin, Stripe,
fotos, reseñas, analytics, design system).

## La construcción: 4 días

**137 commits del 2 al 5 de agosto de 2026.** La plataforma completa:

| Día | Qué se cerró |
|---|---|
| **2-ago** | Docs fundacionales + **Fase 0** (BD propia, árbol 14 sectores/94 sub/410 sub-sub, geografía 32 estados/235 ciudades, pgvector, i18n next-intl) + **Fase 1 completa** (wizard con clasificación IA, panel mi-empresa, disponibilidad Realtime, búsqueda híbrida, verificación RFC, perfil público, split view estilo Indeed) |
| **3-ago** | **Fase 2** (solicitudes de compra, ruteo demanda→oferta, campana Realtime, chat 1-a-1) + **nace la marca ENKORAS** + **deploy a producción con dominio** + Stripe completo (Premium $499) + correos + Google OAuth |
| **4-ago** | Performance (lag del scroll cazado con Playwright: home de 19 tirones a 1), selección instantánea de anuncios, card del anuncio rediseñada |
| **5-ago** | OG images, /cuenta, compartir, paginación, chat endurecido (anti-monólogo, conversaciones que nacen con el primer mensaje), 404, panel rediseñado (riel conmutador), favoritos, moderación, **multi-empresa** ("empresa activa", hasta 5 por cuenta), auth rediseñado, retícula blueprint, página /nosotros con la historia de marca, SEO completo + Search Console (536 URLs) + GA4, pitch comercial, limpieza de datos demo |

Todo con **150+ tests y CI en verde**, trabajado en bloques lineales (un bloque se
termina, se prueba y se cierra antes del siguiente).

## Las decisiones de arquitectura que definen el producto

1. **La IA trabaja al ESCRIBIR, nunca al LEER** — clasificación y embeddings una vez por
   publicación; la búsqueda es matemática pura en Postgres (0.55·semántica + 0.45·texto
   + bonos de disponibilidad/verificación/rating; corte relativo piso 0.55 + brecha 0.09).
   Costo de operación ≈ cero.
2. **Falla suave en todo** — sin IA, la empresa se publica igual y la búsqueda degrada a
   texto. Gemini capa gratuita con 5 keys en rotación automática.
3. **La vigencia es RLS, no cron** — la disponibilidad vencida deja de existir para el
   público por política (`expires_at > now()`). El directorio nunca miente.
4. **RLS como autoridad** — el dueño solo toca lo suyo; columnas de plan/verificación
   protegidas del grant. **Falla cerrada** en suscripciones: nunca regala premium.
5. **Liquidez sagrada** — el comprador nunca paga; el bono Premium en ranking es acotado
   (+0.08): empuja entre relevantes, jamás rescata irrelevantes.
6. **Contactos solo para registrados** — anti-scraping que convierte visitantes en cuentas.
7. **Retención mínima de documentos** — la constancia fiscal se elimina al aprobar/rechazar
   la verificación.

## La marca

**EN | KOR | AS** = Enterprise/Entrar | Core/Núcleo (queda al centro de la palabra) |
Association/Access. Isotipo **"Nexo Central"** diseñado por Javier en Canva (aro naranja,
líneas diametrales, nodos concéntricos). Metáfora del sol: las oportunidades llegan al
núcleo, no se persiguen. Manifiesto: *"No vendemos, no fabricamos, no transportamos —
conectamos."* Paleta: naranja #FF6803 sobre tinta #0B0501. La historia completa vive en
[enkoras.com/nosotros](https://enkoras.com/nosotros).

## El negocio

Freemium con liquidez sagrada: Free incluye todo lo esencial (perfil, catálogo,
disponibilidad, solicitudes, chat, verificación). **Premium $499 MXN/mes, solo mensual**:
badge DESTACADO, prioridad acotada en ranking, aparición preferente. Stripe con cuenta
propia "Enkoras". Estrategia de lanzamiento: 2–3 meses todo gratis para llenar el
directorio → Premium de cortesía a aliados → vender con datos de uso reales. Cupón
FUNDADOR listo en Stripe (pendiente decisión). Aritmética: cada 100 Premium = $49,900
MXN/mes recurrentes con costo marginal ≈ cero.

## Dónde está parada (8-ago-2026)

- Producción estable, BD en ceros (datos demo eliminados), lista para el lanzamiento.
- **Siguiente capítulo — comercial, no técnico:** lanzamiento BC con seed de proveedores
  invitados (red del socio: grupos de WhatsApp empresariales + LinkedIn industrial),
  decisión del cupón FUNDADOR, conversiones en GA4 cuando haya datos.
- Congelados por decisión: correos de notificación, adjuntos de chat, razón social en
  legales.

## Índice de los documentos copiados del repo de producción

| Doc | Qué contiene |
|---|---|
| `01-analisis-y-vision.md` | El fundacional: problema, mercado, competencia, features, arquitectura IA, modelo de negocio, riesgos |
| `02-roadmap.md` | Las fases 0–4 con criterios de salida |
| `03-arbol-categorias.md` | La taxonomía completa 14/94/410 |
| `04` – `08` | Fase 1: mi-empresa, disponibilidad, búsqueda híbrida, verificación RFC, perfil público |
| `09` – `12` | Fase 2: solicitudes, ruteo demanda→oferta, campana, chat |
| `13-suscripcion-premium.md` | Stripe: modelo, flujo, candados, operación |
| `PITCH-COMERCIAL.md` | El documento de venta para el equipo comercial |

> El estado técnico vivo (bloque por bloque) está en el `CLAUDE.md` del repo de
> producción `directorio-b2b` — ese sigue siendo la fuente de verdad del código.
