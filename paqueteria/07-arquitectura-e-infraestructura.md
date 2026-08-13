# ALTAM — Arquitectura e Infraestructura (planeación)

> La planeación técnica del sistema: qué infraestructura es la **correcta** para este tipo
> de sistema (última milla, ~1,000 paquetes/día, 3 componentes), el porqué de cada
> elección, y de ahí el costeo (arranque + mensual) que David pidió (Pregunta 4).
> 9-ago-2026. Números = estimados realistas a verificar al contratar.

---

## 0. Qué tipo de sistema es (para no equivocar la arquitectura)

No es una web más. Es una **plataforma de operaciones de última milla** con tres
componentes que hablan entre sí, y con características que mandan en las decisiones:

- **Tiempo real** (mapa de choferes, dashboard, escaneos que se ven al momento).
- **Móvil de campo** (choferes con mala señal a veces → tiene que funcionar **offline**).
- **Muchas fotos** (~1,000+ al día → el storage es el costo que más crece).
- **Geo** (zonas por CP, cercanía, verificación de entrega por ubicación → base con PostGIS).
- **Integración** con el sistema del cliente (iMile: manifiesto entrante + estatus saliente).
- **Multi-inquilino desde el día 0** (Enkoras construye y RENTA — hoy ALTAM, mañana otros).

---

## 1. Principios de la arquitectura

1. **Reusar lo que ya dominamos** (Next.js + Supabase + Vercel + Gemini) donde aplica, y
   agregar UNA sola pieza nueva (la app móvil). Concentra el riesgo en un solo lugar.
2. **Multi-tenant desde el día 0** — `tenant_id` + RLS en todo. Rediseñar esto con datos en
   producción cuesta 10×.
3. **Offline-first en el móvil** — el chofer escanea/entrega sin señal y sincroniza al
   volver (lo vimos hasta en la app de iMile). No es opcional en reparto.
4. **La IA trabaja al escribir, no al leer** — OCR una vez por foto; el dashboard es
   matemática sobre datos ya guardados. Costo de operación cercano a cero.
5. **Capa de integración por adaptadores** — iMile entra por un módulo aislado (API o
   exports); cambiar la fuente = reescribir un archivo, no el sistema.
6. **Batching donde importa** — el ruteo se calcula por ruta/zona (un puñado), no por
   paquete (miles). Eso mantiene el costo de mapas bajo.

---

## 2. Los 3 componentes y su stack (con el porqué)

### A. Backend / API + Datos — **Supabase**
El cerebro. Supabase da en una sola plataforma:
- **Postgres + PostGIS** — modelos (manifiesto, bins, choferes, unidades, incidencias,
  combustible) y **geo real** (zonas por CP, cercanía al almacén, verificación de entrega
  por distancia). PostGIS es clave y Supabase lo soporta nativo.
- **Auth + RLS** — login de choferes (móvil) y supervisores (web) con roles; RLS como
  autoridad y aislamiento por tenant.
- **Storage** — las fotos (POD de entrega, no-entrega, tanque/odómetro).
- **Realtime** — el mapa de choferes en vivo y los dashboards.
- **Jobs y colas** — `pg_cron` (reintento diario de no entregados, cálculo de
  mantenimiento por km) + `pgmq` (cola de eventos salientes hacia iMile). Todo dentro de
  Postgres, sin infra extra.
- **Edge Functions** — lógica de servidor puntual (ingestión de manifiesto, auto-asignación
  de gaylord, disparar OCR, comparar geo de entrega).
- *Si el procesamiento en background creciera mucho* (GPS a gran escala, sync pesada con
  iMile), se agrega un **worker chico** (Railway/Render ~$5-7/mo). No para v1.

### B. Panel Web (supervisor/gerencia) — **Next.js en Vercel**
La fórmula probada de Enkoras. Dashboards en tiempo real (vía Supabase Realtime), CRUDs,
corrección de rutas, saturación, reportes. Cero novedad — es lo que ya haces.

### C. App Móvil (chofer) — **React Native + Expo**  ← la ÚNICA pieza nueva
Es lo genuinamente nuevo, y Expo es la elección correcta por:
- **Un solo código para Android + iOS** (los choferes tienen de los dos).
- **Módulos listos** para todo lo que pide el scope: cámara + **escaneo de código de
  barras** (`expo-camera`), **GPS en background** (`expo-location` + `expo-task-manager`),
  fotos, **push** (Expo Notifications, gratis).
