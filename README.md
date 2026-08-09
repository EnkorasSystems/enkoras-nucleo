# ENKORAS — Núcleo

> **El núcleo de la empresa — el KOR de la propia marca** (EN·**KOR**·AS: Core · Núcleo,
> "queda al centro de la palabra"). Aquí no vive código de producto — vive el *porqué*,
> el *qué* y el *hacia dónde* de todo lo que Enkoras es y construye: la identidad y la
> historia de la empresa, la visión de cada producto, y los documentos de los proyectos
> con cliente. De estos documentos salen los scopes y roadmaps cuando toca construir.

**Socios:** Fernanda (Co-Founder & CSO) · David (Co-Founder & COO) · Javi (Co-Founder & CTO)
**Fecha de arranque del núcleo:** agosto 2026

---

## La empresa

**ENKORAS ya es la EMPRESA** (no solo la plataforma B2B): 3 socios en partes iguales —
Fernanda (CSO), David (COO), Javi (CTO, creador del nombre). La historia completa y viva
en [`enkoras-empresa/01-historia-y-vision.md`](enkoras-empresa/01-historia-y-vision.md).

## Las carpetas del núcleo

| Carpeta | Qué es | Estado | Nombre |
|---|---|---|---|
| [`enkoras-empresa/`](enkoras-empresa/) | La empresa: historia, identidad, socios, líneas de negocio, plan legal, bitácora | **Documento vivo** — se actualiza conforme pasan las cosas | Definitivo: ENKORAS |
| [`enkoras-b2b/`](enkoras-b2b/) | Plataforma B2B de proveedores — **enkoras.com** | **EN PRODUCCIÓN** (construida 2–5 ago 2026); siguiente capítulo: lanzamiento comercial BC | Definitivo: ENKORAS |
| [`paqueteria/`](paqueteria/) | Sistema de paquetería/rastreabilidad — **Proyecto 1 del cliente** (diagnóstico y SOP para iMile México) | **CONTRATADO — entrega ~1 mes** (sep 2026); la puerta a Shein y Sells | Por definir |
| [`bolsa-invertida/`](bolsa-invertida/) | La bolsa de trabajo invertida — el candidato publica, la empresa busca | Idea definida — proyecto corto plazo (antes de diciembre 2026) | **Temporal** |
| [`pagina-cv/`](pagina-cv/) | Página de expedientes profesionales + práctica de entrevistas por voz | Idea definida — proyecto corto plazo (antes de diciembre 2026); es la "mano derecha" de la bolsa | **Temporal** |

> Las carpetas de **Shein** y **Sells** (proyectos 2 y 3 de la escalera del cliente) se
> crean SOLO cuando haya luz verde — sin luz verde no hay carpeta.

## Cómo se conectan

```
        pagina-cv                    bolsa-invertida
   ┌──────────────────┐          ┌──────────────────────┐
   │ Expediente        │  mismo  │ El candidato ya está  │
   │ profesional (IA)  │  correo │ precargado — solo     │
   │ + práctica de     │ ───────►│ elige publicarse:     │
   │ entrevistas (voz) │◄─────── │ formal / freelancer   │
   └──────────────────┘ vacantes └──────────────────────┘
     cobra al usuario    agendadas    cobra a la empresa
     (expediente +       se precargan (búsqueda/contacto)
      entrevistas)       en práctica
                                          │
        enkoras-b2b (producción) ─────────┘
   misma arquitectura madre: clasificación IA + embeddings +
   búsqueda híbrida + ranking + realtime + verificación + i18n
```

- **Exclusividad:** la bolsa SOLO acepta expedientes creados en la página de CV (nunca PDFs externos) — calidad uniforme del marketplace.
- **Ingreso sin esperar liquidez:** expediente y entrevistas facturan desde el día uno, aunque la bolsa aún no tenga empresas.
- **Distribución compartida:** universidades + expos de fábricas en Tijuana + red del socio — cada evento promociona todo el ecosistema.

## Reglas de este repo

- Repo **personal de CrowSo** — git author `CrowSo <145426431+CrowSo@users.noreply.github.com>`, jamás identidad Goldmex.
- Cada carpeta arranca con su documento fundacional (`01-idea-y-vision.md` o `00-contexto-maestro.md`). Los scopes y roadmaps futuros se derivan de esos documentos — mismo método que se usó con Enkoras B2B (documentar primero, construir por bloques después).
- Los nombres `bolsa-invertida` y `pagina-cv` son de trabajo; los nombres comerciales se deciden antes de construir (mismo proceso que dio ENKORAS).
