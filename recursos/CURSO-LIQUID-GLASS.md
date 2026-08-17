# Guía Maestra: Liquid Glass y Glassmorphism — el material de las interfaces Mac

> **⚠️ Esta es la PARTE C del curso.**
> La **PARTE A** ([`CURSO-FRAMER-ANIMATION.md`](./CURSO-FRAMER-ANIMATION.md)) cubre el movimiento; la **PARTE B** ([`CURSO-UI-DESIGN.md`](./CURSO-UI-DESIGN.md)) cubre lo estático (color, tipografía, componentes).
> Regla de reparto: **si es vidrio — translucidez, blur, refracción, el "look Mac" — está aquí.**

> **Qué es este documento:** la referencia completa del **material vidrio** en interfaces: de dónde viene (Aero → iOS 7 → visionOS → **Liquid Glass**, el lenguaje de diseño que Apple presentó en WWDC junio 2025 para iOS 26 / macOS Tahoe 26), cómo funciona ópticamente, la receta exacta para construirlo en web (CSS + SVG), cuándo usarlo y cuándo NO, las reglas de accesibilidad y performance que lo hacen viable en producción, y su aplicación en Enkoras.
>
> **Estado del ecosistema:** verificado a agosto 2026 con research web real (fuentes al final). Se marca explícitamente qué es estándar, qué es solo-Chromium y qué es marketing de Apple que no aplica a la web.

---

## Índice

**PARTE I — EL LENGUAJE (el "por qué")**
1. Historia: del Aero de Vista al Liquid Glass de Apple
2. Liquid Glass: qué es exactamente el material
3. Las dos variantes del HIG: Regular y Clear
4. El principio rector: el vidrio es el chrome, nunca el contenido

**PARTE II — LA RECETA (el "cómo" en web)**
5. El vidrio calibrado: la fórmula base y su laboratorio
6. Anatomía por capas: frost · tint · rim · sheen · sombra
7. Refracción real (lensing) con SVG — el nivel Chromium
8. El vidrio necesita algo detrás: el fondo
9. Vidrio en dark mode

**PARTE III — REGLAS DE USO (el "cuándo")**
10. Solo lo que flota
11. Legibilidad y accesibilidad (WCAG + reduced transparency)
12. Performance: backdrop-filter es el asesino silencioso
13. El motion del material

**PARTE IV — EN CASA (Enkoras)**
14. Los tokens del sistema y la clase `.glass`
15. Dónde aplica y dónde no en nuestras apps
16. Checklist de cierre del vidrio

---
---

# PARTE I — EL LENGUAJE

## 1. Historia: del Aero de Vista al Liquid Glass de Apple

El vidrio en UI no es nuevo — es un péndulo que regresa cada década con mejor hardware:

- **2006 — Windows Vista "Aero Glass"**: translucidez con blur en los marcos de ventana. Murió por costo de GPU y por moda (flat design).
- **2013 — iOS 7 "vibrancy"**: Apple reintroduce el frosted glass con gusto — Control Center, barras de navegación. Nace el término técnico *vibrancy*: el contenido de atrás "vibra" a través del material saturándose.
- **2020 — macOS Big Sur / la ola "glassmorphism"**: sidebars translúcidos en macOS; en la web, el término **glassmorphism** se populariza (cards translúcidas con blur sobre fondos coloridos). NN/g lo define: *"un estilo visual que usa niveles de translucidez para crear profundidad y contraste entre primer plano y fondo, imitando vidrio esmerilado."*
- **2023 — visionOS**: el vidrio deja de ser decoración y se vuelve **el material del sistema** — ventanas de vidrio flotando en el espacio real, con profundidad y luz física.
- **Junio 2025 — WWDC: "Liquid Glass"**: Apple lo lleva a TODO el ecosistema (iOS 26, iPadOS 26, macOS Tahoe 26, watchOS/tvOS/visionOS 26). Es su rediseño más grande desde iOS 7. La diferencia con el glassmorphism clásico: ya no es un blur estático — es un material **dinámico** con refracción en tiempo real.

**La lección del péndulo:** cada vez que el vidrio volvió, volvió con más disciplina. El Aero fracasó por ponerlo en todo; el Liquid Glass funciona porque Apple lo restringe a **una sola capa funcional**. Esa disciplina es el corazón de esta guía.

## 2. Liquid Glass: qué es exactamente el material

De la presentación oficial de Apple y su documentación (fuentes al final), el material tiene 5 propiedades ópticas:

