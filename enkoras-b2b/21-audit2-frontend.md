# AUDIT 2 — Frontend pre-lanzamiento (2-sep-2026)

Repo: `C:\Users\enriq\Documents\directorio-b2b` · Estándar: cursos A/B/C de `enkoras-nucleo/recursos`
Alcance: todo el frontend construido HOY (nosotros v3, Halo universal, MatrizPlanes, PanelAuth, wizard revisión, bloques nuevos de globals.css). Cada hallazgo está verificado en el código; se citan archivo:línea reales.

## Números

- **`pnpm build`: ✅ exit 0** — Next 16.2.9 (Turbopack), compila en 7.7s, TypeScript 13.7s, **71/71 páginas estáticas generadas** (1.1s). 3 warnings de `metadataBase` (ver S4-10).
- **`npx vitest run`: ❌ exit 1** — **279 pasan / 3 fallan (282 tests, 43 archivos: 41 pasan / 2 fallan)**, 139s. Los 3 fallos son de **tests de DB**, no de frontend: `tests/db/trial-nacimiento.test.ts` (1: "con el flag apagado los avisos DUERMEN…") y `tests/db/candados-plan.test.ts` (2: "candado_de: apagado…", "mi_plan_estado: los topes del mundo de hoy").
- Bundle de Aurora/ogl: chunk cliente propio de **48.0 KB (14.6 KB gzip)** (`.next/static/chunks/237_*.js`), code-split correcto — solo lo cargan /nosotros y las páginas de auth.

---

## S2 — Graves (función visible rota o pérdida de datos en ruta de error)

### S2-1 · Aurora queda EN BLANCO con `prefers-reduced-motion` (el frame estático se borra solo)
`components/nosotros/Aurora.tsx:144-175` + `node_modules/ogl/src/core/Renderer.js:115-127`

Cadena exacta del fallo:
1. Con reduced-motion (`quieto`), el efecto pinta **un solo frame** estático (líneas 170-172) y el rAF-loop jamás corre.
2. Pero en la línea 145 se registró `new ResizeObserver(redimensiona)` — y por spec **todo `observe()` dispara una notificación inicial asíncrona** (llega después de que el efecto ya pintó).
3. `redimensiona` → `renderer.setSize(w,h)` → ogl hace `this.gl.canvas.width = width * this.dpr` **incondicionalmente** (Renderer.js:119) — y asignar `canvas.width`, aunque sea el mismo valor, **borra el buffer del canvas** por spec HTML.
4. No hay re-render porque el loop está apagado → **canvas transparente para siempre**.

Resultado: para usuarios con "reducir movimiento" el hero de /nosotros es tinta plana sin amanecer, el CTA final igual, y el panel de identidad del login pierde su fondo — exactamente lo contrario de lo que promete el comentario del componente ("reduced-motion = un solo frame estático… el amanecer existe, quieto"). Lo mismo pasa en cualquier **resize posterior** (rotar el teléfono, redimensionar ventana) aunque el primer frame hubiera sobrevivido. Fix conceptual (no aplicado): re-render tras cada `setSize` cuando `quieto`.

### S2-2 · El thead sticky de la matriz de planes NO se pega nunca
`components/planes/MatrizPlanes.tsx:162` (wrapper) vs `:166,176` (th sticky)

Los `<th>` llevan `sticky top-0 z-10`, pero la tabla vive dentro de `<div className="hidden md:block … overflow-hidden">` (línea 162, por el `rounded-2xl`). Un ancestro con `overflow: hidden` se convierte en el contenedor de scroll del sticky → el th se "pega" al tope de esa caja **que no scrollea jamás**, no al scroll real de la app (el `main` con `overflow-y-auto` de `app/[locale]/(public)/layout.tsx:31`). Input → rompe: al bajar comparando filas en escritorio, los encabezados de plan/precio se van con la tabla; el patrón que el propio comentario del componente promete ("thead sticky para no perder el contexto al comparar") está inerte. Verificado en CSS compilado: no hay ningún otro mecanismo. Fix conceptual: mover el redondeo/clip a otra técnica (`border-radius` en celdas extremas, o `overflow: clip` con `overflow-clip-margin`… ojo: `clip` también crea clipping — la vía limpia es quitar el overflow del wrapper y redondear thead/última fila).