- **Mismo lenguaje que el web** (TS) → reusas el cliente de Supabase, los tipos y las
  validaciones (Zod) entre web y móvil. El modelo mental transfiere.
- **EAS Build** compila el APK y el IPA en la nube (no necesitas Mac para iOS).
- **Offline** con SQLite local + cola de sincronización.
- **Estrategia: Android primero.** La mayoría de repartidores usan Android → se lanza
  Android-only ($25 una vez) y iOS se agrega cuando se necesite ($99/año). Arranca más
  barato y más rápido.

### Servicios de terceros
- **OCR / visión → Gemini** — ya lo dominas de Enkoras. Corre en las fotos de **odómetro**
  (inicio/fin por unidad, ~fijo) y de **cada carga de gasolina** (variable e impredecible:
  una unidad puede cargar 0, 1 o varias veces según el recorrido — **sin tope**). Aunque el
  número vuele a 50-100+/día, el costo es un error de redondeo: cada llamada cuesta
  fracciones de centavo → aun a ~3,000/mes son ~$2-6, buena parte en capa gratis. Lee
  dígitos de odómetro y del ticket de gasolina sin problema. **El OCR NO es un costo que
  escale con el volumen — el que escala es el storage de fotos de entrega.**
- **Mapas (mostrar flotilla y puntos) → Mapbox o MapLibre** — NO Google para el mapa: a
  este volumen Mapbox (50k cargas/mes gratis) o MapLibre + tiles gratis (OpenStreetMap)
  salen mucho más baratos que Google Maps para *desplegar* el mapa.
- **Ruteo (orden de paradas dentro de zona) → Google Routes v1**, con **VROOM/OSRM
  self-hosted** como plan B si el costo sube. Como se calcula por ruta (no por paquete), el
  volumen de llamadas es bajo → costo bajo.
- **Geo de CPs** → tu JSON de SEPOMEX (ya lo tienes) + coordenadas (del manifiesto si las
  trae, o enriquecidas una vez). Sin API de paga recurrente.
- **Errores/monitoreo → Sentry** (capa gratis). **Backups** → incluidos en Supabase Pro.

---

## 3. El diagrama

```
   ┌─────────────────┐         ┌──────────────────────┐
   │  iMile (DS/API)  │◄───────►│  Adaptador iMile      │  manifiesto entrante
   └─────────────────┘         │  (Edge Function)      │  estatus saliente (cola pgmq)
                               └──────────┬───────────┘
                                          │
   ┌──────────────┐   Supabase JS   ┌─────▼──────────────────────────┐
   │  App Móvil    │◄───────────────►│  SUPABASE                       │
   │  (Expo, RN)   │  auth/data/     │  Postgres+PostGIS · Auth/RLS ·  │
   │  chofer:      │  storage/       │  Storage(fotos) · Realtime ·    │
   │  escaneo,     │  realtime       │  pg_cron(jobs) · pgmq(cola) ·   │
   │  fotos, GPS,  │                 │  Edge Functions                 │
   │  offline      │                 └─────▲──────────────────────────┘
   └──────────────┘                        │ Realtime + API
                                    ┌───────┴────────────┐
   Gemini (OCR) ─ Mapbox (mapa) ─   │  Panel Web          │
   Google Routes (orden paradas)    │  (Next.js / Vercel) │  supervisor/gerencia
                                    └────────────────────┘
```

---

## 4. Decisiones clave (y sus alternativas)

| Decisión | Elección v1 | Alternativa / cuándo cambiar |
|---|---|---|
| Backend + datos | Supabase (todo-en-uno) | Worker dedicado si el background crece |
| App móvil | React Native + Expo | Flutter (si prefieres Dart; pierdes el reuso con el web) |
| Plataforma móvil | **Android primero** | iOS cuando se justifique (+$99/año) |
| Mostrar mapa | Mapbox / MapLibre | Google Maps (más caro para desplegar) |
| Orden de paradas | Google Routes | VROOM/OSRM self-hosted (más barato a volumen) |
| Storage de fotos | Supabase Storage | Cloudflare R2 (sin egreso, más barato al crecer) |
| OCR | Gemini | Google Cloud Vision |

