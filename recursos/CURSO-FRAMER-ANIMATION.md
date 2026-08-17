# Guía Maestra: UI/UX Premium con Motion (Framer Motion), React, Tailwind y CSS moderno

> **⚠️ Este es la PARTE A del curso — cubre el MOVIMIENTO.**
> La **PARTE B** vive en [`CURSO-UI-DESIGN.md`](./CURSO-UI-DESIGN.md) y cubre todo lo estático: sistema de color y cómo se define una paleta · espaciado, radios, bordes, sombras y elevación · tipografía y jerarquía · botones, inputs y formularios · checkbox, select, dropdowns, modales y demás controles · layout, cards, menús y contenedores · recetario de efectos listos para copiar.
> Regla de reparto: **si se mueve, está aquí; si no se mueve, está en la Parte B.**

> **Qué es este documento:** una guía completa para diseñar y construir interfaces web con calidad **premium** — el nivel de pulido de Linear, Vercel, Stripe o Raycast. Combina: (1) los principios universales de motion design para UI/UX, (2) la referencia técnica completa y actualizada de **Motion** (la librería antes conocida como **Framer Motion**), (3) un catálogo de micro-interacciones con especificaciones exactas (duraciones, curvas, escalas), y (4) la implementación práctica con React, Next.js, Tailwind CSS v4 y CSS nativo moderno.
>
> **Para quién es:** para una persona o una IA que va a construir sitios web y necesita que TODO — botones, transiciones, scroll, modales, responsive — se sienta de primer nivel. Está escrito en español con los términos técnicos y el código en inglés.
>
> **Estado del ecosistema:** verificado a agosto 2026. Se marcan explícitamente los cambios recientes que los tutoriales viejos no cubren.

---

## Índice

**PARTE I — PRINCIPIOS (el "por qué")**
1. Filosofía del motion design
2. Duraciones: los números exactos
3. Easing: las curvas y su física
4. Springs: física de resortes
5. Los principios que separan lo premium de lo genérico

**PARTE II — MOTION, LA LIBRERÍA (el "con qué")**
6. Historia, instalación e imports actuales
7. El componente `motion`
8. Transitions a fondo
9. Gestos: hover, tap, focus, inView
10. Variants y orquestación (stagger)
11. AnimatePresence: animaciones de salida
12. Layout animations y `layoutId`
13. Scroll: useScroll, parallax, scroll-linked
14. Motion values: animar sin re-renders
15. Drag
16. Animación imperativa: useAnimate
17. Configuración global y accesibilidad
18. Bundle size: LazyMotion
19. Animación de texto
20. Lo nuevo de la era Motion (que los tutoriales viejos no tienen)

**PARTE III — CATÁLOGO DE MICRO-INTERACCIONES PREMIUM (el "qué construir")**
21. Botones
22. Inputs y formularios
23. Toggles y checkboxes
24. Cards
25. Dropdowns y popovers
26. Modales y drawers
27. Toasts (el spec de Sonner)
28. Tooltips
29. Tabs
30. Accordions
31. Skeletons y estados de carga
32. Transiciones de página
33. Números y contadores
34. Scroll UX: reveals, parallax y etiqueta

**PARTE IV — IMPLEMENTACIÓN Y ECOSISTEMA (el "cómo en producción")**
35. Rendimiento: qué animar y qué jamás animar
36. Tailwind CSS v4 y animación
37. shadcn/ui + Radix: cómo animan y cómo personalizarlos
38. CSS nativo moderno que reduce la necesidad de JS
39. Next.js App Router: transiciones de ruta y Server Components
40. Responsive y móvil: touch, hover y safe areas
41. Accesibilidad: prefers-reduced-motion y WCAG
42. Design tokens de motion: el sistema
43. Panorama de librerías: cuál usar cuándo
44. Colecciones de componentes premium
45. Reglas de oro: el checklist completo
46. Cheat-sheet final

---
---

# PARTE I — PRINCIPIOS

## 1. Filosofía del motion design

La animación en la web **no se trata de hacer que las cosas se muevan, sino de hacer que las cosas tengan sentido**. Cuando el usuario hace hover, click o scroll, el movimiento:

- **Guía el ojo** hacia lo importante.
- **Da feedback**: confirma que algo es clickeable y que la acción funcionó.
- **Hace que la experiencia se sienta viva.**

La mejor animación **se siente invisible** pero hace mucho trabajo pesado: muestra **relaciones** entre elementos, resalta **qué es clickeable**, y da **sentido de lugar** (dónde estoy, de dónde vengo, a dónde voy). El objetivo siempre es **claridad, no ruido**.

### La regla número uno

> **"El motion comunica, no decora."** Cada animación debe responder la pregunta: *¿qué información aporta?* Si la respuesta es "ninguna", se elimina. (Apple HIG: "Don't add motion for the sake of adding motion". Emil Kowalski: "You Don't Need Animations".)

### El movimiento comunica personalidad y tono

- **Snappy, performante y serio** — rápido, seco, sin rebote.
- **Springy y juguetón** — con física de resorte y overshoot.
- **Lento, suave y elegante** — duraciones más largas, easings suaves.

Un sitio premium **elige un tono y lo mantiene en todo el sitio**.

### Los 12 principios de UX in Motion (Issara Willenskomer)

Vocabulario esencial (adaptación web de los 12 principios de Disney):

1. **Easing** — nada se mueve linealmente en el mundo físico.
2. **Offset & Delay** — el stagger comunica jerarquía pre-conscientemente.
3. **Parenting** — los elementos hijos heredan y responden al movimiento del padre.
4. **Transformation** — un elemento se convierte en otro (botón → spinner → checkmark) creando narrativa continua.
5. **Value Change** — los números que cambian animados conectan al usuario con la realidad detrás del dato.
6. **Masking** — revelar contenido recortándolo (clip-path) comunica "esto ya estaba ahí".
7. **Overlay** — capas que se superponen comunican orden en Z.
8. **Cloning** — un elemento nuevo nace desde el elemento que lo originó.
9. **Obscuration** — el blur comunica "hay otro contexto detrás".
10. **Parallax** — velocidades distintas comunican profundidad.
11. **Dimensionality** — flip/fold 3D comunica que algo tiene "otra cara".
12. **Dolly & Zoom** — acercamiento/alejamiento comunica navegación jerárquica.

### Realtime vs non-realtime (distinción clave)

- **Interacción realtime**: el pixel sigue al dedo/cursor (drag, swipe, scroll). → Usa **física** (springs, momentum). Debe responder 1:1 durante el gesto y ser interrumpible en cualquier frame.
- **Transición non-realtime**: ocurre después de una acción (abrir modal, cambiar tab). → Usa **duración + easing**, y debe ser **breve** porque bloquea al usuario mientras ocurre.

---

## 2. Duraciones: los números exactos

### Tabla maestra por tipo de elemento

| Patrón | Duración | Notas |
|---|---|---|
| Feedback inmediato (press, toggle, checkbox) | **~100ms** | El umbral de "manipulación física directa" |
| Micro-interacciones (hover, active) | **100–150ms** | |
| UI estándar (tooltips, dropdowns, popovers) | **150–250ms** | El estándar shadcn/Radix es ~150–200ms |
| Modales, drawers, cambios grandes | **200–300ms** | |
| Drawers con gesto (tipo bottom sheet iOS) | **hasta 500ms** | Solo porque son interrumpibles/arrastrables |
| Scroll reveals | **400–600ms** | Tope absoluto 700–800ms |
| Hero reveals / ilustrativas (una vez por sesión) | **500–700ms** | |

### Reglas duras (memorízalas)

1. **Regla maestra: las animaciones de UI se quedan por debajo de 300ms** (Emil Kowalski). Para interacciones directas, **200ms o menos** para sentirse inmediatas (Rauno Freiberg, Vercel). Un dropdown de 180ms se siente mejor que el mismo dropdown a 400ms.
2. **Por debajo de ~100ms se percibe como instantáneo** — crea la ilusión de manipular físicamente el objeto (umbral de causalidad directa, NN/g).
3. **Por encima de 400–500ms se siente lento.** El error común es hacer animaciones demasiado LARGAS, no demasiado cortas. La diferencia entre 250ms y 300ms ya es perceptible.
4. **Doherty Threshold (400ms)**: el sistema debe responder en menos de 400ms para mantener el "flow"; el feedback ligero (press state, optimistic UI) debe llegar en **100–200ms** aunque el trabajo real tarde más.
5. **Regla de frecuencia**: cuanto más frecuente la acción, más corta y sutil (o inexistente) la animación. Escala: acción usada **100+ veces al día → SIN animación**; ocasional → animación estándar; rara o primera vez → puede ser más elaborada. *"Uso Raycast cientos de veces al día. Si animara cada vez que lo abro, sería insoportable."*
6. **Acciones de teclado: NUNCA se animan.** La selección con flechas animada se siente retrasada respecto a las teclas. Command menus (tipo ⌘K) aparecen sin motion.
7. **Distancia y tamaño escalan la duración**: cambios pequeños = cortos; transiciones de pantalla completa = largos (dentro del techo).
8. **La duración también depende del contraste visual**: un cambio drástico de luminancia (texto negro → blanco sobre fondo oscuro) agradece más tiempo para suavizar el salto perceptual.

### Referencia: tokens de Material Design 3

`short1–4` = 50/100/150/200ms · `medium1–4` = 250/300/350/400ms · `long1–4` = 450/500/550/600ms · `extra-long1–4` = 700–1000ms. Consenso entre design systems: **UI rutinaria 160–240ms; entradas/salidas 240–360ms; nunca >500ms para UI funcional.**

---

## 3. Easing: las curvas y su física

### La intuición física (fundamental)

En el mundo físico, los objetos necesitan **tiempo para recibir energía** (acelerar) y **tiempo para perderla** (frenar). Como una pelota que ruedas suavemente: le transfieres energía gradualmente, y la energía se disipa gradualmente hasta que se detiene. Las curvas de easing modelan eso. **Linear se siente robótico** porque alcanza velocidad máxima instantáneamente y frena instantáneamente — nada en el mundo real hace eso.

### Qué curva usar cuándo

