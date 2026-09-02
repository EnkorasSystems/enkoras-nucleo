# Estudio de precios — Enkoras B2B (México)

**Fecha:** 1-sep-2026 · **Preparó:** CTO (con research web del mismo día)
**Para:** la mesa de socios — la decisión de precios que desbloquea 5.C→5.F
**Alcance:** México primero (decisión de Javi); USA después.

> ⚡ **DECISIÓN (Javi, 1-sep-2026):** se adopta la **columna JUSTO** —
> **Proveedor $649 · Empresa completa $1,190 MXN/mes** — como default
> operativo, **hasta que la mesa diga lo contrario**. Los socios recibieron
> este estudio y la radiografía y harán sus propios análisis en paralelo;
> el precio es reversible por diseño ("cambiar los precios y links y ya").
> Con esto 5.C→5.F quedan desbloqueados.

---

## 1. Qué está vendiendo Enkoras (inventario real, auditado contra el código)

No es un directorio: es **cuatro productos en uno**, y eso define contra quién
se compara el precio.

### A. Presencia y descubrimiento (lo que venden los directorios)
- Perfil público rico: catálogo de servicios, fotos, ubicación, contacto y
  redes, reseñas con calificación. Bilingüe ES/EN (nearshoring-ready).
- **Búsqueda híbrida con IA**: semántica (pgvector) + texto + señales de
  calidad. La IA clasifica a cada empresa en un árbol de 3 niveles al
  registrarse — nadie llena formularios de categorías.
- **Disponibilidad en tiempo real**: "tengo 200 tarimas HOY" en la card del
  buscador — ningún directorio mexicano tiene inventario vivo.
- Ranking meritocrático: disponibilidad > verificada > premium > reseñas
  (el pago empuja, jamás aplasta relevancia).

### B. Verificación (lo que venden las certificadoras de proveedores)
- Verificación por **RFC (único por empresa)** — la credencial que abre las
  funciones de operar. Es además el anti-abuso del trial: un RFC = un trial.

### C. El mercado transaccional (lo que venden las plataformas de e-procurement)
- **Cotizaciones (sobre cerrado)**: publicas la necesidad estructurada
  (cantidad, unidad, especificación, adjuntos), la IA se la lleva SOLO a
  quienes pueden cumplirla, cada proveedor manda su precio a ciegas
  (1 oferta + 2 correcciones), comparas y adjudicas. Constancia PDF.
- **Licitaciones EN VIVO (subasta inversa en tiempo real)**: pulso anónimo
  con posición, mejora de ofertas, anti-sniping (+5 min), contraofertas
  estructuradas, presupuesto privado con semáforo, adjudicación dividida
  entre varios ganadores, ciclo de vida automático. ~90 candados en BD.
- **Ruteo demanda→oferta**: cada necesidad publicada notifica únicamente a
  los proveedores del giro (por categorías + embeddings) — la demanda toca
  la puerta del proveedor, no al revés.
- **Chat empresa↔empresa** con contexto de licitación y constancia
  descargable. Enkoras jamás toca el dinero entre empresas: es el testigo
  (posicionamiento "cable conector").

### D. El equipo (lo que venden los SaaS por asiento)
- Multiusuario real: sillas (3 base / 5 / 7), **8 llaves de permisos**,
  propiedad del trabajo (quién lleva cada puja/licitación/chat, con tomar /
  soltar / transferir / arrebatar), candados de edición en tiempo real entre
  compañeros, todo blindado en BD — no en pantalla.
- Panel por empresa: métricas 7/30/90, actividad, mis bids, plantillas
  reutilizables de compra.

---

## 2. El mercado (research 1-sep-2026, fuentes al pie)

### México — directorios y visibilidad
| Jugador | Qué da | Precio |
|---|---|---|
| **Sección Amarilla digital** | visibilidad + marketing, cero operación | paquetes desde **$599 y $1,299 MXN/mes**; usuarios reportan $675–$5,500/mes según venta consultiva |
| **QuimiNet** (líder industrial MX/LatAm) | directorio industrial + requerimientos de compradores | **precio NO público** — venta consultiva por ejecutivo ("se ajusta a su presupuesto") |
| **Kompass México** | directorio global, presencia gratis + planes | precio NO público, venta consultiva |
| **Cylex / México B2B** | listado básico | gratis / simbólico |

**Señal #1:** los dos serios (QuimiNet, Kompass) esconden el precio y venden
por ejecutivo. Un catálogo de precios PÚBLICO y autoservicio ya es un
diferenciador en México.

### Global — el ancla de credibilidad
| Jugador | Qué da | Precio |
|---|---|---|
| **Alibaba Verified Supplier** | directorio global + credencial verificada | **≈ $9,300 USD/año** (~$14,500 MXN/mes) |
| Alibaba Gold (histórico) | membresía base de vendedor | $2,000–6,000 USD/año (~$3,100–9,300 MXN/mes) |

### eSourcing / subasta inversa como software
| Jugador | Qué da | Precio |
|---|---|---|
| **Market Dojo** (UK) | subasta inversa + eSourcing | **£500/usuario/mes** (~$12,500 MXN) |
| **Prokuria** | RFQ/RFP + subastas | **€15–50/usuario/mes** (~$310–1,030 MXN), tiers altos consultivos |
| Promena, Tenderbill, Egixia | subastas electrónicas | mensual o por evento, mayormente consultivo |

