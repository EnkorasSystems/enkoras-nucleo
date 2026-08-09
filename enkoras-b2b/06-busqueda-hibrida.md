# Búsqueda híbrida + filtros — bloque 1.E

Ruta `/buscar` (todo vive en la URL: `?q=&estado=&ciudad=`). Tres capas
fusionadas en un solo ranking, con las cards de resultado rediseñadas.

## Motor (`lib/busqueda/`)

- **`ranking.ts` — PURO, sin red** (testeable directo):
  - Texto: nombre 0.4 > servicio 0.3 > giro 0.25 > descripción 0.15 (tope 1)
  - Semántica: mejor coseno entre los servicios de la empresa
  - `score = 0.55·sem + 0.45·texto + bonos` (disponibilidad vigente +0.15,
    verificada +0.10, rating hasta +0.05)
  - Corte semántico DINÁMICO (afinado con datos reales el 2-ago): piso
    absoluto `UMBRAL_SEMANTICO = 0.55` + corte relativo `top − BRECHA_SEMANTICA
    (0.09)` — el que resulte más exigente. gemini-embedding-001 comprime los
    cosenos en ~0.55-0.75 y el ruido industrial ronda 0.57-0.62, por lo que un
    umbral fijo no separa; la brecha contra el mejor resultado sí. Con hit de
    texto el corte no aplica
  - Orden determinista: score → rating → id
- **`hibrida.ts` — orquestación de servidor**:
  1. Con consulta: 3 capas en paralelo — ILIKE empresas, ILIKE servicios,
     y embedding de la consulta → RPC `buscar_servicios_semantico` (falla
     suave: sin embedding la búsqueda degrada a texto)
  2. Universo de candidatos con filtros estado/ciudad aplicados en Postgres
  3. Disponibilidad vigente por empresa (señal de ranking + dato de card)
  4. Fusión con el ranking puro; máx 24 resultados
  - Sin consulta: directorio ordenado por señales (disponibilidad arriba)
- **Regla de oro intacta**: cero LLM al buscar. La única llamada externa es
  UN embedding por búsqueda (con reintentos 429/5xx heredados de lib/ia).
- La capa semántica es **multilingüe**: "warehouse storage" encuentra
  catálogos escritos en español (verificado en vivo).

## UI

- `FiltrosBusqueda` (client): input + botón naranja + Selects estado→ciudad
  (custom) + Limpiar. Navega con URLSearchParams — compartible/bookmarkeable.
- `ResultadoCard`: card horizontal premium — logo, verificada, giro,
  ciudad · rating · chips de categoría, y dos líneas de contexto que ningún
  directorio genérico da:
  - **Coincide:** el servicio que hizo match (semántico o texto), con Sparkles
  - **Disponible ahora:** punto verde pulsante + la disponibilidad más próxima
    a vencer (+N más)
- Skeleton de filas horizontales en `loading.tsx`.

## Split view (idea de Javi, estilo Indeed/Computrabajo — 2-ago-2026)

`/buscar` y `/categoria/[slug]` son master-detail (modelo Indeed): lista a la
izquierda + perfil a la derecha, cada columna con scroll INTERNO. **La
selección viaja en la URL como `?sel=slug`** — un refresh (F5/Ctrl+R)
restaura búsqueda, lista y anuncio abierto (el modelo de rutas interceptadas
se descartó: al refrescar perdía el contexto y dejaba una sola columna).

- Sin `?sel`, el servidor abre el PRIMER resultado por default (solo visible
  en desktop; en móvil el panel solo aparece con selección explícita, como
  overlay a pantalla completa con "Volver a resultados" que quita `sel`)
- El panel (`PanelDetalle`) lleva `key={slug}`: cambiar de anuncio arranca
  el scroll arriba; la lista conserva su posición
- **SEO intacto**: la página canónica `/empresa/[slug]` sigue existiendo
  completa; el contenido vive UNA vez en `components/empresa/PerfilEmpresa.tsx`
- La card activa (sel o default) queda resaltada con ring naranja

## Pruebas

`tests/busqueda/ranking.test.ts` — 10 candados puros: umbral corta ruido,
texto+semántica > solo-semántica, nombre > descripción, disponibilidad y
verificación empujan, empates deterministas, y el modo sin consulta no filtra.