| Curva | Comportamiento | Cuándo usarla |
|---|---|---|
| **ease-out** | Arranca a velocidad máxima, frena gradualmente | **Entradas y casi todo lo iniciado por el usuario** — arrancar rápido = sensación de respuesta inmediata |
| **ease-in** | Arranca lento, acelera, frena en seco | Salidas hacia fuera de pantalla — con debate (ver abajo) |
| **ease-in-out** | Acelera y frena gradualmente | **Movimiento dentro de pantalla**: algo visible que se reubica o transforma |
| **ease** (default CSS) | Ligero ease-in-out asimétrico | Hovers y transiciones de color a ~150ms |
| **linear** | Velocidad constante | SOLO spinners, shimmer, marquees, rotaciones perpetuas — cosas que no tienen inicio ni fin visibles |

**Regla de oro heredada de la animación clásica:** *no suavices (ease) un extremo del movimiento que el usuario no va a ver.* Si el elemento entra desde fuera de pantalla, no necesita ease-in (su arranque ocurre fuera de vista) — usa ease-out y dedica **toda la duración** al frenado visible. Si sale de pantalla, no necesita frenar suave. Elimina también transformaciones redundantes: si algo sale de pantalla por movimiento, **el fade de opacity es innecesario**.

**El debate del ease-in en salidas:** Josh Comeau lo acepta para elementos que terminan invisibles/fuera de pantalla. Emil Kowalski es más radical: *"nunca uses ease-in en UI"* — arrancar lento hace sentir la interfaz pesada; él usa ease-out también en salidas (más cortas). Material 3 sí prescribe accelerate para exits. **Regla práctica: ease-in en salida solo si dura ~150–200ms.**

### Curvas concretas que usan los sitios premium (colección)

No uses las keywords built-in de CSS para animaciones protagonistas — son débiles. Estas son las curvas reales:

```css
/* Favoritas de Emil Kowalski (Linear, Sonner, Vaul, animations.dev) */
--ease-out-quint:   cubic-bezier(0.23, 1, 0.32, 1);      /* SU DEFAULT para interacciones UI */
--ease-in-out-quart: cubic-bezier(0.77, 0, 0.175, 1);    /* movimiento dentro de pantalla */
--ease-ios-drawer:  cubic-bezier(0.32, 0.72, 0, 1);      /* la curva del sheet de iOS — usada en Vaul */

/* Familia Penner ease-out (de más suave a más agresiva) */
--ease-out-quad:  cubic-bezier(0.25, 0.46, 0.45, 0.94);
--ease-out-cubic: cubic-bezier(0.215, 0.61, 0.355, 1);
--ease-out-quart: cubic-bezier(0.165, 0.84, 0.44, 1);
--ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);
--ease-out-expo:  cubic-bezier(0.19, 1, 0.22, 1);        /* dramática — count-ups, hero reveals */
--ease-out-circ:  cubic-bezier(0.075, 0.82, 0.165, 1);

/* Familia Penner ease-in-out */
--ease-in-out-quad:  cubic-bezier(0.455, 0.03, 0.515, 0.955);
--ease-in-out-cubic: cubic-bezier(0.645, 0.045, 0.355, 1);
--ease-in-out-quart: cubic-bezier(0.77, 0, 0.175, 1);
--ease-in-out-quint: cubic-bezier(0.86, 0, 0.07, 1);

/* Material Design 3 */
--md-standard:             cubic-bezier(0.2, 0, 0, 1);
--md-emphasized-decelerate: cubic-bezier(0.05, 0.7, 0.1, 1);   /* entradas M3 */
--md-emphasized-accelerate: cubic-bezier(0.3, 0, 0.8, 0.15);   /* salidas M3 */
```

### La asimetría enter/exit (codificada en los specs)

- **Las salidas son MÁS RÁPIDAS que las entradas.** El usuario ya decidió irse; no lo hagas esperar. Material 3 lo codifica: enter 400–500ms emphasized-decelerate / exit 200ms emphasized-accelerate. La guía de Next.js usa `--duration-exit: 150ms; --duration-enter: 210ms`.
- **Matiz para hover**: puede invertirse — entrada rápida (~125ms) y salida relajada (~300ms) se siente intencional y elegante.

---

## 4. Springs: física de resortes

Los springs producen movimiento con **overshoot** (se pasan del punto final y regresan oscilando), como un resorte real. Son la firma del feel "vivo" de iOS y de las mejores apps.

### Cuándo spring y cuándo duración

- **Spring cuando hay gesto o interrupción**: responden a la **velocidad** del gesto previo (un drawer soltado con fuerza sigue esa fuerza) y son **naturalmente interruptibles**. Ideales para: drawers arrastrables, swipe-to-dismiss, tabs con layoutId, drag & drop, toggles.
- **Duración + easing para lo simple**: fades, tooltips, hovers, cambios de color. Un spring ahí es sobre-ingeniería.

### Los dos modos de configurar un spring

**Modo duración/bounce (el fácil de razonar):** defines cuánto dura y cuánto rebota.

- La lógica interna: *"termina todo el rebote antes del límite de tiempo"* — más bounce significa que el movimiento principal ocurre más rápido al inicio para dejar tiempo a los rebotes.
- **Config estilo Apple** (la que enseña Emil Kowalski): `{ type: "spring", duration: 0.5, bounce: 0.2 }`. Rango útil de bounce: **0.1–0.3**. **Bounce 0 para UI seria.**

**Modo físico (stiffness / damping / mass):** la duración es **consecuencia** de la física.

| Parámetro | Metáfora | Efecto |
|---|---|---|
| **stiffness** (rigidez) | Qué tan explosivo es el resorte | Más = movimiento más rápido y brusco → más energía → más rebote |
| **damping** (amortiguación) | El amortiguador de un coche | Más = absorbe el rebote. Menos = oscila muchas veces antes de reposar (0 = para siempre) |
| **mass** (masa) | El peso del objeto | Más = arranca más lento, carga más energía, tarda más en reposar |

Guía de decisión: ¿qué tan rápido debe moverse? → stiffness. ¿Objeto grande/pesado? → sube mass. ¿No quieres rebote eterno? → sube damping.

**Recetas probadas:**

```js
// Snappy y suave — tabs, toggles, indicadores
{ type: "spring", stiffness: 300, damping: 30 }
// o { stiffness: 400, damping: 30 }

// Con personalidad juguetona (hover cards, iconos)
{ type: "spring", stiffness: 200, damping: 18, mass: 1.2 }

// Estilo Apple, sin pensar en física
{ type: "spring", duration: 0.5, bounce: 0.2 }

// visualDuration (lo más nuevo, ver §8): "llega" visualmente en 0.4s y el rebote remata después
{ type: "spring", visualDuration: 0.4, bounce: 0.3 }
```

⚠️ Si defines stiffness/damping/mass, se **ignoran** duration/bounce — son modos excluyentes.

---

## 5. Los principios que separan lo premium de lo genérico

Estos son los rasgos concretos que hacen que Linear, Vercel, Stripe o Raycast se sientan de otro nivel:

### 5.1 Tokens de motion consistentes — el rasgo #1

Dos o tres duraciones (`fast: 150ms`, `base: 250ms`, `slow: 400ms`) y dos easings custom (un ease-out + un ease-in-out) usados en **TODOS** los componentes. Los sitios que se sienten baratos mezclan 12 duraciones y 8 curvas distintas. La cohesión — easing, duración y personalidad en armonía — es lo que Emil Kowalski señala como la clave del feel de Sonner y Vaul.

### 5.2 Restraint (contención) como firma

Linear y Raycast abren su command menu **sin ninguna animación**. Vercel corta las animaciones "que no son el foco". Lo premium **no es más animación** — es animación concentrada en 2–3 momentos de alto impacto (la carga orquestada de la página, el modal, el toast) y **cero fricción** en el uso repetido. El delight solo funciona en lo que se ve poco: si está en todas partes, abruma y pierde impacto.

### 5.3 Interruptibilidad

*"Imagina que pasar la página de un libro fuera una animación que tienes que esperar."* Toda animación debe poder cortarse por la siguiente acción del usuario sin saltos. CSS transitions y springs interrumpen suave por diseño; los keyframes no. Nunca bloquees la interacción esperando que termine una transición.

### 5.4 Origin-aware: todo nace de su origen

Anima **desde donde ocurrió la interacción**. El dropdown crece desde su trigger (`transform-origin` apuntando al botón, no al centro). El modal de una card expandida nace de la card. Nada aparece "del centro de la nada".

### 5.5 Consistencia espacial

La dirección del movimiento construye el modelo mental: si algo entra por la derecha, "está encima en el stack" — y al cerrar, sale por la derecha. El toast sale por donde entró. Forward y back son espejos. La física debe ser plausible: si navegar adelante empuja a la izquierda, regresar empuja a la derecha.

### 5.6 Física plausible en lo que se toca

Lo arrastrable (drawers, toasts, carousels) sigue al dedo **1:1 durante el gesto**, conserva momentum y ángulo al soltar, y puede interrumpirse en cualquier frame. No 0→1 discreto: se siente el delta aplicándose de inmediato, y se anima al pasar un umbral.

### 5.7 Escala proporcional, nunca desde cero

- **Nunca animes desde `scale(0)`** — parte de **0.9–0.95**. Los elementos no nacen de la nada; ya "estaban ahí".
- Escala proporcional al tamaño: botones al presionar `0.96–0.97`; dialogs `0.95–0.97`. Nada de rangos extremos.

### 5.8 El inventario de detalle clase Linear/Vercel

- Hover state en **todo** lo interactivo; active/press state en todo lo clickeable (`scale 0.97`).
- Focus ring visible (con box-shadow, que respeta el border-radius — no outline) en todo; navegación completa por teclado.
- `cursor: pointer` coherente; `user-select: none` en controles; `pointer-events: none` en decoraciones (glows, gradientes) para que no secuestren clicks.
- Hairline borders de 1px, radius consistente (p. ej. 12px en todo), un solo color de acento.
- Grain/noise sutil para evitar banding en gradientes; gradient borders/glow solo en cards destacadas.
- `-webkit-font-smoothing: antialiased`; `font-variant-numeric: tabular-nums` en cifras que cambian; el font-weight **jamás** cambia con hover (produce layout shift).
- El theme switch (dark/light) **no dispara transiciones** en todos los elementos (desactívalas durante el cambio).
- Los loops (marquees, gradientes animados) **se pausan fuera del viewport**.
- Sin tap-highlight de iOS; sin zoom por inputs menores a 16px; sin hover states en touch.