1. **Translucidez con refracción (lensing)**: el material no solo deja pasar el fondo borroso — lo **dobla** en los bordes, como una lente real. El contenido se deforma al pasar bajo el vidrio.
2. **Specular highlights**: reflejos de luz puntuales que **responden al movimiento** (en iOS, al giroscopio; en web, al cursor o al scroll). Es lo que hace que se sienta "vivo".
3. **Construcción por capas**: el material son varias capas apiladas (tinte + blur + filo de luz + sombra) que juntas crean profundidad y luminosidad — no una sola propiedad CSS.
4. **Adaptación inteligente**: ajusta su color y contraste según el contenido de atrás, y cambia solo entre claro/oscuro. Sobre texto, la variante Regular **aumenta su opacidad y su sombra** para mantener legibilidad.
5. **Transformación fluida**: los controles de vidrio se **funden y morfan** entre estados (un botón que se expande en menú no aparece — el vidrio fluye de una forma a otra).

**Dónde lo aplica Apple** (y esto es la clave): botones, switches, sliders, tab bars, sidebars, la Dock, Control Center, notificaciones — es decir, **la capa de controles que flota SOBRE el contenido**. Apple lo dice literal: los controles forman *"una capa funcional distinta que se asienta encima de las apps."* El contenido (fotos, texto, listas, cards de datos) **jamás es de vidrio**.

## 3. Las dos variantes del HIG: Regular y Clear

El Human Interface Guidelines de Apple define **dos variantes que nunca se mezclan**:

| | **Regular** | **Clear** |
|---|---|---|
| Adaptación | Completa: ajusta contraste, translucidez y sombra según el fondo | Ninguna: es permanentemente más transparente |
| Legibilidad | Garantizada en cualquier contexto | Necesita una **capa de atenuación** (dimming) sobre el fondo |
| Cuándo | **El 95% de los casos** — cualquier tamaño, cualquier fondo | Solo si se cumplen LAS TRES: (1) está sobre contenido rico en media, (2) el dimming no daña ese contenido, (3) lo que va encima es bold y brillante |

**Traducción a web:** nuestra `--glass` (0.72) y `--glass-strong` (0.86) son "Regular" — priorizan el texto. Un "Clear" web (alpha ≤ 0.35) solo se justifica sobre un hero con imagen/video y con un gradiente oscuro debajo del texto.

## 4. El principio rector: el vidrio es el chrome, nunca el contenido

La regla que separa el uso premium del disfraz, confirmada por Apple, NN/g y la práctica:

> **El vidrio se aplica SOLO a lo que flota sobre el contenido** — barras de navegación, dropdowns, paletas de comando, sheets, tooltips, hints, titlebars. **Lo que ES contenido — una card de empresa, una lista de resultados, un formulario — va en superficie sólida.**

Por qué es la regla #1:

- **Semántica**: la translucidez comunica "hay otro contexto debajo de mí" (principio Obscuration del curso de motion). Una card de contenido translúcida miente: no hay nada "debajo" que importe.
- **Legibilidad**: el contenido es para leerse; cada punto de translucidez le roba contraste.
- **Jerarquía**: si TODO es vidrio, nada flota. El error #1 documentado en las adopciones malas de Liquid Glass: *"aplicarlo a cada elemento, reduciendo legibilidad y distrayendo del contenido."*
- **Performance** (§12): cada capa de vidrio cuesta GPU cada frame.

---
---

# PARTE II — LA RECETA

## 5. El vidrio calibrado: la fórmula base y su laboratorio

La receta central — tres variables y una clase (ya viven en nuestros `globals.css`):

```css
:root {
  --glass:        rgba(255, 255, 255, 0.72);  /* 0.72, NO 0.3: el texto manda */
  --glass-strong: rgba(255, 255, 255, 0.86);  /* headers internos, zonas con texto denso */
  --glass-edge:   rgba(255, 255, 255, 0.75);  /* el filo de luz de arriba */
  --glass-blur:   20px;   /* menos de 16 se ve sucio; más de 28 se ve gelatina */
  --glass-sat:    180%;   /* el ingrediente secreto de Apple (vibrancy) */
}

.glass {
  background: var(--glass);
  backdrop-filter: blur(var(--glass-blur)) saturate(var(--glass-sat));
  -webkit-backdrop-filter: blur(var(--glass-blur)) saturate(var(--glass-sat));
  border: 1px solid var(--color-border);            /* tinta ~10% */
  box-shadow: var(--shadow-md),
              inset 0 1px 0 var(--glass-edge);       /* el filo de luz */
}
```