**Señal #2:** la subasta inversa, VENDIDA SOLA como software, cuesta desde
$300 hasta $12,500 MXN por usuario al mes. Enkoras la incluye dentro de un
plan PyME **con el mercado de proveedores ya conectado** — Market Dojo te da
la herramienta, tú pones a los proveedores; Enkoras pone ambos.

### El bolsillo PyME mexicano (la referencia de disposición de pago)
| SaaS PyME MX | Planes |
|---|---|
| **Bind ERP** | **$890 / $1,390 / $1,890 MXN/mes** |
| Sección Amarilla | $599 / $1,299 MXN/mes |

**Señal #3:** la PyME mexicana ya paga $600–$1,900 MXN/mes por software que
la ayuda a operar. Esa es la banda psicológica donde Enkoras debe vivir —
y el **$950 del Plan Maestro cae exactamente en el centro de esa banda**.

---

## 3. Posicionamiento de precio

- Contra **Sección Amarilla** ($599–1,299): Enkoras da visibilidad IGUAL o
  mejor (búsqueda IA + disponibilidad viva) **y además opera** — cotiza,
  subasta, equipo. A precio comparable, valor de otra categoría.
- Contra **QuimiNet**: precio público y autoservicio vs. venta consultiva
  opaca; y QuimiNet no tiene subasta inversa ni equipo multiusuario.
- Contra **Market Dojo/Ariba**: 10–20× más barato, y con el directorio de
  proveedores integrado (ellos venden la herramienta vacía).
- Contra **Alibaba**: Enkoras es el juego doméstico/nearshoring — 10× más
  barato que Verified y en el idioma, moneda y RFC de México.

## 4. Recomendación de precios (mensual, MXN, sin plan anual)

| Plan | Mínimo defendible | **JUSTO (recomendado)** | Techo bueno |
|---|---|---|---|
| **Presencia** | $0 | **$0** | $0 |
| **Trial** (Empresa completa, 1 mes, RFC verificado) | gratis | **gratis** | gratis |
| **Premium 1 · Proveedor** (2 empresas, 5 sillas, responde todo) | $499 | **$649** | $799 |
| **Premium 2 · Empresa completa** (4 empresas, 7 sillas, publica todo) | $949 | **$1,190** | $1,490 |
| Silla extra (opcional, futuro) | — | $99–149/silla | — |

**La lógica del JUSTO:**
- **Proveedor $649**: arriba del paquete básico de Sección Amarilla ($599)
  porque da infinitamente más (opera, no solo aparece), y abajo de Bind
  Esencial ($890) porque el ERP es "obligatorio" y el directorio hay que
  ganárselo. La cuota de entrada suave que da liquidez de oferentes — el
  activo que hace valioso al plan caro.
- **Empresa completa $1,190**: banda Bind Profesional ($1,390) menos un
  escalón. El comprador que convoca UNA subasta y ahorra 3–5% en una compra
  de $100,000 ya pagó 3–4 años de plan. El ROI se explica en una frase.
- **Relación P2/P1 ≈ 1.8×**: estándar SaaS (el plan alto entre 1.7× y 2.2×
  del bajo) — suficiente para que el upgrade duela poco y ancle al Proveedor
  como "el barato razonable".
- El **$950 original** queda dentro del rango (mínimo P2 defendible): la
  intuición de la mesa ya estaba bien calibrada; el research la respalda y
  sugiere que hay espacio hacia arriba.

**Contra la meta del Plan Maestro (600 cuentas × $500 = $300K MRR):** con una
mezcla 60% Proveedor ($649) + 40% Empresa completa ($1,190), el ticket
promedio es **$865** → la meta de $300K MRR se alcanza con **~347 cuentas**
en vez de 600. Con solo 600 cuentas a esa mezcla: $519K MRR.

## 5. Lo que queda para la mesa de socios
1. ~~Elegir columna (mínimo / justo / techo)~~ — **decidido 1-sep: JUSTO**
   (default de Javi, reversible; los socios validan con sus análisis).
2. ¿Silla extra como palanca desde el día 1, o después?
3. El precio de lanzamiento: ¿descuento fundador (p. ej. -20% de por vida a
   las primeras 100 cuentas) para sembrar liquidez de proveedores?
4. Validar la mezcla asumida (60/40) contra la conversión real del trial
   cuando haya datos.

## Fuentes
- QuimiNet — precios no públicos, venta consultiva: quiminet.com/ayuda/como_me_anuncio.php
- Sección Amarilla — paquetes: negocios.seccionamarilla.com.mx/paquetes/ · reportes de usuarios: answers.mx
- Kompass — presencia gratis + planes consultivos: es.solutions.kompass.com
- Alibaba Verified/Gold: seller.alibaba.com/pages/price.html · jingsourcing.com/b-alibaba-gold-supplier · accio.com
- Market Dojo pricing: marketdojo.com/pricing-enterprise · alternatives.co
- Prokuria pricing: g2.com/products/prokuria/pricing · prokuria.com/pricing
- Bind ERP precios MX: capterra.com/p/190643/bind-erp · bind.com.mx