### S2-3 · Wizard revisión: el guardado ignora el resultado — "éxito" aunque las categorías NO se guardaron
`components/registro/WizardEmpresa.tsx:399-407` vs `app/actions/empresa.ts:215-237`

`actualizarCategoriasEmpresa` regresa `{ ok: boolean; error?: string }` (devuelve `{ok:false}` por no-auth, selección vacía o error del RPC `reemplazar_categorias`). El wizard hace:

```ts
setGuardando(true)
await actualizarCategoriasEmpresa(companyId, seleccion)   // ← retorno IGNORADO
setGuardando(false)
setFase("exito")
```

Dos fallos concretos:
1. **Éxito falso**: si el action regresa `{ok:false}` (sesión expirada tras un wizard largo + subida de fotos — escenario real), el usuario ve la pantalla de éxito y su empresa queda con las categorías de la IA o **sin el ajuste que él hizo a mano** — invisible para el ruteo de oportunidades, sin ningún aviso. Es la única puerta de escritura de categorías del onboarding.
2. **Spinner eterno**: si el server action **lanza** (red caída), no hay try/catch → unhandled rejection, `guardando` queda `true` y el botón queda deshabilitado con spinner para siempre.

Contraste con el mismo archivo: `crearEmpresa` sí valida `resultado.ok` (líneas ~350-360). Aquí falta el mismo trato.

---

## S3 — Importantes (costo real de rendimiento, a11y o corrección)

### S3-1 · Login móvil paga un contexto WebGL que nadie ve
`components/auth/PanelAuth.tsx:79-81`

El `<aside>` es `hidden lg:flex`, pero `display:none` **no evita el montaje React**: `Aurora` corre su efecto completo en móvil — crea el contexto WebGL, compila el shader simplex, agrega el canvas, y hasta corre 1-2 frames de rAF sobre un canvas de 1×1 (el `visible` inicial es `true` hasta que el IntersectionObserver reporta `false` en Aurora.tsx:149-155). Costo: creación de contexto + compilación de shaders (decenas de ms de main thread en gama media) **en la página de login móvil**, la ruta más sensible a TTI del funnel. Fix conceptual: montar Aurora solo ≥lg (matchMedia/CSS container) o lazy con `dynamic`.

### S3-2 · `.cascada-vista` deja contenido invisible si el JS no llega
`app/globals.css:846` + `components/nosotros/CascadaEnVista.tsx:14-16`

`.cascada-vista > * { opacity: 0 }` aplica **incondicionalmente desde el CSS servido** (sin `@supports`, sin guardia de JS); la clase `.viva` que dispara la animación depende de que hidrate React y dispare `useInView`. Input → rompe: sin JS / hidratación fallida / crawler sin render, las **cifras** (518 categorías…), el **bento del ecosistema** y las **cards de socios** de /nosotros quedan `opacity:0` permanente. El patrón robusto de la propia casa es el de `.aparece-scroll` (globals.css:516-522): esconder solo dentro de `@supports` o con una clase `js` en `<html>`. Nota: Google ejecuta JS, así que el SEO sobrevive — el riesgo real es no-JS y errores de hidratación.

### S3-3 · `.texto-brillo` sobre KOR: el brillo es invisible (acento sobre acento) — repintado infinito para nada
`app/globals.css:892-899` + `app/[locale]/(public)/nosotros/page.tsx:187`

El gradiente es `linear-gradient(110deg, currentColor 40%, var(--color-accent) 50%, currentColor 60%)`. El único uso es `"texto-brillo text-accent"` → **`currentColor` ES `--color-accent`** → las tres paradas son el mismo naranja #FF6803. El barrido existe (anima `background-position` 3.4s infinito = repintado continuo del texto de 72px, no composited) pero **no produce ningún efecto visible**. Es el peor trato posible: costo de paint permanente, cero payoff. Fix conceptual: parada central más clara (p.ej. `#FFB566` o blanco), o quitar la clase.

### S3-4 · Marquesina de valores: bajo reduced-motion PIERDES los valores 5-8; en touch no hay forma de pausarla
`app/[locale]/(public)/nosotros/page.tsx:284-303` + `app/globals.css:880-883`