**El laboratorio de 3 estados** (la prueba de calibración de la referencia — montar los tres sobre el MISMO fondo con halos de color):

| Estado | Config | Veredicto |
|---|---|---|
| Muy transparente | `rgba(255,255,255,.28)` + blur 6px | ✕ **el fondo gana** — el texto pelea y pierde |
| **Calibrado** | `--glass` 0.72 + blur 20 + sat 180% + filo inset | ✓ **se lee Y se siente vidrio** |
| Opaco | `var(--surface)` sólido | — correcto, pero plano: no hay material |

**Por qué cada número:**
- **Alpha 0.72**: debajo de ~0.6 el fondo empieza a competir con el texto (WCAG en riesgo); arriba de ~0.85 deja de leerse como vidrio.
- **Blur 20px**: NN/g lo confirma desde la usabilidad — *"más blur es mejor, especialmente con fondos intrincados"*. El blur alto NO es decoración: es lo que garantiza que cualquier cosa que pase por debajo se vuelva textura abstracta en vez de ruido legible. Techo ~28px por costo GPU (§12) y porque más se ve "gelatina".
- **Saturate 180%**: sin él, el blur produce un gris lodoso. La sobresaturación del fondo es lo que Apple llama vibrancy — el color de abajo "brilla" a través. Es la diferencia entre vidrio y niebla.
- **El filo de luz** (`inset 0 1px 0`): un highlight de 1px arriba simula el borde pulido del vidrio atrapando luz. Sin él, el panel es una calcomanía; con él, tiene grosor.

## 6. Anatomía por capas: frost · tint · rim · sheen · sombra

El material completo se construye apilando (de atrás hacia adelante). La receta base (§5) usa las capas 1, 2, 3a y 5; las capas 3b y 4 son el nivel "showcase":

1. **Frost** — `backdrop-filter: blur() saturate()`: el esmerilado + vibrancy.
2. **Tint** — el `background` translúcido (blanco en light, café-oscuro translúcido en dark §9). Controla la legibilidad.
3. **Rim** — el borde refractivo:
   - **a)** `border: 1px` (tinta ~10%) + `inset 0 1px 0` (el filo de luz superior) — *suficiente para UI de producto*.
   - **b)** Nivel showcase: múltiples inset shadows blancos en opacidades decrecientes (0.55 / 0.30 / 0.20) en distintos offsets — simula el canto grueso de un panel con volumen.
4. **Sheen** — el brillo especular: un `::after` con gradiente diagonal (135°, blanco→transparente) y `mix-blend-mode: screen`, `pointer-events: none`. Para el efecto "vivo", el gradiente puede seguir al cursor (dos CSS vars `--mx/--my` actualizadas con un listener). Requiere `position: relative; isolation: isolate` en el padre. **Solo en piezas hero** — en UI repetida es ruido.
5. **Sombra** — la elevación: `0 18px 44px -14px` en tinta al ~24% para paneles grandes; `--shadow-md` para flotantes menores. La sombra es lo que separa "vidrio flotando" de "vidrio pegado".

## 7. Refracción real (lensing) con SVG — el nivel Chromium

El lensing de Apple — el fondo **doblándose** en los bordes del vidrio — se puede recrear en web con SVG filters. Es el nivel más alto del efecto y el más restringido:

**La física** (Ley de Snell, `n₁sin(θ₁) = n₂sin(θ₂)`, aire n=1 → vidrio n=1.5): en el centro del panel la luz pasa casi recta; hacia los bordes, el ángulo de la superficie crece y la luz se desvía. Apple usa un perfil **convex squircle** (domo suave) — transiciones más elegantes que una esfera al estirarse en rectángulos.

**El pipeline SVG:**

```html
<svg width="0" height="0" aria-hidden="true">
  <filter id="lente">
    <!-- 1. El displacement map: imagen precalculada donde R = desplazamiento X
         y G = desplazamiento Y (128 = neutro, 128±127 = máximo) -->
    <feImage href="data:image/png;base64,..." result="mapa" />
    <!-- 2. La refracción: mueve cada pixel del fondo según el mapa -->
    <feDisplacementMap in="SourceGraphic" in2="mapa"
        scale="60" xChannelSelector="R" yChannelSelector="G" />
    <!-- 3. Opcional: blur atmosférico + specular compositado con feBlend -->
  </filter>
</svg>
```