---
---

# PARTE II — MOTION, LA LIBRERÍA

## 6. Historia, instalación e imports actuales

**Framer Motion se independizó de Framer en 2024-2025 y ahora se llama "Motion"** (motion.dev, repo `motiondivision/motion`). Versión actual verificada: **`motion@13.1.0`** (agosto 2026). El paquete `framer-motion` se sigue publicando en lockstep con el mismo código, pero para proyectos nuevos **usa siempre `motion`**.

```bash
npm install motion
```

```tsx
import { motion, AnimatePresence } from "motion/react"      // React (client components)
import * as motion from "motion/react-client"               // React Server Components (Next.js)
import { animate, scroll, inView, stagger } from "motion"   // vanilla JS (sin React)
import { animate } from "motion/mini"                       // vanilla mini: 2.3kb, solo WAAPI
```

**Migrar desde framer-motion:** `npm uninstall framer-motion && npm install motion`, y reemplazar `"framer-motion"` → `"motion/react"` en los imports. Ya.

**Cambios de API importantes que los tutoriales viejos NO tienen** (detalle en §20):

| Viejo (deprecado/eliminado) | Actual |
|---|---|
| `motion(Component)` / `m(Component)` | `motion.create(Component)` |
| `staggerChildren` / `staggerDirection` | `delayChildren: stagger(...)` |
| `exitBeforeEnter` en AnimatePresence | `mode="wait"` (el viejo LANZA ERROR desde v10) |
| Solo React | API vanilla completa + `motion/mini` |

Motion usa **WAAPI (Web Animations API)** por debajo para animar `transform`, `opacity`, `filter` (y ahora también `backgroundColor` y SVG) **con aceleración por hardware** — las animaciones sobreviven a bloqueos del main thread.

## 7. El componente `motion`

La primitiva central: cualquier elemento HTML/SVG con superpoderes de animación.

```tsx
import { motion } from "motion/react"

<motion.div
  initial={{ opacity: 0, y: 20 }}   // estado ANTES de montar; initial={false} = sin animación de entrada
  animate={{ opacity: 1, y: 0 }}    // objetivo; si cambia entre renders, anima automáticamente
  exit={{ opacity: 0 }}             // al desmontar (requiere AnimatePresence, §11)
  transition={{ duration: 0.5 }}
/>
```

**Valores animables:**

- **Transforms independientes** (gran ventaja sobre CSS): `x`, `y`, `z`, `scale`, `scaleX`, `scaleY`, `rotate`, `rotateX`, `rotateY`, `skewX`, `skewY`, `originX/originY`, `perspective`. Puedes animar `x` sin tocar `scale`.
- **Colores**: hex, rgba, hsla, y los modernos `oklch`, `oklab`, `color-mix()`, `light-dark()`.
- **Filtros y propiedades exóticas**: `filter: "blur(10px)"`, `boxShadow`, `clipPath`, `maskImage`, incluso `display`/`visibility` (se aplican al final o inicio según dirección).
- **Variables CSS**:

```tsx
<motion.ul initial={{ "--rotate": "0deg" }} animate={{ "--rotate": "360deg" }}>
  <li style={{ transform: "rotate(var(--rotate))" }} />
</motion.ul>
```

- **Keyframes** (arrays; `null` = "desde el valor actual", clave para interrupciones suaves):

```tsx
<motion.div
  animate={{ x: [null, 100, 0] }}
  transition={{ duration: 3, times: [0, 0.2, 1] }}  // times posiciona cada keyframe (0–1)
/>
```

**Componentes propios:** `const MotionButton = motion.create(Button)` — el componente debe aceptar `ref` (en React 19, vía props). ⚠️ `motion(Button)` está deprecado.

**Callbacks:** `onUpdate(latest)`, `onAnimationStart`, `onAnimationComplete`.

## 8. Transitions a fondo

**Defaults inteligentes:** Motion usa spring para propiedades físicas (transforms) y tween para visuales (opacity, color). Duración tween por defecto: 0.3s.

```tsx
// Spring físico (incorpora la velocidad de gestos previos)
<motion.div animate={{ x: 100 }} transition={{ type: "spring", stiffness: 400, damping: 30 }} />

// Spring por duración
<motion.div animate={{ scale: 1 }} transition={{ type: "spring", duration: 0.5, bounce: 0.25 }} />

// visualDuration (11.12+): la animación "llega" visualmente en ese tiempo, el rebote remata después.
// La forma moderna de afinar springs por sensación:
<motion.div animate={{ y: 0 }} transition={{ type: "spring", visualDuration: 0.4, bounce: 0.3 }} />

// Tween con cubic-bezier custom (las curvas de §3 van aquí)
<motion.div animate={{ opacity: 1 }} transition={{ type: "tween", duration: 0.25, ease: [0.23, 1, 0.32, 1] }} />
```

Easings con nombre: `"linear"`, `"easeIn"`, `"easeOut"`, `"easeInOut"`, `"circIn/Out/InOut"`, `"backIn/Out/InOut"`, `"anticipate"`. También funciones `(t) => t` y arrays de easings (uno por segmento de keyframes).

**Inertia** (la física del drag momentum): `type: "inertia"` desacelera desde la velocidad actual — `power`, `timeConstant`, `modifyTarget` (para snap a grid), `min`/`max` con `bounceStiffness`/`bounceDamping`.

**Repetición y orquestación:**

```tsx
<motion.div
  animate={{ rotate: 360 }}
  transition={{
    delay: 0.2,          // negativo = empieza a mitad de la animación
    repeat: Infinity,    // o un número
    repeatType: "loop",  // "loop" reinicia | "reverse" alterna dirección (ping-pong) | "mirror" invierte valores
    repeatDelay: 0.5,
    duration: 2,
  }}
/>
```

> Nota de diseño para loops: si el loop es **continuo** (marquee, radar), usa `ease: "linear"` — con ease-in-out el movimiento acelera y frena en cada ciclo. Y **pausa los loops fuera del viewport**; si varios loops deben estar sincronizados entre sí, no los pauses (la pausa los desfasa) — es la única excepción.

**Transición por propiedad** (cada propiedad su propia física):

```tsx
<motion.div
  animate={{ x: 100, opacity: 1 }}
  transition={{
    default: { type: "spring", stiffness: 300 },
    opacity: { ease: "linear", duration: 0.2 },
    layout: { duration: 0.3 },        // también para layout animations
  }}
/>
```

## 9. Gestos: hover, tap, focus, inView

```tsx
<motion.button
  whileHover={{ scale: 1.03 }}
  whileTap={{ scale: 0.97 }}
  whileFocus={{ boxShadow: "0 0 0 2px var(--ring)" }}   // dispara con :focus-visible
  onHoverStart={() => {}} onHoverEnd={() => {}}
  onTap={() => {}}
/>
```

- `whileTap` es **accesible por teclado** (Enter) automáticamente.
- Todos los `while*` aceptan objetos de animación **o nombres de variant** (que se propagan a los hijos).
- `propagate={{ tap: false }}` en un hijo bloquea el gesto del padre sin `stopPropagation` manual.
- Al terminar el gesto, el elemento **regresa solo** al estado de `animate`.

### `whileInView` — scroll-triggered reveals

```tsx
<motion.section
  initial={{ opacity: 0, y: 24 }}
  whileInView={{ opacity: 1, y: 0 }}
  viewport={{
    once: true,        // ⭐ casi siempre true — el replay al re-scrollear es ruido
    amount: 0.3,       // "some" (default) | "all" | 0–1 — porción visible requerida
    margin: "0px 0px -80px 0px",  // como rootMargin de IntersectionObserver
  }}
  transition={{ duration: 0.5, ease: [0.23, 1, 0.32, 1] }}
/>
```

Callbacks: `onViewportEnter(entry)` / `onViewportLeave(entry)`.

## 10. Variants y orquestación (stagger)

Las variants son estados nombrados que **se propagan del padre a los hijos** — la base de la coreografía.

**⚠️ CAMBIO IMPORTANTE (12.22.0, jul-2025): `staggerChildren` y `staggerDirection` están DEPRECADOS.** El patrón actual es `delayChildren: stagger()`:

```tsx
import { motion, stagger } from "motion/react"

const list = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      when: "beforeChildren",           // el padre termina antes de que empiecen los hijos
      delayChildren: stagger(0.08, {    // ANTES ERA: staggerChildren: 0.08
        startDelay: 0.2,                // delay antes del primer hijo
        from: "center",                 // "first" | "last" | "center" | índice
        ease: "easeOut",                // redistribuye los delays
      }),
    },
  },
}

const item = {
  hidden: { opacity: 0, y: 16 },
  visible: { opacity: 1, y: 0 },
}

<motion.ul variants={list} initial="hidden" animate="visible">
  {items.map((it) => <motion.li key={it.id} variants={item}>{it.label}</motion.li>)}
</motion.ul>
```

**Reglas de propagación:** los hijos sin `animate` propio heredan el cambio de variant del padre — solo necesitan declarar `variants`. Se corta con `inherit={false}`. `when: "afterChildren"` invierte el orden (útil en exits).

**Variants dinámicas con `custom`:**

```tsx
const variants = { visible: (i: number) => ({ opacity: 1, transition: { delay: i * 0.05 } }) }
{items.map((item, i) => (
  <motion.div key={item.id} custom={i} initial="hidden" animate="visible" variants={variants} />
))}
```

**Spec de stagger premium:** 50–100ms entre elementos de un grupo; para page loads orquestados, 50–80ms entre bloques y máximo ~5–6 bloques. Demasiado stagger = coreografía que estorba.

## 11. AnimatePresence: animaciones de salida

React desmonta los elementos instantáneamente; `AnimatePresence` los mantiene vivos mientras corre su animación `exit`.

```tsx
import { AnimatePresence, motion } from "motion/react"

<AnimatePresence>
  {show && (
    <motion.div
      key="modal"                        // OBLIGATORIO: key única y ESTABLE
      initial={{ opacity: 0, scale: 0.95 }}
      animate={{ opacity: 1, scale: 1 }}
      exit={{ opacity: 0, scale: 0.97 }}
      transition={{ duration: 0.2 }}
    />
  )}
</AnimatePresence>
```