- Con `prefers-reduced-motion`, `.pista-marquesina { animation: none }` congela la pista en su inicio dentro de una ventana `overflow-hidden` **sin scroll**: en un laptop se ven ~4 de los 8 valores; los demás son inalcanzables visualmente (el DOM sí los tiene, pero un usuario reduced-motion vidente no los ve jamás). El estado estático debería ser un grid/wrap, no la pista recortada.
- WCAG 2.2.2 (curso A §41, línea 1323: "todo contenido en movimiento que dura >5s… debe poder pausarse"): la única pausa es `:hover` (globals.css:842) — **en touch no existe hover** → el loop de 52s corre sin control. Falta pausa por focus/tap o un botón.

### S3-5 · Las animaciones scroll-driven nuevas IGNORAN `prefers-reduced-motion`
`app/globals.css:861-879` (`.linea-dibuja`, `.hito-lado-izq/der`, `.hito-punto`) — y de paso `.aparece-scroll` (516-522, preexistente pero sembrada ~20 veces hoy en /nosotros)

El bloque global de reduced-motion (globals.css:915-926) solo pisa `animation-duration` / `iteration-count` — pero con `animation-timeline: view()` **la duración temporal se ignora por spec**: la animación sigue mapeada al scroll y corre completa. Input → rompe: un usuario con vestibular disorder que activó "reducir movimiento" ve igual los hitos deslizándose ±26px, la línea creciendo y los puntos escalando al scrollear /nosotros. El curso A §41 (línea 1305) lo lista explícito: "**Eliminar bajo reduced-motion: … scroll-driven effects**". Falta un `@media (prefers-reduced-motion: reduce) { .linea-dibuja, .hito-lado-izq, .hito-lado-der, .hito-punto, .aparece-scroll { animation: none } }`.

### S3-6 · A11y de las páginas de auth reescritas hoy
`app/[locale]/auth/login/page.tsx` y `register/page.tsx`

- **Labels sin asociar**: todos los `<label className={labelAuth}>` carecen de `htmlFor` y los inputs de `id` (login:120-128, 132-141; register:124-132, 136-146, 158-167). Un lector de pantalla en el input de contraseña no anuncia "Contraseña". No envuelven al input, así que no hay asociación implícita tampoco.
- **Botón del ojo sin nombre accesible ni target**: login:143-149 y register:147-153 — botón icon-only (Eye/EyeOff 15px) sin `aria-label`, y su área táctil es el propio icono ≈15×15px → falla incluso el mínimo WCAG 2.5.8 de 24×24 (curso UI línea 1962).
- (El link fantasma `text-white/0` se reporta en S4-8.)

### S3-7 · Contrastes nuevos que fallan AA (fuera de las decisiones aceptadas)
- **`text-white/40`** sobre tinta #0B0501 = **3.73:1** en texto de 11-12px: `nosotros/page.tsx:198` (`nombreNota`, text-xs) y `:267` (`manifTitulo`, 11px bold uppercase). AA pide 4.5:1 en ese tamaño.
- **`text-white/45`** = **4.49:1** (justo debajo del umbral): `nosotros/page.tsx:193` (etiquetas "de …", 12px bold) y `:355` (chips tachados no_vender/fabricar/transportar, 13px). Borderline — un pelo de alpha (0.50) los pasa.
- **FocoVivo `opacity-45`** (`components/nosotros/FocoVivo.tsx:36`): las palabras no enfocadas del H2 de cultura quedan #1f1f1f al 45% sobre el fondo blueprint #E9E8E5 ≈ **2.69:1** — falla incluso el 3:1 de texto grande, y son contenido (los verbos de la cultura), no decoración. Con reduced-motion irónicamente se ven mejor (todas al 100%). Subir el piso a ~opacity-60.
- Verificado que NO re-flaggeo: CTA blanco/naranja 2.9:1, estrellas accent, light-only, sombras admin (aceptados).

### S3-8 · Inputs de 14px = zoom forzado de iOS en las pantallas nuevas
Curso UI líneas 718 y 1139: "**font-size de inputs ≥ 16px en móvil. Sin excepciones**."
- `inputAuth` (`components/auth/PanelAuth.tsx:30`) — `text-sm` (14px): email/contraseña del login y registro.
- `inputCls` del wizard (`components/registro/WizardEmpresa.tsx:46`) — `text-sm`: incluye el buscador de categorías de la fase revisión nueva (WizardEmpresa.tsx:590-595).
- `SearchBox` (`components/search/SearchBox.tsx:40`) — `text-sm` en el input del hero (Halo sembrado hoy ahí).
Safari iOS hace zoom al enfocar y desplaza el layout. No hay `maximum-scale` en el viewport (bien — no se "arregla" así), la vía es `text-base` en móvil.