```css
/* Se aplica al PANEL, y el DOM real se refracta debajo:
   el texto sigue seleccionable, los inputs siguen usables */
.panel-lente { backdrop-filter: url(#lente) blur(2px) saturate(180%); }
```

**Las restricciones (verificadas):**
- `backdrop-filter: url(#...)` con filtros SVG **solo funciona en Chromium** (Chrome/Edge/Electron). Safari y Firefox soportan filtros SVG en `filter`, pero NO en `backdrop-filter`.
- Se degrada con `@supports (backdrop-filter: url(#x))` → el resto de navegadores recibe el vidrio calibrado normal (§5). **Progressive enhancement puro**: nadie pierde función, Chromium gana espectáculo.
- Regenerar el displacement map es caro — se precalcula UNA vez; solo `scale` se puede animar barato.
- Alternativa procedural rápida: `feTurbulence` + `feDisplacementMap` da una distorsión "líquida" orgánica sin mapa precalculado (menos "lente", más "agua").

**Veredicto de uso:** para UI de producto (Enkoras), el vidrio calibrado de §5 es el estándar. El lensing SVG es una pieza de exhibición — landing hero, un momento de marca — no el chrome diario.

## 8. El vidrio necesita algo detrás: el fondo

**Sobre blanco plano, el vidrio no existe** — el blur no tiene nada que muestrear y `--glass` se ve como un gris triste. Es la regla más olvidada:

- **El fondo correcto**: textura sutil (nuestra retícula blueprint), o **halos radiales enormes al 6–13%** en colores de marca:

```css
body::before {
  content: ""; position: fixed; inset: 0; z-index: -1; pointer-events: none;
  background:
    radial-gradient(58rem 40rem at 12% -8%, rgba(255,104,3,.13), transparent 62%),
    radial-gradient(52rem 44rem at 92% 8%, rgba(37,30,22,.10), transparent 60%),
    radial-gradient(46rem 38rem at 68% 96%, rgba(255,104,3,.07), transparent 64%),
    linear-gradient(180deg, var(--ground) 0%, var(--ground-deep) 100%);
}
```

- **Los fondos prohibidos**: fotos ruidosas, patrones ocupados, texto detrás del vidrio (NN/g: el texto que se transluce compite con el texto de encima). Si el fondo es media rica, sube el blur y/o la opacidad (Regular se "auto-protege" — eso en web lo haces tú).
- **El punto medio Enkoras**: gris cálido con textura leve. El vidrio se nota al hacer scroll (contenido pasando por debajo del header sticky) — ahí es donde paga.

## 9. Vidrio en dark mode

El vidrio oscuro NO es el claro con menos alpha — es otro tinte (los tokens dark de la referencia):

```css
--glass:        rgba(48, 41, 35, 0.66);   /* café-tinta translúcido, NO negro puro */
--glass-strong: rgba(52, 45, 38, 0.84);
--glass-edge:   rgba(255, 255, 255, 0.10); /* el filo de luz baja a 10% */
```

Claves: el tinte conserva el **hue de la marca** (café cálido, no gris neutro); el filo de luz se atenúa (la luz escasea de noche); las sombras suben de opacidad (0.6–0.75) porque sobre oscuro una sombra tenue desaparece. La "vibrancy" (saturate 180%) se queda igual — es aún más importante en dark.

---
---

# PARTE III — REGLAS DE USO

## 10. Solo lo que flota

La tabla de decisión — pegarla en la pared:

| Elemento | ¿Vidrio? | Por qué |
|---|---|---|
| Navbar / header sticky | ✅ | Flota sobre el contenido que scrollea — el caso canónico |
| Dropdown, popover, menú | ✅ | Capa temporal encima de todo |
| Paleta ⌘K, modales ligeros | ✅ | Ídem |
| Sheets / drawers | ✅ | El fondo atenuado se transluce |
| Titlebar / headers de un panel compuesto | ✅ (`--glass-strong`) | Chrome interno del panel |
| Hints, tooltips, toasts | ✅ | Flotantes efímeros |
| **Card de contenido** (empresa, solicitud) | ❌ sólido | ES el contenido — nada debajo que comunicar |
| **Formularios, inputs** | ❌ sólido | Legibilidad manda; un form es tarea, no chrome |
| **Tablas, listas de datos** | ❌ sólido | Ídem |
| Fondos de página enteros | ❌ | El vidrio sin borde ni sombra no es vidrio: es neblina |