**Modos:**

- `mode="sync"` (default): entrada y salida animan a la vez.
- `mode="wait"`: el que entra **espera** a que el que sale termine. Ideal para cambios de contenido/ruta. (**Reemplaza al viejo `exitBeforeEnter`, que lanza error.**)
- `mode="popLayout"`: saca al saliente del flujo de layout para que los hermanos se reacomoden de inmediato — combínalo con `layout` en los hermanos.

**Props y hooks clave:** `initial={false}` (sin animación en el primer montaje); `onExitComplete`; `propagate` (los AnimatePresence anidados disparan las salidas de sus hijos cuando el padre sale — crucial en transiciones de página); `usePresence()` / `useIsPresent()` para salidas manuales.

**Pitfalls clásicos:**

1. **Keys por índice de array** → al eliminar/reordenar anima el elemento equivocado. Usa IDs estables.
2. **El condicional va DENTRO** de AnimatePresence: `<AnimatePresence>{show && ...}</AnimatePresence>` — nunca `{show && <AnimatePresence>...}`.
3. **Cambiar la `key` con el estado** es la forma idiomática de transicionar entre pantallas: `<motion.div key={route} ... />`.

## 12. Layout animations y `layoutId`

Motion anima cambios de layout con **transform** (técnica FLIP), nunca con width/height directos → sin repaints, 60–120fps, y anima cosas imposibles en CSS como `justify-content` o `flex-direction`.

```tsx
<motion.div layout />              // anima automáticamente cambios de tamaño/posición entre renders
<motion.img layout="position" />   // solo posición (evita distorsión en imágenes/texto)
<motion.div layout="size" />       // solo tamaño
```

### `layoutId` — shared element / "magic motion"

El elemento nuevo **anima desde** el elemento viejo con el mismo `layoutId`. El caso canónico — el underline deslizante de tabs:

```tsx
{tabs.map((tab) => (
  <li key={tab.id} onClick={() => setSelected(tab.id)} style={{ position: "relative" }}>
    {tab.label}
    {tab.id === selected && (
      <motion.div
        layoutId="underline"
        style={{ position: "absolute", bottom: 0, left: 0, right: 0, height: 2, background: "var(--accent)" }}
        transition={{ type: "spring", stiffness: 350, damping: 30 }}
      />
    )}
  </li>
))}
```

Tarjeta expandible: la card en la grilla y la card expandida (modal) comparten `layoutId={card.id}` → morph automático entre ambas; con AnimatePresence también anima de regreso al cerrar. Es el principio "Cloning" de Willenskomer hecho una prop.

**Detalles importantes:**

- `LayoutGroup` agrupa hermanos que se afectan mutuamente pero no re-renderizan juntos (dos acordeones); también namespacea layoutIds con `id="..."`.
- **Distorsión de hijos**: al animar tamaño con scale, los hijos se deforman → pon `layout` también en los hijos (contra-escala automática) o usa `layout="position"`.
- **Gotcha #1**: `borderRadius` y `boxShadow` se corrigen automáticamente **solo si van en `style`**, no por CSS externo (si no, las esquinas se "estiran" durante la animación).
- Performance: `layoutDependency={isOpen}` mide solo cuando ese valor cambia. `layoutScroll` en contenedores con scroll propio; `layoutRoot` en `position: fixed`.
- No funciona en `display: inline`; SVG no soporta `layout`.

**Listas reordenables** (`Reorder`, con drag y layout integrados — desde 13.1.0 multidimensional y RTL):

```tsx
import { Reorder } from "motion/react"

<Reorder.Group axis="y" values={items} onReorder={setItems}>
  {items.map((item) => <Reorder.Item key={item} value={item}>{item}</Reorder.Item>)}
</Reorder.Group>
```

## 13. Scroll: useScroll, parallax, scroll-linked

Dos categorías: **scroll-triggered** (dispara una animación al entrar al viewport → `whileInView`, §9) y **scroll-linked** (el valor está atado 1:1 a la posición del scroll → `useScroll`). En scroll-linked **el visitante conduce la animación** — no hay duración; si quieres suavizado, se hace con springs.

`useScroll` devuelve 4 motion values: `scrollX/scrollY` (px) y `scrollXProgress/scrollYProgress` (0–1).

```tsx
// Barra de progreso de lectura
import { motion, useScroll, useSpring } from "motion/react"

function ProgressBar() {
  const { scrollYProgress } = useScroll()
  const scaleX = useSpring(scrollYProgress, { stiffness: 100, damping: 30, restDelta: 0.001 })
  return <motion.div style={{ scaleX, originX: 0, position: "fixed", top: 0, left: 0, right: 0, height: 3 }} />
}
```

**Tracking de un elemento concreto** (la animación ocurre mientras el elemento cruza el viewport):

```tsx
const ref = useRef(null)
const { scrollYProgress } = useScroll({
  target: ref,
  offset: ["start end", "end start"],
  // "start end" = el inicio del target toca el fin del viewport (entra por abajo)
  // "end start" = el fin del target toca el inicio del viewport (sale por arriba)
})
```

**Parallax** — el principio: *lo cercano se mueve más rápido, lo lejano más lento* (como los árboles vs las montañas desde un coche en movimiento). Si un elemento solapa a otro, está "más cerca" → debe moverse más rápido, o la profundidad se siente falsa:

```tsx
function Parallax({ children, distance = "20%" }) {
  const ref = useRef(null)
  const { scrollYProgress } = useScroll({ target: ref, offset: ["start end", "end start"] })
  const y = useTransform(scrollYProgress, [0, 1], [`-${distance}`, distance])
  return (
    <section ref={ref} style={{ position: "relative", overflow: "hidden" }}>
      <motion.div style={{ y }}>{children}</motion.div>
    </section>
  )
}
```

⚠️ **Parallax con freno**: diferencias de velocidad pequeñas (10–15%), solo decorativo, y apagado bajo `prefers-reduced-motion` (es un trigger vestibular clásico).

**Efectos derivados** con `useTransform` sobre strings: `["blur(0px)", "blur(10px)"]`, `["inset(0 50% 0 50%)", "inset(0 0 0 0)"]` (image reveal con clip-path), colores, etc.

**Scroll horizontal**: contenedor de `300vh` + hijo `position: sticky; top: 0` + `useTransform(scrollYProgress, [0,1], ["0%", "-66%"])` en el `x` del track interno. (El patrón "sticky + conducido por scroll" también es la base de secciones tipo "teléfono fijo que cambia de contenido mientras el texto scrollea".)

## 14. Motion values: animar sin re-renders

Los motion values actualizan el DOM **sin re-renders de React** — la base del rendimiento de Motion.

```tsx
import { motion, useMotionValue, useTransform, useMotionTemplate,
         useSpring, useVelocity, useMotionValueEvent } from "motion/react"

function Card() {
  const x = useMotionValue(0)

  // Mapeo de rangos → números, colores, strings
  const opacity = useTransform(x, [-200, 0, 200], [0, 1, 0])
  const background = useTransform(x, [-200, 200], ["#f00", "#00f"])

  // Forma computada (se suscribe sola a los .get() que use)
  const doubled = useTransform(() => x.get() * 2)

  // Multi-output en una llamada (nuevo)
  const { scale, filter } = useTransform(x, [0, 200], {
    scale: [1, 0.6],
    filter: ["blur(0px)", "blur(10px)"],
  })

  // Componer strings CSS
  const blur = useSpring(0)
  const backdropFilter = useMotionTemplate`blur(${blur}px)`

  const velocity = useVelocity(x)                       // px/s de otro motion value
  useMotionValueEvent(x, "change", (v) => console.log(v))  // suscripción idiomática

  return <motion.div drag="x" style={{ x, opacity, background, scale, filter, backdropFilter }} />
}
```

API imperativa: `.get()`, `.set(v)` (sin re-render), `.jump(v)` (rompe continuidad), `.getVelocity()`, `.stop()`. `useSpring(value, config)` suaviza cualquier valor. `useTime()` para loops procedurales. El mismo motion value puede compartirse entre varios componentes para sincronizarlos.

## 15. Drag

```tsx
<motion.div
  drag                          // true | "x" | "y"
  dragConstraints={{ top: -50, left: -50, right: 50, bottom: 50 }}
  dragElastic={0.2}             // 0–1 (default 0.5) — cuánto "cede" fuera de los límites
  dragMomentum={true}           // inercia al soltar (default true)
  dragSnapToOrigin              // vuelve al origen al soltar
  whileDrag={{ scale: 1.04, cursor: "grabbing" }}
  onDragEnd={(e, info) => { /* info.offset, info.velocity, info.point */ }}
/>
```

**Constraints por ref** (los límites son la caja de otro elemento):

```tsx
const constraintsRef = useRef(null)
<motion.div ref={constraintsRef}>
  <motion.div drag dragConstraints={constraintsRef} />
</motion.div>
```

**Patrón swipe-to-dismiss** (cards, carruseles, toasts — el patrón físico premium):

```tsx
<motion.div
  drag="x"
  dragConstraints={{ left: 0, right: 0 }}    // elástico: siempre regresa…
  onDragEnd={(e, { offset, velocity }) => {
    const swipe = Math.abs(offset.x) * velocity.x       // "swipe power" = distancia × velocidad
    if (swipe < -10000) paginate(1)
    else if (swipe > 10000) paginate(-1)                // …salvo que el swipe supere el umbral
  }}
/>
```

Drag handles: `useDragControls()` + `dragListener={false}` → `dragControls.start(event)`. Gotcha: `<img draggable={false} />` para matar el ghost nativo del navegador.

## 16. Animación imperativa: useAnimate

Para secuencias y control fino fuera del modelo declarativo:

```tsx
import { useAnimate, stagger } from "motion/react"

function Menu() {
  const [scope, animate] = useAnimate()   // scope = ref; los selectores se limitan a sus hijos

  const open = async () => {
    await animate(scope.current, { opacity: 1 })
    animate("li", { x: [-20, 0], opacity: 1 }, { delay: stagger(0.05), duration: 0.3 })
  }
  return <ul ref={scope}>…</ul>
}
```

**Secuencias / timelines** (segmentos en serie; `at` los recoloca):