---

## S4 — Menores / deuda controlada

1. **Halo sin caché de rect ni throttle** (`components/ui/Halo.tsx:20-24`): `getBoundingClientRect()` del padre en **cada** `mousemove` (mouse de 1000Hz = cientos/s). Hoy no fuerza reflow (solo cambian custom props de paint) y solo corre en la card hovereada, pero es gratis cachear el rect en `mouseenter` y escribir con rAF. Mismo patrón en `Reflector` (`components/nosotros/Reflector.tsx:17-23`). Los **360 listeners** de /buscar con 120 cards (límite real: `buscar/page.tsx:40` → `Math.min(120,…)`) son solo memoria idle — la limpieza al desmontar está correcta (ver "limpio").
2. **`hito-entra-der` puede asomar ~2px de scroll horizontal transitorio** en móvil: parte de `translateX(26px)` (globals.css:859) y el contenido llega a 24px del borde (`CAJA` px-6); el `main` scroller (`(public)/layout.tsx:31`) tiene `overflow-y-auto` → `overflow-x` computa `auto`. Efecto: 2px de vaivén lateral posible mientras el hito entra. Un `overflow-x: clip` en la sección de historia lo sella.
3. **Semántica de la tabla de planes** (`MatrizPlanes.tsx:199-212`): la fila de grupo usa `<th scope="colgroup">` que abarca 1 columna + 3 `<td aria-hidden/>` — lo correcto es `colSpan={4}`; los SR de tabla anuncian celdas vacías. Y `aria-label` sobre `<Check>` (svg de lucide, sin `role="img"`, :75-84) y sobre el `<span>—</span>` (:87, rol generic donde aria-label está desaconsejado) se anuncian de forma inconsistente — mejor `<span class="sr-only">`.
4. **Segmented móvil de la matriz**: botones `h-9` = 36px (<44px táctil, curso UI:714) y el cambio de plan no anuncia nada a SR (sin `aria-live` en la lista re-montada, `MatrizPlanes.tsx:106-158`).
5. **El "—" de no-incluido al 2.25:1** (`MatrizPlanes.tsx:87`, `text-fg-subtle/60` = #767676 al 60%): como indicador visual único de "no incluido" queda por debajo del 3:1 de objetos gráficos (WCAG 1.4.11) para baja visión.
6. **`lista-anim` sin `animar-pop` en las cards del tablón** (`licitaciones/(sala)/page.tsx:721,732`): el contenedor reparte delays (globals.css:790-799) pero las cards no traen animación de entrada → delays muertos, cero cascada. Inconsistente con /buscar.
7. **Loops que no se pausan fuera de viewport** (curso A línea 299): `OrbitaViva` (3 anillos + latido, CSS infinito sin mecanismo tipo `.nexo-pausado`) y las 2 órbitas del PanelAuth (:83-88). Además el panel de login acumula **4 loops infinitos visibles a la vez** (aurora + 2 órbitas + `animate-pulse` del punto vivo) contra el presupuesto de la casa de 1 por pantalla (citado en el propio comentario de nosotros/page.tsx:33-34); en /nosotros el hero también dobla (aurora + logo `animar-orbita-lenta`, :91).
8. **Link fantasma "Volver al inicio"** (`PanelAuth.tsx:101-103`): `text-white/0` — texto invisible hasta hover dentro del link de marca; los videntes no lo descubren y duplica el link real de :130-135. O visible o fuera.
9. **Hero de /nosotros con `pointer-events-none`** (:88): el H1 y la definición no se pueden seleccionar/copiar. Bastaría `pointer-events:none` solo en las capas decorativas (que ya lo tienen).
10. **`metadataBase` warnings ×3 en build**: `[locale]/layout.tsx:39` sí lo define — las quejas vienen de rutas fuera de `[locale]` (p.ej. `/_not-found`) o de `NEXT_PUBLIC_SITE_URL` ausente en el build local; en prod con la env presente quedan 0. Verificar la env en el pipeline.
11. **Aurora con `antialias: true`** (Aurora.tsx:110): MSAA no aporta nada a un gradiente fullscreen difuso; quitarlo es GPU gratis en móvil.
12. **Descripción de la hoja pública sin `break-words`** (`licitaciones/[id]/page.tsx:202`): `whitespace-pre-line` no parte palabras — una URL larga pegada por el convocante desborda horizontal en móvil. (El resto de la hoja: limpia, CTAs h-11=44px ✓.)
13. **FocoVivo**: `quieto` nace `false` → con reduced-motion hay 1 frame con la primera palabra en acento antes del efecto (FocoVivo.tsx:17-21, mismatch visual, no de hidratación); y `key={p}` (:32) colisiona si el copy trae verbos repetidos.
14. **Preexistente pero detectado (no es de hoy)**: `animar-pop` con `fill both` termina en `transform:none` y **una animación en fill sigue pisando el estilo normal** → `.card-viva:hover { transform: translateY(-2px) }` (globals.css:397, verificado en CSS compilado) queda muerto en toda card que combine ambas — hoy: `ResultadoCard.tsx:85` (`card-viva animar-pop`). El lift de hover de los resultados de /buscar no ocurre (el borde y la sombra ::after sí — por eso pasó desapercibido). Los `hover:-translate-y-*` de Tailwind v4 NO sufren esto (usan la propiedad `translate`, verificado en el CSS compilado) — solo el transform manual de `.card-viva`. Fix conceptual: quitar el fill al terminar (`animationend`) o mover el lift a `translate`.
15. **Touch target del "quitar categoría"** en la revisión del wizard (`WizardEmpresa.tsx:560-567`): `p-1` + icono 13px ≈ 21px — bajo el mínimo 24px (WCAG 2.5.8).
16. **Halo en touch**: el tap emula `mouseenter`/`mousemove` → el halo se enciende en el punto del tap y puede quedarse encendido (nunca llega `mouseleave` claro); cosmético porque la card navega, pero "en touch no existe" (comentario de Halo.tsx:11) no es exacto.

---

## Revisado y LIMPIO ✅

**Fugas / limpieza (pregunta 1):**
- `Aurora`: cleanup completo y correcto — `cancelAnimationFrame`, `ResizeObserver.disconnect`, `IntersectionObserver.disconnect`, `removeEventListener("visibilitychange")`, `WEBGL_lose_context.loseContext()` y `canvas.remove()` (Aurora.tsx:177-184). El contexto SÍ se libera al navegar. Instancias simultáneas: máx 2 (/nosotros hero+CTA) — dentro del límite de contextos del navegador, y el IO pausa la que no se ve (nunca pintan las dos a la vez en pantallas normales). Pausa por pestaña oculta ✓, dpr tope 2 ✓.
- `Halo`: los 3 listeners del PADRE se remueven en el cleanup con las mismas referencias (Halo.tsx:30-34) — sin fuga al desmontar; verificado `position: relative` en los 9 sitios de siembra (card-viva lo trae de fábrica globals.css:383; PanelAuth:113, planes:106, sala:732, PerfilEmpresa:147/364/455, SearchBox:31, SectorWheel:52, SectorColumns:73 son `relative` explícitos).
- `Contador`: rAF cancelado, `useInView once`, reduced-motion aterriza el dato directo (jamás se esconde) ✓. `FocoVivo`: interval limpiado y condicionado a viewport ✓. `CascadaEnVista`: sin listeners propios ✓. `Reflector`: eventos sintéticos de React, nada que limpiar ✓. Wizard: el borrador module-level con Files es diseño documentado (sobrevive cambio de idioma), no fuga.

**Rendimiento (pregunta 2):**
- Sin `setState` en scroll: la barra pegada de PerfilEmpresa usa IntersectionObserver con sentinel (PerfilEmpresa.tsx:116-124); no existe ningún listener de scroll con setState en navbar/cards (único scroll listener del repo: Select.tsx para cerrar popover, con cleanup ✓).
- `will-change` solo en las 2 pistas de marquesina (globals.css:820, 837) — justificado, cero abuso.
- Hydration: cero `Date.now()`/`Math.random()` en render — el patrón `ahora` del servidor se respeta en las cards nuevas (CompanyCard.tsx:15, ResultadoCard.tsx:15); estados iniciales deterministas en todos los componentes nuevos.
- ogl code-split correcto (48KB/14.6KB gz solo donde hay Aurora); `motion/react` ya estaba en el bundle base.
- CLS: next/font con subsets/weights (layout.tsx:12-23), canvas y decoraciones absolutas — sin desplazamientos por fuentes o entradas; `entrada-pagina`/`pop` terminan en `transform: none` (sin capas permanentes — con el asterisco S4-14).
- Marquesina y orbitas son transform/compositor puro; `content-visibility` en cards fue probado y retirado a conciencia (comentario globals.css:928).

**CSS / corrección visual (pregunta 3):**
- Especificidad de los bloques nuevos: sin pisotones — `.hito-lado-der` sobreescribe `animation-name` por orden con la misma especificidad (correcto), el orden `animation` → `animation-timeline` → `animation-range` está bien en los 3 bloques (el shorthand no resetea el timeline declarado después), y los overrides de reduced-motion de marquesina/texto-brillo van después de sus bases.
- `LineaHistoria` en Firefox/sin soporte: `@supports (animation-timeline: view())` envuelve TODO — la línea de acento se ve completa encima del riel gris, hitos y puntos visibles y quietos ✓ (lo prometido en el comentario se cumple).
- Matemática de la marquesina exacta: `pr-4` compensa el `gap-4` entre clones → el `-50%` cae exactamente al inicio del clon = loop sin costura ✓; clon con `aria-hidden` ✓.
- `texto-brillo` fallback técnico correcto: `background-clip: text` + prefijo tienen soporte universal 2026 y el override de reduced-motion restaura `currentColor` — el fallback NO está roto (el bug real es el S3-3, acento-sobre-acento).
- `.cascada-vista` con `useInView` sí dispara aunque la sección ya esté en viewport al montar (IO reporta intersección inicial) — el riesgo real es solo sin-JS (S3-2).

**A11y (pregunta 4):**
- reduced-motion verificado uno por uno: Contador ✓ (dato directo), FocoVivo ✓ (estático, mismo peso), OrbitaViva ✓ (regla global la detiene), palabra-hero ✓ (instantáneo), cascada-form/hero ✓, btn-vivo ✓ (transición 0.01ms), marquesina ✓ se detiene (pero S3-4), texto-brillo ✓ se apaga, campana ✓ (glow fijo), MotionConfig `reducedMotion="user"` global (MovimientoGlobal.tsx:11) y local en SectorWheel/SectorColumns ✓. Los que fallan quedaron en S2-1 y S3-5.
- Decorativo bien marcado: `aria-hidden` en Aurora (:188), OrbitaViva (:11), halos, chevrons, números fantasma, órbitas del PanelAuth, riel de historia ✓.
- Matriz móvil: `role="group"` + `aria-pressed` en el segmented — patrón válido; celdas con texto real en la lista móvil ✓. `focus-visible`: los controles nuevos son elementos nativos (button/a/input) que heredan el anillo del design system ✓. Sin traps de teclado nuevos; nada requiere `inert` (no hay overlays nuevos).
- i18n: las 12 llaves nuevas muestreadas existen en `es.json` y `en.json` (mxSoloRecibir, alContinuarTerminos, aceptasTerminos, volverInicio, panelChipVivo, hito1Fecha, statCategorias, culturaLinea, mxColFeature, mxSi, mxNo, reviewRestaurar) ✓; `t.rich` con `<Link>` en login/register funciona (build estático de ambas páginas ✓).

**Móvil (pregunta 5):**
- /nosotros: bandas full-bleed sin overflow (hero/CTA/manifiesto con `overflow-hidden`; marquesina en ventana propia; OrbitaViva 17rem cabe justo en 320px) — único residuo: los 2px transitorios de S4-2.
- Matriz de planes: móvil = segmented + lista de una columna, **cero scroll horizontal** ✓, precios y badge legibles.
- Hoja pública de licitación: una columna, dl responsive, CTAs de 44px ✓ (residuo S4-12).
- Wizard revisión: filas `flex-wrap`, botones restaurar/quitar no desbordan ✓ (residuo táctil S4-15). Textos <16px en inputs → S3-8.

**Decisiones aceptadas — respetadas, no re-flaggeadas**: CTA blanco/naranja 2.9:1, light-only, estrellas=accent, sombras rgba del admin, archivos exentos del design system.

## Orden de ataque sugerido
S2-3 (dato del onboarding) → S2-1 (reduced-motion en blanco, fix de 3 líneas: re-render tras setSize) → S2-2 (sticky) → S3-3 (una parada de color) → S3-5 + S3-4 (reduced-motion/WCAG 2.2.2) → S3-6/S3-7/S3-8 (a11y auth + contrastes + 16px) → S3-1/S3-2 → S4 en lote.