**Y la regla de capas de Apple:** máximo **una capa de vidrio a la vez** en un mismo stack visual. Vidrio sobre vidrio = lodo (y doble costo GPU). Si un dropdown de vidrio abre sobre un navbar de vidrio, está bien — no se solapan visualmente; lo prohibido es un panel de vidrio DENTRO de otro panel de vidrio.

## 11. Legibilidad y accesibilidad (WCAG + reduced transparency)

El vidrio es de los estilos con más riesgo de accesibilidad. Las reglas duras:

1. **WCAG sobre el peor fondo posible**: el contraste del texto sobre vidrio se mide contra **lo peor que puede pasar por debajo**, no contra el promedio. Texto normal ≥ 4.5:1, grande ≥ 3:1. Con `--glass` 0.72 + blur 20 sobre nuestros fondos claros, la tinta #1f1f1f pasa siempre; con alphas menores, hay que verificar con capturas reales (plugins de contraste sobre el compuesto).
2. **Más blur en fondos complejos** (NN/g): si no controlas el fondo (contenido de usuario debajo), sube blur y opacidad. La legibilidad SIEMPRE le gana a la estética del material.
3. **`prefers-reduced-transparency` → el vidrio se solidifica**. El usuario que activó "Reducir transparencia" en su OS recibe superficie opaca — la práctica estándar 2026:

```css
@media (prefers-reduced-transparency: reduce) {
  .glass {
    background: var(--color-surface);
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}
```

4. **`prefers-contrast: more`** — mismo tratamiento + borde más fuerte.
5. **El vidrio nunca es el único canal**: la separación entre capas también la dan el borde y la sombra — que sobreviven cuando la translucidez se apaga.

## 12. Performance: backdrop-filter es el asesino silencioso

`backdrop-filter` **lee los pixeles de las capas de abajo en cada frame** — es de las propiedades más caras de CSS. Las reglas (verificadas contra la práctica actual):

1. **Pocos elementos**: 1–3 superficies de vidrio por pantalla. Una lista con 50 cards de vidrio es una masacre de GPU (y viola §10 de todos modos).
2. **Blur ≤ 20px**: el costo crece rápido con el radio.
3. **JAMÁS animar el radio del blur** — re-composita cada frame y mata cualquier equipo no-flagship. Lo que se anima del vidrio: `opacity` y `transform` del panel (composite puro). El blur es estático.
4. **`will-change` solo puntual** (justo antes de una transición del panel, removido después) — cada uno consume memoria GPU permanente.
5. **iOS + `position: fixed` + backdrop-filter** tiene bugs históricos de repintado — probar en dispositivo real; si tiembla, el elemento fijo lleva vidrio "falso" (fondo semi-opaco sin blur).
6. **Gama media/baja**: dosificar. El blur grande es el asesino silencioso en móviles medios (curso A §35). Heurística: `navigator.deviceMemory < 4` → degradar a `--glass-strong` sin blur.
7. El vidrio de piezas ESTÁTICAS (un hero que no scrollea sobre nada vivo) puede falsificarse gratis: captura el fondo, aplícale blur en build, úsalo de `background-image`. Cero costo por frame.

## 13. El motion del material

El vidrio se mueve según las reglas del curso A, con matices propios:

- **El material morfa, no parpadea**: un control de vidrio que se expande (botón → menú) **fluye** — `transform: scale/translate` con spring seco, nunca un fade-swap. Es el principio Transformation aplicado al material (así lo hace Apple).
- **Specular vivo con moderación**: el sheen que sigue al cursor (§6) solo en 1–2 piezas hero. En el chrome diario, el vidrio es quieto — el contenido pasando por debajo YA es el show.
- **Las entradas del vidrio**: paneles flotantes nacen desde su trigger (scale 0.96 + y −4, `transform-origin` en el botón) — dropdown estándar del curso A §25. El blur NO se anima en la entrada (llega ya aplicado, solo anima opacity/transform).
- **Reduced motion**: el material respeta `MotionConfig reducedMotion="user"` como todo lo demás — los morfos se vuelven fades.

---
---

# PARTE IV — EN CASA (Enkoras)

## 14. Los tokens del sistema y la clase `.glass`

Ya viven en `directorio-b2b/app/globals.css` (capa de primitivos):