```tsx
animate([
  ["ul", { opacity: 1 }, { duration: 0.3 }],
  ["li", { x: [-100, 0] }, { delay: stagger(0.05), at: "-0.1" }],   // "-0.1" solapa con el anterior; "<" = junto al anterior
])
```

La animación devuelta es thenable (`await animate(...)`) y tiene controles: `.play()`, `.pause()`, `.stop()`, `.speed = 2` (o `-1` para reversa), `.time`.

## 17. Configuración global y accesibilidad

```tsx
import { MotionConfig } from "motion/react"

<MotionConfig
  transition={{ type: "spring", visualDuration: 0.3, bounce: 0.15 }}  // default global — TUS TOKENS
  reducedMotion="user"   // respeta el setting del OS: desactiva transforms/layout, CONSERVA opacity/color
>
  <App />
</MotionConfig>
```

`reducedMotion="user"` es el enfoque premium correcto: **reemplazar movimiento por fades, no eliminar el feedback** (§41). Para control granular:

```tsx
import { useReducedMotion } from "motion/react"

const shouldReduceMotion = useReducedMotion()  // boolean reactivo
<motion.div animate={shouldReduceMotion ? { opacity: 1 } : { opacity: 1, y: 0 }} />
```

⚠️ **v13.0.0**: si usas styled-components/Emotion, ahora debes inyectar `<MotionConfig isValidProp={isPropValid}>` (antes era automático).

## 18. Bundle size: LazyMotion

Números actuales: `motion` completo ≈ **34kb** · `m` + `LazyMotion` ≈ **<4.6kb** iniciales · `domAnimation` **+15kb** (animaciones, variants, exit, hover/tap/focus) · `domMax` **+25kb** (todo + drag + layout) · `useAnimate` mini **2.3kb**.

```tsx
import { LazyMotion, domAnimation } from "motion/react"
import * as m from "motion/react-m"

<LazyMotion features={domAnimation} strict>
  <m.div animate={{ opacity: 1 }} />   {/* strict lanza error si alguien usa motion.div */}
</LazyMotion>
```

Con carga asíncrona (`features={() => import("./features").then(r => r.default)}`) las features salen del bundle inicial.

## 19. Animación de texto

**No hay splitting integrado gratis en el core.** Opciones reales:

**a) Motion+ (de pago):** `splitText` divide en chars/words/lines con ARIA correcto:

```tsx
import { splitText } from "motion-plus"
import { animate, stagger } from "motion"

useEffect(() => {
  document.fonts.ready.then(() => {          // esperar las fuentes o el split sale mal
    const { chars } = splitText(headingRef.current)
    animate(chars, { opacity: [0, 1], y: [10, 0] }, { duration: 0.8, delay: stagger(0.03) })
  })
}, [])
```

**b) Manual (gratis, el patrón universal):**

```tsx
function AnimatedHeading({ text }: { text: string }) {
  const container = { hidden: {}, visible: { transition: { delayChildren: stagger(0.04) } } }
  const char = {
    hidden: { opacity: 0, y: "0.4em", filter: "blur(6px)" },
    visible: { opacity: 1, y: 0, filter: "blur(0px)", transition: { type: "spring", damping: 16 } },
  }
  return (
    <motion.h1 variants={container} initial="hidden" animate="visible" aria-label={text}>
      {text.split(" ").map((word, wi) => (
        <span key={wi} style={{ display: "inline-block", whiteSpace: "pre" }} aria-hidden>
          {word.split("").map((c, ci) => (
            <motion.span key={ci} variants={char} style={{ display: "inline-block" }}>{c}</motion.span>
          ))}{" "}
        </span>
      ))}
    </motion.h1>
  )
}
```

Claves: `display: inline-block` en cada span (los inline no aceptan transform); chars agrupados por palabra para que el line-wrap no las rompa; `aria-label` en el padre + `aria-hidden` en los spans.

**Reglas de gusto para texto animado:**

- **Word-level > char-level para casi todo.** El patrón "premium AI-era" (Vercel, Linear): por palabra, `opacity 0→1` + `translateY(8–12px)→0` + `blur(4–8px)→0`, ease-out, ~500–700ms total.
- **Char-stagger solo si**: es un hero/headline corto, ocurre UNA vez por sesión, stagger de 10–30ms por char y total <800ms. Gimmicky si: body text, se repite en cada scroll, o retrasa la lectura.
- **Line-mask reveal** (cada línea sube dentro de un `overflow: hidden`) es lo más elegante para headlines multilínea.
- **El texto es para LEERSE. Un poco de motion rinde mucho** — la moderación es la firma premium.

## 20. Lo nuevo de la era Motion (que los tutoriales viejos no tienen)

1. **API vanilla completa**: `animate`, `scroll`, `inView`, `hover`, `press`, `stagger` desde `"motion"` — ya no es solo React. Mini `animate` de 2.3kb (`motion/mini`, WAAPI puro).
2. **`animateView` / `<AnimateView>`** (12.41.0): integración con la View Transitions API — wipes de pantalla completa y shared elements snapshot-based. Complementa (no reemplaza) a las layout animations: layout es interrumpible y no bloquea el puntero; view transitions permiten efectos full-screen pero no son interrumpibles.
3. **`visualDuration`** en springs (11.12.0) — afinar springs por sensación.
4. **`delayChildren: stagger()`** (12.22.0) con `from` y `ease` — reemplaza a `staggerChildren`.
5. **Motion+** (membresía de pago): `AnimateNumber` (tickers, 2.5kb), `splitText`, `Cursor` (cursores custom magnéticos), `Ticker` (marquees), `Carousel`.
6. **RSC/Next.js**: `import * as motion from "motion/react-client"` para usar `motion.div` en Server Components.
7. **Rendimiento**: aceleración por hardware también para `backgroundColor` y SVG; colores `oklch`/`color-mix`; `useTransform` multi-output.

---
---

# PARTE III — CATÁLOGO DE MICRO-INTERACCIONES PREMIUM

Cada patrón con su spec: qué anima, cuánto dura, qué curva, y qué comunica.

## 21. Botones

**Press (lo más importante):** `scale(0.97)` en active/tap, **100–150ms ease-out**. Comunica "la interfaz reaccionó". Escala proporcional al tamaño: botones chicos hasta 0.96; nunca extremos.

```tsx
<motion.button
  whileTap={{ scale: 0.97 }}
  whileHover={{ scale: 1.02 }}
  transition={{ duration: 0.12, ease: "easeOut" }}
/>
```

```css
/* Versión CSS pura (suficiente casi siempre) */
.button { transition: transform 120ms ease-out, background-color 150ms ease; }
.button:active { transform: scale(0.97); transition-duration: 75ms; }
```

**Hover:** cambio de color/background con `ease` 150ms; lift opcional `translateY(-1px)` + sombra. **El font-weight jamás cambia en hover** (layout shift). Evita el "doom flicker": si el hover mueve al elemento y este se sale de debajo del cursor, anima un hijo cosmético, no el que recibe el hover.

**Loading:** deshabilita tras submit (evita dobles requests). Spinner **solo si la espera supera ~300ms** (y si aparece, sostenlo un mínimo de ~300–500ms para que no parpadee). El morph botón → spinner → checkmark es el principio de Transformation: narrativa continua — resérvalo para acciones poco frecuentes (checkout, guardar algo importante).

## 22. Inputs y formularios

- **Focus ring**: con `box-shadow` (respeta el border-radius; `outline` clásico no) — transición de ~120ms o instantánea. **Nunca elimines el ring de `:focus-visible`.**
- **Error shake**: 300–400ms, 2–3 oscilaciones de ±4–8px, ease-in-out, **una sola vez**, siempre acompañado del mensaje de error (el motion nunca es el único canal de información).

```tsx
<motion.div animate={hasError ? { x: [0, -6, 6, -4, 4, 0] } : {}} transition={{ duration: 0.35 }}>
  <input aria-invalid={hasError} />
</motion.div>
```

- **Label flotante**: 150–200ms ease-out, translateY + scale ~0.85.
- **Móvil**: font-size mínimo **16px** en inputs (evita el zoom automático de iOS).

## 23. Toggles y checkboxes

- **El efecto se aplica de inmediato, sin animación de confirmación previa.** Feedback total en ~100ms.
- Knob del toggle: 150–200ms ease-out o spring seco (`stiffness 400, damping 30`). Con layout animation es trivial:

```tsx
<div className={isOn ? "toggle on" : "toggle"} onClick={() => setOn(!isOn)}>
  <motion.div className="knob" layout transition={{ type: "spring", stiffness: 500, damping: 32 }} />
</div>
/* CSS: .toggle { display:flex } .toggle.on { justify-content: flex-end } — Motion anima el cambio de justify-content */
```

- Checkmark: dibujado con `pathLength` (SVG) ~200ms.

## 24. Cards

- Hover: elevación con sombra + `translateY(-2px)`, ~200ms. La sombra se anima BARATO con un pseudo-elemento (§35).
- Asimetría de hover elegante: entrada rápida (~125ms), salida relajada (~250–300ms).
- Tilt 3D solo en contexto showcase/landing — nunca en apps densas.
- Patrón de grupo con `:has()`: al hacer hover en una card, las hermanas se atenúan (§38).
- El contenido esencial de la card **no puede vivir solo en el hover** (en touch no existe hover — §40).

## 25. Dropdowns y popovers

**El spec estándar (shadcn/Radix):** entrada con `opacity 0→1` + `scale(0.95→1)` + `translateY` 4–8px hacia el trigger, **150–200ms ease-out**. Salida más corta (~100–150ms).

- **`transform-origin` apunta al trigger, no al centro** — Radix lo expone: `var(--radix-dropdown-menu-content-transform-origin)`.
- **Nunca `scale(0)`** — desde 0.95.
- Submenús anidados: "prediction cone" o `transition-delay: 300ms` en el cierre para permitir el movimiento diagonal del cursor.
- Abrir en `mousedown`, no en `click` (se siente más rápido).
- Menús contextuales (click derecho): **sin animación** — se usan constantemente.

## 26. Modales y drawers

**Modal/dialog:**

- Overlay: fade 0→1, ~200ms.
- Panel: `opacity` + `scale(0.95→1)` (+ `y: 8px` opcional), **200–300ms ease-out**.
- **Exit MÁS RÁPIDO que enter: ~150–200ms.** El usuario ya decidió irse.

