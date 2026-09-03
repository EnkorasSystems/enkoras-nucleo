# Pendientes menores aparcados (no accionables hoy)

Nota pedida por Javi (2-sep-2026): estos NO van en la cola activa — viven aquí
para no perderse y no hacer ruido. Solo se tocan si su condición se cumple.

## 0. Cuota de Gemini → plan de pago (decisión diferida CON análisis)

- **Hoy:** capa gratuita con 5 llaves de cuotas independientes (y por par
  llave+modelo); techo práctico ≥400 clasificaciones/día en el peor
  escenario medido. Sobra para el flujo actual; falla suave + backfill si
  se rebasa. Alcanza incluso para el semillero de 300 en un día.
- **Cuándo actuar:** al escalar el flujo real (o si el semillero se carga
  con prisa y queremos cero degradación).
- **Esquemas que Javi puso sobre la mesa (2-sep, SIN decidir):**
  (a) ~$200 USD en una llave para clasificación + las otras 4 con ~$25
  para puro embedding, o (b) ~$200 repartidos en 2 llaves de clasificación
  + 3 de embedding. *"Después lo vemos con tu análisis bien de qué es lo
  mejor."*
- **Nota para ese análisis:** el cobro de la API de Gemini es pay-as-you-go
  POR PROYECTO (la llave pertenece a un proyecto) — no hay "plan de $200"
  prepagado; se activa facturación en el proyecto y se ponen topes/alertas
  de presupuesto. El análisis debe traducir sus montos a topes por
  proyecto, dimensionar el costo real por clasificación/embedding y
  decidir cuántos proyectos con billing vs. free.
- **Mismo momento:** revisar con Javi el modelo fijado (gemini-3.5-flash,
  amarrado tras la caída del 31-ago) — probar si conviene uno más nuevo.

## 1. Migrar embeddings — SOLO si Google retira `embedding-001`

- **Qué es:** todos los vectores del search híbrido (empresas, servicios,
  licitaciones) están generados con el modelo `embedding-001` de Google.
- **Cuándo actuar:** únicamente si Google anuncia su retiro. Mientras el
  modelo viva, no hay nada que hacer.
- **Qué implicaría:** re-generar todos los embeddings con el modelo sucesor
  (cuesta cuota de API, no dinero) y validar que el umbral del ranking siga
  sano. Es un script de una tarde, no una obra.

## 2. Sombras `rgba()` a mano en el admin — esperar al rediseño

- **Qué es:** 4 archivos del panel `/admin` (gráficas y tablas) tienen las
  sombras escritas a mano en vez del token del design system. Se ven
  idénticas hoy; solo quedarían atrás si el sistema de sombras cambiara.
- **Decisión de Javi (2-sep):** dejarlas así — "probablemente en unos días
  tengamos que rediseñar la sección de admin"; no tiene caso pulir lo que
  se va a redibujar. Se resuelve solo dentro del rediseño.

## 3. Barrido de huérfanos de Storage — preventivo, hoy CERO

- **Estado real:** la CAUSA ya se arregló (commit 8d07fce, 31-ago): el orden
  de borrado de cuenta es Stripe → usuario → Storage, se limpian los 4
  buckets, y desde entonces no se genera ni un huérfano nuevo. Medido en
  prod: **cero archivos huérfanos**.
- **Qué queda:** nada operativo. Este pendiente es solo "si algún día un
  borrado falla a mitad de camino y deja un archivo colgado, hacer un script
  que barra". Como hoy no existe ese archivo, no hay nada que construir.