**El costo que más crece: las fotos.** ~1,000+ fotos/día. Mitigación obligatoria:
**comprimir en el celular antes de subir** (~300 KB en vez de 3 MB) → recorta storage y
datos ~10×. Y definir **política de retención** (¿cuánto se guardan las fotos de POD?
iMile las conserva ~14-30 días; ALTAM decidirá) para que el storage no crezca infinito.

---

## 5. Costeo — Pregunta 4 de David

> Todos los números son estimados realistas (USD) a verificar al contratar. El **volumen
> base** es ~1,000 paquetes/día (~30k/mes), 10-15 choferes.

### 5.1 Costo de arranque (una sola vez)
| Concepto | Costo | Nota |
|---|---|---|
| Google Play (cuenta desarrollador) | **$25** | una vez, de por vida |
| Apple Developer | **$99** | *solo si se lanza iOS en v1; se puede diferir* |
| Dominio | **~$12-15** | .com anual (técnicamente recurrente pero mínimo) |
| **Total arranque (Android-only)** | **~$40** | lo mínimo para salir |
| **Total arranque (Android + iOS)** | **~$140** | si iOS desde el día 1 |

### 5.2 Costo mensual recurrente
| Servicio | v1 (arranque) | A volumen real (~1,000/día) | Nota |
|---|---|---|---|
| **Supabase Pro** | $25 | $25 + storage | 8GB DB, 100GB storage, backups, sin pausa |
| Storage de fotos (extra) | $0 | ~$1-3 y creciendo | con compresión + retención; sin retención sube a ~$20-30 a 2 años |
| **Vercel Pro** | $20 | $20 | panel web |
| Apple Developer (prorrateado) | $0-8 | $8 | $99/año si hay iOS |
| **Gemini (OCR)** | $0 | ~$0-6 | odómetro + cada carga de gasolina (variable, sin tope); barato aun a 100+/día |
| **Mapbox** (mapa) | $0 | $0-10 | 50k cargas/mes gratis; supervisores pocos |
| **Google Routes** (orden paradas) | $0-5 | ~$2-10 | se calcula por ruta, no por paquete = barato |
| Expo EAS Build | $0 | $0-99 | capa gratis alcanza al inicio; Production si hay muchos builds |
| Sentry / dominio / misc | ~$1 | ~$2 | monitoreo gratis + dominio prorrateado |
| **TOTAL MENSUAL** | **~$50-70** | **~$70-120** | crece sobre todo por fotos |

### 5.3 Los tres escenarios
- **Arranque (poco volumen, Android-only):** ~$40 una vez + **~$50/mes**.
- **Operación real (~1,000/día):** **~$70-120/mes** (lo mueve el storage de fotos y, en
  menor medida, mapas/ruteo).
- **A escala / con iOS / sin retención de fotos:** ~$150-200/mes — y aquí es donde
  conviene migrar fotos a **Cloudflare R2** y ruteo a **self-hosted** para bajar el mensual.

---

## 6. La conclusión que importa para el negocio

**La infraestructura NO es el costo del proyecto — es sorprendentemente barata.** Un
sistema que maneja ~1,000 entregas al día corre por **~$70-120 al mes**. Eso es clave para
el modelo de renta de Enkoras: el **costo marginal por cliente es bajísimo**, así que lo
que ALTAM pague de renta es casi todo margen una vez construido.

**El costo real del proyecto es el tiempo de desarrollo** (construir las 3 piezas,
sobre todo la app móvil nueva). Eso es inversión de una vez que se amortiza con la renta.

**Para Fer y David** (que necesitan el número para poner precio): pueden comprometerse con
el cliente sabiendo que la operación mensual les cuesta **~$100 USD** aun a volumen alto —
cualquier renta razonable (miles de pesos al mes) deja margen amplio. El proyecto es
rentable por diseño; el gasto fijo es mínimo.

---

## 7. Pendientes que afinan el costeo
- ¿El manifiesto de iMile trae **coordenadas**? (evita geocodificar → menos costo).
- **Política de retención** de fotos (define el storage a 1-2 años).
- ¿**iOS** desde v1 o Android primero? (+$99/año y trabajo de publicación).
- Volumen real de fotos por paquete (¿todas llevan foto, o solo entregas/incidencias?).