```tsx
<AnimatePresence>
  {open && (
    <>
      <motion.div key="overlay" className="overlay"
        initial={{ opacity: 0 }} animate={{ opacity: 1 }} exit={{ opacity: 0 }}
        transition={{ duration: 0.2 }} />
      <motion.div key="panel" className="panel"
        initial={{ opacity: 0, scale: 0.95, y: 8 }}
        animate={{ opacity: 1, scale: 1, y: 0, transition: { duration: 0.25, ease: [0.23, 1, 0.32, 1] } }}
        exit={{ opacity: 0, scale: 0.97, transition: { duration: 0.15, ease: "easeIn" } }} />
    </>
  )}
</AnimatePresence>
```

**Drawer / bottom sheet móvil:** el estándar de oro es **Vaul** (Emil Kowalski; es el Drawer de shadcn): sigue al dedo 1:1, snap points, dismiss por velocidad del gesto, curva `cubic-bezier(0.32, 0.72, 0, 1)` (la de iOS), hasta 500ms porque es interrumpible, y el fondo escala sutilmente (profundidad).

## 27. Toasts (el spec de Sonner)

Sonner (Emil Kowalski) es la referencia. Su spec completo:

- **Enter**: `translateY(100%) → 0`, **400ms, easing `ease`** — deliberadamente más lento y elegante que un dropdown, porque el toast **no lo inició el usuario** y no debe competir con su acción en curso.
- **Stack**: `position: absolute`; offset Y = gap × índice; `scale = 1 − 0.05 × índice` (0.95, 0.90…); pseudo-elementos rellenan los gaps para sostener el hover del grupo.
- **Swipe dismiss**: umbral por distancia o **velocidad ≥ 0.11 px/ms**; fricción al arrastrar en la dirección "incorrecta".
- **Exit**: más rápido (~200ms), **hacia el borde por el que entró** (consistencia espacial).
- Timer de 4s que **se pausa en hover y con la pestaña oculta**.

## 28. Tooltips

- Delay largo en el primer hover (Radix: `delayDuration: 700ms`), y **ventana sin delay** para los siguientes (`skipDelayDuration: 300ms`).
- **Regla premium**: con un tooltip ya abierto, pasar a otro trigger lo abre **sin delay Y sin animación** — el usuario está explorando, no descubriendo.
- Animación cuando aplica: fade + `scale 0.96` + 2–4px hacia el trigger, ~120–150ms.
- Nunca contenido interactivo dentro de un tooltip de hover; nunca tooltip sobre un botón disabled.

## 29. Tabs

- El que anima es el **indicador** (underline/fondo), no el contenido: `layoutId` con spring `stiffness 300–400, damping ~30` (código en §12).
- El contenido cambia con fade corto (~150ms) o sin animación.
- Si el cambio de tab es por teclado: sin animación.

## 30. Accordions

- Animar la **altura real**: con Radix, `var(--radix-accordion-content-height)`; en CSS puro, el truco `grid-template-rows: 0fr → 1fr`; o `interpolate-size` en Chromium (§38). **200–300ms ease-out.**
- Nunca `height: auto` directo (no interpola) ni animar padding/margin (layout thrash).
- Contenido interno con fade ligero.

```css
.accordion-panel { display: grid; grid-template-rows: 0fr; transition: grid-template-rows 250ms ease-out; }
.accordion-panel[data-open] { grid-template-rows: 1fr; }
.accordion-panel > div { overflow: hidden; }
```

## 31. Skeletons y estados de carga

- **< 300ms de espera: no muestres NADA** (el flash de un loader que aparece y desaparece es peor que la espera).
- **0.5–3s: skeleton**, solo si **calca el layout real** (mismos tamaños y posiciones — si no, empeora la percepción). Shimmer: gradiente en loop 1.5–2s linear; o pulse de opacidad 0.5↔1 a ~2s. Pausa los loops fuera del viewport.
- **> 3–5s: spinner/progreso con texto honesto.**
- **Optimistic UI**: aplica el cambio visual al instante y reconcilia con el servidor después; toda acción da feedback (press state) en <100ms aunque el trabajo real tarde.
- **Stagger como cortina**: una carga orquestada con reveals escalonados (50–80ms entre bloques, máx 5–6 bloques) enmascara el fetch y concentra el "delight" donde importa.

## 32. Transiciones de página

- Web ≠ app nativa: por defecto **crossfade corto (200–300ms) o nada**.
- Transiciones espaciales (push/slide) **solo** si la app tiene una metáfora de navegación clara — y entonces la dirección es consistente: forward entra por la derecha, back es el espejo exacto.
- **Nunca bloquees la interacción** esperando la transición.
- Elementos persistentes (navbar, sidebar): **no participan** de la transición — se quedan quietos, sus animaciones internas continúan sin interrupción. Que el header se vaya y regrese idéntico es desorientador y grita "template".
- Implementación en Next.js: §39. Shared elements (imagen de la card → imagen del detalle): `layoutId` o View Transitions.

## 33. Números y contadores

- `font-variant-numeric: tabular-nums` **obligatorio** en cifras que cambian (si no, el layout baila).
- Rolling digits (estilo NumberFlow / AnimateNumber de Motion+): los dígitos deslizan verticalmente con spring suave, 500–700ms — solo cuando el cambio de valor **es** la noticia (principio Value Change).
- Count-ups en dashboards: una vez, al entrar al viewport, con ease-out fuerte (expo).

```tsx
// Count-up simple con Motion
function Counter({ to }: { to: number }) {
  const ref = useRef<HTMLSpanElement>(null)
  useEffect(() => {
    const controls = animate(0, to, {
      duration: 1.2, ease: [0.19, 1, 0.22, 1],
      onUpdate: (v) => { if (ref.current) ref.current.textContent = Math.round(v).toLocaleString() },
    })
    return () => controls.stop()
  }, [to])
  return <span ref={ref} style={{ fontVariantNumeric: "tabular-nums" }} />
}
```

## 34. Scroll UX: reveals, parallax y etiqueta

- **Anima UNA sola vez** (`viewport={{ once: true }}`) — el replay al re-scrollear es ruido y cansa.
- Trigger cuando ~20–30% del elemento es visible (`amount: 0.2–0.3`).
- **Distancia de 10–30px, no 100px** — el slide largo se siente "template de agencia".
- Duración 400–600ms ease-out; nunca >800ms.
- Stagger en grupos: 50–100ms entre elementos.
- **El hero no bloquea**: el contenido above-the-fold aparece de inmediato o con stagger total <600ms. Nada de esperar 2 segundos para poder leer el H1.
- **Scroll-jacking = malo, siempre.** Secuestrar velocidad/dirección del scroll rompe la interruptibilidad y el modelo mental. `scroll-behavior: smooth` solo para anclas internas.
- Parallax: sutil (§13), decorativo, apagado con reduced-motion.

---
---

# PARTE IV — IMPLEMENTACIÓN Y ECOSISTEMA

## 35. Rendimiento: qué animar y qué jamás animar

### El pixel pipeline

`Style → Layout → Paint → Composite`. Cuanto más arriba entras, más caro cada frame:

| Tier | Propiedades | Costo |
|---|---|---|
| ✅ **Composite** | `transform`, `opacity` (y condicionalmente `filter`, `clip-path`) | GPU pura — gratis en la práctica |
| ⚠️ **Paint** | `background-color`, `color`, `border-radius`, `box-shadow`, **CSS variables** | Repintar cada frame |
| ❌ **Layout** | `width`, `height`, `top`, `left`, `margin`, `padding`, `display` | Recalcular geometría de todo el árbol |
| 💀 **Layout thrashing** | Leer `offsetHeight` y escribir estilos intercalado en un loop | Muerte del frame rate |

**Regla absoluta: las interacciones animan SOLO `transform` y `opacity`.** Presupuesto: 60fps = ~16.7ms/frame (real ~10ms); 120fps = ~8.3ms.

### Técnicas clave

- **`will-change`: con moderación y solo ante un problema real.** Cada uno promueve una capa que consume memoria GPU. Aplícalo justo antes de animar (p. ej. solo en `:hover`), nunca global.
- **Sombras baratas**: pre-renderiza la sombra final en un `::after` con `opacity: 0` y transiciona solo la opacidad:

```css
.card { position: relative; box-shadow: 0 1px 2px rgba(0,0,0,.15); }
.card::after {
  content: ""; position: absolute; inset: 0; z-index: -1; border-radius: inherit;
  box-shadow: 0 8px 24px rgba(0,0,0,.25);
  opacity: 0; transition: opacity 250ms ease;
}
.card:hover::after { opacity: 1; }
```

- **`content-visibility: auto`** en secciones largas: el navegador se salta layout/paint fuera del viewport (Baseline sept 2025; mejoras de 7x en render inicial documentadas).
- **FLIP** (First, Last, Invert, Play): medir posición inicial y final, invertir con transform, animar hacia identidad — es lo que hacen las layout animations de Motion por dentro; anima "cambios de layout" sin pagar layout por frame.
- **CSS vs JS**: estados y micro-interacciones → CSS (sobreviven a bloqueos del main thread); orquestación, gestos y física → JS (Motion, que además usa WAAPI acelerado por hardware para transform/opacity/filter).
- Los blurs grandes (`backdrop-filter`) son el asesino silencioso en móviles de gama media — dosifícalos.

## 36. Tailwind CSS v4 y animación

**Cambio grande de v4: config CSS-first.** Ya no hay `tailwind.config.js` por defecto; los tokens viven en CSS con `@theme` (y cada token genera su utility automáticamente):

```css
@import "tailwindcss";

@theme {
  /* Tokens de motion → generan duration-fast, ease-out-quint, animate-wiggle, etc. */
  --duration-fast: 150ms;
  --duration-base: 250ms;
  --duration-slow: 400ms;
  --ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);
  --ease-io-quart: cubic-bezier(0.77, 0, 0.175, 1);

  --animate-wiggle: wiggle 1s ease-in-out infinite;
  @keyframes wiggle {
    0%, 100% { transform: rotate(-3deg); }
    50%      { transform: rotate(3deg); }
  }
}
```

```html
<button class="transition duration-fast ease-out-quint hover:bg-accent active:scale-95">…</button>
<div class="animate-wiggle">…</div>
<div class="duration-[420ms] ease-[cubic-bezier(0.32,0.72,0,1)]">…</div>  <!-- valores arbitrarios -->
```