```css
--glass:        rgba(255, 255, 255, 0.72);
--glass-strong: rgba(255, 255, 255, 0.86);
--glass-edge:   rgba(255, 255, 255, 0.75);
--glass-blur:   20px;
--glass-sat:    180%;
```

Pendientes de adoptar cuando toque: la clase `.glass` utilitaria global (hoy el estilo vive inline en `SectorColumns`), el fallback `prefers-reduced-transparency` (§11.3), y los tokens dark (§9) el día que abramos dark mode.

## 15. Dónde aplica y dónde no en nuestras apps

**Ya construido:**
- El **column finder de /categorias** (`SectorColumns`): panel de vidrio con headers `--glass-strong` — el caso "panel compuesto flotante".

**Candidatos naturales (cuando toque):**
- Dropdowns del navbar (campana, mensajes, idioma) — nacimiento origin-aware + vidrio.
- El navbar mismo al volverse sticky sobre contenido que scrollea.
- Una futura paleta ⌘K (el atajo global de categorías).
- Sheets móviles.

**Prohibido (por §10):** cards de empresa/solicitud, formularios del wizard, tablas del admin, paneles de contenido de mi-empresa. Todo eso queda sólido — y es una DECISIÓN, no una omisión.

## 16. Checklist de cierre del vidrio

Antes de dar por terminada cualquier superficie de vidrio:

1. ¿Es chrome que **flota** sobre contenido? (Si es contenido → sólido, fuera.)
2. ¿Alpha ≥ 0.7 en zonas con texto? ¿El texto pasa 4.5:1 sobre el peor fondo posible?
3. ¿Tiene las 4 capas mínimas? (frost + tint + borde/filo de luz + sombra)
4. ¿`saturate(180%)` presente? (sin él es niebla, no vidrio)
5. ¿Hay algo detrás que lo justifique? (textura/halos/contenido que scrollea — sobre blanco plano no hay material)
6. ¿Una sola capa de vidrio en el stack?
7. ¿`prefers-reduced-transparency` lo solidifica?
8. ¿Blur estático ≤ 20px, sin animarse jamás?
9. ¿-webkit- prefijo presente? (Safari)
10. ¿En móvil real se mueve a 60fps con el vidrio en pantalla?

**La prueba final:** quita el vidrio y pon superficie sólida. Si la interfaz no perdió *significado* (jerarquía de capas, sensación de flotar), el vidrio era maquillaje — quítalo de verdad. Si perdió, está bien puesto.

---

*Fuentes de la investigación (verificadas agosto 2026):*

- [Apple Newsroom — Apple introduces a delightful and elegant new software design (WWDC, jun 2025)](https://www.apple.com/newsroom/2025/06/apple-introduces-a-delightful-and-elegant-new-software-design/)
- [Apple Developer — Meet Liquid Glass (WWDC25, sesión 219)](https://developer.apple.com/videos/play/wwdc2025/219/)
- [Nielsen Norman Group — Glassmorphism: Definition and Best Practices](https://www.nngroup.com/articles/glassmorphism/)
- [kube.io — Liquid Glass in the Browser: Refraction with CSS and SVG](https://kube.io/blog/liquid-glass-css-svg/)
- [WebTricks — Liquid glass in CSS: frost, rim, sheen and real refraction](https://webtricks.dev/blog/liquid-glass-css)
- [LogRocket — Adopting Apple's Liquid Glass: Examples and best practices](https://blog.logrocket.com/ux-design/adopting-liquid-glass-examples-best-practices/)
- [LogRocket — How to create Liquid Glass effects with CSS and SVG](https://blog.logrocket.com/how-create-liquid-glass-effects-css-and-svg/)
- [Axess Lab — Glassmorphism Meets Accessibility](https://axesslab.com/glassmorphism-meets-accessibility-can-frosted-glass-be-inclusive/)
- [Orizon — Glassmorphism in 2026: How to Use Frosted Glass Without Killing UX](https://www.orizon.co/blog/glassmorphism-in-2026-how-to-use-frosted-glass-without-killing-ux)
- [GitHub — nikdelvin/liquid-glass (recreación CSS+SVG)](https://github.com/nikdelvin/liquid-glass)
- [Macworld — macOS Tahoe prioritizes productivity over Liquid Glass](https://www.macworld.com/article/2862474/macos-tahoe-prioritizes-productivity-over-liquid-glass.html)
- Referencia interna: `Mypersonalspace/explorador-categorias.html` — el laboratorio de vidrio calibrado y los tokens de la casa.