- Built-ins: `transition`, `transition-colors/opacity/transform/shadow`, `duration-*`, `delay-*`, `ease-*`, `animate-spin/pulse/ping/bounce/none`.
- **`tw-animate-css` reemplaza a `tailwindcss-animate`** (el plugin viejo es incompatible con v4; shadcn/ui ya migró): `npm i -D tw-animate-css` + `@import "tw-animate-css";` → te da `animate-in`/`animate-out` con `fade-in-*`, `zoom-in-*`, `slide-in-from-*`.
- Reduced motion nativo: `motion-safe:animate-...` / `motion-reduce:animate-none`.
- **⚠️ Cambio de v4 que muerde:** `hover:` ahora solo aplica dentro de `@media (hover: hover)` — se acabó el sticky hover en móvil, PERO si usabas `hover:` para revelar contenido esencial, en touch ya no aparece nunca (§40).

## 37. shadcn/ui + Radix: cómo animan y cómo personalizarlos

El trío: **Radix expone el estado como atributo** (`data-state="open|closed"`, `data-side`) + **CSS keyframes** + **tw-animate-css**:

```tsx
<PopoverContent className="
  data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:zoom-in-95
  data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95
  data-[side=bottom]:slide-in-from-top-2
  duration-200
" />
```

- Los dropdowns entran "desde" su trigger (`data-[side=...]` + slide direccional) — origin-aware gratis.
- Radix **retrasa el unmount hasta que termina la animación de salida** (detecta la animation en curso) — por eso `animate-out` funciona.
- Como el código shadcn vive en tu repo, personalizar = editar clases: cambia `zoom-in-95` por `slide-in-from-bottom-4`, ajusta `duration-*`, o usa tus keyframes de `@theme`.

**Combinar con Motion cuando necesitas más** (springs, drag): Radix maneja estado/portal/accesibilidad, Motion la animación, con `forceMount` + AnimatePresence:

```tsx
<AnimatePresence>
  {open && (
    <Dialog.Portal forceMount>
      <Dialog.Content asChild forceMount>
        <motion.div
          initial={{ opacity: 0, scale: 0.95, y: 8 }}
          animate={{ opacity: 1, scale: 1, y: 0 }}
          exit={{ opacity: 0, scale: 0.97 }}
          transition={{ type: "spring", duration: 0.3, bounce: 0.1 }}
        >…</motion.div>
      </Dialog.Content>
    </Dialog.Portal>
  )}
</AnimatePresence>
```

Complementos del mismo nivel: **Vaul** (drawer, §26) y **Sonner** (toasts, §27) — ambos integrados en shadcn.

## 38. CSS nativo moderno que reduce la necesidad de JS

Estado de soporte verificado a 2026:

### View Transitions API — ✅ Baseline (same-document, oct 2025)

```css
@view-transition { navigation: auto; }   /* cross-document: activa transiciones entre páginas MPA */
::view-transition-old(root) { animation: 200ms ease-out both fade-out; }
::view-transition-new(root) { animation: 300ms ease-in both fade-in; }
.hero-img { view-transition-name: hero; }  /* morph de elemento compartido */
```

Same-document en todos los navegadores; cross-document falta solo Firefox (flag).

### `@starting-style` + `allow-discrete` — ✅ Baseline (ago 2024)

Animar entrada/salida desde `display: none` **sin JS** — mata gran parte del caso de uso de AnimatePresence para popovers/dialogs nativos:

```css
dialog {
  opacity: 0; translate: 0 12px;
  transition: opacity 250ms, translate 250ms, display 250ms allow-discrete, overlay 250ms allow-discrete;
}
dialog[open] {
  opacity: 1; translate: 0 0;
  @starting-style { opacity: 0; translate: 0 12px; }
}
```

### Scroll-driven animations — Chrome/Edge 115+, Safari 26; Firefox aún flag

Reveals y parallax **fuera del main thread**, sin librería — usar con `@supports` como progressive enhancement:

```css
@supports (animation-timeline: view()) {
  .reveal {
    animation: fade-slide-in linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 60%;
  }
}
@keyframes fade-slide-in { from { opacity: 0; transform: translateY(1.5rem); } }

.progress-bar { animation: grow linear both; animation-timeline: scroll(root); }
@keyframes grow { from { transform: scaleX(0); } }
```

### `linear()` — springs en CSS puro — ✅ cross-browser

No hay `spring()` nativo; se muestrea la curva del resorte en puntos que `linear()` interpola. Motion los genera: `import { spring } from "motion"` → string para CSS. Generadores online disponibles.

### `:has()` — ✅ Baseline

Interacciones de grupo sin JS — el patrón "atenuar hermanos":

```css
.grid:has(.card:hover) .card:not(:hover) { opacity: 0.55; scale: 0.98; transition: 250ms ease; }
```

### `interpolate-size` — ⚠️ solo Chromium

Animar `height: auto` por fin — progressive enhancement puro (sin soporte, el cambio es instantáneo):

```css
:root { interpolate-size: allow-keywords; }
.panel { height: 0; overflow: clip; transition: height 300ms ease-out; }
.panel.open { height: auto; }
```

**Tendencia del ecosistema** (confirmada por HeroUI v3 eliminando framer-motion): **CSS para estados, JS solo para orquestación y gestos.**

## 39. Next.js App Router: transiciones de ruta y Server Components

### El problema

App Router desmonta la página saliente de inmediato → los **exit animations con AnimatePresence son poco fiables** en rutas. Opciones de más simple a más nueva:

**a) `template.tsx`** — se re-monta en cada navegación (a diferencia de layout.tsx) → animación de **entrada** en cada ruta:

```tsx
// app/template.tsx
"use client"
import { motion } from "motion/react"

export default function Template({ children }: { children: React.ReactNode }) {
  return (
    <motion.div initial={{ opacity: 0, y: 12 }} animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3, ease: "easeOut" }}>
      {children}
    </motion.div>
  )
}
```

**b) React `<ViewTransition>` — el camino oficial (Next.js 16.3+, sin flag):** `import { ViewTransition } from 'react'`; las navegaciones de ruta son transitions automáticamente. Shared elements entre rutas:

```tsx
<ViewTransition name={`photo-${photo.id}`}>
  <Image src={photo.src} alt={photo.title} />
</ViewTransition>
```

(En versiones previas requería `experimental.viewTransition`; el paquete `next-view-transitions` fue el puente mientras tanto. Sigue siendo API experimental de React, pero es la dirección oficial.)

### Server Components + Motion

No conviertas árboles enteros en client components. **Patrón wrapper con children** (los children siguen siendo RSC):

```tsx
// components/fade-in.tsx
"use client"
import { motion } from "motion/react"

export function FadeIn({ children, delay = 0 }: { children: React.ReactNode; delay?: number }) {
  return (
    <motion.div initial={{ opacity: 0, y: 20 }} whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, margin: "-80px" }} transition={{ duration: 0.5, delay }}>
      {children}
    </motion.div>
  )
}

// app/page.tsx — Server Component: hace fetch, Motion no infla su bundle
<FadeIn><PricingSection data={await getPricing()} /></FadeIn>
```

### El flash de hydration

Con SSR, el HTML llega visible; al hidratar, Motion aplica `initial={{ opacity: 0 }}` → **flash visible → invisible → animar**. Soluciones:

1. Servir el elemento ya oculto: `<motion.div style={{ opacity: 0 }} animate={{ opacity: 1 }} />` (o `className="opacity-0"`).
2. **Los reveals del hero, en CSS puro** (`@starting-style` / keyframes corren antes de hidratar, cero flash) — reserva Motion para interacción.
3. Scroll-reveals: renderiza visible por defecto y oculta solo cuando el JS carga (mejor SEO y no-JS).

## 40. Responsive y móvil: touch, hover y safe areas

- **En touch no existe hover** (y el `:hover` se "pega" tras el tap). Guarda: `@media (hover: hover) and (pointer: fine)`. **Tailwind v4 ya guarda `hover:` automáticamente.** Consecuencia: el contenido esencial jamás puede vivir solo en el hover — visible por defecto en touch o accesible por tap.
- **El reemplazo del hover en touch es el active/press state**: feedback inmediato al tap (`active:scale-95`, ~75–100ms).
- Detalles que gritan calidad en móvil:

```css
* { -webkit-tap-highlight-color: transparent; }   /* mata el flash gris/azul del tap */
.interactive { touch-action: manipulation; }       /* elimina el delay de double-tap zoom */
.control { user-select: none; }                    /* sin selección accidental durante gestos */
input, textarea, select { font-size: 16px; }       /* evita el zoom automático de iOS */
```

- **Safe areas** (notch/home indicator) — crítico para bottom sheets y docks fijos:

```css
.drawer { padding-bottom: calc(1rem + env(safe-area-inset-bottom)); }
/* requiere viewport-fit=cover en el meta viewport */
```

- Gama baja: usa `navigator.hardwareConcurrency` / `deviceMemory` como heurística para degradar partículas, blurs y parallax.
- Container queries (nativas en Tailwind v4: `@container` + `@sm:`): decide la animación según el **contenedor**, no el viewport — una card en un sidebar angosto puede desactivar su hover-lift aunque el viewport sea desktop.

## 41. Accesibilidad: prefers-reduced-motion y WCAG

**Obligatorio en cada animación.** La estrategia premium es **REEMPLAZAR, no eliminar** — el motion también comunica; sustituye movimiento por fades y cambios de color:

- **Eliminar bajo reduced-motion**: parallax, slide-ins grandes, zooms de objetos grandes, carousels autoplay, scroll-driven effects — los triggers vestibulares (vértigo, náusea, migraña).
- **Conservar**: opacity fades <200ms, cambios de color, focus rings, indicadores de progreso. Un modal que hace fade en vez de subir sigue comunicando el cambio de estado.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

En Motion: `<MotionConfig reducedMotion="user">` (desactiva transforms/layout, conserva opacity/color — exactamente la estrategia correcta) + `useReducedMotion()` para lo granular.

**WCAG 2.2 relevante:**

- **2.2.2 Pause, Stop, Hide (A)**: todo contenido en movimiento que dura >5s junto a otro contenido debe poder pausarse (marquees, carousels, videos de fondo).
- **2.3.1 Three Flashes (A)**: nada parpadea más de 3 veces por segundo.
- **2.3.3 Animation from Interactions (AAA)**: el motion disparado por interacción debe poder desactivarse salvo que sea esencial.

Extra: el motion **nunca es el único canal** de información (Apple HIG); el cambio de tema no dispara transiciones globales.

## 42. Design tokens de motion: el sistema

El rasgo #1 del feel premium (§5.1) hecho código. Una sola fuente de verdad para CSS y JS:

```css
/* app.css — con Tailwind v4, @theme genera las utilities automáticamente */
@theme {
  --duration-instant: 100ms;   /* feedback inmediato: active, toggles */
  --duration-fast: 150ms;      /* hovers, tooltips, exits */
  --duration-base: 250ms;      /* dropdowns, popovers, la mayoría de la UI */
  --duration-slow: 400ms;      /* modals, drawers — el techo para UI funcional */
  --duration-reveal: 600ms;    /* scroll reveals, hero (una vez por sesión) */

  --ease-out: cubic-bezier(0.23, 1, 0.32, 1);       /* entradas y casi todo */
  --ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);   /* movimiento en pantalla */
  --ease-ios: cubic-bezier(0.32, 0.72, 0, 1);       /* sheets y drawers */
}
```

```ts
// lib/motion-tokens.ts — el espejo para Motion
export const duration = { instant: 0.1, fast: 0.15, base: 0.25, slow: 0.4 } as const
export const easing = { out: [0.23, 1, 0.32, 1], inOut: [0.77, 0, 0.175, 1], ios: [0.32, 0.72, 0, 1] } as const
export const springs = {
  snappy: { type: "spring", stiffness: 400, damping: 30 },
  smooth: { type: "spring", visualDuration: 0.35, bounce: 0.15 },
  bouncy: { type: "spring", visualDuration: 0.5, bounce: 0.3 },
} as const
```

Y el default global: `<MotionConfig transition={springs.smooth}>` — todo lo que no especifique transición hereda el tono del sitio. Nota la **asimetría deliberada** que codifican los mejores sistemas: salir rápido (150ms), entrar suave (210–250ms).

## 43. Panorama de librerías: cuál usar cuándo

| Librería | Bundle (gzip) | Fuerte en | Cuándo |
|---|---|---|---|
| **Motion** (ex Framer Motion) | 34kb / **4.6kb** con LazyMotion | Enter/exit, layout/FLIP, gestos, springs, WAAPI | **El default para UI en React** |
| **GSAP** + ScrollTrigger | ~23–33kb | Timelines, scroll storytelling, SVG (Morph/Draw), SplitText | Sitios tipo Awwwards, secuencias cinematográficas. **100% GRATIS desde abril 2025** (todos los plugins, uso comercial — adquirida por Webflow) |
| **react-spring** | ~12–18kb | Física pura | Hoy Motion cubre casi todo su nicho |
| **@formkit/auto-animate** | ~3kb | Listas add/remove/reorder con CERO config | Dashboards, forms dinámicos — "motion gratis" |
| **Lottie** | runtime pesado | Animaciones de diseñador (After Effects), lineales | Onboarding, empty states, branding |
| **Rive** | binarios 10–15x más chicos que Lottie, ~60fps móvil | Animación **interactiva** con state machines | Personajes, controles animados |

**Stack típico de sitio premium 2025–2026:** Tailwind v4 + shadcn/ui (tw-animate-css para estados) + **Motion** para orquestación/gestos/layout + CSS scroll-driven (o GSAP ScrollTrigger para storytelling pesado) + View Transitions para navegación + Lottie/Rive solo para assets de marca.

## 44. Colecciones de componentes premium

Todas con filosofía shadcn (copy-paste, el código es tuyo). Leer su código fuente es la forma más rápida de aprender los patrones:

- **Magic UI** (magicui.design) — 150+ componentes MIT, el complemento marketing de shadcn: Marquee, Bento Grid, Animated Beam, **Border Beam** (luz que recorre el borde), Number Ticker, Text Reveal, Dock estilo macOS, Globe.
- **Aceternity UI** (ui.aceternity.com) — lo más espectacular para heros: 3D Card, Spotlight, Lamp Effect, Background Beams, Hero Parallax, Sparkles.
- **Motion Primitives** (motion-primitives.com) — primitivas sobrias y componibles; ideal para product UI.
- **Animate UI** — componentes Radix ya envueltos en Motion (el patrón del §37 hecho).
- **Animata**, **React Bits**, **21st.dev** (marketplace que agrega todo).
- **HeroUI** (ex NextUI) — librería completa; su v3 **eliminó framer-motion en favor de CSS nativo** (señal de la tendencia: CSS para estados, JS para gestos).

Cómo están hechos los efectos estrella (para replicarlos): un **Border Beam** es un pseudo-elemento con `conic-gradient` + `offset-path` + animation; un **Number Ticker** es `useSpring` + `useTransform`; un **Marquee** es `animation: marquee Xs linear infinite` con el contenido duplicado y `mask-image` desvaneciendo los bordes.

## 45. Reglas de oro: el checklist completo

**Propósito**
1. Cada animación responde "¿qué información aporta?" — si nada, se elimina.
2. El motion apoya el contenido, no distrae de él. El texto es para leerse.
3. Lo premium es contención: 2–3 momentos de alto impacto, cero fricción en lo repetido.
4. Nada se mueve sin causa: origen en el trigger, dirección con significado, un dato que comunicar.

**Números**
5. UI < 300ms (ideal ≤ 200ms). Feedback inmediato ~100ms. Scroll reveals 400–600ms.
6. Exits más rápidos que enters (~150 vs ~250ms).
7. Acciones frecuentes: animación mínima o ninguna. Teclado: NUNCA animar. Command menus: sin motion.
8. Scale nunca desde 0 — desde 0.95. Press: scale 0.97.
9. Scroll reveals: `once: true`, 10–30px de desplazamiento, 20–30% de visibilidad como trigger.
10. Stagger: 50–100ms entre elementos; por carácter 10–30ms; máximo 5–6 bloques.

**Física**
11. Ease-out para entradas y para casi todo lo iniciado por el usuario; ease-in-out para movimiento en pantalla; linear solo para loops continuos; ease-in solo en salidas muy cortas.
12. No suavices el extremo del movimiento que no se ve; no muevas más distancia de la visible; elimina el fade si el elemento sale por movimiento.
13. Springs para gestos e interrupciones (bounce 0.1–0.3; 0 para UI seria); duración+easing para lo simple.
14. Todo lo arrastrable sigue al dedo 1:1, conserva momentum, y es interrumpible en cualquier frame.

**Técnica**
15. Animar SOLO `transform` y `opacity` en interacciones. Sombras con pseudo-elemento. Nunca width/height/margin/top.
16. `will-change` solo ante un problema real y de forma acotada.
17. `transform-origin` en el trigger (dropdowns, popovers, tooltips).
18. Keys estables en AnimatePresence; el condicional va dentro; `mode="wait"` para rutas.
19. `tabular-nums` en cifras animadas; el font-weight nunca cambia con el estado.
20. Loops pausados fuera del viewport (salvo que deban estar sincronizados entre sí).

**Sistema**
21. Tokens únicos: 3–4 duraciones + 2–3 easings en TODO el sitio (CSS variables + espejo en JS + MotionConfig).
22. `prefers-reduced-motion`: reemplazar movimiento por fades, nunca eliminar el feedback.
23. Hover guardado con `(hover: hover)`; active states para touch; contenido esencial nunca solo en hover.
24. Focus ring visible siempre; el theme switch no dispara transiciones.
25. Elementos persistentes (nav) no participan en transiciones de página.

## 46. Cheat-sheet final

```css
/* ===== El kit mínimo para UI premium por defecto ===== */
:root {
  --ease-out: cubic-bezier(0.23, 1, 0.32, 1);      /* entradas, casi todo */
  --ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);  /* movimiento en pantalla */
  --ease-ios: cubic-bezier(0.32, 0.72, 0, 1);      /* sheets/drawers */
  --dur-fast: 150ms;   /* hover, tooltips, exits */
  --dur-base: 200ms;   /* dropdowns, popovers */
  --dur-slow: 300ms;   /* modals — techo para UI */
}
* { -webkit-tap-highlight-color: transparent; }
@media (prefers-reduced-motion: reduce) { /* reemplazar motion por fades */ }
```

```tsx
// ===== Motion: los 6 patrones que cubren el 90% =====
import { motion, AnimatePresence, stagger } from "motion/react"

// 1. Entrada
<motion.div initial={{ opacity: 0, y: 16 }} animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.25, ease: [0.23, 1, 0.32, 1] }} />

// 2. Interacción
<motion.button whileHover={{ scale: 1.02 }} whileTap={{ scale: 0.97 }} />

// 3. Scroll reveal (una vez, sutil)
<motion.section initial={{ opacity: 0, y: 24 }} whileInView={{ opacity: 1, y: 0 }}
  viewport={{ once: true, amount: 0.3 }} transition={{ duration: 0.5 }} />

// 4. Stagger de lista
const parent = { visible: { transition: { delayChildren: stagger(0.06) } } }

// 5. Salida (modal/toast) — exit más corto que enter
<AnimatePresence>{open && <motion.div key="x" exit={{ opacity: 0, scale: 0.97, transition: { duration: 0.15 } }} />}</AnimatePresence>

// 6. Indicador compartido (tabs)
{isActive && <motion.div layoutId="indicator" transition={{ type: "spring", stiffness: 350, damping: 30 }} />}
```

**La prueba final de todo motion:** si la animación no sobrevive a la pregunta *"¿qué me está diciendo?"*, un sitio premium la borra.

---

*Fuentes principales: motion.dev (docs y changelog oficial, verificado contra motion@13.1.0), Emil Kowalski (emilkowal.ski, animations.dev, autor de Sonner y Vaul), Rauno Freiberg (interfaces.rauno.me, Vercel), Nielsen Norman Group, Material Design 3, Apple HIG, Issara Willenskomer (UX in Motion Manifesto), Josh Comeau, web.dev, MDN, Chrome Developers, WebKit, tailwindcss.com, ui.shadcn.com, nextjs.org.*
