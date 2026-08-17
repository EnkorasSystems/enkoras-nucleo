# Guía Maestra: Diseño de Interfaz Premium — color, forma, tipografía y componentes

> **Qué es este documento:** la **PARTE B** del curso de UI/UX premium. La PARTE A (`CURSO-FRAMER-ANIMATION.md`) cubre el **movimiento**: duraciones, curvas, springs, Motion/Framer Motion y micro-interacciones animadas. Esta parte cubre todo lo **estático**: cómo se define una paleta, cuánto padding lleva un botón, qué radio va dentro de qué radio, cómo se construye una sombra que no se ve barata, la anatomía y los estados de cada control, y cómo se arma el esqueleto de una app.
>
> **Para quién es:** para una persona o una IA que construye apps web con React 19 + Next.js App Router + Tailwind CSS v4 + shadcn/ui y quiere el nivel de pulido de Linear, Vercel, Stripe, Raycast y Notion.
>
> **Cómo usarlo:** no se lee de corrido. Se consulta la sección del componente que estás construyendo, y se cierra con el checklist de la §12 de cada parte. Todo número que aparece aquí sale de una fuente primaria citada al final de cada parte.
>
> **Estado:** verificado a agosto de 2026.

---

## Índice general

**PARTE 1 — Sistema de color** · paletas, escalas de 12 pasos, OKLCH, tokens, superficies, estados, bordes, texto, dark mode, contraste, el "look de IA genérica"

**PARTE 2 — Espaciado, forma y elevación** · escala de 4, proximidad, densidad, padding por componente, alineación óptica, radios y radios anidados, bordes, sombras en capas, elevación, focus ring, glass y grain

**PARTE 3 — Tipografía y jerarquía** · elegir fuente, escala real de UI, line-height, tracking, pesos, los 4 canales de jerarquía, números, acabado, microcopy

**PARTE 4 — Botones, acciones y formularios** · jerarquía de acciones, tamaños, los 7 estados, disabled, loading, anatomía de inputs, atributos, validación, formularios

**PARTE 5 — Controles de selección y capas flotantes** · checkbox, radio, switch, select, combobox, menús, popovers, modales, drawers, tooltips, toasts, tabs, command menu

**PARTE 6 — Layout, contenedores y navegación** · shell, anchos, grid, cards, secciones, sidebar, tablas, los 4 estados, densidad, responsive, z-index

**PARTE 7 — Recetario de efectos** · skeletons, border beam, fondos, texto, listas, scroll, celebración y de dónde copiarlos

---
---

## Sistema de color

Todo lo estático del color: escalas, espacios, tokens, superficies, estados, contraste.

### 1. Construir una paleta desde cero

#### 1.1 El error de origen: pocas variantes

Refactoring UI lo diagnostica así: *"you'll need more greys than you think"* — tres o cuatro tonos parecen suficientes hasta que necesitas algo entre el #2 y el #3.

| Familia | Cuántos tonos | Para qué |
|---|---|---|
| Grises | **8–10** | texto, fondos, paneles, form controls, bordes — el 80% de la UI |
| Primario / marca | **5–10** | acciones primarias, navegación activa, foco |
| Acentos semánticos | **5–10 cada uno** | mínimo rojo, ámbar, verde; opcional azul info |
| Total realista | **35–50+** tonos | una UI compleja llega a ~10 hues × 5–10 tonos |

**Regla dura:** nunca generes tonos con `opacity` sobre el color base. Define valores explícitos. Un color con alpha cambia al moverlo de fondo; un token opaco no.

#### 1.2 Elegir el hue de marca

1. Un solo hue de marca. Dos hues de marca es un sistema con dos jefes.
2. Evita el rango 255–290° (índigo/violeta) salvo razón de marca. Ver §12.
3. Comprueba el **croma máximo alcanzable en sRGB a la luminancia que vas a usar**. No todos los hues llegan igual de lejos:

| Hue (OKLCH) | Croma máx. en sRGB a L=0.65 |
|---|---|
| 25° (rojo) | 0.235 |
| 60° (naranja) | 0.152 |
| 145° (verde) | 0.204 |
| 200° (cian) | 0.110 |
| 250° (azul) | 0.184 |
| 300° (magenta) | 0.221 |

Si eliges cian a L=0.65 tienes la mitad de croma disponible que con rojo: el resultado es un "azul apagado" que no es culpa tuya sino del gamut. O mueves L, o vas a P3, o cambias de hue.

#### 1.3 El modelo de 12 pasos de Radix (el mejor modelo mental)

Radix asigna **una función a cada paso**, no una luminancia arbitraria. Eso convierte una rampa en un sistema.

| Paso | Uso oficial | `slate` claro | `slate` oscuro | `blue` claro |
|---|---|---|---|---|
| **1** | App background | `#fcfcfd` | `#111113` | `#fbfdff` |
| **2** | Subtle background | `#f9f9fb` | `#18191b` | `#f4faff` |
| **3** | UI element background | `#f0f0f3` | `#212225` | `#e6f4fe` |
| **4** | Hovered UI element background | `#e8e8ec` | `#272a2d` | `#d5efff` |
| **5** | Active / Selected UI element background | `#e0e1e6` | `#2e3135` | `#c2e5ff` |
| **6** | Subtle borders and separators | `#d9d9e0` | `#363a3f` | `#acd8fc` |
| **7** | UI element border and focus rings | `#cdced6` | `#43484e` | `#8ec8f6` |
| **8** | Hovered UI element border | `#b9bbc6` | `#5a6169` | `#5eb1ef` |
| **9** | **Solid background** (el paso puro, mayor croma) | `#8b8d98` | `#696e77` | `#0090ff` |
| **10** | Hovered solid background | `#80838d` | `#777b84` | `#0588f0` |
| **11** | Low-contrast text | `#60646c` | `#b0b4ba` | `#0d74ce` |
| **12** | High-contrast text | `#1c2024` | `#edeef0` | `#113264` |

Tres cosas que hay que memorizar:

- **1–2 fondos · 3–5 componentes · 6–8 bordes · 9–10 sólidos · 11–12 texto.**
- El **paso 9 es el único "puro"**: el de mayor croma. Es tu color de marca real; todo lo demás es infraestructura.
- Los pasos **11 y 12 están garantizados a Lc 60 y Lc 90 de APCA** sobre el paso 2. Medido en WCAG 2: `slate11` sobre `slate2` = **5.65:1**, `slate12` = **15.58:1**.

El truco de diseño: los pasos claros y oscuros **mapean 1:1**. `slate3` es "fondo de componente" en ambos modos, aunque uno sea `#f0f0f3` y otro `#212225`. Cambias de tema reasignando variables, no reescribiendo componentes.

#### 1.4 La variante de 10 pasos de Vercel (Geist)

| Paso | Significado literal |
|---|---|
| `100` | Default background |
| `200` | Hover background |
| `300` | Active background |
| `400` | Default border |
| `500` | Hover border |
| `600` | Active border |
| `700` | High contrast background |
| `800` | Hover high contrast background |
| `900` | Secondary text and icons |
| `1000` | Primary text and icons |

Geist separa además `gray` (opaco) de `gray-alpha` (translúcido): alpha para bordes, divisores, overlays y hovers; opaco para texto y rellenos sólidos (§7).

#### 1.5 Tailwind v4: 50 → 950

Tailwind usa 11 pasos **sin semántica asignada**. Es una rampa de luminancia, no un sistema de roles: sirve como capa de primitivos; la semántica la pones tú (§4).

---

### 2. Espacios de color: por qué OKLCH en 2026

#### 2.1 El fallo de HSL, con números

Cinco colores con **exactamente el mismo `L: 50%` y `S: 90%`**:

| CSS | Hex | L real (OKLCH) | Contraste vs `#fff` | vs `#000` |
|---|---|---|---|---|
| `hsl(220 90% 50%)` | `#0d59f2` | **52.9%** | 5.63:1 | 3.73:1 |
| `hsl(280 90% 50%)` | `#a60df2` | **56.8%** | 5.30:1 | 3.96:1 |
| `hsl(0 90% 50%)` | `#f20d0d` | **60.6%** | 4.34:1 | 4.84:1 |
| `hsl(140 90% 50%)` | `#0df259` | **83.8%** | 1.51:1 | 13.86:1 |
| `hsl(60 90% 50%)` | `#f2f20d` | **93.0%** | **1.20:1** | 17.48:1 |

El amarillo y el azul comparten "lightness 50%" y difieren en **40 puntos de luminancia perceptual** y en un factor **4.7×** de contraste sobre blanco. Es exactamente el problema que Stripe documentó: al oscurecer todos los hues hasta pasar 4.5:1 *"perdimos mucho del brillo; los colores pasan las guías, pero son oscuros y turbios"*.

La misma prueba en OKLCH, `L=0.65 C=0.15` fijos:

| CSS | Hex | Contraste vs `#fff` |
|---|---|---|
| `oklch(0.65 0.15 25)` | `#dc655f` | 3.47:1 |
| `oklch(0.65 0.15 60)` | `#d0750a` | 3.38:1 |
| `oklch(0.65 0.15 145)` | `#4aa651` | 3.05:1 |
| `oklch(0.65 0.15 250)` | `#3a93e6` | 3.23:1 |
| `oklch(0.65 0.15 300)` | `#9e77dc` | 3.43:1 |

Rango 3.05–3.47 en vez de 1.20–5.63. Linear migró su generación de temas de HSL a LCH por esto: su sistema pasó de 98 variables por tema a **tres** (base, accent, contrast).

#### 2.2 Sintaxis y gamut

```css
/* oklch(L C H / alpha) */
--brand:   oklch(64.9% 0.193 251.8);   /* = #0090ff (Radix blue9) */
--brand-a: oklch(64.9% 0.193 251.8 / 0.12);
```

- **L**: `0`–`1` o `0%`–`100%`, perceptual. **C**: croma absoluto, `0` → ~`0.37`; el máximo depende de L y H. **H**: ángulo `0`–`360`.

```css
.accent { background: oklch(0.7 0.18 145); }        /* seguro en sRGB */
@media (color-gamut: p3) {
  .accent { background: oklch(0.7 0.26 145); }      /* más croma donde se puede */
}
/* fallback para navegadores viejos */
.btn { background: #0090ff; }
@supports (color: oklch(0 0 0)) { .btn { background: oklch(64.9% 0.193 251.8); } }
```

#### 2.3 `color-mix()` para derivar estados

**Interpola siempre en `oklab`/`oklch`, nunca en `srgb`** (mezclar en sRGB pasa por zonas grises muertas).

| Mezcla sobre `#0090ff` | Resultado | OKLCH | vs `#fff` |
|---|---|---|---|
| `mix 92% + black` | `#0080e4` | `oklch(59.6% 0.178 251.8)` | 4.01:1 |
| `mix 88% + black` | `#0079d7` | `oklch(57.2% 0.169 251.6)` | 4.47:1 |
| `mix 84% + black` | `#0071ca` | `oklch(54.5% 0.162 251.7)` | 4.98:1 |
| `mix 92% + white` | `#309aff` | `oklch(67.7% 0.176 251.5)` | 2.92:1 |

Regla práctica: **mezclar 8% de negro ≈ −5 puntos de L OKLCH**. Ese es tu escalón de hover. Con *relative color syntax* es aún más directo:

```css
.btn:hover { background: oklch(from var(--brand) calc(l - 0.05) c h); }
```

#### 2.4 `light-dark()` y `color-scheme`

`color-scheme` le dice al navegador qué pintar en scrollbars, form controls nativos, `<input type=date>`, autofill y spellcheck. **Sin él, tu dark mode tiene scrollbars blancos.** `light-dark()` lo **requiere**:

```css
:root {
  color-scheme: light dark;
  --surface: light-dark(#ffffff, #18191b);
  --text:    light-dark(#1c2024, #edeef0);
}
/* con toggle manual, además: */
:root.light { color-scheme: only light; }
:root.dark  { color-scheme: only dark;  }
```

Y en `<head>`, para que no haya flash antes del CSS: `<meta name="color-scheme" content="light dark">`.

---

### 3. Grises

#### 3.1 Por qué `#808080` se ve barato

Un gris puro tiene croma 0. No existe en el mundo físico iluminado: toda superficie real refleja el color de su luz. Un gris puro junto a un azul de marca lee como "sin diseñar".

#### 3.2 Cuánto croma teñir

| Familia (Tailwind v4) | `500` OKLCH | Croma | Hue | Carácter |
|---|---|---|---|---|
| `neutral` | `oklch(55.55% 0 0)` | **0.000** | — | gris puro, sin temperatura |
| `zinc` | `oklch(55.17% 0.0138 285.94)` | 0.014 | 286° | frío mínimo |
| `stone` | `oklch(55.34% 0.0116 58.07)` | 0.012 | 58° | cálido mínimo |
| `gray` | `oklch(55.1% 0.0234 264.36)` | 0.023 | 264° | frío medio |
| `slate` | `oklch(55.44% 0.0407 257.42)` | **0.041** | 257° | frío marcado |

Rango útil: **C entre 0.005 y 0.045**. Por encima de ~0.05 deja de ser gris y compite con el acento. El tinte se nota más en los medios que en los extremos.

#### 3.3 Cálido vs frío

| | Hue OKLCH | Comunica | Se usa en |
|---|---|---|---|
| Frío | 240–290° | preciso, técnico, denso, "software" | dashboards, dev tools, fintech |
| Neutro | — | ninguno; el color de marca manda | producto con marca fuerte de color |
| Cálido | 40–90° | editorial, humano, calmado, "papel" | contenido, docs, marketplaces, salud |

**Regla dura:** elige UNA familia neutra y no la mezcles. Un `slate-200` junto a un `stone-700` produce una deriva de hue visible en bordes adyacentes.

#### 3.4 Emparejar gris con acento (pares de Radix)

| Gris | Acentos que acompaña |
|---|---|
| `mauve` | tomato, red, ruby, crimson, pink, plum, purple, violet |
| `slate` | iris, indigo, blue, sky, cyan |
| `sage` | mint, teal, jade, green |
| `olive` | grass, lime |
| `sand` | yellow, amber, orange, brown |
| `gray` | neutral: funciona con todo |

Advertencia literal de Radix: *"cuidado con los grises saturados para el fondo de la app, especialmente en dark mode"*. **El fondo de página usa el gris con menos croma de tu escala**; bordes y texto secundario pueden llevar más.

---

### 4. Arquitectura de tokens

#### 4.1 Tres capas, siempre

```
Capa 1 — PRIMITIVOS   --blue-9, --slate-2               (qué color es)
Capa 2 — SEMÁNTICOS   --color-surface, --color-border   (qué significa)
Capa 3 — COMPONENTE   --button-bg, --card-border        (dónde vive)   [opcional]
```

**Por qué nunca un primitivo en un componente:** si escribes `background: var(--blue-9)` en el botón, para hacer dark mode tienes que editar el botón. Si escribes `var(--color-accent-solid)`, cambias una línea en el tema. La capa 3 solo se justifica cuando un componente rompe el sistema por una razón real; si tienes más de ~10 tokens de componente, tu capa semántica está incompleta.

#### 4.2 La convención `background`/`foreground` de shadcn

*"Pares semánticos de fondo y primer plano."* El sufijo `-background` se omite: `primary` + `primary-foreground`.

| Token | Claro | Oscuro |
|---|---|---|
| `background` | `oklch(1 0 0)` | `oklch(0.145 0 0)` |
| `foreground` | `oklch(0.145 0 0)` | `oklch(0.985 0 0)` |
| `card` | `oklch(1 0 0)` | `oklch(0.205 0 0)` |
| `muted` | `oklch(0.97 0 0)` | `oklch(0.269 0 0)` |
| `muted-foreground` | `oklch(0.556 0 0)` | `oklch(0.708 0 0)` |
| `border` | `oklch(0.922 0 0)` | `oklch(1 0 0 / 10%)` |
| `input` | `oklch(0.922 0 0)` | `oklch(1 0 0 / 15%)` |
| `ring` | `oklch(0.708 0 0)` | `oklch(0.556 0 0)` |
| `destructive` | `oklch(0.577 0.245 27.325)` | `oklch(0.704 0.191 22.216)` |

Dos detalles: **el default de shadcn es croma 0 en todo** (es una base, no un diseño terminado — téñelo, §3), y **`border` en oscuro es alpha y en claro es opaco** (§7).

#### 4.3 Ejemplo completo en Tailwind v4

```css
@import "tailwindcss";
@custom-variant dark (&:is(.dark *));

/* ---------- CAPA 1: PRIMITIVOS (no se usan en componentes) ---------- */
:root {
  /* neutro teñido con el hue de marca (257°), croma 0.004 → 0.016 */
  --slate-1:  oklch(99.1% 0.001 286);   --slate-2:  oklch(98.3% 0.003 286);
  --slate-3:  oklch(95.6% 0.004 286);   --slate-4:  oklch(93.2% 0.005 286);
  --slate-5:  oklch(91.0% 0.007 277);   --slate-6:  oklch(88.7% 0.009 286);
  --slate-7:  oklch(85.3% 0.011 280);   --slate-8:  oklch(79.4% 0.016 278);
  --slate-9:  oklch(64.5% 0.017 278);   --slate-10: oklch(61.1% 0.015 273);
  --slate-11: oklch(50.2% 0.014 264);   --slate-12: oklch(24.1% 0.010 248);

  --brand-3:  oklch(96.0% 0.020 239);   --brand-4:  oklch(93.8% 0.035 235);
  --brand-5:  oklch(90.5% 0.051 240);   --brand-8:  oklch(73.4% 0.121 243);
  --brand-9:  oklch(64.9% 0.193 252);   --brand-10: oklch(62.2% 0.183 252);
  --brand-11: oklch(55.6% 0.162 252);   --brand-12: oklch(32.4% 0.096 259);
}

.dark {
  --slate-1:  oklch(17.8% 0.004 286);   --slate-2:  oklch(21.3% 0.004 264);
  --slate-3:  oklch(25.2% 0.006 271);   --slate-4:  oklch(28.3% 0.007 248);
  --slate-5:  oklch(31.2% 0.008 256);   --slate-6:  oklch(34.7% 0.010 254);
  --slate-7:  oklch(39.9% 0.012 253);   --slate-8:  oklch(48.9% 0.015 252);
  --slate-9:  oklch(53.7% 0.015 262);   --slate-10: oklch(58.3% 0.015 267);
  --slate-11: oklch(76.9% 0.010 258);   --slate-12: oklch(94.9% 0.003 264);

  --brand-3:  oklch(27.5% 0.066 254);   --brand-4:  oklch(32.0% 0.097 252);
  --brand-5:  oklch(36.7% 0.106 251);   --brand-8:  oklch(54.1% 0.140 253);
  --brand-9:  oklch(64.9% 0.193 252);   --brand-10: oklch(68.8% 0.169 251);
  --brand-11: oklch(76.4% 0.126 250);   --brand-12: oklch(90.7% 0.051 238);
}

/* ---------- CAPA 2: SEMÁNTICOS (lo único que tocan los componentes) ---------- */
:root {
  --color-canvas:            var(--slate-1);   /* fondo de página */
  --color-surface:           #ffffff;          /* card, panel */
  --color-surface-subtle:    var(--slate-2);   /* thead, code block, sidebar */
  --color-surface-raised:    #ffffff;          /* popover, dropdown */

  --color-element:           var(--slate-3);
  --color-element-hover:     var(--slate-4);
  --color-element-active:    var(--slate-5);

  --color-border-subtle:     oklch(0% 0 0 / 0.06);
  --color-border:            oklch(0% 0 0 / 0.10);
  --color-border-strong:     oklch(0% 0 0 / 0.16);
  --color-border-control:    oklch(0% 0 0 / 0.42);  /* input/checkbox: 3:1 real */
  --color-focus-ring:        var(--brand-9);

  --color-text:              var(--slate-12);  /* 15.6:1 sobre surface-subtle */
  --color-text-secondary:    var(--slate-11);  /*  5.7:1 */
  --color-text-tertiary:     var(--slate-10);  /*  3.6:1 — solo ≥18px o no-texto */
  --color-text-disabled:     var(--slate-8);

  --color-accent-solid:      var(--brand-9);
  --color-accent-solid-hover:var(--brand-10);
  --color-accent-text:       var(--brand-11);
  --color-accent-bg:         var(--brand-3);
  --color-accent-bg-hover:   var(--brand-4);
  --color-on-accent:         #ffffff;

  --color-overlay:           oklch(0% 0 0 / 0.45);
}

.dark {
  --color-canvas:        var(--slate-1);
  --color-surface:       var(--slate-2);
  --color-surface-raised:var(--slate-3);
  --color-border-subtle: oklch(100% 0 0 / 0.06);
  --color-border:        oklch(100% 0 0 / 0.10);
  --color-border-strong: oklch(100% 0 0 / 0.18);
  --color-overlay:       oklch(0% 0 0 / 0.60);
}

/* ---------- Puente a utilidades de Tailwind ---------- */
@theme inline {
  --color-canvas:         var(--color-canvas);
  --color-surface:        var(--color-surface);
  --color-border:         var(--color-border);
  --color-text:           var(--color-text);
  --color-text-secondary: var(--color-text-secondary);
  --color-accent:         var(--color-accent-solid);
  --color-on-accent:      var(--color-on-accent);
}
```

`@theme inline` (no `@theme`) es lo correcto cuando el valor es una `var()` que cambia por tema: emite la referencia en vez de congelar el valor.

#### 4.4 Nomenclatura

**Patrón:** `--color-{rol}-{variante}-{estado}`. Roles: `canvas`, `surface`, `element`, `border`, `text`, `accent`, `on-*`. Variantes: `subtle`, `strong`, `raised`, `secondary`, `tertiary`. Estados: `hover`, `active`, `selected`, `disabled`.
**Prohibido en la capa semántica:** nombres de color (`--color-blue-button`), de ubicación (`--sidebar-gray-thing`) o números sin significado (`--color-2`).

---

### 5. Superficies y elevación por color

| Nivel | Claro | Oscuro | Separación en claro | Separación en oscuro |
|---|---|---|---|---|
| Canvas (página) | `#fcfcfd` | `#111113` (L 17.8%) | — | — |
| Surface (card/panel) | `#ffffff` | `#18191b` (L 21.3%) | borde + sombra | **color** (+3.5 L) |
| Surface subtle | `#f9f9fb` | `#18191b` | color (−0.8 L) | igual que surface |
| Raised (popover) | `#ffffff` | `#212225` (L 25.2%) | sombra fuerte | **color** (+3.9 L) |
| Overlay (modal) | `#ffffff` | `#272a2d` (L 28.3%) | sombra + scrim | **color** (+3.1 L) |
| Scrim | `oklch(0% 0 0 / 0.45)` | `oklch(0% 0 0 / 0.60)` | | |

**El dato clave:** en dark mode cada nivel sube **≈ 3–4 puntos de L OKLCH**. En light mode la diferencia entre canvas y surface es de **menos de 1 punto de L** — la elevación en claro no la hace el fondo, la hacen el borde y la sombra.

**Por qué en oscuro la elevación es color:** una sombra es *ausencia de luz*; sobre `#111113` no hay luz que quitar. La convención inversa (más elevado = más claro) la formalizó Material 2 con overlays de blanco por elevación (**1dp = 5%, 2dp = 7%, 4dp = 9%, 8dp = 12%, 16dp = 15%, 24dp = 16%**); Material 3 lo reemplazó por cinco roles sólidos `surface-container-lowest/low/default/high/highest`.

#### Card: ¿fondo distinto o solo borde?

| Situación | Qué usar |
|---|---|
| Card sobre canvas, **light** | `background: surface` (#fff) + `border: 1px`. Sin sombra o mínima |
| Card sobre canvas, **dark** | fondo más claro (+3–4 L) + borde alpha 6–10%. Nunca solo borde |
| Card dentro de otra card | **Solo borde**, mismo fondo |
| Sección no interactiva | Solo separador o espacio |
| Elemento flotante | Fondo + borde + sombra, los tres |

**Error que se ve barato:** tres niveles de fondo gris anidados. Máximo **dos** niveles de fondo visibles a la vez; a partir del tercero, borde.

---

### 6. Estados de un elemento interactivo

#### 6.1 Sobre superficie (ghost button, item de lista, fila)

| Estado | Paso Radix | Claro | Oscuro |
|---|---|---|---|
| default | transparente | — | — |
| hover | 3 (o 4) | `#f0f0f3` | `#212225` |
| active / pressed | 5 | `#e0e1e6` | `#2e3135` |
| selected | 5 + texto paso 12 | `#e0e1e6` | `#2e3135` |
| selected con acento | accent 3 + texto accent 11 | `#e6f4fe` / `#0d74ce` | `#0d2847` / `#70b8ff` |
| disabled | sin fondo, texto paso 8 | `#b9bbc6` | `#5a6169` |

```css
.item:hover  { background: color-mix(in oklab, var(--color-text) 5%, transparent); }
.item:active { background: color-mix(in oklab, var(--color-text) 9%, transparent); }
.item[aria-selected="true"] { background: color-mix(in oklab, var(--color-accent) 12%, transparent); }
```

Usar alpha aquí es lo correcto: el mismo hover funciona sobre canvas, sobre card y sobre una fila rayada.

#### 6.2 Sobre botón sólido

| Estado | Regla | `#0090ff` → |
|---|---|---|
| default | paso 9 | `#0090ff` |
| hover | paso 10 = `mix 92% + black` en claro | `#0588f0` |
| active | `mix 86% + black` (≈ −8 L) | `#0074cd` |
| disabled | `opacity: 0.5` **o** paso 3 con texto paso 8; nunca gris genérico |

**Inversión en dark mode:** el hover de un sólido en oscuro debe **aclarar**, no oscurecer. Si copias `mix + black` al dark mode, el botón "se apaga" al pasar el ratón y se lee como deshabilitado.

**Regla dura:** un escalón entre default y hover (≈ 4–6 puntos de L), dos entre default y active. Saltos mayores parecen un bug de estado.

---

### 7. Bordes

#### 7.1 Por qué alpha y no gris opaco

Un borde opaco `#e4e4e7` calculado para `#ffffff` se ve **más oscuro que su fondo** sobre `#f9f9fb`, e invisible sobre `#f0f0f3`. Un borde alpha se recalcula solo.

Negro sobre fondos claros / blanco sobre `#18191b`:

| Alpha | Sobre `#fff` | Contraste | Sobre `#18191b` | Contraste |
|---|---|---|---|---|
| 6% | `#f0f0f0` | 1.14:1 | `#262729` | 1.17:1 |
| **8%** | `#ebebeb` | 1.20:1 | `#2a2b2d` | 1.25:1 |
| **10%** | `#e6e6e6` | 1.25:1 | `#2f3032` | 1.33:1 |
| 12% | `#e0e0e0` | 1.32:1 | `#343536` | 1.42:1 |
| 16% | `#d6d6d6` | 1.45:1 | `#3b3b3d` (15%) | 1.58:1 |

**Rangos operativos:** 6–12% en claro, 8–15% en oscuro. Para bordes que **deben cumplir WCAG 1.4.11** (contorno de input, checkbox, radio, switch) hace falta mucho más: **42–43% de negro sobre blanco** (`#949494`, 3.04:1) o **33% de blanco sobre `#18191b`** (`#646566`, 3.01:1). Son **dos tokens distintos**, no uno.

#### 7.2 Borde vs sombra vs fill

| Herramienta | Cuándo | Coste visual |
|---|---|---|
| **Borde 1px alpha** | separar elementos en el mismo plano | bajo — el default |
| **Sombra** | indicar que algo flota | alto — no lo uses para agrupar |
| **Fill** | agrupar contenido relacionado | medio — máx. 2 niveles |
| **Espaciado** | separar secciones | cero — pruébalo primero |

**Regla dura:** borde **y** sombra **y** fondo distinto a la vez, en algo que no flota, es sobre-diseño.

---

### 8. Texto

| Nivel | Claro | vs `#fff` | Oscuro | vs `#18191b` | Uso |
|---|---|---|---|---|---|
| Primary | `#1c2024` | **16.39:1** | `#edeef0` | **15.15:1** | body, headings |
| Secondary | `#60646c` | **5.94:1** | `#b0b4ba` | **8.45:1** | labels, metadata |
| Tertiary | `#80838d` | **4.19:1** | `#777b84` | 4.15:1 | timestamps, placeholders |
| Disabled | `#b9bbc6` | 1.82:1 | `#5a6169` | 2.81:1 | solo estado (exento de 1.4.11) |

**Por qué no negro puro:** `#000` sobre `#fff` da 21:1 — produce halación (el texto "vibra"), fatiga en sesiones largas y lee como "sin estilo". `#1c2024` da 16.39:1, sobra para AAA (7:1). **El texto más oscuro de tu sistema vive en L 20–26%, no en 0%.**

**Nunca uses el paso 9 como color de texto** sobre fondo claro: `#0090ff` sobre blanco da 3.26:1 y falla AA. Usa el paso 11: `#0d74ce` = 4.77:1.

---

### 9. Acento y semánticos

#### 9.1 60-30-10 traducido a UI

| Proporción | Qué es en una app |
|---|---|
| **60%** | canvas + surfaces (grises 1–3). El fondo |
| **30%** | texto, iconos, bordes, chrome (grises 6–12). La estructura |
| **10%** | acento: CTA primario, estado activo, links, focus, gráficas |

**Si hay más de un botón sólido de acento visible a la vez, uno de los dos no era primario.**

#### 9.2 Los cuatro semánticos

| Rol | Hue | Solid (paso 9) | Text (paso 11, sobre claro) |
|---|---|---|---|
| `danger` | 20–30° | `oklch(0.63 0.21 25)` | `#cc272e` (5.38:1) |
| `warning` | 75–90° | `oklch(0.80 0.16 85)` | `#966800` (4.88:1) |
| `success` | 140–150° | `oklch(0.65 0.16 145)` | `#308639` (4.57:1) |
| `info` | 245–255° | `oklch(0.65 0.19 250)` | `#0073cf` (4.82:1) |

Fijar la **misma L** ya iguala mucho, pero no del todo: (1) el croma disponible difiere por hue — a L=0.65 el rojo llega a C=0.235 y el cian solo a 0.110, así que un `danger` a croma máximo "grita" más; (2) **el ámbar no existe a L baja**: `oklch(0.55 0.14 85)` es `#966800`, un marrón. El "warning amarillo" a nivel de texto no existe sobre fondo claro — invierte el patrón: fondo `amber-3` con texto `amber-11`.

**Regla operativa:** iguala L entre semánticos y baja el croma del rojo un 10–15% respecto al verde.

#### 9.3 Daltonismo

**8% de los hombres** tiene alguna deficiencia; la deuteranomalía (~5–6%) confunde precisamente rojo y verde.

| Señal | Canal 1 | Canal 2 obligatorio |
|---|---|---|
| Error en input | borde rojo | icono + mensaje de texto |
| Estado en tabla | badge de color | texto de la etiqueta |
| Gráfica de líneas | color por serie | patrón de trazo + etiqueta directa |
| Diff | fondo verde/rojo | `+` / `−` |

Prueba rápida: pon la pantalla en escala de grises. Si dejas de entenderla, no está terminada.

---

### 10. Dark mode

1. **No es una inversión.** Es un segundo set de valores para los mismos roles semánticos.
2. **Ni `#000` ni `#fff` puros.** Material fijó `#121212` como fondo estándar (el negro puro causa halación y *smearing* en OLED al scrollear). Rangos: fondo **L 15–20%**, texto primario **L 92–96%**.
3. **Los colores de marca cambian:** luminancia +10 a +25 puntos, croma −20% a −40%. Verificado en Radix `blueDark`: el paso 12 pasa de C=0.096 a C=0.051, casi la mitad.
4. **Las sombras dejan de funcionar.** `0 1px 3px rgb(0 0 0 / 0.1)` sobre `#111113` es invisible:

```css
--shadow-raised: 0 1px 2px oklch(0% 0 0 / 0.06), 0 4px 12px oklch(0% 0 0 / 0.08);
.dark {
  --shadow-raised: 0 1px 2px oklch(0% 0 0 / 0.40), 0 8px 24px oklch(0% 0 0 / 0.32),
                   inset 0 1px 0 oklch(100% 0 0 / 0.06);
}
```

5. **Imágenes:** fotos con `filter: brightness(0.9) contrast(1.05)` o variante propia; logos con blanco necesitan versión aparte; iconos SVG siempre `fill="currentColor"`.
6. **El cambio de tema no debe animarse.** Si hay `transition: background-color` en cualquier parte, al togglear la pantalla "hierve":

```js
function setTheme(next) {
  const style = document.createElement('style')
  style.appendChild(document.createTextNode('*,*::before,*::after{transition:none!important}'))
  document.head.appendChild(style)
  document.documentElement.classList.toggle('dark', next === 'dark')
  void window.getComputedStyle(style).opacity   // fuerza el repaint; setTimeout NO es fiable
  document.head.removeChild(style)
}
```

**Checklist:** `color-scheme` en `:root` · `<meta name="color-scheme">` · script de tema antes del primer paint · ningún `#000`/`#fff` · bordes con alpha · elevación por color · croma reducido 20–40% · transiciones desactivadas en el toggle.

---

### 11. Contraste

| SC | Requisito | Aplica a |
|---|---|---|
| **1.4.3** AA | **4.5:1** | texto normal |
| **1.4.3** AA | **3:1** | texto grande: ≥24px, o ≥18.66px bold |
| **1.4.6** AAA | 7:1 / 4.5:1 | igual, nivel AAA |
| **1.4.11** AA | **3:1** | **componentes de UI y objetos gráficos** |

**1.4.11 es el que casi todo el mundo incumple.** Cubre bordes necesarios para identificar un control, indicadores de estado (focus, selected, checked), elementos gráficos dentro de controles (la palomita de un checkbox) y partes de gráficas. Exento: componentes deshabilitados y estilos por defecto no modificados. Y *"los valores no deben redondearse — 2.999:1 no cumple 3:1"*.

**APCA** (candidato a WCAG 3) devuelve **Lc** con signo y considera tamaño y peso:

| Lc | Significado |
|---|---|
| **90** | preferido para body text |
| **75** | mínimo para body text |
| **60** | texto de contenido no-body |
| **45** | titulares y texto grande |
| **30** | placeholders, disabled |
| **15** | divisores; por debajo, trátalo como invisible |

WCAG 2 **sobrestima** el contraste de colores oscuros — justo donde vive el dark mode. Cumple WCAG 2.2 para el checklist legal y usa APCA para decidir cuál de dos opciones conformes se ve mejor.

**"Pasa AA pero se ve mal" — los tres casos:** (1) el botón sólido `#0090ff` con texto blanco = 3.26:1, falla para un label de 14px; (2) el ámbar turbio; (3) texto medio sobre fondo oscuro que WCAG 2 aprueba y APCA rechaza. **Mide siempre contra el fondo renderizado**, no el teórico.

---

### 12. El look "IA genérica" — qué evitar

El corpus de entrenamiento está saturado de Tailwind con su índigo por defecto: cuando el prompt no decide, el modelo devuelve la moda.

| Tell | Por qué grita template | Qué hacer |
|---|---|---|
| `indigo-500` / `violet-600` de acento | el default de la doc de Tailwind durante años | elige hue por razón de marca |
| Gradiente morado→rosa en el hero | fondo por defecto de todo landing generado | color plano; si hay gradiente, ≤12 puntos de L y ≤40° de hue |
| Orbes violeta desenfocados | decoración sin función | nada, o grano al 2–3% |
| `#000` fondo / `#fff` texto | nadie eligió los valores | L 15–20% / L 92–96% |
| `backdrop-blur` en todo | destruye jerarquía | solo en overlays reales |
| Grises croma 0 | shadcn sin personalizar | tiñe 0.005–0.045 |
| Tres cards iguales con icono arriba | composición por defecto | jerarquía asimétrica |
| Badge pill sobre el H1 | tic de landing generado | quítalo |
| Borde izquierdo de color en cards | plantilla de blog | fondo sutil o icono |
| `shadow-purple-500/50` | ninguna luz real es morada | sombra neutra o con el hue del **fondo** |
| Emoji como iconos de features | | set de iconos consistente |
| Un acento distinto por sección | no hay sistema | un acento, uno |

**Diagnóstico rápido:** si al quitar el logo tu app podría ser de cualquier SaaS, el color no está haciendo trabajo.

---

### Reglas duras del color

1. Un hue de marca. Un gris. Cuatro semánticos. Nada más.
2. 12 pasos (Radix) o 10 (Geist). Nunca 3.
3. Define en OKLCH. Interpola en `oklab`. Nunca generes escalas en HSL.
4. Croma del gris entre **0.005 y 0.045**. Nunca 0 en toda la app.
5. Ningún componente lee un primitivo. Solo tokens semánticos.
6. Bordes con alpha: **6–12%** claro, **8–15%** oscuro. El borde de un control necesita **~42%** (3:1).
7. Texto primario en **L 20–26%** / **L 92–96%**. Nunca `#000` ni `#fff`.
8. Máximo **dos** niveles de fondo visibles a la vez.
9. En dark mode la elevación es **+3–4 puntos de L**, no una sombra.
10. Hover = un escalón (**4–6 L**). Active = dos. En dark mode el hover **aclara**.
11. Acento ≤ **10%** de la superficie. Un solo botón sólido por pantalla.
12. El color nunca es el único canal de información.
13. `color-scheme: light dark` siempre; transiciones desactivadas durante el toggle.
14. Mide el contraste contra el fondo **renderizado**.

### Fuentes — color

Radix Colors *Understanding the scale* · *Composing a palette* · *Aliasing* · repo `radix-ui/colors` (hex verificados) · Tailwind CSS v4 *Colors* y release post · Kyrylo Silin *OKLCH variables for Tailwind v4* · shadcn/ui *Theming* · Vercel Geist *Colors* · Evil Martians *OKLCH in CSS* · Josh Comeau *Color Formats in CSS* · MDN `color-mix()`, `light-dark()`, `color-scheme` · Refactoring UI *Building Your Color Palette* · Erik Kennedy *Color in UI Design* · Stripe *Designing accessible color systems* · Linear *How we redesigned the Linear UI (II)* · Material 2 *Dark theme* · Material 3 *Tone-based surfaces* · Apple HIG *Color* · W3C *Understanding SC 1.4.11* · Myndex *APCA in a Nutshell* · Paco Coursey *Disable transitions on theme toggle*.

---
---

## Espaciado, forma y elevación

La tipografía y el color deciden si tu UI se **entiende**. El espaciado, la forma y la elevación deciden si se siente **construida** o **ensamblada**. Es la capa donde se gana o se pierde, porque son decisiones de 2px que nadie nombra pero todos perciben.

> Regla que gobierna todo el capítulo: **ninguna medida de tu UI debe ser una decisión individual.** Cada número sale de una escala. Si te descubres escribiendo `padding: 13px`, no tienes un problema de padding: tienes un problema de sistema.

### 1. La escala de espaciado

**Por qué base 4:** es el máximo común divisor práctico (16px = 4×4); renderiza limpio en cualquier densidad (4 CSS px = 8 device px @2x, 12 @3x — nunca medio píxel). Regla corta: **dentro de componentes, base 4; entre bloques, base 8.**

La escala no es lineal: los saltos crecen porque, a mayor tamaño, 4px es imperceptible (4 vs 8 es el doble; 60 vs 64 no es nada).

| Token | px | rem | Uso canónico |
|---|---|---|---|
| `0.5` | 2 | 0.125 | Micro-ajuste óptico, offset de ring |
| `1` | 4 | 0.25 | Icono ↔ etiqueta pegados, gap de badges |
| `1.5` | 6 | 0.375 | Label ↔ input, gap de icono en botón |
| `2` | 8 | 0.5 | Elementos hermanos muy relacionados |
| `2.5` | 10 | 0.625 | Padding-x de botón sm |
| `3` | 12 | 0.75 | Padding-x de input, celda de tabla |
| `4` | 16 | 1 | **Gap por defecto.** Campo ↔ campo, card compacta |
| `5` | 20 | 1.25 | Padding de card default |
| `6` | 24 | 1.5 | Card grande, modal, gutter de página |
| `8` | 32 | 2 | Entre grupos de campos |
| `10` | 40 | 2.5 | Entre subsecciones |
| `12` | 48 | 3 | Entre secciones de página |
| `16` | 64 | 4 | Bloques mayores; mínimo en marketing |
| `20`/`24` | 80/96 | 5/6 | Solo marketing |

Doce valores cubren el 100% de una aplicación. Si tu design system tiene 30, no tiene ninguno.

**Tailwind v4:** desapareció la escala enumerada; todo se deriva de una variable — `.mt-8 → calc(var(--spacing) * 8)`. Consecuencia poderosa: `p-13` funciona sin configurar nada. Consecuencia peligrosa: **la escala ya no te protege.**

```css
/* A) Perilla global de densidad: escala TODO de golpe */
@theme { --spacing: 0.25rem; }   /* 0.2rem = denso · 0.3rem = amplio */

/* B) Escala cerrada: mata la escala dinámica. `p-13` deja de compilar. */
@theme {
  --spacing-*: initial;
  --spacing-0: 0px;      --spacing-px: 1px;
  --spacing-1: 0.25rem;  --spacing-2: 0.5rem;   --spacing-3: 0.75rem;
  --spacing-4: 1rem;     --spacing-5: 1.25rem;  --spacing-6: 1.5rem;
  --spacing-8: 2rem;     --spacing-10: 2.5rem;  --spacing-12: 3rem;  --spacing-16: 4rem;
}
```

**Por qué la escala mata la mitad de las decisiones:** sin escala, "cuánto espacio va aquí" tiene infinitas respuestas y todas parecen defendibles. Con escala la pregunta es "¿16 o 24?" — binaria, resoluble en dos segundos, y cualquiera de las dos produce una UI coherente.

### 2. Proximidad y agrupación

> **El espacio DENTRO de un grupo debe ser menor que el espacio ENTRE grupos.** Y no un poco: al menos **1.5×**, idealmente **2×**. Por debajo de 1.5× el ojo no distingue el nivel y todo se lee como lista plana.

| Relación | px | Tailwind |
|---|---|---|
| Icono ↔ texto (dentro de un botón) | 6 | `gap-1.5` |
| Label ↔ input | 6 | `space-y-1.5` |
| Input ↔ ayuda / error | 6 | `mt-1.5` |
| Campo ↔ campo (mismo grupo) | 16–20 | `space-y-4` / `space-y-5` |
| Grupo ↔ grupo | 24–32 | `space-y-6` / `space-y-8` |
| Título de sección ↔ contenido | 12–16 | `mb-3` / `mb-4` |
| Sección ↔ sección (app) | 32–48 | `space-y-8` / `space-y-12` |
| Header de página ↔ contenido | 24–32 | `mb-6` / `mb-8` |
| Sección ↔ sección (marketing) | 64–96 | `py-16` / `py-24` |

**Regla de Refactoring UI:** el espacio *interno* de un contenedor debe ser **≤** el espacio que lo rodea. Un card con `p-6` dentro de una grilla con `gap-4` se ve mal: las cards se tocan entre sí más de lo que su contenido toca sus bordes.

**Cómo detectarlo a simple vista:**
1. **Prueba del blur.** Captura y aplica `blur(6px)`. Debes ver bloques discretos. Masa gris uniforme = **no respira** (todos los gaps iguales). Islas desconectadas = **flota**.
2. **Entrecerrar los ojos.** Deberías poder contar los grupos sin leer.
3. Síntoma de "no respira": **todo con `gap-4`** — el error #1 de UI generada por IA.
4. Síntoma de "flota": `p-10` de contenedor con gaps internos pequeños.

### 3. Densidad

| Densidad | Altura control | Tailwind | Font | Padding-x | Fila de tabla | Para qué |
|---|---|---|---|---|---|---|
| Ultra compact | 28px | `h-7` | 12–13 | 8 | 32px | Toolbars, filtros, chips |
| **Compact** | 32px | `h-8` | 13 | 10–12 | 36–40px | Linear, Superhuman, tablas de datos |
| **Default** | 36px | `h-9` | 14 | 12–16 | 44–48px | **El default de shadcn.** SaaS B2B |
| Comfortable | 40px | `h-10` | 14–15 | 16 | 52px | Formularios importantes, onboarding |
| Touch | 44px | `h-11` | 15–16 | 20 | 56–64px | Mobile, kioscos |

- **No mezcles densidades dentro de una misma región.** Un `h-9` junto a un `h-10` en la misma toolbar es el error más visible del capítulo.
- **La densidad es una feature.** Si tienes tablas grandes, expón el selector.
- **`font-size` de inputs ≥ 16px en móvil** (si no, iOS hace zoom al enfocar). Sin excepciones.

```css
/* Perillas de densidad */
html { font-size: 87.5%; }                    /* 14px base: escala TODO lo que esté en rem */
@theme { --spacing: 0.2rem; }                 /* solo espaciado y sizing */
[data-density="compact"] { --spacing: 0.2rem; --control-h: 2rem; }
```

Nunca escales con `zoom` ni `transform: scale()`: texto borroso y hit areas desalineadas.

**Mínimos táctiles:** WCAG 2.2 SC 2.5.8 (AA) = **24×24 CSS px**; SC 2.5.5 (AAA) y Apple HIG = **44×44**; Material = 48×48. Un botón de icono de 28px se ve perfecto en una toolbar densa y viola el mínimo. La solución no es agrandarlo:

```css
.hit-area { position: relative; }
.hit-area::before {
  content: ""; position: absolute; top: 50%; left: 50%; translate: -50% -50%;
  width: max(100%, 44px); height: max(100%, 44px);   /* invisible, recibe el pointer */
}
```

Complemento: **en listas no dejes zonas muertas entre items.** Si necesitas separación, auméntala con `padding` del item (clickeable), nunca con `margin`.

### 4. Padding interno por componente

**Botones — la regla: `padding-x` ≈ 2–2.5× `padding-y`.** Un botón con padding igual en ambos ejes se ve achaparrado.

| Size | Altura | `padding-x` | `padding-y` | Gap icono | Font | Radius |
|---|---|---|---|---|---|---|
| `xs` | 24 | 8 | 4 | 4 | 12 | 4 |
| `sm` | 32 | 12 | 6 | 6 | 13 | 6 |
| `md` | 36 | 16 | 8 | 6 | 14 | 6–8 |
| `lg` | 40 | 20 | 10 | 8 | 15 | 8 |
| `xl` | 44 | 24 | 12 | 8 | 16 | 10 |
| icon-only | = altura | — | — | — | — | igual que su size |

Los icon-only son **cuadrados perfectos** (`size-9`). Un botón de icono más ancho que alto es un error de sistema.

| Componente | Padding | Notas |
|---|---|---|
| Input | `px-3 py-2` (h-9) | Con icono prefijo: `pl-9` y el icono en `absolute`, **nunca** hermano en flex |
| Textarea | `px-3 py-2.5` | Más `py` porque es multilínea |
| Select | `pl-3 pr-8` | Asimétrico: el chevron necesita aire |
| Card compacta / default / destacada | 16 / 20–24 / 32–40 | |
| Card con header-footer | `px-6` constante; `py-4` header, `py-4` body | |
| Item de menú | `px-2 py-1.5` (h-8) o `py-1` (h-7) | Contenedor del menú: `p-1` |
| Item de command palette | `px-3 py-2` | Más aire: se navega con teclado |
| Modal / Dialog | 24 | Título ↔ descripción 6–8; footer separado 24 |
| Sheet / Drawer | 24 | `padding-bottom: max(24px, env(safe-area-inset-bottom))` |
| Celda de tabla | `px-3 py-2` (compact) / `px-4 py-3` | Primera y última columna alinean con el padding del card |
| Tooltip | `px-2 py-1` | Font 12, radius 6 |
| Toast | 16 | Radius 12, acción a la derecha |
| Badge / Chip | `px-2 py-0.5` | Con `x` de cerrar: `pr-1` |
| Contenedor de página | `px-6` desktop / `px-4` mobile | `max-w-7xl mx-auto` |

**Cuándo el padding es asimétrico** (siempre es corrección óptica, no capricho): botón con icono a la izquierda → −2/4px de `padding-left`; select → `pr` 28–32; chip con cerrar → `pr` 4–6; texto con `letter-spacing` → −1/2px de `padding-right` (el tracking añade un espacio fantasma tras el último glifo).

### 5. Alineación óptica vs matemática

El centro matemático es el del *bounding box*. El centro óptico es el del **centroide de masa visual**. Cuando difieren, el ojo siempre gana.

| Caso | Corrección | Por qué |
|---|---|---|
| Icono de play en botón circular | `translate-x: 1–2px` | El centroide de un triángulo está a 1/3 de su base |
| Iconos en botones | `gap-1.5` en vez de `gap-2` | El viewBox aporta ~2px de aire fantasma por lado |
| Círculo junto a cuadrado igual | El círculo necesita 2–4% más | *Overshoot*: las curvas se perciben más pequeñas |
| MAYÚSCULAS centradas vertical | `padding-top: +1px` | No hay descendentes; la caja se desbalancea |
| Cifras en columna | `tabular-nums` | Sin esto el `1` es angosto y los decimales bailan |
| Comillas al inicio de un blockquote | `hanging-punctuation` + fallback `text-indent: -0.4em` | La comilla es aire, no masa |
| Chevron de acordeón | Súbelo 1px | El centro del glifo `˅` está bajo |

**Cómo detectar el "1px que decide":** (1) espejo — `scaleX(-1)`: si se ve *distinto*, hay desbalance; (2) blur agresivo — la mancha debe quedar centrada; (3) inversión de luminancia. La corrección es **siempre manual y siempre pequeña** (1–2px); si te salen 4px o más, el problema no es óptico.

### 6. Border radius

| Token | px | Aplicar a |
|---|---|---|
| `none` | 0 | Tablas edge-to-edge, elementos que llegan al borde |
| `sm` | 4 | Checkbox, badge, celda de calendario |
| `md` | 6 | **Botones e inputs.** El default moderno |
| `lg` | 8 | Botones grandes, cards pequeñas, items de dropdown |
| `xl` | 12 | **Cards, popovers, dropdowns, toasts** |
| `2xl` | 16 | Modales, sheets, cards destacadas |
| `3xl` | 24 | Hero cards, bottom sheets |
| `full` | ∞ | Avatares, pills, toggles, botones circulares, progress |

- **Proporción:** el radio nunca supera **~25–30% de la altura**. Un botón de 36px con radius 12 lee como pastilla accidental.
- **Cantidad:** un producto necesita **tres radios + `full`**. Cinco radios distintos en una pantalla es el síntoma más rápido de que no hay sistema.

```css
:root { --radius: 0.625rem; }              /* 10px — el valor de shadcn */
@theme inline {
  --radius-sm: calc(var(--radius) - 4px);  /*  6 */
  --radius-md: calc(var(--radius) - 2px);  /*  8 */
  --radius-lg: var(--radius);              /* 10 */
  --radius-xl: calc(var(--radius) + 4px);  /* 14 */
}
/* Bajarlo a 0.25rem = producto técnico y severo. Subirlo a 1rem = amable de consumo. Una línea. */
```

#### La fórmula de radios anidados

Si un elemento redondeado vive dentro de otro, las curvas deben ser **concéntricas** o el margen visual varía a lo largo de la esquina (grueso en las diagonales, fino en los ejes).

> **R_exterior = R_interior + padding** ⟺ **R_interior = R_exterior − padding**

Card `radius 16` + `padding 8` → hijo `radius 8`. Contenedor de menú `radius 12` + `p-1` → items `radius 8`.

```css
.card {
  --card-radius: 0.75rem; --card-pad: 0.25rem;
  border-radius: var(--card-radius); padding: var(--card-pad);
}
.card > * { border-radius: max(0px, calc(var(--card-radius) - var(--card-pad))); }
```

#### Squircles

```css
.squircle { border-radius: 24px; corner-shape: squircle; }  /* ≡ superellipse(2), la curva de iOS */
```
**Soporte 2026: Chrome/Edge 139+. Safari y Firefox no lo implementan.** Progressive enhancement puro — un squircle sí y otro no en la misma pantalla es peor que ninguno.

**Errores:** `rounded-full` en inputs de texto largo (las esquinas comen el área útil); mezclar 5 radios (`grep -o "rounded-[a-z0-9]*" -r src | sort | uniq -c` — más de cuatro es deuda); radio grande + borde 1px opaco (revela el antialiasing de la curva: con radios ≥16 usa borde con alpha).

### 7. Bordes

**El hairline de 1px.** En HiDPI, 1px CSS se dibuja nítido pero se **lee más pesado** de lo que esperarías. La tentación es `0.5px`: no lo hagas (Chrome redondea inconsistentemente y los bordes aparecen y desaparecen al scrollear). **La solución es bajar el contraste, no el grosor.**

```css
:root      { --border: oklch(0 0 0 / 0.08); }
.dark      { --border: oklch(1 0 0 / 0.08); }   /* blanco con alpha, nunca gris opaco */
.subtle-border { border: 1px solid color-mix(in oklab, currentColor 10%, transparent); }
```

| | `border` | `box-shadow: 0 0 0 1px` (`ring-1`) | `outline` |
|---|---|---|---|
| Ocupa layout | **Sí** | **No** | No |
| Respeta `border-radius` | Sí | Sí | Sí desde Safari 16.4 |
| Visible en `forced-colors` | Sí | **No** | **Sí** |
| Múltiples capas | No | Sí | No |

- **Estado en reposo → `border`.** Semántico y sobrevive a `forced-colors`.
- **Estado transitorio (hover, selected) → `ring`/`box-shadow`.** No provoca layout shift. El error clásico es añadir `border` en hover y ver saltar 1px todo el contenido.
- **Focus → `outline`** (§10).
- **Borde interno con `overflow: hidden`** → `box-shadow: inset 0 0 0 1px var(--border)`.

**Separadores:** > **Un solo mecanismo de separación por nivel jerárquico.** Nunca línea + espacio grande + cambio de fondo a la vez — es la marca inconfundible del diseño amateur: separa tres veces por miedo. Línea cuando el espacio es escaso y la jerarquía es la misma (filas de tabla, secciones internas de un card, header sticky). Solo espacio cuando ya hay otra señal.

### 8. Sombras

**Una sola fuente de luz:** arriba, ligeramente al frente. En la práctica, `x = 0` y `y > 0` para **todas** las sombras. Si un elemento proyecta hacia abajo y otro hacia abajo-derecha, el cerebro registra dos soles.

**Por qué 2–5 capas:** una sombra real no tiene un solo grado de difusión — la penumbra crece con la distancia. Una sola `box-shadow` produce ese degradado plano y gomoso que delata el `0 4px 12px rgba(0,0,0,.2)` escrito a mano. El patrón: **capas que duplican offset y blur**.

Escala de Tailwind v4 (base sólida, con `spread` negativo para que no se desborde):

| Token | Valor |
|---|---|
| `--shadow-2xs` | `0 1px rgb(0 0 0 / 0.05)` |
| `--shadow-xs` | `0 1px 2px 0 rgb(0 0 0 / 0.05)` |
| `--shadow-sm` | `0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)` |
| `--shadow-md` | `0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)` |
| `--shadow-lg` | `0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)` |
| `--shadow-xl` | `0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1)` |
| `--shadow-2xl` | `0 25px 50px -12px rgb(0 0 0 / 0.25)` |

> **Trampa de migración v3→v4:** los nombres se corrieron. El antiguo `shadow` ahora es `shadow-sm`; el antiguo `shadow-sm` es `shadow-xs`. Igual con radios: `rounded` → `rounded-sm`.

La versión premium, tintada y en capas:

```css
@theme {
  /* hue 250 = azul frío; ajústalo al hue de tu background */
  --shadow-1: 0 1px 2px -1px oklch(0.30 0.03 250 / 0.10);
  --shadow-2: 0 1px 2px -1px oklch(0.30 0.03 250 / 0.08),
              0 2px 4px -2px oklch(0.30 0.03 250 / 0.08);
  --shadow-3: 0 2px 4px -2px oklch(0.30 0.03 250 / 0.07),
              0 4px 8px -4px oklch(0.30 0.03 250 / 0.07),
              0 8px 16px -8px oklch(0.30 0.03 250 / 0.07);
  --shadow-4: 0 4px 8px -4px oklch(0.30 0.03 250 / 0.06),
              0 8px 16px -8px oklch(0.30 0.03 250 / 0.06),
              0 16px 32px -16px oklch(0.30 0.03 250 / 0.06);
}
```

- `blur ≈ 2 × offset-y`. `spread` negativo entre −1 y −12, creciendo con el nivel.
- **La opacidad baja conforme sube la elevación.** Una sombra grande y oscura no dice "elevado", dice "pegado con Photoshop".
- **Nunca negro puro:** toma el hue del fondo. Fondo `hsl(220 20% 97%)` → sombra `hsl(220 25% 20% / α)`. En un botón de marca, tinta la sombra con el hue del botón (`shadow-blue-500/25`): hace que parezca emitir su propio color.

**En dark mode** casi no funcionan. Sustitutos, por orden: (1) **lightness escalonada de superficie** — base L11%, sidebar 14%, card 17%, popover 21%, modal 24%; (2) **highlight interior superior** `inset 0 1px 0 rgb(255 255 255 / 0.06)` — *el* detalle que usan Linear y Vercel, y la diferencia entre "botón" y "objeto"; (3) borde blanco al 6–10%; (4) sombra negra muy difusa solo para modales.

```css
.btn-primary {
  background: linear-gradient(oklch(0.62 0.19 258), oklch(0.58 0.19 258));
  box-shadow:
    inset 0 1px 0 rgb(255 255 255 / 0.15),          /* canto superior iluminado */
    0 1px 2px -1px oklch(0.35 0.10 258 / 0.4),
    0 2px 6px -2px oklch(0.35 0.10 258 / 0.3);
}
```

**Coste:** `box-shadow` **no es compositable** — animarla fuerza repaint por frame. El patrón correcto:

```css
.card { position: relative; box-shadow: var(--shadow-1); }
.card::after {
  content: ""; position: absolute; inset: 0; border-radius: inherit;
  box-shadow: var(--shadow-3); opacity: 0;
  transition: opacity 180ms ease; pointer-events: none;
}
.card:hover::after { opacity: 1; }
```

### 9. Elevación como sistema

La elevación es un **contrato de tres partes** — superficie, sombra y z-index — asignado por rol, no por gusto.

| Nivel | Rol | Superficie light | Dark (L) | Sombra | z-index |
|---|---|---|---|---|---|
| 0 | Fondo de página | `oklch(0.98 0 0)` | 11% | ninguna | auto |
| 1 | Card, panel, tabla | `#fff` | 15% | `--shadow-1` + border | `--z-raised: 10` |
| 2 | Header sticky, toolbar | `#fff/.9` + blur | 15% | `--shadow-2` | `--z-sticky: 20` |
| 3 | Dropdown, select, menú | `#fff` | 19% | `--shadow-3` | `--z-dropdown: 50` |
| 4 | Popover, hovercard | `#fff` | 20% | `--shadow-3` | `--z-popover: 60` |
| 5 | Overlay + Modal | `#fff` | 22% | `--shadow-5` | `100` / `110` |
| 6 | Toast | `#fff` | 22% | `--shadow-4` | `--z-toast: 200` |
| 7 | Tooltip | inverso del texto | 26% | `--shadow-2` | `--z-tooltip: 250` |

Los niveles 1 y 2 **comparten superficie pero no sombra**; en dark mode la sombra casi desaparece y la superficie hace todo el trabajo. Material reserva sus niveles altos para estados interactivos: **si algo sube al hacer hover, sube exactamente un nivel.**

```css
@theme {
  --z-base: 0;      --z-raised: 10;   --z-sticky: 20;  --z-nav: 30;
  --z-dropdown: 50; --z-popover: 60;  --z-overlay: 100; --z-modal: 110;
  --z-toast: 200;   --z-tooltip: 250;
}
```

- **Ningún `z-index` fuera de la escala. Cero excepciones.** `z-9999` no es un valor, es una rendición.
- Si necesitas más, **el elemento está en el lugar equivocado del DOM** — sácalo a un portal.
- Cuidado con los *stacking contexts* de `transform`, `filter`, `backdrop-filter`, `opacity < 1`, `will-change`, `isolation`, `container-type`. Cuando un z-index "no funciona", el culpable es casi siempre un ancestro con una de estas.

### 10. Focus ring

**Spec: 2px de color + 2px de separación del color de la superficie**, respetando el radio.

```css
/* Vía preferida en 2026 */
:where(a, button, input, select, textarea, summary, [tabindex]):focus-visible {
  outline: 2px solid var(--ring);
  outline-offset: 2px;
}

/* Vía box-shadow: cuando el elemento vive dentro de un overflow:hidden */
.focus-shadow:focus-visible {
  outline: none;
  box-shadow: 0 0 0 2px var(--surface),   /* interior = color del fondo → crea el gap */
              0 0 0 4px var(--ring);      /* exterior = el visible */
}
@media (forced-colors: active) {
  .focus-shadow:focus-visible { outline: 2px solid Highlight; }
}
```

- **El doble anillo** garantiza contraste sobre cualquier fondo, incluso si el elemento es del mismo color que el ring.
- **`:focus-visible`, no `:focus`.** `:focus` dispara también con click de mouse, lo que lleva al pecado capital: `outline: none` global.
- **Contraste ≥ 3:1** contra el fondo adyacente (WCAG 1.4.11).
- **Reserva espacio:** si el elemento está pegado a un `overflow: hidden`, deja 4px de padding o el anillo se corta.

En Tailwind v4 `ring` pasó de 3px a **1px** por defecto; shadcn usa `focus-visible:ring-[3px] ring-ring/50` — grueso pero semitransparente.

### 11. Glass/blur y grain

**Cuándo `backdrop-filter` suma:** solo cuando hay algo que desenfocar **y ese algo se mueve** — header sticky sobre contenido que scrollea, command palette sobre la app, sheet sobre un mapa o vídeo.

**Cuándo delata a un principiante:** sobre un fondo plano (pagas GPU por cero información); con contraste de texto insuficiente (el fondo cambia al scrollear); en todas partes (el glass es un acento); **sin `background` con alpha** — si el fondo es opaco el filtro corre y no se ve nada: es el bug #1.

```css
.glass {
  background: rgb(255 255 255 / 0.72);
  backdrop-filter: blur(12px) saturate(180%);
  border-bottom: 1px solid rgb(0 0 0 / 0.06);
}
.dark .glass { background: rgb(10 10 12 / 0.6); border-bottom-color: rgb(255 255 255 / 0.08); }
@supports not (backdrop-filter: blur(1px)) { .glass { background: rgb(255 255 255 / 0.96); } }
```

`blur(4px)` = textura sutil · `blur(12px)` = el punto dulce · `>24px` = caro y sin información añadida. `saturate(180%)` compensa el lavado de color (sin él el cristal se ve gris). **Gotcha:** `backdrop-filter` crea siempre un nuevo stacking context.

**Grain contra el banding.** Los degradados largos de baja diferencia cromática (típicos en dark mode) producen bandas porque 8 bits no dan suficientes pasos. Un ruido del 3% rompe el patrón:

```css
.grain { position: relative; isolation: isolate; }
.grain::after {
  content: ""; position: absolute; inset: 0; pointer-events: none; z-index: 1;
  opacity: 0.035;   /* 0.02–0.05. Más de 0.06 se ve sucio */
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

`baseFrequency` 0.65–0.9 es el rango útil. El filtro se computa una vez y se cachea como imagen: coste despreciable.

### 12. Checklist: "por qué esto se ve barato"

| Síntoma | Causa real | Corrección |
|---|---|---|
| Todo con el mismo `gap-4` | No hay jerarquía de proximidad | Desigualdad de §2: ≥1.5× entre niveles |
| Sombra negra dura | Una capa, negro puro, opacidad alta | 2–3 capas, hue del fondo, alpha 0.06–0.10 |
| Cinco radios distintos | Sin token base | Un `--radius` + `calc()` |
| Hijo y padre con el mismo radio | Falta la fórmula concéntrica | `R_int = R_ext − padding` |
| Bordes gris opaco (`#e5e7eb`) | Color fijo | Borde con alpha |
| Todo tiene sombra | Elevación sin sistema | La mayoría del contenido es nivel 0–1 |
| El layout salta en hover | `border` añadido en hover | `ring` / `box-shadow` |
| Anillo de focus cuadrado sobre botón redondo | `outline: none` a medias | `outline` + `outline-offset` con `:focus-visible` |
| Botón achaparrado | `padding-x` = `padding-y` | `x ≈ 2–2.5 × y` |
| Botones de 34, 37 y 42px | Alturas ad hoc | Escala 28/32/36/40/44 |
| Dark mode plano | Se copiaron las sombras del light | Escalonar lightness + `inset 0 1px 0 rgb(255 255 255/.06)` |
| Bandas en el degradado | 8-bit banding | Grain al 3% |
| Cristal sobre fondo sólido | `backdrop-filter` decorativo | Quítalo |
| Padding gigante, contenido perdido | Interno > externo | Interno ≤ circundante |
| `z-index: 9999` | No hay escala | Tokens `--z-*` + portals |
| Línea + espacio + fondo separando lo mismo | Triple señal | Un mecanismo por nivel |
| Botón de icono difícil de acertar | 28×28 real | Pseudo-elemento 44×44 |
| El input hace zoom en iPhone | `font-size < 16px` | `text-base` en móvil |
| "Huecos muertos" al navegar una lista | Separación por `margin` | `padding` del item |

### Fuentes — espaciado, forma y elevación

Tailwind CSS *Theme variables*, *Box Shadow/ring*, release v4 · Josh Comeau *Designing Beautiful Shadows in CSS* y *Shadow Palette Generator* · Rauno Freiberg *Interfaces* · Cloud Four *The Math Behind Nesting Rounded Corners* · 30 seconds of code *Perfect nested border radius* · Material 3 *Elevation* · Material 2 *Dark theme* · Radix Themes *Spacing* · shadcn/ui *Theming* y *Tailwind v4* · Smashing *Beyond border-radius: corner-shape* · squircle.js *Squircles in CSS (2026)* · W3C *C44: targets 44×44* y *SC 2.5.8* · Piccalilli *double focus ring* · CSS-Tricks *Grainy Gradients* · Codrops *feTurbulence* · MDN *backdrop-filter* · Atlassian *Elevation/Spacing* · Refactoring UI · Optical Center *Perceptual Centering* · Emil Kowalski *design-eng*.

---
---

## Tipografía y jerarquía

> En una app de producto la tipografía no "decora": **estructura**. Cada decisión (tamaño, peso, color, espacio) es una señal de prioridad. Si todo pesa igual, el usuario tiene que leer todo. Ese es el costo real de una mala jerarquía.

### 1. Elegir la tipografía de producto

Seis propiedades medibles, no gusto:

| Propiedad | Por qué importa | Cómo verificarla |
|---|---|---|
| **x-height alta** (~72–75% del cap height) | La lectura en 11–14px depende de la minúscula | Compara `x` contra `H` a 12px |
| **Aperturas abiertas** (`a c e s`) | Se colapsan a tamaños chicos | Renderiza `caceres` a 11px |
| **Formas inequívocas** `1 l I` y `0 O` | Un SKU mal leído es un bug de negocio | Escribe `Il1 O0 rn m` |
| **Cifras tabulares (`tnum`)** | Sin ellas ninguna tabla alinea | `font-variant-numeric: tabular-nums` |
| **Variable font (`wght`, ideal `opsz`)** | Un archivo cubre 400/500/600 | Un `.woff2` con eje `wght 100..900` |
| **Latina extendida** | Español: `á é í ó ú ñ ü ¿ ¡ «»` | Prueba `¿Qué añadiré?` |

| Fuente | Licencia | Cuándo | Cuándo no |
|---|---|---|---|
| **Inter / InterVariable** | OFL | Default seguro. `wght 100–900` + `opsz 14–32`, `tnum`, `zero`, `ss01–08`, `cv01–13` | Cuando no quieres verte "como todos los SaaS" |
| **Geist Sans / Mono** | OFL (`npm i geist`) | Developer tooling; par sans+mono diseñado junto | Producto cálido de consumo (es fría) |
| **`system-ui` stack** | 0 bytes | Apps internas, admin, cuando el LCP manda | Cuando el diseño debe ser idéntico en Win y macOS |
| **SF Pro** | Solo apps Apple | Nunca en web (violación de licencia); se accede vía `system-ui` | — |
| **Söhne / Suisse Int'l** | De pago | La marca es diferenciador y hay presupuesto | MVPs |
| **Instrument Sans** | OFL | Alternativa con más carácter (`wght` + `wdth`) | Densidades extremas: Inter aguanta mejor a 11px |

**Cuántas familias:** **una sans + una mono**. La mono existe para código, IDs, hashes y folios. Una tercera solo si es *display* de marketing y nunca entra al producto.

```css
--font-system:
  system-ui, -apple-system, BlinkMacSystemFont,
  "Segoe UI Variable Text", "Segoe UI", Roboto,
  "Helvetica Neue", Arial, "Noto Sans", sans-serif,
  "Apple Color Emoji", "Segoe UI Emoji", "Noto Color Emoji";
```

```ts
// app/fonts.ts
import { Inter } from "next/font/google";
import localFont from "next/font/local";

export const sans = Inter({
  subsets: ["latin"], display: "swap", variable: "--font-sans",
  axes: ["opsz"],                       // sin esto next/font solo baja wght
});
export const mono = localFont({
  src: "./GeistMonoVF.woff2", display: "swap", variable: "--font-mono",
  declarations: [{ prop: "font-feature-settings", value: '"tnum" 1' }],
});
```

- `next/font` **autohospeda** el archivo e **inyecta métricas de fallback** (`size-adjust`, `ascent-override`) → CLS ~0.
- `display: 'swap'` (default) = **FOUT**; `'block'` = **FOIT** (texto invisible hasta ~3s). En producto **siempre `swap`**: texto invisible es contenido perdido.
- `preload` solo precarga los `subsets` que declaras.

**Se ve amateur cuando…** cargas la fuente con `<link>` a `fonts.googleapis.com`, cargas 6 pesos estáticos en vez de una variable, o usas 3 familias en la misma pantalla.

### 2. La escala real de UI

Las escalas modulares puras (1.25, 1.333) producen 12.8 / 16 / 20 / 25 / 31.25 / 39: decimales que caen en medio píxel, huecos justo donde está la acción (11–16px) y valores que se disparan arriba. La escala de producto es **hecha a mano**:

**11 · 12 · 13 · 14 · 15 · 16 · 18 · 20 · 24 · 30 · 36**

| px | rem | Uso | line-height | tracking | weight |
|---:|---:|---|---:|---:|---:|
| 11 | 0.6875 | Overline MAYÚSCULAS, badges | 1.45 (16) | **+0.06em** | 600 |
| 12 | 0.75 | Caption, ayuda, timestamps | 1.33 (16) | +0.005em | 400/500 |
| 13 | 0.8125 | Celda de tabla densa, sidebar | 1.38 (18) | 0 | 400/500 |
| **14** | 0.875 | **Body de herramientas**, labels, botones | 1.43 (20) | −0.006em | 400/500 |
| 15 | 0.9375 | Body cómodo, descripciones | 1.47 (22) | −0.008em | 400 |
| **16** | 1 | **Body de consumo**, inputs móvil, prosa | 1.5 (24) | −0.01em | 400 |
| 18 | 1.125 | Lead, título de card | 1.44 (26) | −0.012em | 500/600 |
| 20 | 1.25 | Título de sección, header de modal | 1.4 (28) | −0.014em | 600 |
| 24 | 1.5 | **Título de página** | 1.33 (32) | −0.018em | 600 |
| 30 | 1.875 | Hero interno, KPI grande | 1.2 (36) | −0.022em | 600 |
| 36 | 2.25 | Display, empty state grande | 1.11 (40) | −0.026em | 600/700 |

**El 80% de la pantalla usa un solo tamaño.** ~4 tamaños activos por diseño. **Se ve amateur cuando…** hay 9 tamaños en una pantalla, o el salto entre título y body es de 2px (no lee como jerarquía, lee como error).

### 3. Tamaño base: 14 vs 16

| Contexto | Base | Por qué |
|---|---|---|
| Herramienta densa (dashboards, tablas, admin) | **14px** | Más información por scroll; estándar de Linear, Notion, Jira, GitHub |
| Consumo, onboarding, docs, web pública | **16px** | Default del navegador; lectura sostenida sin cansancio |
| Móvil | **16px mínimo en inputs** | Ver abajo |

**El mínimo de 16px en inputs es obligatorio.** Safari iOS hace zoom al enfocar cualquier control cuyo `font-size` **computado** sea menor a 16px, desplaza el layout y no siempre revierte.

```css
input, select, textarea { font-size: max(1rem, 0.875rem); }
/* o */ @media (pointer: coarse) { input, select, textarea { font-size: 1rem; } }
```

Nunca lo "arregles" con `user-scalable=no`: rompe WCAG 1.4.4.

**13px es legítimo** en celdas, sidebar, chips y metadatos — texto **escaneado**. **Es un error** en párrafos, descripciones y mensajes de error. Por debajo de 12px no pongas nada que importe.

```css
html { font-size: 87.5%; }   /* ✅ 14px respetando la preferencia del usuario */
html { font-size: 14px;  }   /* ❌ ignora a quien configuró 20px en su navegador */
```

### 4. Line-height

**Inversamente proporcional al tamaño.** Butterick: 120–145% del cuerpo para texto corrido.

| Tamaño | LH | Absoluto |  | Tamaño | LH | Absoluto |
|---|---:|---:|---|---|---:|---:|
| 36 | 1.10 | 40 | | 15 | 1.47 | 22 |
| 30 | 1.20 | 36 | | 14 | 1.43 | 20 |
| 24 | 1.33 | 32 | | 13 | 1.38 | 18 |
| 20 | 1.40 | 28 | | 12 | 1.33 | 16 |
| 18 | 1.44 | 26 | | 11 | 1.45 | 16 (sube: el uppercase con tracking necesita aire) |
| 16 | 1.50 | 24 | | | | |

**Excepción:** en texto de **una sola línea** (botones, badges, celdas) el line-height es **altura de caja** — fija el absoluto (`leading-5`) para que el botón mida lo que esperas y no dependa de la fuente cargada.

**Longitud de línea:** 45–75 caracteres (`max-w-[65ch]`, `max-w-prose`). **Línea más larga ⇒ line-height mayor.** En columnas <40ch baja a 1.4; en >75ch sube a 1.6 — o mejor, angosta la columna.

### 5. Letter-spacing (tracking)

Las fuentes se dibujan con espaciado óptimo para ~16px. Al alejarte, deja de serlo: grande ⇒ demasiado suelto (tracking **negativo**); pequeño ⇒ demasiado apretado (**positivo**); MAYÚSCULAS ⇒ siempre suelto.

| Caso | letter-spacing |
|---|---:|
| 36–48px display | **−0.025 a −0.03em** |
| 24–30px títulos | −0.018 a −0.022em |
| 18–20px | −0.012 a −0.014em |
| 14–16px body | −0.006 a −0.01em (o 0) |
| 12–13px | 0 a +0.005em |
| 11–12px **UPPERCASE** | **+0.05 a +0.1em** |
| Números tabulares en tabla | **0** — nunca trackees cifras |

```css
body { font-optical-sizing: auto; }              /* default; Baseline desde mar-2020 */
.tight { font-variation-settings: "opsz" 32; }   /* forzar el corte display */
```

`text-rendering`: déjalo en `auto`. `optimizeLegibility` desactiva optimizaciones y provoca saltos de render.

**Se ve amateur cuando…** un `<h1>` de 40px va con tracking 0 (se ve desarmado), o una etiqueta `ESTADO` de 11px va sin tracking (se lee como un bloque sólido).

### 6. Pesos

**400 / 500 / 600. Punto.**

| Peso | Uso |
|---:|---|
| 400 | Body, celdas, descripciones, valores |
| 500 | Labels, botones secundarios, énfasis suave, nav activo |
| 600 | Títulos, headers de tabla, botón primario, KPIs |
| 700 | Reservado a display de marketing. En UI casi nunca |
| <400 | Nunca: a 14px un Light se desintegra |

A 14px la diferencia entre 600 y 700 apenas se percibe pero el 700 engorda y ensucia. **El salto 400 → 700 sin 500** es el error del que solo carga dos pesos: todo énfasis se vuelve un grito.

```css
body { font-synthesis-weight: none; font-synthesis-style: none; }
```

Sin esto, pedir `600` a una familia que solo tiene 400 produce **faux bold**: métricas alteradas, spacing roto, saltos de línea distintos.

**NUNCA cambies el peso en `:hover` / `aria-current`.** El texto en 600 es más ancho: el elemento cambia de tamaño y empuja a sus vecinos (en un menú horizontal se ve como un temblor).

```css
.nav-item { display: grid; }
.nav-item::after {                 /* fantasma en 600 que fija el ancho */
  content: attr(data-label); grid-area: 1/1; font-weight: 600;
  height: 0; visibility: hidden; pointer-events: none;
}
.nav-item > span { grid-area: 1/1; }
.nav-item[aria-current="page"] > span { font-weight: 600; }
```

Alternativas: cambiar **color** en lugar de peso (lo correcto el 90% de las veces), o `text-shadow: 0 0 0.35px currentColor`.

### 7. Jerarquía sin abusar del tamaño

Cuatro canales, en orden de eficiencia dentro de una app densa: **1. Color · 2. Peso · 3. Espacio · 4. Tamaño.** Un principiante hace jerarquía solo con tamaño y termina con títulos de 32px en un dashboard.

> **Baja el contraste de lo secundario, no subas el de lo primario.** Si el título ya es negro y quieres que destaque más, apaga el metadato de al lado. La jerarquía es relativa; siempre es más barato bajar que subir.

| Nivel | Qué es | Light (sobre `#fff`) | Ratio | Dark (sobre `#0a0a0a`) | Ratio |
|---|---|---|---:|---|---:|
| **L1 primario** | Títulos, valores | `#171717` | ≈17.9:1 | `#ededed` | ≈16.9:1 |
| **L2 secundario** | Body, descripciones, labels | `#525252` | ≈7.8:1 | `#a1a1a1` | ≈7.6:1 |
| **L3 terciario** | Metadatos, timestamps, placeholders | `#737373` | ≈4.7:1 | `#7f7f7f` | ≈5.5:1 |
| *(Disabled)* | Estado, **nunca información** | `#a3a3a3` | ≈2.5:1 | `#525252` | — |

**Tres niveles. Tres.** `#a3a3a3` no pasa 4.5:1: solo puede usarse en controles deshabilitados (exentos de 1.4.3).

**Se ve amateur cuando…** hay 8 grises de texto (nadie percibe la diferencia entre `#666` y `#6b6b6b`, pero sí la inconsistencia), o cuando todo es `#000` (vibra y aplana la jerarquía).

### 8. Números y datos

**`tabular-nums` es obligatorio** en tablas, precios, contadores, timers, porcentajes que se actualizan, IDs, paginación — cualquier cifra que **cambie en el mismo lugar**. Sin él, el `1` es angosto y el número "baila" en cada render.

```css
/* Por rol, no global: el texto corrido lee mejor con cifras proporcionales */
.tabular, table td, [role="cell"], .metric, .timer, .money {
  font-variant-numeric: tabular-nums;
  font-feature-settings: "tnum" 1;
}
.identifier { font-family: var(--font-mono); font-variant-numeric: tabular-nums slashed-zero; }
```

**`slashed-zero`** donde `0` y `O` conviven: folios, SKUs, tokens, placas, VINs, guías.

| Tipo de columna | Alineación |
|---|---|
| Números, moneda, % | **Derecha** |
| Fechas y horas | Derecha o izquierda, pero **consistente** |
| Texto, nombres, estados | Izquierda |
| Encabezado de columna | **Igual que su contenido** |

```ts
const money = new Intl.NumberFormat("es-MX", {
  style: "currency", currency: "MXN", minimumFractionDigits: 2, maximumFractionDigits: 2,
});
money.format(1234.5);   // "$1,234.50"

new Intl.NumberFormat("es-MX", { notation: "compact" }).format(12400);      // "12 mil"
new Intl.NumberFormat("es-MX", { style: "percent", maximumFractionDigits: 1 }).format(0.0734); // "7.3 %"
```

- **Fija los decimales** en columnas de dinero: si una fila muestra `$1,200` y otra `$1,200.50`, la columna deja de alinear aunque tengas `tnum`.
- **Compacto solo en KPIs y gráficas**, nunca en tablas que se comparan o exportan.
- Los ceros a la izquierda de folios (`GBX-26-00042`) van en `font-mono`.

### 9. Detalles de acabado

**`-webkit-font-smoothing: antialiased`** es no estándar y **solo afecta a macOS**. Desactiva el subpixel rendering, que triplica la resolución efectiva.
- **Sí:** texto claro sobre fondo oscuro y tamaños grandes.
- **No:** texto oscuro sobre claro a ≤14px — ahí adelgaza el texto y le **baja el contraste real**.
- Nunca global sin comparar lado a lado.

**`text-wrap`** (Baseline oct-2024):

```css
h1, h2, h3, .card-title { text-wrap: balance; }   /* ≤6 líneas Chromium, ≤10 Firefox */
p, .prose             { text-wrap: pretty;  }     /* Chrome 117+, Safari 26+; Firefox aún no */
[contenteditable]     { text-wrap: stable;  }
```

**Viudas y huérfanas:** `pretty` mata la huérfana; para lo que no puedes dejar al azar usa `&nbsp;` o `nowrap` en el fragmento final.

**Tipografía española correcta** (no es adorno, es la diferencia entre un producto y un formulario):

| Mal | Bien | Uso |
|---|---|---|
| `"texto"` | `«texto»` (RAE) o `“texto”` | Comillas |
| `- inciso -` | `—inciso—` (U+2014) | Incisos |
| `2024-2026` | `2024–2026` (U+2013) | Rangos |
| `...` | `…` (U+2026) | Suspensivos |
| `Que?` | `¿Qué?` `¡Listo!` | Apertura obligatoria |
| `5 kg` con salto | `5&nbsp;kg` | Cifra + unidad |

`&nbsp;` obligatorio en `12&nbsp;%`, `página&nbsp;3`, `v1.2.0&nbsp;beta`, `Sr.&nbsp;Calixto`.

```css
.truncate-1 { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; min-width: 0; }
.truncate-2 { display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
.identifier-wrap { overflow-wrap: anywhere; }   /* rompe solo si hace falta — preferido */
.hash { word-break: break-all; }
.narrow-column { hyphens: auto; }               /* requiere lang="es" en <html> */
```

**`min-width: 0` es el bug que todos tienen:** dentro de un flex item, `truncate` no funciona porque el default `min-width: auto` impide encoger. Siempre `min-w-0` en el hijo flex que trunca. Y siempre que truncas, deja el texto completo disponible (`title` o tooltip).

### 10. Microcopy y labels

**El texto es ~90% de la UI.** Un botón es un rectángulo con una palabra dentro; la palabra es el producto.

- **Sentence case, no Title Case.** En español la decisión es fácil: **Title Case no existe en español**, capitalizar cada palabra es un anglicismo. `Crear entrada`, no `Crear Entrada`.
- **MAYÚSCULAS solo para overlines** (11–12px, 600, +0.06em, ≤3 palabras). Nunca en botones: destruye la forma de la palabra.
- **Labels de botón: 1–3 palabras, empezando por verbo.** Nunca `OK`/`Aceptar` en diálogos destructivos: el botón debe decir qué hace (`Eliminar cliente`), porque es lo único que mucha gente lee.
- **Labels de campo: sustantivo corto, sin dos puntos.** `Correo`, no `Correo:` ni `Ingresa tu correo`.
- **Errores: qué pasó + qué hacer**, sin culpar y sin jerga. ❌ `Error 422: validation failed` · ✅ `El folio ya existe. Usa uno distinto o abre la entrada existente.`
- **Consistencia léxica > sinónimos elegantes.** Si es "entrada", es "entrada" en toda la app.

### 11. Implementación en Tailwind v4

En v4 un `--text-*` lleva **line-height, letter-spacing y font-weight emparejados**: `text-sm` deja de ser "un tamaño" y pasa a ser **una decisión tipográfica completa**.

```css
@import "tailwindcss";

@theme inline {   /* `inline` es obligatorio al referenciar var() de next/font */
  --font-sans: var(--font-sans), system-ui, -apple-system, "Segoe UI Variable Text",
               "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  --font-mono: var(--font-mono), ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
}

@theme {
  --text-*: initial;                     /* borra la escala default y deja solo la tuya */

  --text-2xs: 0.6875rem;  --text-2xs--line-height: 1rem;
  --text-2xs--letter-spacing: 0.06em;    --text-2xs--font-weight: 600;

  --text-xs: 0.75rem;     --text-xs--line-height: 1rem;
  --text-xs--letter-spacing: 0.005em;    --text-xs--font-weight: 400;

  --text-sm: 0.8125rem;   --text-sm--line-height: 1.125rem;
  --text-sm--letter-spacing: 0em;        --text-sm--font-weight: 400;

  --text-base: 0.875rem;  --text-base--line-height: 1.25rem;    /* BODY */
  --text-base--letter-spacing: -0.006em; --text-base--font-weight: 400;

  --text-lg: 1rem;        --text-lg--line-height: 1.5rem;
  --text-lg--letter-spacing: -0.01em;

  --text-xl: 1.125rem;    --text-xl--line-height: 1.625rem;
  --text-xl--letter-spacing: -0.012em;   --text-xl--font-weight: 600;

  --text-2xl: 1.25rem;    --text-2xl--line-height: 1.75rem;
  --text-2xl--letter-spacing: -0.014em;  --text-2xl--font-weight: 600;

  --text-3xl: 1.5rem;     --text-3xl--line-height: 2rem;         /* título de página */
  --text-3xl--letter-spacing: -0.018em;  --text-3xl--font-weight: 600;

  --text-4xl: 1.875rem;   --text-4xl--line-height: 2.25rem;
  --text-4xl--letter-spacing: -0.022em;  --text-4xl--font-weight: 600;

  --text-5xl: 2.25rem;    --text-5xl--line-height: 2.5rem;
  --text-5xl--letter-spacing: -0.026em;  --text-5xl--font-weight: 600;

  --font-weight-*: initial;              /* solo los pesos que existen */
  --font-weight-normal: 400; --font-weight-medium: 500; --font-weight-semibold: 600;

  --color-fg: oklch(0.18 0 0);           /* #171717 */
  --color-fg-muted: oklch(0.44 0 0);     /* #525252 */
  --color-fg-subtle: oklch(0.57 0 0);    /* #737373 */
}

@layer base {
  html { font-size: 87.5%; -webkit-text-size-adjust: 100%; }
  body {
    font-family: var(--font-sans);
    font-optical-sizing: auto;
    font-synthesis-weight: none;
    color: var(--color-fg);
  }
  h1, h2, h3, h4 { text-wrap: balance; }
  p              { text-wrap: pretty;  }
  input, select, textarea { font-size: max(1rem, inherit); }
  :where(td, th, .tabular) { font-variant-numeric: tabular-nums; }
}
```

Ahora `class="text-3xl"` ya trae 24px + 32px de line-height + −0.018em + 600, y quien quiera otra cosa tiene que escribirlo explícitamente — que es exactamente la fricción que quieres.

### 12. Accesibilidad

| Requisito | Número | Criterio |
|---|---|---|
| Contraste texto normal | **4.5:1** | WCAG 1.4.3 (AA) |
| Contraste texto grande | **3:1** (≥24px, o ≥18.7px bold) | WCAG 1.4.3 |
| Contraste de componentes | 3:1 | WCAG 1.4.11 |
| Zoom | **200% sin pérdida de contenido ni función** | WCAG 1.4.4 |
| Text spacing sin romper | LH 1.5×, párrafo 2×, letter 0.12em, word 0.16em **simultáneos** | WCAG 1.4.12 |
| Mínimo práctico | 11px absoluto, 12px legible, **16px en inputs móviles** | HIG / iOS Safari |

Prueba el zoom al 200% en 1280px: si el sidebar tapa el contenido o una tabla se corta sin scroll, fallaste. **Texto en imágenes = error**: no escala, no se traduce, no se copia, no lo lee un lector de pantalla. La única excepción es un logotipo.

### Chuleta: las 12 reglas duras

1. Una sans + una mono. Nada más.
2. `next/font` con `display: 'swap'` y self-hosting. Nunca `<link>` a Google Fonts.
3. Escala hecha a mano: 11/12/13/14/15/16/18/20/24/30/36. Nada de ratios puras.
4. Base 14px en herramientas, 16px en consumo. **16px mínimo en todo input.**
5. Line-height inverso al tamaño: 1.1 arriba, 1.5 abajo. Línea de 45–75 caracteres.
6. Tracking negativo arriba de 18px, positivo abajo de 12px y en MAYÚSCULAS.
7. Pesos 400/500/600. `font-synthesis-weight: none`.
8. El peso **nunca** cambia en hover. Cambia el color.
9. Tres niveles de color de texto. No cuatro, no ocho.
10. `tabular-nums` en toda cifra que cambie o se alinee. Números a la derecha.
11. `balance` en títulos, `pretty` en párrafos, `min-w-0` en todo lo que trunca.
12. Empareja tamaño + line-height + tracking + peso en `@theme`.

### Fuentes — tipografía

Tailwind CSS *font-size*, *Theme variables*, *line-clamp*, *font-smoothing* · Next.js *Font Module* · MDN `text-wrap`, `font-variant-numeric`, `font-optical-sizing`, `font-synthesis-weight`, `Intl.NumberFormat` · Inter (rsms.me) · Vercel Geist Font · Material 3 *Type scale tokens* · Apple HIG *Typography* y WWDC20 *The details of UI typography* · Erik Kennedy *Font Size Guidelines* y *Ultimate Guide to Font Sizes* · Butterick *Line spacing* · Refactoring UI · CSS-Tricks *16px prevents iOS zoom* · Defensive CSS *Input zoom on iOS* · David Bushell *WebKit Font Smoothing* · A List Apart *Say No to Faux Bold* · Daring Fireball *text-wrap: pretty* y *Title vs Sentence Case* · John Saito *Making a case for letter case* · Deque *WCAG 1.4.12* · WebAIM Contrast Checker · modern-font-stacks.

---
---

## Botones, acciones y formularios

> Los controles son el 90% de la superficie de contacto de una app. Aquí no hay espacio para "casi bien": un botón mal jerarquizado o un input sin `autocomplete` se sienten baratos aunque el resto sea impecable.

### 1. Jerarquía de acciones

**Regla dura #1: una sola acción primaria por pantalla o sección delimitada.** Refactoring UI lo formula al revés y es más útil así: *si tu acción principal no destaca, no la hagas más grande — apaga a las que compiten con ella*.

| Variante | Peso visual | Cuándo | Ejemplo |
|---|---|---|---|
| `primary` (solid) | Máximo: fondo sólido de marca | LA acción de la pantalla. Máx. 1 visible | "Crear factura" |
| `secondary` (soft/tinted) | Medio: color al 10–15%, texto del color | Alterna frecuente que sí quieres que se vea | "Guardar borrador" |
| `outline` | Medio-bajo: borde 1px, fondo transparente | Alterna sobre fondos con contenido; el estándar para "Cancelar" | "Cancelar", "Exportar" |
| `ghost` | Bajo: solo hover | Toolbars, menús, acciones por fila, icon-only | Iconos de tabla, "Más" |
| `link` | Mínimo: texto + underline en hover | Terciaria o navegación | "¿Olvidaste tu contraseña?" |
| `destructive` (solid) | Máximo + rojo | Solo cuando borrar **es** la acción principal de ese contexto (el botón dentro del dialog) | "Eliminar cuenta" |
| `destructive-ghost` | Bajo + rojo | El disparador del flujo destructivo dentro de una página normal | "Eliminar" en Configuración |

**Errores que delatan a un principiante:** dos botones sólidos lado a lado; "Cancelar" con más peso que "Confirmar"; el botón rojo grande viviendo en la página en vez del dialog; tres primarios "porque los tres son importantes" (si los tres lo son, ninguno lo es: colapsa dos en un menú o split button); usar `link` para algo que muta datos.

**En un dialog destructivo el botón dice el verbo** (`Eliminar 3 pedidos`), no "OK": debe ser autosuficiente sin leer el título.

### 2. Anatomía y tamaños

Escala real de shadcn/ui (Tailwind v4, `new-york`):

| Size | Alto | Padding-X | Font | Gap icono | Icono | Radio |
|---|---|---|---|---|---|---|
| `xs` | `h-6` 24px | `px-2` 8 | 12 | 4 | 12 | `rounded-md` |
| `sm` | `h-8` 32px | `px-3` 12 | 14 | 6 | 16 | `rounded-md` |
| `default` | `h-9` 36px | `px-4` 16 | 14 | 8 | 16 | `rounded-md` |
| `lg` | `h-10` 40px | `px-6` 24 | 14 | 8 | 16 | `rounded-md` |
| `icon` | `size-9` 36×36 | 0 | — | — | 16 | `rounded-md` |

Verificación de la regla `x ≈ 2–2.5× y`: `h-8` → 6 vs 12 = **2.0×**; `h-9` → 8 vs 16 = **2.0×**; `h-10` → 10 vs 24 = **2.4×**.

Detalle que casi nadie hace y shadcn sí: **cuando el botón contiene un icono, reduce el padding horizontal** (`has-[>svg]:px-3`), porque el icono ya aporta masa óptica.

**Icon-only:** siempre cuadrados; `aria-label` **obligatorio** + tooltip que diga lo mismo. Área táctil (WCAG 2.5.8 AA = 24×24; 2.5.5 AAA y Apple = 44×44): un `size-9` cumple AA pero no HIG —

```tsx
<button className="relative size-9 after:absolute after:left-1/2 after:top-1/2 after:size-11
                   after:-translate-x-1/2 after:-translate-y-1/2 after:content-['']">
```

**Ancho completo** solo en: móvil <640px, auth de una columna, dentro de cards angostos, CTA de bottom sheet. Nunca en un formulario de escritorio de 640px.

**Button groups:** `role="group"` + `aria-label`; radios compartidos y bordes colapsados con `-ml-px`. El detalle premium es `focus-visible:relative focus-visible:z-10` — sin él, el anillo de foco queda cortado por el botón vecino.

**Orden de los botones** (la respuesta real es *depende del contenedor*):

| Contexto | Orden |
|---|---|
| Dialog / modal / sheet | Secundario izquierda, **primario derecha** (Apple HIG) |
| Windows / Android nativo | Afirmativo a la izquierda |
| Página de formulario (web) | **Primario a la izquierda**, alineado con el borde izquierdo de los campos (GOV.UK) |
| Toolbar / inline | Sigue el flujo de lectura |

NN/g midió esto (*OK-Cancel or Cancel-OK?*): **no hay diferencia significativa**. Lo que importa es respetar la convención del contexto y ser consistente.

### 3. Los 7 estados de todo control

| Estado | Selector | Qué cambia exactamente |
|---|---|---|
| **default** | — | `bg-primary`, `shadow-xs`, borde si es outline |
| **hover** | `hover:` | Fondo −8/−10% de luminosidad. **Nada más**: no mover, no crecer |
| **active** | `active:` | Fondo un paso más oscuro + `scale-[0.98]` + `shadow-none`. Entrada instantánea, salida ~150ms |
| **focus-visible** | `focus-visible:` | `ring-[3px] ring-ring/50` + `border-ring`. En destructive el ring va rojo |
| **disabled** | `:disabled` / `[aria-disabled]` | `opacity-50` + `pointer-events-none` (o `cursor-not-allowed` con `aria-disabled`) |
| **loading** | `[data-loading]` | `aria-busy="true"`, spinner, **ancho congelado**, `cursor-progress`, **no** `disabled` |
| **selected/on** | `[data-state=on]`, `[aria-pressed]` | `bg-accent` + `font-medium`. **Debe ser distinguible del hover**: si son iguales, está roto |

Contraste obligatorio: texto **4.5:1**, borde/ring/estado **3:1**. El anillo de focus se mide contra **ambos** fondos: el del botón y el de la página.

### 4. Disabled es una trampa

Un botón deshabilitado sin explicación es hostil. Además **no recibe foco** con `disabled` nativo (desaparece del recorrido de teclado) y **no dispara eventos** (no puedes ni mostrar un tooltip).

| | `disabled` | `aria-disabled="true"` |
|---|---|---|
| Focusable | No | **Sí** |
| Recibe eventos | No | Sí (tú decides ignorarlos) |
| Se envía en el form | No | Sí (cuidado) |
| Estilo automático | Sí (gris del navegador) | **Ninguno, lo pintas tú** |

- **Formularios:** no deshabilites el submit por validación. Déjalo activo; al hacer click muestra los errores y mueve el foco al primero.
- **Sin permiso / prerrequisito:** `aria-disabled` + tooltip que explique *por qué* + link a resolverlo.
- **Solo tras el submit:** ahí sí — pero eso es `loading`, no `disabled`.

WCAG 1.4.3 exime a los componentes inactivos del 4.5:1, pero apunta a **≥3:1**: un `opacity-50` sobre texto que era 4.5:1 te deja en ~2.5:1, ilegible.

### 5. Loading

- **Umbral: no muestres spinner por debajo de ~300ms** — el parpadeo se percibe peor que la espera. Los tres límites de Nielsen siguen vigentes: **0.1s** instantáneo, **1s** el usuario mantiene el hilo, **10s** límite de atención (ahí necesitas progreso con porcentaje).
- **Congela el ancho.** El salto de layout al cambiar "Guardar" por un spinner es el error más común. Mantén el label con `opacity-0` y superpón el spinner en `absolute inset-0`. **Nunca** reemplaces el nodo de texto.
- **Texto honesto**: "Guardando…", "Procesando pago…" con `…` (U+2026).
- **No uses `disabled`, usa `aria-disabled` + `aria-busy`.** React Aria: *"los botones pending siguen siendo focusables, pero por lo demás están deshabilitados"*. Con `disabled`, el foco salta al `<body>` y el usuario de teclado se pierde.
- **El spinner debe estar en el árbol de accesibilidad**: ocúltalo con `opacity: 0`, nunca con `display:none`.
- **Doble envío:** bloquéalo en el handler (`if (loading) return`), no solo visualmente; y en el servidor con idempotency key si hay dinero.
- **Optimista** para acciones reversibles y de baja latencia (like, archivar, toggle) con revert + toast si falla. **Nunca** para lo irreversible o con dinero.

En React 19 sale casi gratis: `const [state, formAction, isPending] = useActionState(...)` y `useFormStatus()` dentro del `<form>`.

### 6. Inputs — anatomía

**Label SIEMPRE visible, arriba del campo.** No negociable. NN/g documenta 7 fallas del placeholder-as-label: carga la memoria de trabajo, impide revisar antes de enviar, impide recuperarse de errores, los usuarios de teclado no alcanzan a leerlo, los campos vacíos atraen la mirada y los llenos no, se confunde con datos prellenados, y el contraste bajo lo hace inaccesible. Label arriba además permite leer label+campo en **una sola fijación ocular**.

**Placeholder = ejemplo, nunca instrucción.** `placeholder="ana@empresa.com"` ✅ / `placeholder="Ingresa tu correo"` ❌.

```
Label            text-sm font-medium
  ↕ 6px
[Helper text]    text-sm text-muted-foreground   ← ANTES del input (GOV.UK)
  ↕
[Control]        h-9, border, rounded-md, px-3
  ↕
[Error]          text-sm text-destructive, role="alert"
```

Entre campos 24px; entre grupos 28px; entre secciones con separador 40–48px.

**Obligatorio vs opcional** — dos escuelas, ambas válidas:
- **GOV.UK:** *"Nunca marques los obligatorios con asterisco."* Marca los **opcionales** con "(opcional)". Funciona cuando la mayoría son requeridos.
- **Baymard (e-commerce):** marca **ambos**. En sus tests, marcando solo los opcionales, **32% de los usuarios** tuvo un error de validación por saltarse un requerido.

Regla: si >70% de los campos son obligatorios → solo "(opcional)". Si está mezclado → ambos. El asterisco solo, sin leyenda y sin `aria`, es una falla de accesibilidad.

**Contador de caracteres:** bajo el campo, a la derecha, y **no uses `maxlength` solo** — trunca en silencio. El patrón GOV.UK permite excederse y muestra "Te sobran 12 caracteres" bloqueando el submit. Anúncialo con `aria-live="polite"` solo en el último ~20%.

**El ancho del campo comunica el formato esperado.** Un input de código postal del mismo ancho que uno de dirección es la señal #1 de un formulario sin diseñar.

| Contenido | Ancho | Tailwind |
|---|---|---|
| Año, día, CVV | 3–4ch | `w-16` |
| Código postal (MX) | 6ch | `w-24` |
| Teléfono | 12ch | `w-40` |
| Ciudad, estado | 20ch | `w-64` |
| Nombre, correo, dirección | full | `w-full` |
| Cantidad monetaria | 10ch + prefijo `$` | `w-32` |

**Prefijos e iconos dentro del campo:** el addon va dentro del borde, el input transparente, y el focus ring en el **contenedor**. Detalle crítico: **el addon va después del input en el DOM** y se posiciona con CSS, para no romper el orden de tabulación.

### 7. Los atributos que casi nadie pone

| Campo | `type` | `inputmode` | `autocomplete` | `enterkeyhint` | Extras |
|---|---|---|---|---|---|
| Nombre | `text` | — | `name` / `given-name` | `next` | `autocapitalize="words"` |
| Email | `email` | `email` | `email` | `next` | `spellcheck="false"` `autocapitalize="none"` |
| Teléfono | `tel` | `tel` | `tel` | `next` | |
| Contraseña login / registro | `password` | — | `current-password` / `new-password` | `go` / `done` | |
| Código OTP | `text` | `numeric` | `one-time-code` | `done` | `pattern="[0-9]*"` |
| Cantidad / entero | `text` | `numeric` | — | `next` | **NO `type="number"`** |
| Decimal / precio | `text` | `decimal` | — | `next` | |
| Código postal | `text` | `numeric` | `postal-code` | `next` | |
| Dirección / Ciudad / Estado | `text` | — | `address-line1` / `address-level2` / `address-level1` | `next` | |
| Tarjeta | `text` | `numeric` | `cc-number`, `cc-name`, `cc-exp`, `cc-csc` | `next` | |
| Búsqueda | `search` | `search` | `off` | `search` | botón de limpiar |

Y el que se olvida siempre: **`name`**. Sin `name` el navegador no autocompleta y el form nativo no envía el dato. `autocomplete` correcto cumple **WCAG 1.3.5**.

**Por qué `type="number"` es un problema** (GOV.UK lo eliminó de su Design System): NVDA lo anuncia **sin etiqueta**; Safari **redondeaba** valores de 16+ dígitos al hacer blur (corrompía números de tarjeta); Chrome **descarta silenciosamente** las letras; y la rueda del mouse **cambia el valor** al scrollear sobre el campo enfocado. Reemplazo: `<input type="text" inputmode="numeric" pattern="[0-9]*">`.

**Password con revelar:** botón ghost icon-only dentro del campo, `aria-label` que cambia + `aria-pressed`. No bloquees pegar. **Search con limpiar:** `×` solo si hay valor, `aria-label="Limpiar búsqueda"`, y al limpiar **devuelve el foco al input** (`[&::-webkit-search-cancel-button]:appearance-none` para quitar la nativa de Safari).

**Textarea autosize sin JS** — `field-sizing: content` alcanzó Baseline en **junio de 2026**:

```css
textarea { field-sizing: content; min-height: 5rem; max-height: 20rem; }
```
Con `field-sizing: content`, `rows`/`cols` dejan de tener efecto y fijar `height` lo anula. Deja `rows` como fallback.

**Date:** el nativo sirve para fechas cercanas; para fecha de nacimiento, GOV.UK usa **tres campos separados** (día/mes/año con `inputmode="numeric"`). **File:** el `<input type="file">` debe seguir existiendo y ser focusable — el dropzone es *enhancement*. Muestra límite de tamaño y formatos **antes** de que el usuario elija.

### 8. Validación

**La regla de oro (`reward early, punish late`):**

> Si el campo estaba **válido** y lo están editando → valida **después** de la captura (blur o submit).
> Si el campo estaba **inválido** → valida **durante** la captura (cada tecla), para quitar el error en cuanto se corrija.

```ts
useForm({ mode: "onTouched", reValidateMode: "onChange" })   // React Hook Form
```

**Nunca valides mientras se escribe por primera vez.** El usuario que va en la tercera letra de su correo ya está viendo "Correo inválido" en rojo. Es la falla más común y la más irritante. **Campos vacíos: solo en submit** (alguien que entra y sale de un campo sin escribir no cometió un error todavía).

**Excepción legítima:** validación en vivo cuando el fallo es estructural y caro de descubrir después — disponibilidad de username, IBAN mal formado, política de contraseña (con checklist en vivo, no con error rojo).

```tsx
<input
  id="email"
  aria-invalid={hasError || undefined}
  aria-describedby={hasError ? "email-desc email-error" : "email-desc"}
/>
<p id="email-desc">Usaremos este correo para la factura.</p>
{hasError && <p id="email-error" role="alert">Escribe un correo con formato nombre@dominio.com</p>}
```

`aria-describedby` acepta **múltiples ids separados por espacio**. `role="alert"` para errores individuales/asíncronos — pero **si en el submit aparecen 8 errores a la vez, 8 `role="alert"` se atropellan**: ahí el anuncio lo hace el resumen.

**Resumen de errores arriba** (patrón GOV.UK, obligatorio en formularios largos): va al inicio del `<main>`, **recibe el foco** al renderizarse (`tabIndex={-1}` + `.focus()`), encabezado "Hay un problema", cada error es un **link al campo**, el texto es **idéntico** al del error en línea (distinto texto = el usuario cree que son dos errores), y prefija el `<title>` con `"Error: "`.

**Tono del mensaje** — GOV.UK es tajante: di **qué pasó y cómo arreglarlo**, en lenguaje llano. Prohibido: "campo inválido", jerga técnica, "por favor", "lo sentimos", humor. El mensaje debe **espejear el label**.

| ❌ | ✅ |
|---|---|
| "Email inválido" | "Escribe un correo con formato nombre@dominio.com" |
| "Campo requerido" | "Escribe tu RFC, lo encuentras en tu constancia fiscal" |
| "Error 422" | "No pudimos guardar los cambios. Revisa el folio e intenta de nuevo" |
| "Contraseña débil" | "Usa al menos 8 caracteres. Te faltan 3" |

Color **más** icono, nunca solo color. Mensaje pegado al campo. Ayuda extra tras el segundo intento fallido.

### 9. Formularios

- **Una sola columna.** Excepción: campos cortos y semánticamente ligados en la misma línea (Ciudad/Estado/CP, MM/AA/CVV).
- **`<fieldset>` + `<legend>` es obligatorio** para radios y checkboxes — sin él el lector de pantalla anuncia "Sí" sin decir de qué.
- **Espaciado como jerarquía:** label→control 6, entre campos 24, entre grupos 28, entre secciones 40–48.
- **Pasos vs formulario largo:** GOV.UK favorece *one thing per page* para trámites complejos y de baja frecuencia; para uso repetido por expertos, formulario largo con secciones. Regla: **>12 campos o >2 dominios distintos → pasos**.
- **Guardar borrador:** autosave con debounce ~2s + "Guardado hace un momento"; si no puedes, `sessionStorage` con aviso de recuperación.
- **El submit siempre visible:** en formularios largos, barra sticky abajo (`sticky bottom-0 border-t bg-background/80 backdrop-blur`) con el estado ("3 campos por completar").
- **Al enviar:** loading → éxito → redirección con toast, o mensaje inline persistente. **Nunca** un `alert()`, nunca un toast de 3s como única confirmación de algo irreversible.

### 10. Errores del formulario completo

- **Error de servidor (5xx / red):** mensaje a nivel de formulario con **botón de reintentar** que reenvía sin recapturar nada.
- **Errores de validación del servidor** (RFC ya registrado, cupón vencido): se mapean al campo; si no corresponden a ninguno, al resumen.
- **Regla dura #2: nunca pierdas lo capturado.** Ni al fallar, ni al recargar, ni al volver atrás. Con Server Actions, devuelve los valores en el `state` y repóblalos con `defaultValue`. Perder 20 campos por un 500 es imperdonable.
- **Prerrequisito faltante:** no muestres campos deshabilitados — muestra un empty state con la acción que desbloquea.
- **Salida con cambios sin guardar:** `beforeunload` + intercepción del router, solo si hay cambios (dirty).

### 11. Accesibilidad transversal

- **`<label htmlFor>` asociado al `id`.** Amplía el área clickeable: si tu label no enfoca al hacer click, está roto.
- **`<div onClick>` nunca sustituye a `<button>`.** Pierdes gratis: foco por Tab, Enter **y** Espacio, rol anunciado, `disabled`, submit del form, menú contextual, y la cancelación al soltar fuera. Si necesitas otro elemento, `asChild`/`Slot` de Radix.
- **`type` en los botones:** dentro de un `<form>` el default es `submit`. Todo botón que no envía **debe** llevar `type="button"`. Es el bug #1 de los formularios en React.
- **Enter dentro de un input** envía el form (nativo, deseable). En `<textarea>` usa ⌘/Ctrl+Enter y dilo en el helper.
- **Tab order = orden del DOM.** No uses `tabIndex` positivo, jamás.

### 12. Código

```tsx
// components/ui/button.tsx — variantes, estados completos y loading que no salta
"use client"
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"
import { Loader2Icon } from "lucide-react"
import { cn } from "@/lib/utils"

const buttonVariants = cva(
  [
    "relative inline-flex shrink-0 select-none items-center justify-center gap-2",
    "whitespace-nowrap rounded-md text-sm font-medium",
    "transition-[color,background-color,border-color,box-shadow,transform] duration-150 ease-out",
    "outline-none focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50",
    "active:scale-[0.98] motion-reduce:transition-none motion-reduce:active:scale-100",
    "disabled:pointer-events-none disabled:opacity-50",
    "aria-disabled:cursor-not-allowed aria-disabled:opacity-50",
    "data-[loading=true]:cursor-progress data-[loading=true]:opacity-100",
    "[&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
  ],
  {
    variants: {
      variant: {
        primary:   "bg-primary text-primary-foreground shadow-xs hover:bg-primary/90 active:bg-primary/95",
        secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        soft:      "bg-primary/10 text-primary hover:bg-primary/15 dark:bg-primary/15",
        outline:   "border border-input bg-background shadow-xs hover:bg-accent hover:text-accent-foreground",
        ghost:     "hover:bg-accent hover:text-accent-foreground",
        link:      "text-primary underline-offset-4 hover:underline focus-visible:ring-0",
        destructive: "bg-destructive text-white shadow-xs hover:bg-destructive/90 focus-visible:ring-destructive/40",
        "destructive-ghost": "text-destructive hover:bg-destructive/10 focus-visible:ring-destructive/40",
      },
      size: {
        xs:   "h-6 gap-1 rounded-sm px-2 text-xs [&_svg:not([class*='size-'])]:size-3",
        sm:   "h-8 gap-1.5 px-3 has-[>svg]:px-2.5",
        md:   "h-9 px-4 has-[>svg]:px-3",
        lg:   "h-10 px-6 has-[>svg]:px-4",
        icon: "size-9 p-0",
      },
      block: { true: "w-full", false: "" },
    },
    defaultVariants: { variant: "primary", size: "md", block: false },
  },
)

type ButtonProps = React.ComponentProps<"button"> & VariantProps<typeof buttonVariants> & {
  asChild?: boolean
  /** Estado de envío. NO usa `disabled`: el botón sigue siendo focusable. */
  loading?: boolean
  loadingLabel?: string
}

export function Button({
  className, variant, size, block, asChild = false,
  loading = false, loadingLabel = "Procesando",
  children, disabled, onClick, ...props
}: ButtonProps) {
  const Comp = asChild ? Slot : "button"
  const inert = loading || disabled

  return (
    <Comp
      data-slot="button"
      data-loading={loading ? "true" : undefined}
      aria-busy={loading || undefined}
      aria-disabled={inert || undefined}
      disabled={!asChild && disabled && !loading ? true : undefined}
      onClick={(e: React.MouseEvent<HTMLButtonElement>) => {
        if (inert) { e.preventDefault(); e.stopPropagation(); return }
        onClick?.(e)
      }}
      className={cn(buttonVariants({ variant, size, block }), className)}
      {...props}
    >
      {/* El label conserva su ancho: opacity-0, no display:none → cero layout shift */}
      <span className={cn("inline-flex items-center gap-2", loading && "opacity-0")}>{children}</span>
      {loading && (
        <span className="absolute inset-0 grid place-items-center">
          <Loader2Icon className="size-4 animate-spin" aria-hidden="true" />
        </span>
      )}
      <span aria-live="polite" className="sr-only">{loading ? loadingLabel : ""}</span>
    </Comp>
  )
}
```

```tsx
// components/ui/field.tsx — label + helper + error cableados por contexto
"use client"
import * as React from "react"
import { cn } from "@/lib/utils"

type FieldCtx = { controlId: string; descriptionId: string; errorId: string; invalid: boolean }
const FieldContext = React.createContext<FieldCtx | null>(null)
const useFieldContext = () => {
  const ctx = React.useContext(FieldContext)
  if (!ctx) throw new Error("Los subcomponentes de Field requieren un <Field> padre.")
  return ctx
}

export function Field({ invalid = false, className, children, ...props }:
  React.ComponentProps<"div"> & { invalid?: boolean }) {
  const uid = React.useId()
  const value = React.useMemo<FieldCtx>(() => ({
    controlId: `${uid}-control`, descriptionId: `${uid}-description`,
    errorId: `${uid}-error`, invalid,
  }), [uid, invalid])

  return (
    <FieldContext.Provider value={value}>
      <div role="group" data-invalid={invalid || undefined}
        className={cn("group/field flex w-full flex-col gap-1.5", className)} {...props}>
        {children}
      </div>
    </FieldContext.Provider>
  )
}

export function FieldLabel({ optional = false, className, children, ...props }:
  React.ComponentProps<"label"> & { optional?: boolean }) {
  const { controlId } = useFieldContext()
  return (
    <label htmlFor={controlId}
      className={cn("flex w-fit select-none items-center gap-1.5 text-sm font-medium",
        "group-data-[invalid]/field:text-destructive", className)} {...props}>
      {children}
      {optional && <span className="font-normal text-muted-foreground">(opcional)</span>}
    </label>
  )
}

export function FieldDescription({ className, ...props }: React.ComponentProps<"p">) {
  const { descriptionId } = useFieldContext()
  return <p id={descriptionId} className={cn("text-sm text-muted-foreground", className)} {...props} />
}

/** `role="alert"` para un solo campo. En un submit con muchos errores, `announce={false}`
 *  y deja que el resumen haga el anuncio. */
export function FieldError({ announce = true, className, children, ...props }:
  React.ComponentProps<"p"> & { announce?: boolean }) {
  const { errorId, invalid } = useFieldContext()
  if (!invalid || !children) return null
  return <p id={errorId} role={announce ? "alert" : undefined}
    className={cn("text-sm text-destructive", className)} {...props}>{children}</p>
}

export function FieldInput({ className, ...props }: React.ComponentProps<"input">) {
  const { controlId, descriptionId, errorId, invalid } = useFieldContext()
  return (
    <input
      id={controlId}
      aria-invalid={invalid || undefined}
      aria-describedby={invalid ? `${descriptionId} ${errorId}` : descriptionId}
      className={cn(
        "h-9 w-full min-w-0 rounded-md border border-input bg-transparent px-3 py-1",
        "text-base shadow-xs outline-none transition-[color,box-shadow] md:text-sm",
        "placeholder:text-muted-foreground",
        "focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50",
        "aria-invalid:border-destructive aria-invalid:ring-destructive/20",
        "disabled:cursor-not-allowed disabled:opacity-50", className,
      )}
      {...props}
    />
  )
}
```

```tsx
// Uso, con "reward early, punish late" a mano
function EmailField() {
  const [value, setValue] = React.useState("")
  const [error, setError] = React.useState<string | null>(null)
  const validate = (v: string) =>
    /^[^@\s]+@[^@\s]+\.[^@\s]{2,}$/.test(v) ? null : "Escribe un correo con formato nombre@dominio.com"

  return (
    <Field invalid={!!error}>
      <FieldLabel>Correo de facturación</FieldLabel>
      <FieldDescription>Aquí enviaremos el CFDI de cada pedido.</FieldDescription>
      <FieldInput
        name="billing_email" type="email" inputMode="email" autoComplete="email"
        enterKeyHint="next" spellCheck={false} autoCapitalize="none"
        placeholder="ana@empresa.com" value={value}
        onBlur={(e) => setError(validate(e.target.value))}                 // punish late
        onChange={(e) => { setValue(e.target.value); if (error) setError(validate(e.target.value)) }} // reward early
      />
      <FieldError>{error}</FieldError>
    </Field>
  )
}
```

### Checklist: "esto delata a un principiante"

1. Dos botones sólidos compitiendo, o "Cancelar" con más peso que "Confirmar".
2. Submit deshabilitado desde que carga, sin decir qué falta.
3. Placeholder usado como label.
4. Error rojo apareciendo en la tercera letra del correo.
5. "Este campo es inválido" como mensaje.
6. Botón que cambia de ancho al entrar en loading.
7. `<div onClick>` en vez de `<button>`.
8. `<button>` sin `type="button"` dentro de un form, recargando la página.
9. Botón icon-only sin `aria-label`.
10. `outline: none` sin anillo de reemplazo.
11. Campo de código postal del mismo ancho que el de dirección.
12. `type="number"` para un folio o una tarjeta.
13. Formulario que pierde 20 campos cuando el servidor devuelve 500.
14. Cero `autocomplete` / cero `name`.
15. Errores solo en el resumen sin marcar los campos (o al revés).

### Fuentes — botones y formularios

shadcn/ui (source y docs de Button, Input, Field, Button Group, Input Group) · Base UI *Field* · React Aria *Button* (`isPending`) · Vercel Geist *Button* · Tailwind v4 *Theme variables* · React Hook Form *useForm* · GOV.UK Design System *Error message*, *Error summary*, *Text input*, *Question pages* · GOV.UK Technology Blog *Why we changed the input type for numbers* · NN/g *Placeholders in Form Fields Are Harmful*, *How to Report Errors in Forms*, *Form Design White Space*, *Response Times: The 3 Important Limits*, *OK-Cancel or Cancel-OK?* · Baymard *Mark Both Required and Optional Fields* · Mihael Konjević *Reward early, punish late* · Smashing *A Complete Guide To Live Validation UX* · W3C *SC 2.5.8 Target Size* · MDN `autocomplete`, `enterkeyhint`, `field-sizing`, `aria-disabled` · Kitty Giraudel *On disabled & aria-disabled* · Material 3 *Buttons specs* · Apple HIG *Buttons* · Refactoring UI.


---
---

## Controles de selección y capas flotantes

Guía de referencia con specs verificadas contra Radix UI Primitives, shadcn/ui (registry `new-york-v4`), Base UI, React Aria, WAI-ARIA APG, Floating UI, MDN y WCAG 2.2. Todo lo que aparece con número está tomado de una fuente primaria o es una recomendación explícitamente marcada como tal.

---

### 1. Cuál usar cuándo — el árbol de decisión

La pregunta no es "¿qué componente se ve mejor?" sino **tres preguntas en orden**:

1. ¿El resultado se aplica de inmediato o hay que **guardar**?
2. ¿Cuántas opciones hay?
3. ¿Se elige **una** o **varias**?

```
¿Es una ACCIÓN (verbo) o un VALOR (sustantivo)?
├─ Acción ────────────────► Button / DropdownMenu / ContextMenu / CommandMenu
└─ Valor
   ├─ Booleano (on/off)
   │  ├─ se aplica YA, sin Guardar ──────────► Switch
   │  └─ vive dentro de un form con Guardar ─► Checkbox
   ├─ Una opción de N
   │  ├─ N = 2–5, todas visibles, comparables ─► RadioGroup
   │  ├─ N = 2–5, cambia la VISTA del mismo contenido ─► Segmented control
   │  ├─ N = 5–15 ─────────────────────────────► Select (listbox)
   │  └─ N > 15 o desconocido/remoto ──────────► Combobox con búsqueda
   └─ Varias opciones de N
      ├─ N ≤ 7, todas visibles ────────────────► Lista de Checkbox
      └─ N > 7 ────────────────────────────────► Multi-combobox (chips/tags)
```

**Regla dura del número de opciones** (heurística de diseño, no estándar):

| Nº de opciones | Selección única | Selección múltiple |
| --- | --- | --- |
| 1 (booleano) | Switch (inmediato) / Checkbox (con Guardar) | — |
| 2–5 | RadioGroup o Segmented control | Lista de Checkbox |
| 5–15 | Select | Lista de Checkbox con scroll, o multi-select |
| 15–50 | Combobox con filtro | Multi-combobox con chips |
| > 50 / remoto | Combobox async (debounce 200–300 ms) | Multi-combobox async |

**El error clásico: el switch con botón "Guardar".** NN/g es explícito: *"toggle switches should take immediate effect and should not require the user to click Save or Submit"*. Si el toggle vive en un formulario largo que termina en **Guardar**, el usuario no puede saber si su cambio ya se aplicó → usa **checkbox**. El switch es un interruptor de luz; el checkbox es una casilla de un formulario.

**Segmented control ≠ tabs ≠ radio.** El segmented control cambia la **presentación** del mismo contenido (Lista / Tarjetas / Mapa) y se aplica al instante; las tabs navegan a **contenido distinto**; el radio es un campo de formulario que se envía. Si al cambiar de segmento el usuario espera *la misma información en otro formato*, es segmented control; si espera *otra información*, son tabs.

**Otros errores frecuentes:**
- Select para 3 opciones → 2 clics y ocultamiento innecesario. Usa radios.
- Radios para 20 países → usa combobox.
- Checkbox único sin etiqueta afirmativa ("No enviarme correos" → doble negación). La etiqueta describe siempre lo que ocurre cuando está **activo**.
- Dropdown menu usado como select (guarda un valor). Son componentes distintos, ver §5.

---

### 2. Checkbox y radio

#### Specs

| Propiedad | Valor recomendado | Fuente / nota |
| --- | --- | --- |
| Caja del control | **16px** (denso, tipo Linear) · **18px** (default) · **20px** (táctil) | M3 usa 18dp de contenedor + 18dp de icono |
| Radio de esquina (checkbox) | 4px (M3: 2dp); radio = 50% | shadcn: `rounded-[4px]` |
| Grosor del check | 2px, `stroke-linecap: round` | — |
| Hit area real | **≥ 24×24px** (WCAG 2.2 SC 2.5.8, AA) · **44×44px** ideal (SC 2.5.5, AAA) · 48dp en Android | W3C / Material 3 |
| Separación entre targets | ≥ 8px | M3 |
| Gap control ↔ label | 8px (denso) – 12px | — |
| Alto de línea del label | 20px para texto de 14px | — |
| Ring de foco | 3px con 50% de opacidad | shadcn v4: `focus-visible:ring-[3px] ring-ring/50` |

**Hit area invisible.** El cuadro de 16px NO es el target. WCAG 2.2 SC 2.5.8 exige 24×24 CSS px (con excepción de espaciado); SC 2.5.5 pide 44×44. Como el `<label>` clicable ya suele dar altura suficiente, el mínimo real que debes garantizar es **la fila completa clicable**:

```tsx
// El label es el hit area. Nunca un checkbox suelto sin label asociado.
<label className="flex min-h-11 items-start gap-3 py-2 cursor-pointer select-none">
  <Checkbox id="tos" className="mt-0.5 size-4" />
  <span className="grid gap-0.5">
    <span className="text-sm leading-5 font-medium">Recibir notificaciones</span>
    <span className="text-sm leading-5 text-muted-foreground">
      Te avisamos cuando cambie el estado de un envío.
    </span>
  </span>
</label>
```

**Alineación con la primera línea de texto.** Este es el detalle que separa lo pulido de lo amateur. El control **nunca** se centra verticalmente respecto al bloque de texto: se alinea ópticamente con la **primera línea**. Con `items-start`, un control de 16px y una línea de 20px, el offset es `(20 − 16) / 2 = 2px` → `mt-0.5`. Si el control es de 20px sobre línea de 20px, `mt-0`.

**Estado indeterminate.** Solo tiene un uso legítimo: **el checkbox padre de un grupo parcialmente seleccionado** ("Seleccionar todo" con 3 de 7 marcados). No es un tercer estado que el usuario pueda elegir con clic: al hacer clic sobre un padre indeterminado, marca **todos**. Radix lo modela como `checked: boolean | 'indeterminate'` y expone `data-state="indeterminate"`.

```tsx
const allChecked = items.every(i => i.checked)
const someChecked = items.some(i => i.checked)

<Checkbox
  checked={allChecked ? true : someChecked ? "indeterminate" : false}
  onCheckedChange={(v) => setAll(v === true)}
  aria-label="Seleccionar todo"
/>
// Estiliza el guion con: data-[state=indeterminate]:bg-primary
```

**Grupos y `fieldset`/`legend`.** Un grupo de radios o de checkboxes relacionados necesita un nombre accesible de grupo. Dos formas válidas:

```html
<!-- Nativo, gratis, sin JS -->
<fieldset>
  <legend class="text-sm font-medium">Modalidad de envío</legend>
  <!-- radios -->
</fieldset>
```

```tsx
{/* Radix: el Root ya lleva role="radiogroup" */}
<div role="group" aria-labelledby="ship-label">
  <span id="ship-label" className="text-sm font-medium">Modalidad de envío</span>
  <RadioGroup defaultValue="air"> … </RadioGroup>
</div>
```

**Teclado esperado**

| Control | Teclado |
| --- | --- |
| Checkbox | `Tab` entra/sale · `Space` alterna. **Enter no hace nada** (salvo que envíes el form) |
| RadioGroup | `Tab` entra al radio seleccionado (o al primero) y sale del grupo entero · `↑↓←→` mueven **y seleccionan** (roving tabindex) · `Space` selecciona el enfocado |

Un grupo de radios es **una sola parada de tab**. Si tu implementación hace que Tab recorra los 5 radios, está mal.

**`accent-color` nativo vs control custom.** `accent-color` tiene soporte amplio hoy (Chrome, Edge, Firefox, y Safari ya lo soporta), y conserva gratis: comportamiento nativo, estados de foco del SO, accesibilidad, alto contraste y modo forzado. Pero **solo cambia el color** y el dibujo del check varía entre navegadores y sistemas operativos.

```css
/* Vale la pena cuando: prototipo, admin interno, forms densos, o el checkbox
   no es parte de la identidad visual del producto. Coste: 1 línea. */
:root { accent-color: var(--color-primary); }
```

Ve a control custom (Radix/shadcn) cuando necesites: **radio de esquina propio, animación del check, estado indeterminate estilizado, ring de foco de tu design system, o tamaño distinto al del SO**. Es el caso de cualquier producto que aspire al acabado de Linear o Stripe. El coste es que ahora **tú** eres responsable del foco, del `aria-checked` y del hit area.

---

### 3. Switch / toggle

**Regla número uno: se aplica de INMEDIATO.** Sin botón Guardar, sin confirmación. Si la operación es remota, el switch se mueve al instante (optimistic UI) y el sistema reconcilia después.

#### Specs

| Tamaño | Track (w × h) | Knob | Recorrido | Padding interno |
| --- | --- | --- | --- | --- |
| sm | 36 × 20px | 16px | 16px | 2px |
| **default** | 44 × 24px | 20px | 20px | 2px |
| lg | 52 × 32px | 28px | 20px | 2px |

Referencia real de shadcn v4 (más compacta, estilo Linear): root `data-[size=default]:h-[1.15rem] data-[size=default]:w-8` (≈18.4 × 32px) con thumb `size-4` y `data-[state=checked]:translate-x-[calc(100%-2px)]`. Material 3 va al extremo opuesto: track ~52dp de ancho y el handle **crece** de 16dp (off) a 24dp (on) y 28dp al presionar.

El truco del recorrido: `translate-x-[calc(100%-2px)]` en lugar de un valor fijo hace que el switch sea **agnóstico al tamaño** — cambias el ancho del track y el knob sigue cayendo bien.

```tsx
<div className="flex items-center justify-between gap-4 py-3">
  <div className="grid gap-0.5">
    <Label htmlFor="2fa" className="text-sm font-medium">
      Verificación en dos pasos
    </Label>
    <p className="text-sm text-muted-foreground">
      Pediremos un código adicional al iniciar sesión.
    </p>
  </div>
  <Switch id="2fa" checked={on} onCheckedChange={apply} />
</div>
```

**Layout canónico:** label a la **izquierda**, switch a la **derecha**, alineados con `justify-between`. Es la convención de iOS, macOS, Android y de todo panel de ajustes moderno. El label descriptivo (con descripción secundaria) ocupa el flujo natural de lectura y el estado se escanea en una columna vertical limpia.

**Por qué no lleva texto "ON/OFF" dentro.** Tres razones: (a) obliga a un track más ancho y desalinea la columna de switches; (b) es ambiguo — ¿el texto visible es el estado actual o la acción a ejecutar?; (c) no se internacionaliza (en alemán *"Eingeschaltet"* no cabe). El estado se comunica por **posición del knob + color del track**, y ambos deben cambiar (NN/g: usa colores de alto contraste y el switch debe **moverse** visiblemente). Nunca dependas solo del color: el knob desplazado es la señal redundante que salva a usuarios daltónicos.

**La etiqueta describe el estado ACTIVO.** Test de NN/g: di la etiqueta en voz alta seguida de "activado". *"Verificación en dos pasos activada"* ✓. *"¿Notificaciones?" activada* ✗. Nada de preguntas, nada de etiquetas neutras.

**Si la acción puede fallar** — este es el detalle que casi nadie implementa:

```tsx
async function apply(next: boolean) {
  setOn(next)                              // 1. optimista, inmediato
  try {
    await updateSetting({ twoFactor: next })
  } catch (e) {
    setOn(!next)                           // 2. revertir al estado anterior
    toast.error("No se pudo guardar el ajuste", {   // 3. avisar por qué
      description: "Revisa tu conexión e inténtalo de nuevo.",
      action: { label: "Reintentar", onClick: () => apply(next) },
    })
  }
}
```

No pongas un spinner **dentro** del switch: rompe la ilusión de inmediatez. Si la operación tarda > 1 s de forma sistemática, el switch es el componente equivocado; usa un botón con estado de carga.

---

### 4. Select nativo vs custom

#### Qué gana el nativo `<select>`

- **Móvil**: iOS abre el picker de rueda; Android su hoja nativa. Ningún listbox custom iguala esa ergonomía a una mano.
- **Accesibilidad**: role, estados y anuncios correctos por definición; funciona con VoiceOver rotor, Dragon, switch control.
- **Teclado gratis**: flechas, Home/End, typeahead multicarácter, Alt+↓ para abrir, Escape.
- **Cero JS**, cero bundle, cero bugs de posicionamiento, renderiza en el top layer del SO (jamás lo corta un `overflow: hidden`).
- Se envía solo en un `<form>`, incluso sin JavaScript.

#### Qué obliga a ir a custom

Búsqueda/filtro · opciones ricas (avatar, badge, dos líneas, iconos) · selección múltiple con chips · grupos con estilo · carga asíncrona · opción "Crear «…»" · virtualización de miles de filas · animación de apertura acorde al design system.

#### Estilar un `<select>` en 2026: `appearance: base-select`

La API de *customizable select* llegó estable en **Chrome/Edge 135**; Firefox y Safari van en camino pero **aún no envían**. Es progressive enhancement puro: el navegador que no entiende la regla renderiza un `<select>` normal y funcional.

```html
<select class="cs">
  <button>
    <selectedcontent></selectedcontent>
    <span class="chev" aria-hidden="true">▾</span>
  </button>
  <option value="air"><img src="/air.svg" alt=""> Aéreo <small>2–3 días</small></option>
  <option value="sea"><img src="/sea.svg" alt=""> Marítimo <small>18–25 días</small></option>
</select>
```

```css
.cs, .cs::picker(select) { appearance: base-select; }

.cs {
  display: flex; align-items: center; gap: .5rem;
  height: 2.25rem; padding: 0 .75rem;
  border: 1px solid var(--color-border); border-radius: .5rem;
  background: var(--color-background); font-size: .875rem;
}
.cs::picker(select) {
  border: 1px solid var(--color-border); border-radius: .5rem;
  padding: .25rem; box-shadow: var(--shadow-md);
  /* animación de entrada: allow-discrete + @starting-style */
}
.cs option { padding: .375rem .5rem; border-radius: .25rem; }
.cs option:checked::before { content: "✓"; }
```

**Recomendación 2026:** usa `base-select` para lo que hoy resolverías con un `<select>` feo (filtros de admin, forms internos) — mejora en Chromium sin coste. Para el componente de producto que debe verse idéntico en todos los navegadores, sigue con Radix/Base UI hasta que Firefox y Safari envíen.

#### El patrón listbox accesible completo

Radix Select "adheres to the ListBox WAI-ARIA design pattern". Lo que eso implica, según APG:

| Tecla | Comportamiento |
| --- | --- |
| `Space` / `Enter` / `↓` / `↑` / `Alt+↓` | Abre el listbox desde el trigger |
| `↓` / `↑` | Mueve el foco al siguiente/anterior option; en single-select **la selección sigue al foco** |
| `Home` / `End` | Primer / último option (APG: *recomendado en listas de más de cinco opciones*) |
| Caracteres imprimibles | **Typeahead**: salta al primer option cuya etiqueta empieza por lo tecleado (buffer que se reinicia a ~500 ms) |
| `Enter` / `Space` | Confirma el option enfocado y cierra |
| `Esc` | Cierra sin cambiar el valor y **devuelve el foco al trigger** |
| `Tab` | Cierra y sale (comportamiento nativo) |
| Multi: `Shift+↓/↑`, `Shift+Space`, `Ctrl+A` | Extiende / selecciona rango / selecciona todo |

ARIA mínimo: `role="listbox"` en el contenedor, `role="option"` en cada ítem, `aria-selected` (o `aria-checked`, pero **nunca los dos**), `aria-multiselectable="true"` si aplica, y `aria-activedescendant` en el elemento que conserva el foco DOM. Añade `aria-setsize`/`aria-posinset` si cargas dinámicamente.

**Cuándo añadir búsqueda:** a partir de **~10–15 opciones**, o siempre que el usuario ya sepa qué busca (países, monedas, usuarios, aeropuertos). Por debajo de 10, el typeahead nativo del listbox basta y un input de búsqueda solo añade ruido. Al añadir búsqueda ya no es un select: es un **combobox** (`role="combobox"` + `aria-expanded` + `aria-controls` + `aria-autocomplete="list"`), y su `Escape` primero limpia/cierra el popup, no el valor.

**Detalle de posicionamiento del select:** Radix Select usa `position="item-aligned"` por defecto — coloca el popup de forma que el **ítem seleccionado quede sobre el trigger**, como los menús de macOS. Base UI hace lo mismo con `alignItemWithTrigger` (default `true`), y lo desactiva automáticamente en touch o si no hay viewport suficiente. Si prefieres el comportamiento "dropdown" clásico, pasa `position="popper"` (side `bottom`, sideOffset `0`, align `start`).

---

### 5. Dropdown menu ≠ select

| | DropdownMenu | Select |
| --- | --- | --- |
| Contiene | **Acciones** (verbos): Duplicar, Exportar, Eliminar | **Valores** (sustantivos): Aéreo, Marítimo |
| Estado | No guarda estado; ejecuta y cierra | Guarda un valor persistente |
| Trigger | Muestra un nombre fijo ("Acciones", "⋯") | Muestra **el valor actual** |
| Roles | `menu` / `menuitem` | `listbox` / `option` |
| Se envía en un form | No | Sí |
| Análogo Apple | Pull-down button | Pop-up button |

Si tu "dropdown" cambia lo que dice su propio trigger, es un select. Si al elegir un ítem pasa algo, es un menú.

#### Anatomía y specs

| Elemento | Valor |
| --- | --- |
| Padding del item | **6–8px vertical / 8–10px horizontal** (shadcn: `px-2 py-1.5` = 8/6px) |
| Altura del item | **32–36px** (32px con texto 14px/20px + 6px de padding) |
| Padding del contenedor | 4px (`p-1`) — permite el hover redondeado con aire |
| Radio: contenedor / item | 8px (`rounded-md`) / 4px (`rounded-sm`) |
| Ancho mínimo | **8rem = 128px** (shadcn: `min-w-[8rem]`); o `--radix-dropdown-menu-trigger-width` si debe igualar al trigger |
| Ancho máximo | 280–320px; trunca con `truncate` |
| Separador | 1px, `-mx-1 my-1` (sangra hasta el borde del panel) |
| Label de grupo | `text-xs font-medium text-muted-foreground`, mismo padding horizontal que el item |
| Icono | 16px (`size-4`), gap 8px, color `text-muted-foreground` |
| Atajo (derecha) | `ml-auto text-xs tracking-widest text-muted-foreground` |
| Sombra | `shadow-md` en el panel raíz, `shadow-lg` en submenús |
| Altura máxima | `max-h-(--radix-dropdown-menu-content-available-height)` + `overflow-y-auto` |

```tsx
<DropdownMenuContent
  align="end"
  sideOffset={4}
  collisionPadding={8}
  className="z-50 min-w-[8rem] max-h-(--radix-dropdown-menu-content-available-height)
             origin-(--radix-dropdown-menu-content-transform-origin)
             overflow-y-auto overflow-x-hidden rounded-md border bg-popover p-1
             text-popover-foreground shadow-md"
>
  <DropdownMenuLabel>Envío GBX-26-00184</DropdownMenuLabel>
  <DropdownMenuItem>
    <Pencil /> Editar <DropdownMenuShortcut>⌘E</DropdownMenuShortcut>
  </DropdownMenuItem>
  <DropdownMenuSub>
    <DropdownMenuSubTrigger><Share2 /> Compartir</DropdownMenuSubTrigger>
    <DropdownMenuSubContent>…</DropdownMenuSubContent>
  </DropdownMenuSub>
  <DropdownMenuSeparator />
  <DropdownMenuItem variant="destructive">
    <Trash2 /> Eliminar
  </DropdownMenuItem>
</DropdownMenuContent>
```

**Reglas duras del menú:**
- Iconos **todos o ninguno** dentro de un mismo grupo. Un icono suelto rompe la columna óptica.
- Si hay ítems con checkmark (`menuitemcheckbox`), reserva el carril izquierdo para **todos** los ítems del grupo → shadcn lo hace con `data-[inset]:pl-8`.
- El **item destructivo va al final**, después de un separador, en rojo. shadcn: `data-[variant=destructive]` tiñe texto e icono y usa `focus:bg-destructive/10`.
- Máximo **~2 niveles** de submenú. Tres niveles es un árbol, no un menú.
- Atajos alineados a la derecha con `ml-auto`; usa `tracking-widest` para que `⌘⇧K` respire.

**El "safe triangle" / prediction cone.** Al mover el ratón en diagonal desde un `SubTrigger` hacia el submenú, el cursor pasa por encima de otros ítems del menú padre y el submenú se cierra. Radix ya lo resuelve: mantiene una **zona de gracia triangular** entre el puntero y las esquinas del submenú, y mientras el cursor esté dentro, ningún otro ítem se activa. Si construyes tu propio menú, o implementas el triángulo, o pon un **delay de cierre de 150–300 ms**. El delay es el fallback pobre; el triángulo es lo correcto.

**Teclado (APG Menu + Menu Button, coincide con Radix):**

| Tecla | Comportamiento |
| --- | --- |
| `Enter` / `Space` / `↓` en el trigger | Abre y enfoca el **primer** ítem |
| `↑` en el trigger | Abre y enfoca el **último** ítem |
| `↓` / `↑` | Ítem siguiente / anterior (con wrap) |
| `Home` / `End` | Primer / último ítem |
| Caracteres | Typeahead sobre las etiquetas |
| `→` sobre un SubTrigger | Abre el submenú y enfoca su primer ítem |
| `←` dentro de un submenú | Cierra el submenú, foco al `SubTrigger` padre |
| `Enter` | Activa el ítem y cierra el menú |
| `Space` sobre `menuitemcheckbox` | Alterna **sin cerrar** el menú |
| `Esc` | Cierra el menú y **devuelve el foco al trigger** |

Los ítems deshabilitados llevan `aria-disabled="true"` y **siguen siendo enfocables** (para que el lector de pantalla los anuncie), no se saltan.

---

### 6. Posicionamiento de capas flotantes

Todo popup se ancla al trigger. La ecuación es siempre la misma: `placement` deseado → detectar overflow → `flip` (voltear al lado opuesto) → `shift` (deslizar en el eje transversal) → `size` (recortar altura).

**Orden del middleware en Floating UI** — no es cosmético, cada uno consume el resultado del anterior:

```
offset() → flip() / autoPlacement() → shift() → size() → arrow() → hide()
```

Documentación oficial: *"`offset()` should always go at the beginning of the middleware array, while `arrow()` and `hide()` at the end."*

**Defaults de Floating UI que conviene conocer:**

| Middleware | Opción | Default |
| --- | --- | --- |
| `flip` | `mainAxis` / `crossAxis` | `true` / `true` |
| `flip` | `fallbackAxisSideDirection` | `'none'` |
| `flip` | `fallbackPlacements` | `[placement opuesto]` |
| `flip` | `fallbackStrategy` | `'bestFit'` |
| `shift` | `mainAxis` | `true` |
| `shift` | `crossAxis` | `false` (activarlo puede solapar el trigger) |
| `shift` | `limiter` | ninguno → usa `limitShift()` para que no se despegue |
| ambos | `padding` | `0` → **pon 8** |

**Defaults de Radix** (que envuelve Floating UI): `sideOffset: 0`, `align: "center"` (menús/popovers) o `"start"` (select popper), `alignOffset: 0`, `collisionPadding: 0`, `avoidCollisions: true`, `sticky: "partial"`, `hideWhenDetached: false`.

**Valores que debes cambiar siempre:**

```tsx
sideOffset={4}          // menús y selects pegados al trigger
sideOffset={8}          // popovers y tooltips con más respiro
collisionPadding={8}    // nunca dejes que toque el borde del viewport
```

**`transform-origin` apuntando al trigger.** Sin esto, la animación de `zoom-in-95` crece desde el centro del panel y se siente desconectada del botón. Radix calcula el origen exacto y lo expone como CSS var; en Tailwind v4 se consume con la sintaxis de paréntesis:

```
origin-(--radix-dropdown-menu-content-transform-origin)
```

Combinado con el desplazamiento direccional que ya trae shadcn:

```
data-[side=bottom]:slide-in-from-top-2  data-[side=top]:slide-in-from-bottom-2
data-[side=left]:slide-in-from-right-2  data-[side=right]:slide-in-from-left-2
data-[state=open]:animate-in  data-[state=open]:fade-in-0  data-[state=open]:zoom-in-95
data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95
```

El truco: el panel entra **desde el lado del trigger**. Si abre hacia abajo, entra desde arriba. Radix escribe `data-side` tras resolver el flip, así que la animación se corrige sola cuando el menú voltea.

**Scroll dentro del menú.** Nunca uses `max-h-[300px]` fijo en un menú anclado: si el trigger está a 200px del borde inferior, el menú se sale. Usa la altura disponible calculada:

```
max-h-(--radix-dropdown-menu-content-available-height) overflow-y-auto
```

**Ancho igual al trigger — ¿sí o no?**
- **Select / combobox: SÍ.** El popup es la continuación visual del campo. `w-(--radix-select-trigger-width)` o `min-w-(--radix-popover-trigger-width)`.
- **Dropdown menu: NO.** El menú se dimensiona por su contenido (`min-w-[8rem]`), porque un botón "⋯" de 32px no puede dictar el ancho de "Exportar como CSV".

**Portales y stacking context.** Renderiza siempre en portal (`<DropdownMenu.Portal>`). Un menú dentro de un ancestro con `overflow: hidden`, `transform`, `filter` o `contain` queda recortado o atrapado en un stacking context del que `z-index` no lo salva. Con portal + `z-50` es suficiente; el problema real casi nunca es el z-index, es el `transform` del abuelo.

**Estado 2026: CSS Anchor Positioning + `popover`.**

| Pieza | Estado |
| --- | --- |
| `popover` attribute (`auto` / `manual` / `hint`), `popovertarget`, light dismiss, top layer, `::backdrop` | **Baseline desde enero 2025** — usable ya |
| `<dialog>` + `showModal()` | Baseline widely available desde marzo 2022 |
| `anchor-name` / `position-anchor` / `anchor()` | Chrome/Edge 125+, **Firefox 147 (enero 2026)**, Safari con soporte parcial desde 18.x |
| `position-area`, `@position-try`, `position-try-fallbacks`, `position-visibility`, `anchor-size()`, `anchor-scope` | Chromium completo; Safari incompleto en 18.x |

```css
.trigger { anchor-name: --trg; }
.panel {
  position: fixed;
  position-anchor: --trg;
  position-area: block-end span-inline-start;   /* abajo, alineado al inicio */
  margin-block-start: 4px;
  min-width: anchor-size(width);                 /* ancho ≥ trigger */
  position-try-fallbacks: flip-block, flip-inline;
  position-visibility: anchors-visible;          /* se oculta si el ancla scrollea fuera */
}
```

**Recomendación práctica 2026:** `popover` + `<dialog>` ya son producción para *light dismiss* y *top layer* (dos de los tres problemas difíciles). El **anclaje** sigue siendo el eslabón débil por Safari, así que **Floating UI / Radix continúa siendo el default de producción**, y anchor positioning entra como progressive enhancement dentro de `@supports (anchor-name: --x)`. Lo que sí debes adoptar hoy sin excusa: **top layer** en lugar de `z-index: 9999`.

---

### 7. Dialog / modal

**Cuándo un modal es la respuesta correcta** (NN/g):
- Prevenir un error irreversible o pérdida de datos (confirmación destructiva, cambios sin guardar).
- Pedir información crítica sin la cual el proceso no puede continuar.
- Partir un flujo complejo en pasos, **con indicador de progreso**.
- Avisar de un estado del sistema que exige acción inmediata.

**Cuándo NO** — la mayoría de las veces:
- Información no esencial o ajena a la tarea en curso.
- Procesos de alto valor (checkout): un modal encima de un carrito es una fuga de conversión.
- Decisiones que requieren datos que están **fuera** del modal (el usuario no puede ir a buscarlos sin cerrarlo → poner esos datos dentro, o usar una página).
- Marketing (newsletter, cookies con oscurecimiento total).
- **Formularios largos** → página dedicada o drawer.
- Errores de validación de un campo → mensaje inline.

Coste real del modal según NN/g: interrumpe el flujo, obliga a recordar el contexto anterior, tapa información de fondo que quizá se necesita para responder, y exige una acción obligatoria para salir.

#### Tamaños

| Token | max-width | Uso |
| --- | --- | --- |
| `sm` | **400px** | Confirmaciones, 1–2 campos |
| `md` | **512px** | Default. Formulario corto (`sm:max-w-lg` de shadcn = 512px) |
| `lg` | **640px** | Formulario con dos columnas |
| `xl` | **800px** | Tablas, previsualizaciones, editores |
| `full` | `calc(100vw - 2rem)` | Móvil, o considera drawer |

Ancho responsivo obligatorio: `w-full max-w-[calc(100%-2rem)] sm:max-w-lg` — nunca un `max-w` fijo sin la válvula de escape de móvil. Alto máximo: `max-h-[calc(100dvh-4rem)]` con `100dvh` (no `vh`) para no pelear con la barra de Safari iOS.

#### Anatomía

```tsx
<DialogContent className="grid max-h-[calc(100dvh-4rem)] grid-rows-[auto_1fr_auto]
                          gap-4 p-6 sm:max-w-lg">
  <DialogHeader>
    <DialogTitle>Publicar cotización</DialogTitle>       {/* OBLIGATORIO */}
    <DialogDescription>
      El cliente recibirá un correo con el desglose de costos.
    </DialogDescription>
  </DialogHeader>

  <div className="-mx-6 overflow-y-auto px-6">           {/* scroll INTERNO */}
    …
  </div>

  <DialogFooter className="flex flex-col-reverse gap-2 sm:flex-row sm:justify-end">
    <DialogClose asChild><Button variant="outline">Cancelar</Button></DialogClose>
    <Button>Publicar</Button>
  </DialogFooter>
</DialogContent>
```

Cuatro detalles que casi nadie hace bien:
1. **`grid-rows-[auto_1fr_auto]`**: header y footer fijos, el cuerpo scrollea. Sin esto, el scroll se lleva el título y el usuario pierde el contexto.
2. **`-mx-6 px-6`** en el área scrollable: el scrollbar queda pegado al borde del modal, no flotando dentro del padding.
3. **`flex-col-reverse` en móvil**: la acción primaria queda **arriba** en la pila vertical (donde cae el pulgar), y a la derecha en desktop.
4. **`DialogTitle` es obligatorio** — Radix lo exige para el nombre accesible y avisa en consola si falta. Si el diseño no lleva título visible, envuélvelo en un `VisuallyHidden`, no lo elimines.

#### Gestión del foco

| Momento | Comportamiento |
| --- | --- |
| Al abrir | Foco al **primer elemento interactivo**; si el contenido es largo, semántico o scrollea, foco a un elemento estático al inicio (título) con `tabindex="-1"` |
| Acción destructiva | APG: enfoca la **acción menos destructiva** (Cancelar) si deshacer es difícil o imposible |
| Durante | `Tab` / `Shift+Tab` **ciclan dentro** del diálogo (focus trap); nada fuera es alcanzable |
| `Esc` | Cierra el diálogo |
| Al cerrar | El foco **vuelve al trigger** que lo abrió (Radix lo hace solo; en `<dialog>` nativo lo tienes que hacer tú si el trigger se desmontó) |

ARIA: `role="dialog"` + `aria-modal="true"` + `aria-labelledby` (o `aria-label`) + `aria-describedby` opcional. APG advierte: marca `aria-modal` **solo** si de verdad el resto es inaccesible e inerte; omite `aria-describedby` cuando el contenido tiene estructura semántica que hay que recorrer.

**Click fuera: cuándo NO debe cerrar.** Si hay cambios sin guardar o el usuario lleva rato escribiendo, cerrar por click accidental es destrucción de trabajo. Intercepta:

```tsx
<DialogContent
  onPointerDownOutside={(e) => { if (isDirty) e.preventDefault() }}
  onEscapeKeyDown={(e) => { if (isDirty) { e.preventDefault(); setConfirmClose(true) } }}
>
```

Regla: **click fuera cierra por defecto**; se bloquea solo con estado sucio, y entonces Escape abre una confirmación en vez de cerrar en seco. `Esc` nunca debe quedar completamente muerto (es la vía de escape de teclado).

**Scroll lock sin salto de layout.** Bloquear el body con `overflow: hidden` elimina el scrollbar y todo el contenido salta ~15px a la derecha. La solución 2026 es CSS puro:

```css
html { scrollbar-gutter: stable; }   /* Chromium 94+, Firefox 97+, Safari 18.2+ */
```

Reserva el carril del scrollbar permanentemente, así que bloquear el body no reflowa nada. En macOS con overlay scrollbars la propiedad no hace nada — pero tampoco hay salto. El viejo hack de `padding-right: ${scrollbarWidth}px` sigue siendo el fallback para navegadores antiguos.

**Anidamiento de modales: evítalo.** Rompe el retorno de foco, apila backdrops que oscurecen hasta lo ilegible, deja al usuario sin saber cuántos Escape faltan y en `<dialog>` nativo cada Escape cierra solo el más reciente (correcto, pero desconcertante). Las dos únicas excepciones tolerables: (a) confirmación de descarte sobre un form sucio; (b) un `AlertDialog` de confirmación destructiva. En ambos casos el segundo nivel es **pequeño, sin scroll y de una sola decisión**. Todo lo demás: navega dentro del mismo modal con pasos, o usa una página.

**`<dialog>` nativo vs Radix en 2026.** El nativo ya te da top layer, `::backdrop`, inertización automática del resto, focus trap del navegador, Escape, y `closedby="any"` para light dismiss. Lo que sigue sin darte: animación de entrada/salida sin `allow-discrete` + `@starting-style`, scroll lock del body, retorno de foco si el trigger desapareció, composición con otros componentes controlados, y SSR sin parpadeo. **Radix/shadcn sigue siendo el default en React**; el nativo es excelente para diálogos aislados en HTML sin framework, y su motor (top layer + inert) es lo que deberías estar usando por debajo.

**Confirmación destructiva.** El botón dice **la acción**, no "OK":

```tsx
<AlertDialogContent className="sm:max-w-[400px]">
  <AlertDialogTitle>¿Eliminar 3 entradas?</AlertDialogTitle>
  <AlertDialogDescription>
    Se eliminarán de forma permanente los folios GBX-26-00184, 00185 y 00186.
    Esta acción no se puede deshacer.
  </AlertDialogDescription>
  <AlertDialogFooter>
    <AlertDialogCancel>Cancelar</AlertDialogCancel>
    <AlertDialogAction className="bg-destructive text-white">
      Eliminar entradas
    </AlertDialogAction>
  </AlertDialogFooter>
</AlertDialogContent>
```

Checklist: `role="alertdialog"` · el título nombra el objeto y la cantidad · la descripción dice qué se pierde y si es reversible · el botón repite el verbo ("Eliminar entradas", nunca "OK"/"Sí") · el foco inicial va a **Cancelar** · sin click-fuera para cerrar. Y antes de todo esto pregúntate si no bastaba con **deshacer** (§10).

---

### 8. Drawer / sheet

**Cuándo sustituye al modal:**
- **Móvil**: casi siempre. Un modal centrado en un teléfono es un drawer mal implementado. El bottom sheet cae en la zona del pulgar y se cierra arrastrando.
- **Contenido largo** con scroll propio (filtros, detalle de registro, formulario de 10 campos).
- **Flujo lateral** que acompaña a la vista principal sin reemplazarla (panel de detalle junto a una tabla, historial de actividad).
- **Multi-paso** donde el usuario navega hacia dentro y hacia atrás.

Sigue con modal cuando la decisión es **binaria y corta** (confirmar/cancelar): un drawer para "¿Eliminar?" es teatro innecesario.

| Lado | Uso | Tamaño |
| --- | --- | --- |
| `right` | Detalle, edición, filtros (desktop) | `w-3/4 sm:max-w-sm` (384px); 480–560px para formularios |
| `left` | Navegación, árbol | 280–320px |
| `bottom` | **Móvil**: acciones, pickers, filtros | `h-auto`, máx `85dvh` |
| `top` | Búsqueda, banners de sistema | `h-auto` |

Referencia shadcn (`sheet.tsx`): `inset-y-0 right-0 h-full w-3/4 border-l sm:max-w-sm`, con `slide-in-from-right` / `slide-out-to-right`. Durations: **500ms al abrir, 300ms al cerrar** — la asimetría es intencional; salir siempre más rápido que entrar.

**Gesto de arrastre y snap points.** En móvil el drawer debe cerrarse arrastrando hacia abajo. Vaul (construido sobre Radix Dialog) es el estándar de facto en React: física de resorte, cierre por **velocidad** (un flick rápido cierra aunque no llegue al umbral) y snap points para bottom sheets tipo mapa.

```tsx
<Drawer snapPoints={[0.4, 0.9]} activeSnapPoint={snap} setActiveSnapPoint={setSnap}>
  <DrawerContent className="max-h-[85dvh] pb-[env(safe-area-inset-bottom)]">
    <div className="mx-auto mt-3 h-1 w-10 rounded-full bg-muted" /> {/* grabber */}
    …
  </DrawerContent>
</Drawer>
```

Reglas: expón siempre el **grabber** (barrita de 40×4px, 12px del borde) — es la única señal de que se puede arrastrar; máximo **2–3 snap points**; el drag debe ceder al scroll interno cuando el contenido ya está scrolleado hacia abajo (bug clásico: el drawer se arrastra en vez de scrollear).

**Safe areas.** En iOS el indicador home come ~34px. Todo drawer inferior necesita `padding-bottom: env(safe-area-inset-bottom)`, y su footer de acciones también (si no, el botón primario queda debajo de la barra del sistema). Requiere `viewport-fit=cover` en el meta viewport. Aviso: Safari devuelve `0px` para `env(safe-area-inset-bottom)` en algunos estados con la toolbar oculta — no bases un layout crítico solo en ese valor; usa `max(env(safe-area-inset-bottom), 12px)`.

---

### 9. Tooltip

**Regla número uno: información complementaria, nunca esencial.** NN/g: *"instrucciones u otra información directamente accionable, como los requisitos de un campo, no deberían estar en un tooltip"*. Si el usuario **necesita** leerlo para completar la tarea, va como texto visible o como helper text bajo el campo.

**Regla número dos: nunca contenido interactivo.** Sin links, sin botones, sin inputs. `role="tooltip"` no es focusable ni navegable con lector de pantalla como región. Si tienes un link dentro, necesitas un **Popover** o un **HoverCard**.

**Regla número tres: no existe en touch.** Los tooltips se abren con `hover` y `focus`; en un teléfono no hay ninguno de los dos. Radix lo confirma en la práctica (issues #1573, #2278, #2589: no abre al tocar en iOS). Qué hacer:

| Situación | Solución en touch |
| --- | --- |
| Icon button sin etiqueta | `aria-label` obligatorio + **etiqueta visible** en el layout móvil |
| Explicación de un campo | Helper text permanente bajo el label |
| Ayuda contextual "?" | **Toggletip**: botón que abre un Popover al **click** (funciona en todos los dispositivos) |

```tsx
{/* Toggletip: el patrón correcto para ayuda contextual */}
<Popover>
  <PopoverTrigger aria-label="Qué es el mBOL"
    className="inline-grid size-6 place-items-center rounded-full text-muted-foreground">
    <HelpCircle className="size-4" />
  </PopoverTrigger>
  <PopoverContent side="top" sideOffset={8} className="max-w-xs text-sm">
    El master Bill of Lading agrupa varias casas bajo un mismo embarque.
  </PopoverContent>
</Popover>
```

#### Delays y la "ventana sin delay"

Radix Tooltip.Provider: `delayDuration` **700ms**, `skipDelayDuration` **300ms**.

- **`delayDuration`** evita que el tooltip parpadee mientras el cursor cruza la interfaz de paso. 700ms es el default de Radix (heredado de la convención del SO). Para toolbars densas donde el usuario está explorando, baja a **300–500ms**; shadcn v4 lo pone en **0** en su wrapper, apostando a que quien pasa el ratón sobre un icon button ya quiere saber qué es.
- **`skipDelayDuration`** es la ventana sin delay: tras cerrar un tooltip, si mueves a **otro** trigger dentro de esos 300ms, el siguiente abre **al instante**. Sin esto, recorrer una toolbar de 8 botones es esperar 700ms ocho veces. Es el detalle que hace que las barras de herramientas de Linear y Figma se sientan vivas.
- **Cierre**: inmediato al salir (0ms), salvo `disableHoverableContent={false}`, que da una gracia para llegar al contenido.
- **Teclado**: `Tab` abre/cierra **sin delay**; `Escape`, `Enter` y `Space` cierran sin delay.

#### Specs

| Propiedad | Valor |
| --- | --- |
| Tipografía | 12px (`text-xs`), line-height 16px |
| Padding | `px-3 py-1.5` (12/6px) |
| Ancho máximo | **200–280px**; `text-balance` para que las líneas queden parejas |
| Longitud | ≤ 2 líneas, ~80 caracteres. Más largo → no es un tip |
| `sideOffset` | 4–8px |
| Contraste | Invertido (`bg-foreground text-background`) — así se distingue de un popover |
| Flecha | Opcional; útil cuando hay varios triggers juntos (NN/g) |
| Animación | 100–150ms fade + `zoom-in-95`. **Sin animación de salida** perceptible |

**Tooltip vs Popover vs HoverCard**

| | Tooltip | Popover | HoverCard |
| --- | --- | --- | --- |
| Se abre con | hover + focus | **click** | hover (con delay ~700ms) |
| Contenido | Texto plano corto | Cualquier cosa, interactiva | Preview rica, no esencial |
| Focusable | No | Sí (recibe foco) | No |
| Funciona en touch | **No** | Sí | No |
| `Esc` cierra | Sí | Sí, y devuelve foco al trigger | Sí |
| Ejemplo | "Duplicar (⌘D)" | Selector de fecha, form corto | Tarjeta de perfil al pasar sobre @usuario |

---

### 10. Toast / notificación

**Toast vs banner inline vs modal**

| Usa | Cuando | Ejemplo |
| --- | --- | --- |
| **Toast** | Confirmación de una acción del usuario, efímera, no requiere acción | "Entrada archivada" + Deshacer |
| **Banner inline** | El mensaje pertenece a un lugar concreto de la página y debe **persistir** | "3 filas no se importaron", error de validación de un campo, aviso de cuota |
| **Modal** | Bloquea el flujo hasta que se decide | "Tienes cambios sin guardar" |
| **Página de estado** | El error impide toda la vista | 403, 500, sin conexión |

#### Specs

| Propiedad | Valor |
| --- | --- |
| Posición | Escritorio: `bottom-right` (default de Sonner) o `top-right`. Móvil: **`top`**, ancho completo con margen — abajo choca con la navegación del pulgar |
| Duración | **4000ms** (default Sonner) · Radix Toast: **5000ms** · Rango sano **4–6s**; 6–8s si lleva botón de acción |
| Duración de error | **Sin auto-dismiss** o ≥ 10s + botón de cerrar |
| Pila máxima | **3 visibles**; el resto se apila colapsado o se descarta (el más viejo sale primero) |
| Ancho | 356–400px; móvil `calc(100vw - 2rem)` |
| Gap entre toasts | 8–14px, o apilado con offset de 8–16px y escala 0.95 |
| Swipe | Radix: dirección `right`, umbral **50px** |
| Hotkey de foco | Radix: **`F8`** enfoca el viewport de toasts |

**Pausa en hover — obligatoria.** El temporizador se detiene mientras el puntero está encima o el foco está dentro. Sin esto, el usuario ve un toast con un botón "Deshacer" y este desaparece justo cuando va a hacer clic. Radix expone `onPause` / `onResume`; Sonner lo trae de serie. También debe pausarse cuando la pestaña pierde visibilidad (`document.hidden`) — si no, el toast se consume mientras el usuario está en otra pestaña.

**"Deshacer" vence a "¿estás seguro?".** Un diálogo de confirmación cobra un impuesto de fricción al **100%** de las acciones para proteger del error al ~2%. Deshacer invierte la economía: la acción es instantánea para todos y solo quien se equivoca paga. Es el patrón de Gmail, Notion, Linear y Figma.

```tsx
async function archive(id: string) {
  await archiveEntry(id)                       // ejecuta ya
  toast("Entrada archivada", {
    duration: 6000,
    action: { label: "Deshacer", onClick: () => unarchiveEntry(id) },
  })
}
```

Requisitos para que aplique: la acción debe ser **reversible de verdad** y **barata de revertir**. Reserva el diálogo de confirmación para lo irreversible (borrado permanente, envío de dinero, publicación externa, borrado en masa).

**Errores que NO deben ser toast:**
- **Validación de formulario** → mensaje inline junto al campo + foco al primer campo inválido. Un toast se va antes de que el usuario encuentre el campo.
- **Errores que exigen una decisión** ("¿Reintentar o descartar?") → modal o banner persistente.
- **Fallos que dejan la vista inutilizable** → estado de error en la propia vista, con botón de reintento.
- **Cualquier cosa que el usuario deba poder releer** → un toast desaparecido es información perdida sin historial.

**Accesibilidad**
- `role="status"` (implica `aria-live="polite"` + `aria-atomic="true"`) para confirmaciones. Es preferible a `aria-live="polite"` suelto porque agrupa la configuración correcta para lectores modernos.
- `role="alert"` (implica `assertive`) **solo** para errores urgentes: interrumpe y vacía la cola del lector.
- El contenedor live region debe existir **vacío en el DOM desde el inicio** e inyectarse el texto después; si lo montas y rellenas en el mismo tick, muchos lectores no lo anuncian.
- **Nunca muevas el foco al toast.** WCAG 2.1 SC 4.1.3 exige que los status messages se determinen programáticamente **sin recibir foco**. Radix resuelve el acceso por teclado con el hotkey `F8`.
- Si el toast lleva acción, esa acción debe existir también en otro sitio: un usuario de teclado o un lector puede perder la ventana.

---

### 11. Tabs, segmented control y accordion

| | Tabs | Segmented control | Accordion |
| --- | --- | --- | --- |
| Qué hace | Navega entre **contenidos distintos** | Cambia la **presentación** del mismo contenido | Muestra/oculta secciones **en el flujo** |
| Nº de opciones | 2–7 | 2–5 | Sin límite |
| Contenido | Independiente por tab | El mismo, otro formato | Independiente, secuencial |
| Visible a la vez | 1 | 1 | 1 o varias |
| Roles | `tablist`/`tab`/`tabpanel` | `radiogroup`/`radio` (o tablist si cambia vista) | `button` + `aria-expanded` |
| Ejemplo | Resumen / Documentos / Actividad | Lista / Tarjetas / Mapa | FAQ, opciones avanzadas, filtros |

#### Tabs

Specs: altura del trigger **36–40px**; padding `px-3` (denso) a `px-4`; indicador de **2px** en el borde inferior (underline) o pill con fondo; gap 4–8px; el texto activo sube a `font-medium`, nunca a `font-bold` (el cambio de peso desplaza el ancho y "salta"). Si necesitas negrita, reserva el ancho con un pseudo-elemento del mismo texto en bold e invisible.

**Teclado (APG + Radix):**

| Tecla | Comportamiento |
| --- | --- |
| `Tab` | Entra al tab **activo** (no al primero), y luego salta al panel |
| `→` / `←` | Tab siguiente / anterior, con wrap (Radix `loop: true`) |
| `↓` / `↑` | Igual, si `orientation="vertical"` |
| `Home` / `End` | Primer / último tab |
| `Space` / `Enter` | Activa, solo en modo manual |
| `Delete` | Cierra el tab (opcional, tabs cerrables) |

**Automática vs manual.** APG recomienda **activación automática** (la selección sigue al foco) cuando el contenido ya está cargado y no hay latencia. Radix usa `activationMode="automatic"` por defecto. Cambia a `"manual"` cuando cada tab dispara un fetch: con automática, recorrer 5 tabs con las flechas lanza 5 peticiones. Los tabpanels sin contenido focusable necesitan `tabindex="0"` para que el teclado pueda scrollearlos.

#### Accordion

- **Uno o varios abiertos**: Radix lo fuerza a decidir con `type="single" | "multiple"` (requerido). `single` para wizards y FAQ donde comparar no aporta; `multiple` cuando el usuario quiere ver dos secciones a la vez. Con `single`, añade `collapsible` (default `false`) si debe poder quedar todo cerrado.
- **El encabezado es un `<button>` dentro de un heading** con `aria-level`. No un `<div onClick>`. Y el header debe ocupar el **ancho completo** de la fila: el target es la fila entera, no solo el texto.
- **Altura animable**: la altura `auto` no es interpolable. Radix mide el contenido y expone `--radix-accordion-content-height`:

```css
@keyframes acc-down { from { height: 0 } to { height: var(--radix-accordion-content-height) } }
@keyframes acc-up   { from { height: var(--radix-accordion-content-height) } to { height: 0 } }
[data-state="open"]   { animation: acc-down 200ms cubic-bezier(0.32,0.72,0,1) }
[data-state="closed"] { animation: acc-up   180ms cubic-bezier(0.32,0.72,0,1) }
```

En 2026 también existe `interpolate-size: allow-keywords` + `transition-behavior: allow-discrete` para animar a `height: auto` en CSS puro, pero su soporte todavía no es universal — la CSS var de Radix sigue siendo lo seguro.

- Teclado (Radix, más generoso que APG): `Space`/`Enter` alternan · `↓`/`↑` mueven entre triggers · `Home`/`End` al primero/último · `Tab` recorre normal.

**Cuándo el contenido NO debe esconderse:**
- Cuando el usuario necesita **comparar** secciones (precios, planes, especificaciones).
- Cuando el contenido es **corto** (< 3 líneas): el accordion cuesta un clic para ahorrar 40px.
- Cuando debe ser **buscable con Ctrl+F** o indexable — si es contenido SEO, renderízalo en el DOM y solo ocúltalo visualmente.
- Cuando hay **campos obligatorios** dentro: un error de validación en una sección colapsada es invisible. Si usas accordion en un form, ábrelo automáticamente al fallar y lleva el foco al campo.
- Cuando **todas** las secciones acaban abriéndose siempre → no era un accordion, era una página larga con encabezados.

---

### 12. Command menu (⌘K)

**Por qué se volvió estándar.** Resuelve el problema estructural de las apps densas: a partir de cierta complejidad, ninguna navegación jerárquica alcanza todo en 2 clics. El command menu colapsa **navegación + acciones + búsqueda** en una sola superficie de entrada, escala sin ocupar píxeles y convierte a los usuarios frecuentes en usuarios de teclado. Linear, Vercel, Raycast, GitHub, Notion, Slack y Figma lo tienen; **es una expectativa, no un diferenciador**. Y no reemplaza la navegación visible: es la vía rápida para quien ya sabe adónde va.

**Anatomía** (coincide en Geist, cmdk y shadcn): `Input` con placeholder orientado a acción terminado en "…" → `List` con `max-h-[300px]` y scroll → `Group` con heading en Title Case de 1–2 palabras → `Item` con icono 16px + label + `Shortcut` a la derecha → `Empty` → `Separator`.

**Atajos:**

| Tecla | Acción |
| --- | --- |
| `⌘K` / `Ctrl+K` | Abrir globalmente (no dependas solo de un botón) |
| `↑` / `↓` | Navegar resultados |
| `Enter` | Ejecutar el ítem resaltado |
| `Esc` | Cerrar (o volver a la página anterior si estás en un subnivel) |
| `Backspace` con input vacío | **Volver al nivel anterior** (Geist) |
| `⌘↵` | Variante de la acción (abrir en pestaña nueva) |

**Sin animación al abrir — o casi.** El command menu es una herramienta de velocidad: el usuario ya está escribiendo cuando la ventana aparece. Un `zoom-in` de 200ms le come tres caracteres. Máximo un **fade de 80–120ms**, sin escala ni desplazamiento, y el input **enfocado en el mismo frame**. Cerrar: instantáneo. Este es el único popup donde la animación es una regresión.

**Reglas de contenido:** ítems como frases verbales en Title Case ("Crear entrada", "Abrir configuración"); estado por defecto con "Recientes" + acciones frecuentes (nunca una lista vacía); resultados agrupados por tipo; los atajos visibles en el menú son la forma principal de **enseñarlos**; `aria-live="polite"` anunciando el número de resultados.

**Librerías:** **cmdk** (Paco Coursey) es el estándar — sin estilos, con fuzzy search y navegación de teclado incluidas, y es el motor detrás de Linear, Vercel, Raycast y del componente `Command` de shadcn. Para un ⌘K global, envuélvelo en un Dialog (`CommandDialog`) y monta el listener en el layout raíz:

```tsx
useEffect(() => {
  const onKey = (e: KeyboardEvent) => {
    if (e.key === "k" && (e.metaKey || e.ctrlKey)) { e.preventDefault(); setOpen(o => !o) }
  }
  document.addEventListener("keydown", onKey)
  return () => document.removeEventListener("keydown", onKey)
}, [])
```

---

### 13. Tabla resumen

| Componente | Cuándo usarlo | Teclado esperado | Error típico |
| --- | --- | --- | --- |
| **Checkbox** | Booleano dentro de un form con Guardar; multi-selección de ≤7 opciones | `Tab` entra/sale · `Space` alterna | Hit area de 16px sin label clicable; `indeterminate` usado como tercer estado elegible |
| **Radio** | 2–5 opciones excluyentes, todas visibles y comparables | `Tab` = **una** parada · `↑↓←→` mueven y seleccionan · `Space` selecciona | Que Tab recorra cada radio; falta de `fieldset`/`legend`; ninguna opción por defecto cuando debía haberla |
| **Switch** | Ajuste booleano que se aplica **de inmediato** | `Tab` · `Space` alterna (`Enter` también en Radix) | Ponerlo en un form con botón Guardar; texto "ON/OFF" dentro; no revertir si falla |
| **Select** | 5–15 valores, selección única | `↓`/`Alt+↓`/`Enter` abre · `↑↓` navega · `Home`/`End` · typeahead · `Enter` confirma · `Esc` cierra y devuelve foco | Usarlo para 3 opciones; ancho del popup distinto al trigger; perder el typeahead |
| **Combobox** | >15 opciones, o lista remota/desconocida | Igual que select + escritura filtra · `Esc` cierra el popup antes de limpiar | Anunciar mal los resultados (falta `aria-autocomplete`/live region); no manejar "sin resultados"; no funcionar en móvil |
| **Segmented control** | 2–5 vistas del **mismo** contenido, cambio inmediato | `←→` mueven y activan · `Home`/`End` | Usarlo como navegación (eso son tabs) o con 8 segmentos |
| **DropdownMenu** | Lista de **acciones** sobre un objeto | `Enter`/`Space`/`↓` abre · `↑↓` navega · `→`/`←` submenús · typeahead · `Esc` cierra y devuelve foco | Usarlo como select; submenú sin safe triangle; destructivo sin separar ni teñir |
| **Popover** | Contenido rico e **interactivo** anclado a un trigger, abierto por click | `Enter`/`Space` abre · `Tab` dentro · `Esc` cierra y devuelve foco | Usarlo donde bastaba un tooltip; `modal` mal elegido; sin `collisionPadding` |
| **Dialog / Modal** | Decisión bloqueante, error irreversible, paso crítico | `Tab` cicla dentro · `Esc` cierra · foco vuelve al trigger | Sin `Title`; sin focus trap; salto de layout al bloquear el scroll; anidar modales; footer que dice "OK" |
| **Drawer / Sheet** | Móvil, contenido largo, panel lateral, flujo multi-paso | Igual que dialog + arrastre + snap points | Sin grabber visible; sin `safe-area-inset-bottom`; drag que se come el scroll interno |
| **Tooltip** | Etiqueta o dato **complementario** para un control | `Tab` abre sin delay · `Esc` cierra | Contenido esencial o interactivo dentro; asumir que existe en touch; sin `skipDelayDuration` |
| **Toast** | Confirmar una acción del usuario, efímera, con Deshacer | `F8` enfoca el viewport (Radix); nunca robar el foco | Errores de validación como toast; sin pausa en hover; pila infinita; `role="alert"` para todo |
| **Tabs** | 2–7 contenidos hermanos e independientes | `Tab` al activo · `←→` navegan · `Home`/`End` | Activación automática con fetch por tab; `Tab` recorriendo cada trigger; el indicador salta por cambio de peso tipográfico |
| **Accordion** | Secciones largas, consulta ocasional, FAQ | `Space`/`Enter` · `↑↓` entre triggers · `Home`/`End` | Ocultar contenido comparable o buscable; `div` en vez de `button`; campos requeridos escondidos dentro |
| **Command menu** | Acceso global por teclado a navegación + acciones | `⌘K` abre · `↑↓` · `Enter` · `Esc` · `Backspace` retrocede | Animación de apertura; estado vacío sin recientes; ser el único camino a una función |

---

### Fuentes consultadas

**Estándares y accesibilidad**
- WAI-ARIA Authoring Practices — Combobox Pattern — https://www.w3.org/WAI/ARIA/apg/patterns/combobox/
- WAI-ARIA APG — Listbox Pattern — https://www.w3.org/WAI/ARIA/apg/patterns/listbox/
- WAI-ARIA APG — Menu and Menubar Pattern — https://www.w3.org/WAI/ARIA/apg/patterns/menu/
- WAI-ARIA APG — Menu Button Pattern — https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/
- WAI-ARIA APG — Dialog (Modal) Pattern — https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/
- WAI-ARIA APG — Tabs Pattern — https://www.w3.org/WAI/ARIA/apg/patterns/tabs/
- WAI-ARIA APG — Accordion Pattern — https://www.w3.org/WAI/ARIA/apg/patterns/accordion/
- W3C — Web Content Accessibility Guidelines (WCAG) 2.2 (SC 2.5.5 y 2.5.8) — https://www.w3.org/TR/WCAG22/
- W3C WAI — ARIA22: Using role=status to present status messages — https://www.w3.org/WAI/WCAG21/Techniques/aria/ARIA22
- Sara Soueidan — Accessible notifications with ARIA Live Regions — https://www.sarasoueidan.com/blog/accessible-notifications-with-aria-live-regions-part-1/

**Librerías (código y docs reales)**
- Radix UI Primitives — Dropdown Menu — https://www.radix-ui.com/primitives/docs/components/dropdown-menu
- Radix UI Primitives — Select — https://www.radix-ui.com/primitives/docs/components/select
- Radix UI Primitives — Dialog — https://www.radix-ui.com/primitives/docs/components/dialog
- Radix UI Primitives — Popover — https://www.radix-ui.com/primitives/docs/components/popover
- Radix UI Primitives — Tooltip — https://www.radix-ui.com/primitives/docs/components/tooltip
- Radix UI Primitives — Toast — https://www.radix-ui.com/primitives/docs/components/toast
- Radix UI Primitives — Tabs — https://www.radix-ui.com/primitives/docs/components/tabs
- Radix UI Primitives — Accordion — https://www.radix-ui.com/primitives/docs/components/accordion
- Radix UI Primitives — Checkbox — https://www.radix-ui.com/primitives/docs/components/checkbox
- Radix Primitives — issue #2589 "Tooltip doesn't react on touch" — https://github.com/radix-ui/primitives/issues/2589
- shadcn/ui — código fuente del registry `new-york-v4` (dropdown-menu, dialog, select, checkbox, switch, tooltip, sheet, command) — https://github.com/shadcn-ui/ui/tree/main/apps/v4/registry/new-york-v4/ui
- Base UI (MUI) — Select — https://base-ui.com/react/components/select
- React Aria (Adobe) — ComboBox / useComboBox — https://react-aria.adobe.com/ComboBox/useComboBox.html
- Floating UI — Middleware — https://floating-ui.com/docs/middleware
- Floating UI — flip — https://floating-ui.com/docs/flip
- Floating UI — shift — https://floating-ui.com/docs/shift
- Sonner — Toast API — https://sonner.emilkowal.ski/toast
- Vercel Geist — Command Menu — https://vercel.com/geist/command-menu
- Smashing Magazine — Better Context Menus With Safe Triangles — https://www.smashingmagazine.com/2023/08/better-context-menus-safe-triangles/

**Plataforma web (estado 2026)**
- MDN — Popover API — https://developer.mozilla.org/en-US/docs/Web/API/Popover_API
- MDN — `<dialog>` element — https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dialog
- MDN — Using CSS anchor positioning — https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_anchor_positioning/Using
- MDN — Customizable select elements — https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Customizable_select
- MDN — `accent-color` — https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/accent-color
- Chrome for Developers — The `<select>` element can now be customized with CSS — https://developer.chrome.com/blog/a-customizable-select
- OddBird — Anchor Positioning Updates for Fall 2025 — https://www.oddbird.net/2025/10/13/anchor-position-area-update/

**Guidelines de diseño**
- Nielsen Norman Group — Toggle-Switch Guidelines — https://www.nngroup.com/articles/toggle-switch-guidelines/
- Nielsen Norman Group — Modal & Nonmodal Dialogs: When (& When Not) to Use Them — https://www.nngroup.com/articles/modal-nonmodal-dialog/
- Nielsen Norman Group — Tooltip Guidelines — https://www.nngroup.com/articles/tooltip-guidelines/
- Nielsen Norman Group — Listboxes vs. Dropdown Lists — https://www.nngroup.com/articles/listbox-dropdown/
- Nielsen Norman Group — Dropdowns: Design Guidelines — https://www.nngroup.com/articles/drop-down-menus/
- Material Design 3 — Checkbox specs — https://m3.material.io/components/checkbox/specs
- Material Design 3 — Switch specs — https://m3.material.io/components/switch/specs
- Android Accessibility — Touch target size (48dp) — https://support.google.com/accessibility/android/answer/7101858
- Apple Human Interface Guidelines — Toggles — https://developer.apple.com/design/human-interface-guidelines/toggles
- Apple Human Interface Guidelines — Pop-up buttons / Pull-down buttons — https://developer.apple.com/design/human-interface-guidelines/pop-up-buttons


---
---

## Layout, contenedores y navegación

El motion se siente; el layout se *habita*. Una app premium se reconoce antes de que nada se mueva: por dónde vive el scroll, cuánto respira una tabla, si el sidebar te dice dónde estás. Este capítulo es el esqueleto.

### 1. El shell de una app

El shell es lo que **no cambia** al navegar. Cuatro patrones cubren el 95%:

| Patrón | Cuándo | Anchos típicos | Ejemplos |
|---|---|---|---|
| **Sidebar fija** | 5–15 destinos; la navegación es contexto permanente | 240–280px | Linear, Vercel, Stripe |
| **Sidebar colapsable a riel** | Igual, pero el contenido necesita ancho (tablas, editores, canvas) | 256px ↔ 48–96px | Notion, Height, Figma |
| **Topbar sola** | ≤5 secciones, producto orientado a contenido | 48–64px de alto | Vercel (nivel proyecto), docs |
| **Split view de 3 paneles** | Lista → detalle → inspector | 240 / 320–400 / flexible | Mail, Superhuman, Linear |

**Números verificados.** shadcn fija en `sidebar.tsx`: `SIDEBAR_WIDTH = 16rem` (256px), `SIDEBAR_WIDTH_MOBILE = 18rem` (288px), `SIDEBAR_WIDTH_ICON = 3rem` (48px), atajo `⌘/Ctrl+B`, estado persistido en la cookie `sidebar_state` con `max-age` de 7 días. Material 3 define el navigation rail colapsado en **96dp** y expandido en **220–360dp**, con 3–7 destinos.

> **Regla dura.** 48px es el riel mínimo funcional; 56–76px es más cómodo si lleva label bajo el icono. Por debajo de 48 el target táctil se rompe.

#### El scroll vive dentro del contenido, no en la ventana

Es *la* diferencia entre una app y una página. Si el `<body>` scrollea, el header sticky flota mal, las tablas no pueden tener `thead` pegado y el sidebar rebota en iOS.

```tsx
// app/layout.tsx — la ventana NO scrollea
<body className="h-dvh overflow-hidden bg-background antialiased">
```

```tsx
// app/(app)/layout.tsx — el shell. No se re-renderiza al navegar.
export default function AppLayout({ children }: { children: React.ReactNode }) {
  return (
    <SidebarProvider style={{ "--sidebar-width": "16rem", "--sidebar-width-icon": "3rem" } as React.CSSProperties}>
      <AppSidebar />                                   {/* persistente */}
      <SidebarInset className="flex min-h-0 flex-1 flex-col">
        <AppTopbar className="sticky top-0 z-(--z-sticky) h-14 shrink-0 border-b bg-background/80 backdrop-blur" />
        <main className="min-h-0 flex-1 overflow-y-auto overscroll-contain [scrollbar-gutter:stable]">
          {children}
        </main>
      </SidebarInset>
    </SidebarProvider>
  )
}
```

Tres detalles que casi nadie pone:
- `min-h-0` en **cada** contenedor flex de la cadena. Sin él, el hijo scrolleable no puede encogerse y el scroll se escapa a la ventana.
- `overscroll-contain`: corta el *scroll chaining* con el elemento de debajo.
- `scrollbar-gutter: stable`: evita el salto de 15px cuando aparece la barra.

#### Qué es persistente

En Next.js App Router, `layout.tsx` **no se vuelve a renderizar** al navegar entre rutas que lo comparten (*"Layouts do not rerender. They can be cached and reused"*). Consecuencias:
- Sidebar, topbar, ⌘K, providers y el scroll del sidebar viven en el layout → gratis.
- Un layout **no puede** leer `pathname` ni `searchParams`. El estado activo del nav va en un Client Component con `usePathname()`.
- Si el layout hace fetch no cacheado, bloquea la navegación: envuélvelo en su propio `<Suspense>`.

**Error común:** meter el sidebar dentro de `page.tsx` "porque es más fácil". Cada navegación lo desmonta, pierde scroll y parpadea. Se nota.

### 2. Anchos y medidas

| Medida | Valor | Razón |
|---|---|---|
| Contenedor de app | 1280–1440px (`max-w-7xl` = 1280) | Más allá, el ojo pierde la fila |
| Contenedor de contenido | 1136–1280px | Convención de sistemas modernos |
| Ancho de lectura | **65–75ch** | Butterick: 45–90 caracteres |
| Formulario de una columna | 440–560px (`max-w-md`/`max-w-xl`) | Un solo *saccade* horizontal |
| Modal estándar | 480–640px | |
| Full-bleed | Solo tablas densas, canvas, mapas, hero, editores | |

Escala `--container-*` de Tailwind v4: `xs` 320 · `sm` 384 · `md` 448 · `lg` 512 · `xl` 576 · `2xl` 672 · `3xl` 768 · `4xl` 896 · `5xl` 1024 · `6xl` 1152 · `7xl` 1280.

```html
<div class="mx-auto w-full max-w-7xl px-4 sm:px-6 lg:px-8">
```

| Breakpoint | Margen lateral |
|---|---|
| < 640px | 16px |
| 640–1023px | 24px |
| ≥ 1024px | 32px |

#### `min-w-0`: el bug clásico

Un flex item **se niega** a encogerse por debajo del ancho de su contenido (`min-width: auto` → `min-content`). Por eso `truncate` no funciona dentro de un `flex`.

```html
<!-- ❌ el texto empuja y el badge se sale de la fila -->
<div class="flex items-center gap-2">
  <span class="truncate">Nombre de cliente extremadamente largo …</span>
  <Badge>Activo</Badge>
</div>

<!-- ✅ -->
<div class="flex items-center gap-2">
  <span class="min-w-0 flex-1 truncate">Nombre de cliente extremadamente largo …</span>
  <Badge class="shrink-0">Activo</Badge>
</div>
```

**Regla dura:** todo hijo flex/grid con texto truncable lleva `min-w-0`; todo hijo que no debe encogerse lleva `shrink-0`. El equivalente vertical en scroll anidado es `min-h-0`.

### 3. Grid y composición

| Situación | Herramienta |
|---|---|
| Página con posiciones deliberadas (8/4, 7/5) | Grid de 12 columnas |
| Colección homogénea que debe fluir | `auto-fit`/`auto-fill` + `minmax()` |
| Alinear internos de items de alto distinto | `subgrid` |

```html
<div class="grid grid-cols-12 gap-6">
  <section class="col-span-12 lg:col-span-8">…</section>
  <aside   class="col-span-12 lg:col-span-4">…</aside>
</div>

<!-- min() evita el desbordamiento en 320px -->
<ul class="grid gap-4 grid-cols-[repeat(auto-fill,minmax(min(18rem,100%),1fr))]">
```

`auto-fill` deja pistas vacías (la grilla conserva su ritmo con 2 items); `auto-fit` colapsa las vacías y estira los items. Para dashboards con conteo variable, **`auto-fill` casi siempre**.

**Subgrid** resuelve el problema eterno: cards con títulos de 1 y 2 líneas cuyos footers no alinean.

```html
<ul class="grid gap-4 grid-cols-[repeat(auto-fill,minmax(18rem,1fr))]">
  <li class="row-span-3 grid grid-rows-subgrid gap-2 rounded-xl border p-4">
    <h3 class="font-medium">…</h3>                    <!-- fila 1: títulos alineados -->
    <p class="text-sm text-muted-foreground">…</p>    <!-- fila 2 -->
    <div class="flex justify-end">…</div>             <!-- fila 3: footers alineados -->
  </li>
</ul>
```

**Gaps:** `gap-2` dentro de un componente, `gap-4` entre componentes, `gap-6` entre grupos, `gap-8`/`gap-12` entre secciones. Si necesitas `gap-5`, casi siempre el problema es otro.

**Plantilla vs composición.** Un layout "de plantilla" se reconoce porque *todo* pesa igual: cuatro cards iguales, tres columnas iguales, todo con borde. Uno compuesto tiene un elemento dominante (60–70% del área visual), uno o dos de apoyo, y el resto en texto plano sin contenedor.

### 4. Cards — el capítulo crítico

NN/g define la card como *"un contenedor para unas pocas piezas cortas y relacionadas de información"* que funciona como **punto de entrada enlazado**. Dos palabras clave: *pocas* y *enlazado*.

**Una card se justifica si cumple las tres:** (1) agrupa contenido **heterogéneo**; (2) es **item de una colección** comparable; (3) es **navegable o accionable** como unidad.

| Síntoma | Alternativa |
|---|---|
| Items homogéneos, mismo tipo de dato | Lista con separadores (`divide-y`) |
| El usuario **busca**, no explora | Lista o tabla — las cards "deemphasize ranking" |
| Comparación entre items | Tabla (posiciones consistentes) |
| Una sola card en toda la pantalla | Sección con encabezado, sin contenedor |
| Card dentro de card | Sección + `divide-y` interno |
| Formulario partido en 6 cards | Secciones con `<h2>` + separador |

El "todo es una card" aplana la jerarquía: si todo está en una caja con borde y sombra, **nada** destaca. Es el equivalente visual de escribir todo en negritas.

> **Regla dura:** máximo **un** nivel de card. Card dentro de card = rediseño.
> **Regla dura:** si quitas el borde y no se pierde información, no era una card.

**Anatomía y paddings** (shadcn v4, verificado): `Card` = `flex flex-col gap-6 rounded-xl border py-6 shadow-sm`; `CardHeader` = `grid auto-rows-min gap-2 px-6`; `CardContent`/`CardFooter` = `px-6`. Traducido: **padding 24px, gap 24px entre bloques, 8px entre título y descripción, `rounded-xl` (12px), borde 1px + `shadow-sm`.**

| Densidad | Padding | Radio | Uso |
|---|---|---|---|
| Compacta | 12–16px | 8px | Grillas de 4+ columnas, KPIs |
| Estándar | 20–24px | 12px | Default |
| Amplia | 32px | 16px | Card única, pricing, onboarding |

**Regla:** el padding de la card ≥ el gap de la grilla. Con `gap-4` y `p-3`, las cards se ven infladas y mal separadas.

| Técnica | Quién la usa | Cuándo |
|---|---|---|
| **Borde 1px** | Vercel/Geist, Linear, shadcn | Default en apps densas |
| **Fondo elevado** | Notion, Slack | Cuando el borde ya está saturado; en dark mode es más natural que la sombra |
| **Sombra** | Material, Stripe (marketing) | Elevación real: dropdowns, popovers, modales, drag |

En 2026 la tendencia es **borde + fondo sutil, sombra reservada a lo que flota de verdad**. En dark mode el delimitador es el borde (`border-white/8`) o la elevación por fondo.

**Card clickeable** — el patrón accesible: **no** envuelvas la card entera en un `<a>` (el lector lee todo el contenido como etiqueta y cualquier link interno queda anidado, lo cual es inválido).

```html
<li class="group relative rounded-xl border p-6 transition-colors hover:bg-accent/50
           has-[a:focus-visible]:ring-2 has-[a:focus-visible]:ring-ring">
  <h3 class="font-medium">
    <a href="/pedidos/1042" class="after:absolute after:inset-0 after:content-[''] focus-visible:outline-none">
      Pedido GBX-26-01042
    </a>
  </h3>
  <p class="text-sm text-muted-foreground">3 artículos · MXN 12,480</p>
  <div class="relative z-10 mt-4 flex justify-end">   <!-- acciones por encima del ::after -->
    <Button variant="ghost" size="sm">Duplicar</Button>
  </div>
</li>
```

**Coste conocido:** el overlay bloquea la selección de texto. Mitigación: `position: relative` en los elementos que deban seleccionarse, o un handler que solo navegue si el clic duró <200ms. El anillo de foco va en el **contenedor**, no en el `<a>` invisible.

Estados: `hover` = cambio de fondo o de borde, **nunca** `transform: scale` en grillas densas (crea stacking context y desalinea). `selected` = borde de acento + `bg-accent` + `aria-selected`.

| Contenido | Ancho mínimo de card | Columnas máx. |
|---|---|---|
| KPI / métrica | 200–240px | 4–6 |
| Título + meta | 280–320px | 3–4 |
| Con imagen | 320–360px | 3 |
| Con acciones y detalle | 360–420px | 2–3 |

Más de 4 columnas de cards con texto = mosaico ilegible. Ahí ya era una tabla.

### 5. Secciones y encabezados de página

```html
<header class="flex flex-wrap items-start justify-between gap-4 border-b pb-6">
  <div class="min-w-0 space-y-1">
    <Breadcrumb class="text-xs text-muted-foreground" />
    <h1 class="truncate text-2xl font-semibold tracking-tight">Entradas</h1>
    <p class="text-sm text-muted-foreground">124 entradas · 8 pendientes de clasificar</p>
  </div>
  <div class="flex shrink-0 items-center gap-2">
    <Button variant="outline">Exportar</Button>
    <Button>Nueva entrada</Button>
  </div>
</header>
```

| Elemento | Tamaño | Notas |
|---|---|---|
| Breadcrumb | 12px, gris | Solo si hay ≥3 niveles reales |
| Título H1 | 24–30px, `font-semibold`, `tracking-tight` | Uno por pantalla |
| Descripción | 14px, muted | Máx. 1 línea |
| Título → contenido | 24–32px | |
| Encabezado de sección | 16–18px `font-medium` | + eyebrow `text-xs uppercase tracking-wide` |
| Entre secciones | 32–48px | |

**Ritmo vertical:** una sola escala (`space-y-8` entre secciones, `space-y-4` dentro). Los separadores solo cuando el espacio no basta — si ya hay 48px de aire, la línea es ruido.

| Caso | Dónde va la acción principal |
|---|---|
| Creación sobre una colección | Header, arriba a la derecha |
| Guardar en formulario corto | Al final, alineado a la izquierda del contenido |
| Guardar en formulario largo o editor | Barra sticky inferior |
| Acción principal en móvil | FAB o barra fija con `pb-[env(safe-area-inset-bottom)]` |

**Error común:** 4 botones del mismo peso en el header. Uno sólido, uno `outline`, el resto en `⋯`.

### 6. Navegación

| Decisión | Regla |
|---|---|
| Items por grupo | 5–7. Más y el usuario deja de escanear |
| Grupos por sidebar | 2–4, cada uno con su label (11–12px) |
| Total visible | ≤12–15; el resto en "Más" o en ⌘K |
| Orden | **Por frecuencia de uso**, no alfabético ni por jerarquía del backend |
| Iconos | Sí si hay riel colapsable o ≥8 items |
| Profundidad | Máx. 2 niveles. El tercero va como tabs de página |
| Badges de conteo | Solo para lo accionable. Un badge que siempre dice "128" es decoración |
| Alturas | `h-8` (32px) default, `h-7` sm, `h-12` lg |

```tsx
"use client"
const pathname = usePathname()
const isActive = pathname === href || pathname.startsWith(href + "/")

<SidebarMenuButton asChild isActive={isActive}>
  <Link href={href} aria-current={isActive ? "page" : undefined}>
    <Icon className="size-4" /> <span>{label}</span>
  </Link>
</SidebarMenuButton>
```

El activo necesita **dos señales**, no una: fondo + peso/color, o barra de acento de 2px. Solo color falla en daltonismo y en pantallas malas. Siempre `aria-current="page"`. **Al colapsar a riel los labels desaparecen → el tooltip es obligatorio.**

**Navegación secundaria (tabs de página):** para sub-vistas del **mismo objeto**. 2–6 tabs, sin scroll horizontal en desktop, cada tab es **una URL real** (se debe poder compartir el link), indicador de 2px pegado al borde inferior.

**Breadcrumbs:** arriba, separador `>`, **el último item no es enlace**, y *"no deben reemplazar la navegación global"*. Aportan con ≥3 niveles reales y deep links; en apps de 2 niveles con sidebar son ruido.

| Patrón móvil | Cuándo | Items |
|---|---|---|
| **Bottom bar** | Destinos de nivel superior, cambio frecuente | **3–5** (M3: altura 80dp, 64dp Expressive) |
| **Drawer** | >5 destinos o navegación secundaria | Sin límite, con grupos |
| **Ambos** | Bottom bar con 4 + item "Más" que abre el drawer | |

Lo que está escondido se usa la mitad: si un destino es crítico, no va en el drawer.

### 7. Tablas y listas densas

Escala de altura de fila (Carbon Design System):

| Densidad | Altura | Uso |
|---|---|---|
| xs | 24px | Vistas de análisis, cientos de filas |
| sm | 32px | **Default de app densa** (Linear, Height) |
| md | 40px | Default de app general |
| lg | 48px | Filas con avatar + 2 líneas |
| xl | 64px | Media o metadatos apilados |

Padding horizontal de columna: **16px**. Header: 14px `font-semibold`, o 12px `font-medium` gris (estilo Linear/Vercel).

| Tipo | Alineación | Extra |
|---|---|---|
| Texto, nombres, estados | Izquierda | `truncate` + `title` |
| Números, moneda, % | **Derecha** | `tabular-nums` obligatorio |
| Fechas | Consistente en toda la tabla | `tabular-nums` |
| Booleanos / iconos | Centro | |
| Acciones | Derecha, columna fija de 40–56px | |

```tsx
<div className="min-h-0 flex-1 overflow-auto">
  <table className="w-full border-separate border-spacing-0 text-sm">
    <thead className="sticky top-0 z-10">
      <tr className="[&>th]:h-9 [&>th]:border-b [&>th]:bg-background [&>th]:px-3
                     [&>th]:text-left [&>th]:text-xs [&>th]:font-medium [&>th]:text-muted-foreground">
        …
      </tr>
    </thead>
    <tbody className="[&>tr]:border-b [&>tr:hover]:bg-muted/50">
```

> **Gotcha:** con `border-collapse: collapse` el borde del `thead` sticky **no se pega**. Usa `border-separate border-spacing-0` y pon el borde en las celdas. Y el `bg-*` va en las celdas, no en el `<tr>`: un `<tr>` sticky sin fondo deja ver las filas pasar por debajo.

**Líneas vs zebra:** las apps modernas usan **líneas de 1px muy sutiles + hover de fila**, no zebra. La zebra duplica los valores de fondo (rompe el dark mode y los estados de selección), y borders/zebra/hover cumplen **la misma función** — con una basta. Zebra solo en tablas de >8 columnas, y ahí normalmente lo correcto es congelar la primera columna.

- **Selección:** checkbox en columna 0 de 40px; fila con `data-[state=selected]`; al haber selección aparece una **barra de acciones masivas**, nunca un botón por fila.
- **Ordenamiento:** el header es un `<button>` completo, flecha visible en hover si no está activa, `aria-sort`.
- **Acciones por fila:** `⋯` a la derecha, `opacity-0 group-hover:opacity-100 focus-visible:opacity-100` — si depende solo del hover, en táctil no existe.

| Patrón | Cuándo |
|---|---|
| **Paginación** | Datos operativos donde importa "¿cuántos hay?". Default de negocio |
| **"Cargar más"** | Listas exploratorias; conserva el footer accesible |
| **Scroll infinito** | Feeds de consumo. **Nunca** con footer útil debajo |
| **Virtualización** | >500 filas visibles a la vez |

```html
<th class="sticky left-0 z-20 bg-background after:absolute after:inset-y-0 after:right-0
           after:w-px after:bg-border">Folio</th>
```

La columna congelada necesita `sticky left-0`, **fondo opaco**, z-index mayor que las celdas (y el header esquinero mayor que ambos) y una línea/sombra a la derecha: sin esa señal el usuario no sabe que puede scrollear.

**La tabla en móvil:** scroll horizontal con primera columna congelada (cuando la tabla *es* el producto), tarjetas apiladas (≤5 campos), o **lista de 2 líneas** (identificador arriba, 2–3 datos abajo en gris, tap → detalle). La de 2 líneas gana casi siempre: convertir 9 columnas en 9 pares `label: valor` produce una card de 400px que nadie lee.

### 8. Los cuatro estados que casi nadie diseña

**Vacío** — tres variantes distintas, con copy distinto:

| Variante | Título | Cuerpo | Acción |
|---|---|---|---|
| **Primer uso** | "Aún no tienes entradas" | "Las entradas registran cada embarque que ingresa a la aduana. Crea la primera para empezar." | Primario "Nueva entrada" + link "Ver cómo funciona" |
| **Sin resultados** | "Sin resultados para «pedimento 4021»" | "Revisa la ortografía o quita algún filtro." | Secundario "Limpiar filtros (3)" |
| **Error** | "No pudimos cargar las entradas" | "Ocurrió un problema de conexión. Vuelve a intentarlo." | "Reintentar" + `<details>` con el ID de error |
| **Sin permisos** | "No tienes acceso a estas entradas" | "Pide acceso al administrador de tu equipo." | "Solicitar acceso" |

Specs: contenedor centrado `max-w-sm`, `py-16`, icono de 32–40px en círculo `bg-muted` (o ilustración, **nunca** ambos), título 16px `font-medium`, cuerpo 14px muted, gap 8px título→cuerpo, 24px cuerpo→acción.

> **Regla dura:** un empty state sin acción es un callejón sin salida. Y el de "sin resultados" **jamás** ofrece "Crear" como acción principal — el usuario estaba buscando, no creando.

**Carga** (límites de Nielsen: 0.1s instantáneo · 1s el flujo no se interrumpe · 10s límite de atención):

| Duración esperada | Qué mostrar |
|---|---|
| < 300ms | **Nada.** Un skeleton que parpadea 120ms se percibe *más lento* |
| 300ms – 3s | **Skeleton que calca el layout real** |
| > 3s | Skeleton + progreso ("Procesando 240 de 1,200…") |
| Acción de botón | Spinner **dentro del botón**, ancho fijo |

```tsx
// El skeleton reutiliza las mismas clases de la fila real
export function RowSkeleton() {
  return (
    <tr className="border-b">
      <td className="h-8 px-3"><div className="h-3 w-24 animate-pulse rounded bg-muted" /></td>
      <td className="h-8 px-3"><div className="h-3 w-40 animate-pulse rounded bg-muted" /></td>
      <td className="h-8 px-3 text-right"><div className="ml-auto h-3 w-16 animate-pulse rounded bg-muted" /></td>
    </tr>
  )
}
```

Un skeleton genérico de 3 barras grises es peor que nada: al llegar el contenido, todo salta (CLS). En App Router, `loading.js` da el skeleton de la página, `<Suspense>` granular para partes lentas — pero `loading.js` **no** cubre el fetch del propio layout.

**Error:** qué pasó (lenguaje humano) + qué puede hacer + cómo reportarlo. Pantalla completa (`error.tsx`) con "Reintentar" (`reset()`) y el `digest` copiable en `text-xs`. **Error de sección:** no tumbes la pantalla entera — `error.tsx` por segmento. **Error de acción:** toast + el formulario conserva los datos. Copy: "No pudimos guardar los cambios." ✅ · "Error: Request failed with status code 500" ❌ · "¡Ups! Algo salió mal 😅" ❌.

**Parcial / offline** — el que nadie diseña: banner discreto ("Mostrando datos de hace 4 min · **Actualizar**"), celdas que fallaron con `—` en gris y `title="No disponible"` (no `null`, no `0`), y offline con `role="status"` + deshabilitar escrituras.

### 9. Densidad y respiración

| Tipo de producto | Densidad objetivo | Fila / padding |
|---|---|---|
| Herramienta operativa (ERP, aduanas, trading) | Alta: 30–50 filas visibles | 32px / `p-3` |
| SaaS general | Media: 15–25 items | 40px / `p-4` |
| Consumo / contenido | Baja: 5–10 items | 48–64px / `p-6` |
| Onboarding, pricing, marketing | Muy baja | `p-8`+ |

**"Quitar hasta que duela", en orden:** (1) lista cada elemento y responde qué decisión permite tomar; (2) lo que no permite ninguna → fuera o al detalle; (3) ordena lo que queda en tres niveles; (4) **baja el contraste** de secundario y terciario en vez de achicarlos; (5) quita bordes: reemplázalos por diferencia de fondo o por espacio.

**El error de rellenar todo el ancho:** un dashboard de 1920px con 4 KPIs estirados a 460px se ve vacío y roto. Fija `max-w-*`, usa `auto-fill` para que aparezca una 5ª columna en vez de estirar, o deja aire deliberado a la derecha (el "margen editorial" es una decisión, no un bug).

### 10. Responsive en 2026

Breakpoints de Tailwind v4: `sm` 640 · `md` 768 · `lg` 1024 · `xl` 1280 · `2xl` 1536 (+ variantes `max-*`). Material 3 window size classes (dp): compact <600 · medium 600–839 · expanded 840–1199 · large 1200–1599 · extra-large ≥1600.

**Pero los breakpoints se eligen por contenido, no por dispositivos:** ensancha el navegador hasta que el layout se rompa — ahí va el breakpoint, aunque caiga en 712px (`min-[712px]:` funciona sin configurar nada).

Para el shell, los cortes que importan: **<768px** sidebar → drawer (288px), tabla → lista, acciones → barra inferior. **768–1279px** sidebar → riel, contenido a una columna. **≥1280px** sidebar expandida, 2 columnas, panel de detalle.

#### Container queries: el cambio real

Un componente reutilizable no debe saber qué tan ancha es la ventana; debe saber qué tan ancho es **su hueco**.

```html
<article class="@container rounded-xl border p-4">
  <div class="flex flex-col gap-3 @md:flex-row @md:items-center @md:gap-6">
    <img class="w-full rounded-lg @md:w-32 @md:shrink-0" src="…" alt="">
    <div class="min-w-0">
      <h3 class="truncate font-medium">…</h3>
      <p class="text-sm text-muted-foreground @max-sm:line-clamp-2">…</p>
    </div>
  </div>
</article>
```

Escala de container queries: `@xs` 320 · `@sm` 384 · `@md` 448 · `@lg` 512 · `@xl` 576 · `@2xl` 672 · `@3xl` 768 · `@4xl` 896 · `@5xl` 1024 · `@6xl` 1152 · `@7xl` 1280.

> **Ojo:** `@md` = 448px, `md` = 768px. **No son lo mismo.** Los containers son más chicos porque los componentes viven en huecos, no en viewports.

Adicional: containers nombrados `@container/card` → `@md/card:flex-row`; rangos `@sm:@max-md:flex-col`; arbitrarios `@min-[475px]:`; unidades `cqw`/`cqb`.

> **Gotcha:** `container-type: inline-size` **crea un stacking context** y contención de tamaño — un hijo con `position: fixed` se ancla al container, no al viewport.

**Regla de reparto:** media queries para el *shell* (¿hay sidebar?, ¿cuántos paneles?); container queries para los *componentes* (¿la card es horizontal o vertical?).

**Móvil:** `100vh` es igual a `100lvh` — por eso el contenido queda debajo de la barra de Safari; usa `h-dvh` en el shell (y `min-h-svh` cuando quieras la garantía conservadora). Safe areas: `viewport-fit=cover` + `env(safe-area-inset-*)`:

```css
@theme { --spacing-safe-b: max(1rem, env(safe-area-inset-bottom)); }
/* <nav class="pb-(--spacing-safe-b)"> */
```

**Teclado virtual:** por defecto `interactive-widget=resizes-visual` — el layout no se re-mide y una barra `fixed bottom-0` queda tapada. Para apps con inputs anclados abajo (chat, comentarios):

```html
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover, interactive-widget=resizes-content">
```

**Orden al colapsar:** prioridad (1) identidad y estado del objeto, (2) acción principal, (3) contenido, (4) metadatos, (5) acciones secundarias. Usa `order-*` con moderación: **desalinea el orden de tabulación** (falla WCAG 1.3.2). Si necesitas reordenar mucho, reordena el DOM.

### 11. Z-index y capas

```css
@theme {
  --z-base: 0; --z-sticky: 10; --z-frozen: 20; --z-drawer: 30;
  --z-overlay: 40; --z-modal: 50; --z-popover: 60; --z-toast: 70;
}
/* <thead class="sticky top-0 z-(--z-sticky)"> */
```

**Stacking contexts que sorprenden** (lista MDN): `position` + `z-index ≠ auto`; `fixed`; `sticky`; `opacity < 1`; `transform`, `scale`, `rotate`, `translate`, `filter`, `backdrop-filter`, `perspective`, `clip-path`, `mask*`; `mix-blend-mode`; `isolation: isolate`; `container-type`; `contain`; `will-change` con cualquiera de esas; y el *top layer* (`<dialog>` modal, popover).

Las tres que muerden: (1) un `transform` en una card atrapa su dropdown; (2) `backdrop-blur` en el topbar encapsula cualquier z-index interno; (3) `@container` en un wrapper hace lo mismo y además rompe `position: fixed` de los hijos.

**La solución real son los portales.** Radix renderiza `DropdownMenu`, `Select`, `Popover`, `Tooltip` y `Dialog` en un portal al final del `<body>`. Regla: **cualquier cosa que flote sobre otra usa portal**; el z-index solo decide el orden entre portales. `isolation: isolate` en el contenedor de contenido es un buen seguro.

### 12. Checklist de layout — antes de dar la pantalla por terminada

**Alineación y ritmo** — [ ] todo alineado a la rejilla, sin valores sueltos · [ ] los ejes verticales coinciden (título, primer item, footer) · [ ] separación entre secciones consistente.

**Jerarquía** — [ ] hay **un** elemento dominante · [ ] máximo un botón sólido por área · [ ] ningún nivel de card anidada · [ ] lo secundario se distingue por color/peso, no por tamaño.

**Medidas** — [ ] prosa entre 65–75ch · [ ] contenedor con `max-w-*` · [ ] `min-w-0` en cada flex item truncable y `shrink-0` en iconos · [ ] `min-h-0` en toda la cadena hasta el scroll.

**Estados** — [ ] vacío de primer uso, vacío por filtros, error y carga, cada uno con copy real · [ ] el skeleton calca el layout final · [ ] nada de skeleton por debajo de 300ms · [ ] parcial/offline resuelto o descartado explícitamente.

**Tablas** — [ ] números a la derecha con `tabular-nums` · [ ] `thead` sticky con fondo opaco y `border-separate` · [ ] acciones por fila accesibles por teclado · [ ] la tabla scrollea dentro de su contenedor.

**Navegación** — [ ] activo con dos señales + `aria-current` · [ ] el título de la pantalla coincide con el label del nav · [ ] breadcrumb solo con ≥3 niveles.

**Responsive** — [ ] probado a 320, 375, 768, 1024, 1280, 1440 y 1920 · [ ] los componentes reutilizables usan `@container` · [ ] `h-dvh` en el shell, safe areas, teclado virtual en un iPhone real · [ ] el orden del DOM al colapsar sigue siendo el orden lógico.

**Foco y capas** — [ ] `focus-visible` en todo lo interactivo, incluida la card clickeable · [ ] ningún z-index mágico · [ ] ningún `transform`/`backdrop-filter` atrapando un popover.

**Densidad** — [ ] cada elemento permite tomar una decisión. Lo que no, se fue.

### Fuentes — layout y navegación

shadcn/ui *Sidebar* (+ `sidebar.tsx` y `card.tsx` verificados), *Data Table*, *Sidebar blocks* · Tailwind v4 *Responsive design*, *max-width*, *grid-template-columns/subgrid* · Next.js *layout.js* · MDN `min-width`, `svh/lvh/dvh`, `overscroll-behavior`, *Stacking context*, `font-variant-numeric`, meta viewport · Radix Themes *Container* · Vercel Geist *Introduction* y *Grid* · Carbon *Data table style* · Material 3 *Window size classes*, *Navigation rail*, *Navigation bar* · NN/g *Cards*, *Data Tables: Four Major User Tasks*, *Designing Empty States*, *Breadcrumbs*, *Response Times* · Inclusive Components *Cards* · Refactoring UI *7 Practical Tips* · Butterick *Line length* · Stan Vision *UI Card Design: card overuse*.


---
---

## Recetario de efectos

> Esta sección no explica *por qué* animar (eso ya está en la parte de principios): es un catálogo de efectos ya resueltos, con el código listo para pegar en un proyecto **React 19 + Next.js App Router + Tailwind CSS v4 + Motion**. Todo el CSS asume `@import "tailwindcss";` en `globals.css`.

### Convenciones y soporte de navegador (estado a 2026)

| API | Estado 2026 | Notas para producción |
|---|---|---|
| `@property` | **Baseline** (jul-2024): Chrome/Edge 85+, Safari 16.4+, Firefox 128+ | Seguro. Solo lo necesitas para interpolar valores que CSS no sabe interpolar solo: `<angle>` dentro de `conic-gradient`, `<color>` o `<percentage>` como *color stop*. |
| `conic-gradient`, `radial-gradient` | Baseline desde 2020 | Seguro. |
| `mask-image` sin prefijo | **Baseline** (dic-2023): Chrome/Edge 120+, Safari 15.4+, Firefox 53+ | Añade `-webkit-mask-*` si soportas Safari < 15.4. `mask-composite: exclude` ↔ `-webkit-mask-composite: xor`. |
| `offset-path` / `offset-distance` | **Baseline widely available** desde 2022 | Seguro, sin fallback. |
| `animation-timeline` (`scroll()` / `view()`) | **NO Baseline** — MDN: *limited availability*. Chrome/Edge 115+, **Safari 26+** (sep-2025, threaded en 26.4), **Firefox tras flag** `layout.css.scroll-driven-animations.enabled` (aún en 152, jun-2026); prioridad Interop 2026 | **Obligatorio** envolver en `@supports`. Ver el gotcha de la sección Scroll. |
| `animation-trigger` / `timeline-trigger` | **Solo Chrome 145/146+** (2026) | Experimental. No lo uses como base. |
| `sibling-index()` / `sibling-count()` | Chrome/Edge 138+ (jun-2025), Safari 26.2; **Firefox no** | Progressive enhancement para stagger sin JS. |
| View Transitions **same-document** | **Baseline newly available**: Chrome 111+, Safari 18+, Firefox 144+ (oct-2025) | Usable ya con guard `if (!document.startViewTransition)`. |
| View Transitions **cross-document** | Chrome 126+, Safari 18.2+; **Firefox no** (candidato Interop 2026) | Progressive enhancement puro. |
| `@starting-style` + `transition-behavior: allow-discrete` | **Baseline**: Chrome 117/121+, Safari 17.4/17.5+, Firefox 129+ (~91% caniuse) | Seguro, con el gotcha de especificidad (abajo). |
| `popover` | **Baseline newly available** (ene-2025), ~88-90% | Seguro. |
| `interpolate-size` / `calc-size()` | **Solo Chromium 129+**. No Firefox, no Safari | Fallback obligatorio (`grid-template-rows: 0fr→1fr`). |
| `text-wrap: balance` | **Baseline 2024**: Chrome 114+, Firefox 121+, Safari 17.5+ | Seguro. Chromium lo limita a ~6 líneas por coste. |
| `text-wrap: pretty` | Chrome 117+, Safari 26+; Firefox parcial | Degrada solo, sin romper nada. |
| `backdrop-filter` | Baseline widely available | Seguro pero **caro**: cada capa fuerza un blur de fondo por frame. |
| `:has()` | **Baseline** dic-2023 | Seguro; acota el selector, no lo pongas sobre listas de 500 filas. |

**Motion**: el paquete se llama `motion` desde finales de 2024; se importa `from "motion/react"`. `framer-motion` sigue re-exportando pero está congelado.

---

### Estados de carga

#### Skeleton con shimmer

Comunica "esto va a llegar, y va a tener esta forma". **Sí**: listas y cards cuya geometría conoces de antemano y que tardan >400 ms. **No**: si el layout real difiere del skeleton (peor que un spinner) ni para esperas <300 ms. Coste: el barrido debe ir por `transform`, nunca por `background-position` (eso repinta la capa entera cada frame).

```css
/* globals.css */
@theme {
  --animate-shimmer: shimmer 1.6s ease-in-out infinite;
  @keyframes shimmer {
    100% { transform: translateX(100%); }
  }
}

@utility skeleton {
  position: relative;
  overflow: hidden;
  isolation: isolate;
  border-radius: var(--radius-md);
  background-color: color-mix(in oklab, currentColor 12%, transparent);
  &::after {
    content: "";
    position: absolute;
    inset: 0;
    transform: translateX(-100%);
    background-image: linear-gradient(
      90deg,
      transparent,
      color-mix(in oklab, currentColor 10%, transparent),
      transparent
    );
    animation: var(--animate-shimmer);
  }
}

@media (prefers-reduced-motion: reduce) {
  .skeleton::after { animation: none; }
}
```

```tsx
<div className="space-y-2">
  <div className="skeleton h-4 w-2/3" />
  <div className="skeleton h-4 w-1/2" />
</div>
```

#### Pulse

`animate-pulse` de Tailwind ya existe. Úsalo solo cuando **no** conoces la forma final. Es más barato que el shimmer (una sola propiedad `opacity`, compositable).

#### Spinner (SVG + `animate-spin`)

```tsx
export function Spinner({ className = "size-4" }: { className?: string }) {
  return (
    <svg className={`${className} animate-spin`} viewBox="0 0 24 24" fill="none" role="status" aria-label="Cargando">
      <circle cx="12" cy="12" r="10" stroke="currentColor" strokeOpacity="0.2" strokeWidth="3" />
      <path d="M22 12a10 10 0 0 1-10 10" stroke="currentColor" strokeWidth="3" strokeLinecap="round" />
    </svg>
  );
}
```

#### Spinner con `stroke-dasharray` (el de dos ejes, tipo Material)

Comunica progreso *vivo* en lugar de giro plano. Cuesta más: anima `stroke-dasharray`/`stroke-dashoffset`, que **no** son compositables — repinta el SVG. Un spinner por pantalla, nunca veinte en una tabla.

```css
@theme {
  --animate-spin-slow: spin 2s linear infinite;
  --animate-dash: dash 1.5s ease-in-out infinite;
  @keyframes dash {
    0%   { stroke-dasharray: 1 150;  stroke-dashoffset: 0; }
    50%  { stroke-dasharray: 90 150; stroke-dashoffset: -35; }
    100% { stroke-dasharray: 90 150; stroke-dashoffset: -124; }
  }
}
```

```tsx
<svg className="size-6 animate-spin-slow" viewBox="0 0 50 50">
  <circle
    className="animate-dash"
    cx="25" cy="25" r="20"
    fill="none" stroke="currentColor" strokeWidth="4" strokeLinecap="round"
  />
</svg>
```

#### Barra de progreso indeterminada

Para operaciones sin porcentaje conocido (import, sync). Si **sí** conoces el porcentaje, usa una barra determinada: la indeterminada miente.

```css
@theme {
  --animate-indeterminate: indeterminate 1.4s cubic-bezier(0.65, 0, 0.35, 1) infinite;
  @keyframes indeterminate {
    0%   { transform: translateX(-100%) scaleX(0.35); }
    55%  { transform: translateX(35%)   scaleX(0.7); }
    100% { transform: translateX(170%)  scaleX(0.35); }
  }
}
```

```tsx
<div role="progressbar" aria-label="Procesando" className="h-0.5 w-full overflow-hidden rounded-full bg-neutral-200 dark:bg-neutral-800">
  <div className="h-full w-full origin-left rounded-full bg-violet-500 animate-indeterminate motion-reduce:animate-none" />
</div>
```

#### Streaming / typing de respuestas de IA

El patrón que se volvió estándar 2025-2026: cada palabra entra con `blur → 0` en ~250 ms. El blur enmascara la llegada *en lotes* de tokens mucho mejor que un `opacity` puro. Clave: **la key debe ser estable por índice** para que las palabras ya renderizadas no re-animen cuando llega el siguiente chunk.

```tsx
"use client";
import { motion } from "motion/react";

export function StreamedText({ text }: { text: string }) {
  const words = text.split(/(\s+)/); // conserva los espacios
  return (
    <p className="text-pretty leading-relaxed">
      {words.map((w, i) => (
        <motion.span
          key={i}
          initial={{ opacity: 0, filter: "blur(4px)", y: 2 }}
          animate={{ opacity: 1, filter: "blur(0px)", y: 0 }}
          transition={{ duration: 0.26, ease: [0.22, 1, 0.36, 1] }}
          className="inline-block whitespace-pre"
        >
          {w}
        </motion.span>
      ))}
    </p>
  );
}
```

Cursor parpadeante mientras el stream está abierto:

```css
@theme {
  --animate-caret: caret 1s step-end infinite;
  @keyframes caret { 50% { opacity: 0; } }
}
```
```tsx
{isStreaming && <span aria-hidden className="ml-0.5 inline-block h-[1em] w-[2px] translate-y-[0.15em] bg-current animate-caret" />}
```

> Coste: `filter: blur()` se compone en GPU pero cada palabra es una capa. Con respuestas de >800 palabras, aplica el efecto **solo al último párrafo** y renderiza el resto plano.

---

### Realce de bordes y superficies

#### Border beam (luz que recorre el borde)

Comunica "aquí está pasando algo / esto es lo premium". **Sí**: una card de pricing destacada, un estado "procesando" en una landing. **No**: en una app de trabajo diario — es una animación infinita en el campo visual periférico, la peor categoría para tareas de concentración.

**Versión CSS pura** (`@property` + `conic-gradient` + `mask`). El `@property` es **imprescindible**: sin él, `--beam-angle` es un token sin tipo y CSS no lo interpola — el gradiente saltaría en lugar de girar.

```css
@property --beam-angle {
  syntax: "<angle>";
  initial-value: 0deg;
  inherits: false;
}

@keyframes beam-spin { to { --beam-angle: 360deg; } }

@utility border-beam {
  position: relative;
  isolation: isolate;
  &::before {
    content: "";
    position: absolute;
    inset: 0;
    z-index: 1;
    border-radius: inherit;
    padding: 1px;                     /* = grosor del borde */
    pointer-events: none;
    background: conic-gradient(
      from var(--beam-angle),
      transparent 0turn 0.82turn,
      var(--beam-from, oklch(0.78 0.17 60)) 0.92turn,
      var(--beam-to,   oklch(0.68 0.22 300)) 0.97turn,
      transparent 1turn
    );
    /* recorta todo menos el anillo de 1px */
    -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    -webkit-mask-composite: xor;
    mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
    mask-composite: exclude;
    animation: beam-spin 6s linear infinite;
  }
}
@media (prefers-reduced-motion: reduce) { .border-beam::before { animation: none; } }
```

**Versión Motion + `offset-path`** (es la que usa Magic UI). La luz recorre el *perímetro real* a velocidad constante, en lugar de barrer angularmente desde el centro; en cards muy anchas se nota la diferencia.

```tsx
"use client";
import { motion } from "motion/react";

export function BorderBeam({ size = 60, duration = 6, from = "#ffaa40", to = "#9c40ff" }) {
  return (
    <div
      aria-hidden
      className="pointer-events-none absolute inset-0 rounded-[inherit] border border-transparent
                 [mask-clip:padding-box,border-box] [mask-composite:intersect]
                 [mask-image:linear-gradient(transparent,transparent),linear-gradient(#000,#000)]"
    >
      <motion.div
        className="absolute aspect-square bg-gradient-to-l from-(--beam-from) via-(--beam-to) to-transparent"
        style={{
          width: size,
          offsetPath: `rect(0 auto auto 0 round ${size}px)`,
          "--beam-from": from,
          "--beam-to": to,
        } as React.CSSProperties}
        initial={{ offsetDistance: "0%" }}
        animate={{ offsetDistance: "100%" }}
        transition={{ repeat: Infinity, ease: "linear", duration }}
      />
    </div>
  );
}
```

#### Gradient border estático

Dos técnicas. La de `background-clip` es la más corta pero **exige fondo opaco** (el `padding-box` tapa lo que hay detrás). Si necesitas translucidez o `backdrop-filter`, ve por la de `mask`.

```css
/* A) background-clip — fondo opaco obligatorio */
.gradient-border {
  border: 1px solid transparent;
  border-radius: 0.75rem;
  background:
    linear-gradient(var(--color-neutral-950), var(--color-neutral-950)) padding-box,
    linear-gradient(135deg, oklch(0.7 0.15 260), oklch(0.75 0.16 320)) border-box;
}

/* B) mask — funciona sobre fondo translúcido */
.gradient-border-glass {
  position: relative;
  border-radius: 0.75rem;
  background: color-mix(in oklab, white 6%, transparent);
  backdrop-filter: blur(12px);
}
.gradient-border-glass::before {
  content: "";
  position: absolute; inset: 0;
  border-radius: inherit; padding: 1px; pointer-events: none;
  background: linear-gradient(135deg, oklch(0.8 0.14 260 / 0.9), transparent 60%);
  -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
  -webkit-mask-composite: xor;
  mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
  mask-composite: exclude;
}
```

#### Glow / halo

```css
.glow {
  box-shadow:
    0 0 0 1px color-mix(in oklab, var(--color-violet-500) 25%, transparent),
    0 8px 30px -8px color-mix(in oklab, var(--color-violet-500) 45%, transparent);
}
```
Coste: `box-shadow` grande + `blur` implícito repinta al cambiar. **Nunca lo animes** — anima la `opacity` de un pseudo-elemento que ya lo lleva.

#### Spotlight que sigue al cursor

Coste casi cero **si no pasas por React state**: el handler solo escribe dos custom properties, no hay re-render, solo un repaint de una capa.

```tsx
"use client";
import { useRef } from "react";

export function SpotlightCard({ children }: { children: React.ReactNode }) {
  const ref = useRef<HTMLDivElement>(null);
  return (
    <div
      ref={ref}
      onPointerMove={(e) => {
        const el = ref.current;
        if (!el) return;
        const r = el.getBoundingClientRect();
        el.style.setProperty("--mx", `${e.clientX - r.left}px`);
        el.style.setProperty("--my", `${e.clientY - r.top}px`);
      }}
      className="group relative overflow-hidden rounded-xl border border-white/10 bg-neutral-950 p-6"
    >
      <div
        aria-hidden
        className="pointer-events-none absolute inset-0 opacity-0 transition-opacity duration-300 group-hover:opacity-100
                   [background:radial-gradient(220px_circle_at_var(--mx)_var(--my),color-mix(in_oklab,white_12%,transparent),transparent_70%)]"
      />
      <div className="relative">{children}</div>
    </div>
  );
}
```

Para una **grilla** de cards, pon un solo `onPointerMove` en el contenedor y deja que `--mx/--my` se hereden; cada card usa `radial-gradient(... at calc(var(--mx) - <su offset>) ...)`. Ahorra N listeners. Envuelve el efecto en `@media (hover: hover)`: en táctil no aporta nada.

#### Shine / sheen al hover

```css
@utility sheen {
  position: relative;
  overflow: hidden;
  isolation: isolate;
  &::after {
    content: "";
    position: absolute; inset: 0;
    translate: -150% 0;
    background: linear-gradient(75deg, transparent 30%,
      color-mix(in oklab, white 28%, transparent) 50%, transparent 70%);
    transition: translate 700ms cubic-bezier(0.22, 1, 0.36, 1);
    pointer-events: none;
  }
  &:hover::after, &:focus-visible::after { translate: 150% 0; }
}
```

#### Highlight interior superior (tipo Linear)

El truco más rentable de toda la lista: **una línea de 1px de luz arriba** + un degradado descendente sutil. Cuesta cero (es estático) y es el 80% de la sensación "premium" en superficies oscuras.

```css
.surface-linear {
  border-radius: 0.75rem;
  background:
    linear-gradient(180deg, color-mix(in oklab, white 6%, transparent), transparent 40%),
    var(--color-neutral-900);
  box-shadow:
    inset 0 1px 0 0 color-mix(in oklab, white 9%, transparent),
    0 1px 2px 0 rgb(0 0 0 / 0.4),
    0 8px 24px -12px rgb(0 0 0 / 0.5);
}
```

---

### Fondos

#### Aurora / mesh gradient animado

Comunica atmósfera, no información. **Solo landing**. Coste: `blur-3xl` es `filter: blur(64px)` sobre una capa enorme — es el efecto más caro del recetario en móvil. Regla: anima **solo `transform`** de los blobs (el blur se calcula una vez y se compone), limita a 2-3 blobs, y desactívalo por debajo de `md`.

```css
@theme {
  --animate-blob: blob 18s ease-in-out infinite;
  @keyframes blob {
    0%, 100% { transform: translate3d(0, 0, 0) scale(1); }
    33%      { transform: translate3d(6%, -8%, 0) scale(1.12); }
    66%      { transform: translate3d(-7%, 5%, 0) scale(0.94); }
  }
}
```

```tsx
<div aria-hidden className="pointer-events-none absolute inset-0 -z-10 overflow-hidden">
  <div className="absolute -top-24 left-1/4 size-[38rem] rounded-full bg-violet-600/30 blur-3xl will-change-transform md:animate-blob motion-reduce:animate-none" />
  <div className="absolute top-1/3 right-0 size-[30rem] rounded-full bg-cyan-500/25 blur-3xl will-change-transform md:animate-blob [animation-delay:-6s] motion-reduce:animate-none" />
</div>
```

Mesh estático (barato, sin animación) para el resto de casos:

```css
.mesh {
  background:
    radial-gradient(at 20% 20%, oklch(0.70 0.18 280 / 0.55) 0px, transparent 50%),
    radial-gradient(at 80% 10%, oklch(0.75 0.16 200 / 0.45) 0px, transparent 50%),
    radial-gradient(at 60% 80%, oklch(0.72 0.20 330 / 0.45) 0px, transparent 50%),
    var(--color-neutral-950);
}
```

#### Patrón de puntos y de rejilla (con `mask-image` para desvanecer)

Estos **sí** valen en producto: son estáticos, cuestan un tile rasterizado, y dan profundidad a áreas vacías (empty states, canvas, fondos de dashboard).

```html
<!-- Puntos -->
<div aria-hidden class="pointer-events-none absolute inset-0 -z-10
  [background-image:radial-gradient(circle_at_1px_1px,var(--color-neutral-700)_1px,transparent_0)]
  [background-size:16px_16px]
  [mask-image:radial-gradient(ellipse_60%_50%_at_50%_0%,#000_60%,transparent_100%)]"></div>

<!-- Rejilla -->
<div aria-hidden class="pointer-events-none absolute inset-0 -z-10
  [background-image:linear-gradient(to_right,var(--color-neutral-800)_1px,transparent_1px),linear-gradient(to_bottom,var(--color-neutral-800)_1px,transparent_1px)]
  [background-size:40px_40px]
  [mask-image:linear-gradient(to_bottom,#000,transparent_85%)]"></div>
```

#### Noise / grain con `feTurbulence` en data URI

**Por qué mata el banding**: un degradado suave en 8 bits produce escalones discretos que el ojo detecta como franjas. Añadir ruido de ±1 LSB dithera el borde entre escalones y el ojo lo integra como continuo. Parámetros útiles: `baseFrequency` 0.6–1.0, `opacity` 0.15–0.35.

Coste: se rasteriza **una vez** y se tilea. Es prácticamente gratis — siempre que **no lo animes** (mover el grano fuerza repintado a pantalla completa cada frame).

```css
.grain {
  position: relative;
  isolation: isolate;
}
.grain::after {
  content: "";
  position: absolute;
  inset: 0;
  z-index: 1;
  pointer-events: none;
  opacity: 0.22;
  mix-blend-mode: overlay;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='160' height='160'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

#### Gradiente que respira

Anima la **opacidad de una segunda capa**, no `background-position` ni los color stops: opacity es compositable, lo otro repinta.

```css
@theme {
  --animate-breathe: breathe 9s ease-in-out infinite;
  @keyframes breathe { 0%, 100% { opacity: 0.35; } 50% { opacity: 0.8; } }
}
```

**Coste en móvil, de menor a mayor**: patrón de puntos/rejilla ≈ grain (gratis) < gradiente estático (gratis) < gradiente respirando (una capa compuesta) << aurora con blur (reserva 8-16 MB de textura GPU y repinta al redimensionar) <<< cualquier fondo WebGL/three.js (descártalo salvo en un hero de landing con `IntersectionObserver` que lo pause).

---

### Texto

#### Reveal por palabra con blur — el patrón "premium AI-era"

**Sí**: hero de landing, primera frase de una sección. **No**: nunca sobre párrafos largos ni sobre contenido que el usuario ya vio (al volver de otra pestaña). `once: true` es obligatorio.

```tsx
"use client";
import { motion } from "motion/react";

export function BlurReveal({ text, className = "" }: { text: string; className?: string }) {
  return (
    <p className={`text-balance ${className}`}>
      {text.split(" ").map((word, i) => (
        <motion.span
          key={i}
          className="inline-block will-change-[filter,transform,opacity]"
          initial={{ opacity: 0, filter: "blur(8px)", y: 8 }}
          whileInView={{ opacity: 1, filter: "blur(0px)", y: 0 }}
          viewport={{ once: true, amount: 0.4 }}
          transition={{ duration: 0.45, delay: Math.min(i, 24) * 0.035, ease: [0.22, 1, 0.36, 1] }}
        >
          {word}&nbsp;
        </motion.span>
      ))}
    </p>
  );
}
```

`Math.min(i, 24)` es el tope de seguridad: sin él, un título de 60 palabras tarda 2 s en terminar de aparecer.

#### Line-mask reveal

El clásico editorial: la línea sube desde detrás de un borde invisible.

```tsx
<span className="block overflow-hidden">
  <motion.span
    className="block"
    initial={{ y: "110%" }}
    whileInView={{ y: 0 }}
    viewport={{ once: true, amount: 0.6 }}
    transition={{ duration: 0.6, ease: [0.22, 1, 0.36, 1] }}
  >
    Una línea del título
  </motion.span>
</span>
```

#### Typewriter (CSS puro)

**Solo landing, solo una línea, solo texto de longitud fija.** Depende de `ch` (falla con tipografías proporcionales) y no es responsive. Para contenido real, usa JS.

```css
@theme {
  --animate-typing: typing 2.4s steps(24, end) forwards;
  --animate-caret: caret 0.8s step-end infinite;
  @keyframes typing { from { width: 0; } to { width: 24ch; } }
  @keyframes caret { 50% { border-color: transparent; } }
}
```
```html
<span class="inline-block w-0 overflow-hidden whitespace-nowrap border-r-2 border-current
             animate-typing motion-reduce:w-auto motion-reduce:animate-none">Texto de 24 caracteres.</span>
```

#### Texto con gradiente animado

Aquí **no** hace falta `@property`: `background-position` es interpolable de forma nativa.

```css
@theme {
  --animate-text-gradient: text-gradient 4s linear infinite;
  @keyframes text-gradient { to { background-position: 200% center; } }
}

.text-gradient {
  background-image: linear-gradient(90deg,
    var(--color-violet-400), var(--color-cyan-300), var(--color-violet-400));
  background-size: 200% auto;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: var(--animate-text-gradient);
}
@media (prefers-reduced-motion: reduce) { .text-gradient { animation: none; } }
```

Variante **text shimmer** (el "pensando…" de los chats de IA): mismo mecanismo, con una banda clara estrecha.

```css
.text-shimmer {
  background-image: linear-gradient(90deg,
    var(--color-neutral-500) 0 45%,
    var(--color-neutral-50) 50%,
    var(--color-neutral-500) 55% 100%);
  background-size: 250% 100%;
  -webkit-background-clip: text; background-clip: text; color: transparent;
  animation: text-shimmer 2s linear infinite;
}
@keyframes text-shimmer { from { background-position: 150% 0; } to { background-position: -50% 0; } }
```

> `background-clip: text` obliga a `color: transparent`. Si el degradado falla, el texto desaparece: en contextos críticos envuélvelo en `@supports (background-clip: text) or (-webkit-background-clip: text)`.

#### Number ticker / count-up

**Sí**: métricas de una landing, cierre de un dashboard mensual. **No**: valores que se refrescan solos cada 5 s — el usuario nunca llega a leer el número. Y `tabular-nums` no es opcional: sin él el ancho salta en cada frame y el layout tiembla.

```tsx
"use client";
import { useEffect, useRef } from "react";
import { animate, motion, useInView, useMotionValue, useTransform } from "motion/react";

export function NumberTicker({ value, decimals = 0 }: { value: number; decimals?: number }) {
  const ref = useRef<HTMLSpanElement>(null);
  const inView = useInView(ref, { once: true, amount: 0.5 });
  const mv = useMotionValue(0);
  const text = useTransform(mv, (v) =>
    new Intl.NumberFormat("es-MX", {
      minimumFractionDigits: decimals,
      maximumFractionDigits: decimals,
    }).format(v)
  );

  useEffect(() => {
    if (!inView) return;
    const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    if (reduce) { mv.set(value); return; }
    const controls = animate(mv, value, { duration: 1.2, ease: [0.16, 1, 0.3, 1] });
    return () => controls.stop();
  }, [inView, value, mv]);

  return <motion.span ref={ref} className="tabular-nums tracking-tight">{text}</motion.span>;
}
```

#### `text-wrap` para títulos

```css
h1, h2, h3, .title { text-wrap: balance; }  /* reparte las líneas, mata la huérfana */
p, li            { text-wrap: pretty; }     /* solo evita la última palabra sola */
```
Tailwind: `text-balance` / `text-pretty`. **Nunca `balance` en párrafos**: Chromium corta el algoritmo a ~6 líneas por coste y el resultado es incoherente.

---

### Listas y grillas

#### Stagger de entrada

Regla dura: **el último elemento no puede tardar más de ~400 ms** en aparecer. Con 30 filas y 50 ms de delay, la última entra 1.5 s tarde y se siente rota.

```tsx
"use client";
import { motion } from "motion/react";

const container = { hidden: {}, show: { transition: { staggerChildren: 0.035, delayChildren: 0.05 } } };
const item = {
  hidden: { opacity: 0, y: 8 },
  show: { opacity: 1, y: 0, transition: { duration: 0.3, ease: [0.22, 1, 0.36, 1] } },
};

export function StaggerList({ rows }: { rows: { id: string; label: string }[] }) {
  return (
    <motion.ul variants={container} initial="hidden" animate="show">
      {rows.map((r) => <motion.li key={r.id} variants={item}>{r.label}</motion.li>)}
    </motion.ul>
  );
}
```

Sin JS, con `sibling-index()` (**Chrome/Edge 138+, Safari 26.2, Firefox no** — envolver siempre):

```css
@supports (animation-delay: calc(sibling-index() * 1ms)) {
  @media (prefers-reduced-motion: no-preference) {
    .stagger > * {
      animation: enter 300ms cubic-bezier(0.22, 1, 0.36, 1) both;
      animation-delay: calc(min(sibling-index(), 10) * 35ms);
    }
    @keyframes enter { from { opacity: 0; translate: 0 0.5rem; } }
  }
}
```

#### `@formkit/auto-animate` — add / remove / reorder sin configurar nada

3.28 kB gzip, cero dependencias, **respeta `prefers-reduced-motion` por defecto**. Es el único paquete de este recetario que resuelve un problema cotidiano de app interna (filtros, tablas reordenables, drag & drop) con una línea. Versión actual **0.10.0** (jul-2026), sigue pre-1.0 con cadencia lenta pero mantenida.

```bash
pnpm add @formkit/auto-animate
```
```tsx
"use client";
import { useAutoAnimate } from "@formkit/auto-animate/react";

export function Rows({ items }: { items: { id: string; name: string }[] }) {
  const [parent] = useAutoAnimate<HTMLUListElement>({ duration: 200, easing: "ease-out" });
  return (
    <ul ref={parent}>
      {items.map((i) => <li key={i.id}>{i.name}</li>)}
    </ul>
  );
}
```
Devuelve `[ref, enable]`; `enable(false)` lo apaga en caliente (útil durante una carga masiva).

#### Atenuar hermanos al hover con `:has()`

Comunica foco sin mover nada. Barato **si acotas el selector**. En una tabla de 500 filas, el motor reevalúa `:has()` en cada `mousemove` sobre el contenedor: limítalo a grillas de ≤40 elementos.

```css
@media (hover: hover) {
  .cards:has(.card:hover) .card:not(:hover) {
    opacity: 0.55;
    filter: saturate(0.7);
  }
  .cards .card { transition: opacity 200ms, filter 200ms; }
}
```

#### Marquee infinito

Tres piezas obligatorias: **contenido duplicado exactamente una vez** (y la copia con `aria-hidden`), **traslación de `-50%`** (no `-100%`), y **`mask-image` en los bordes** para que no aparezca ni desaparezca de golpe. Cuarta pieza que casi todos olvidan: **pausarlo fuera del viewport**; una animación infinita a 60 fps en una sección que nadie está mirando quema batería.

```css
@theme {
  --animate-marquee: marquee var(--marquee-duration, 40s) linear infinite;
  @keyframes marquee { to { transform: translateX(-50%); } }
}
```

```tsx
"use client";
import { useEffect, useRef } from "react";

export function Marquee({ children }: { children: React.ReactNode }) {
  const ref = useRef<HTMLDivElement>(null);
  useEffect(() => {
    const el = ref.current;
    if (!el) return;
    const io = new IntersectionObserver(
      ([e]) => { el.style.animationPlayState = e.isIntersecting ? "running" : "paused"; },
      { threshold: 0 }
    );
    io.observe(el);
    return () => io.disconnect();
  }, []);

  return (
    <div className="group relative overflow-hidden
      [mask-image:linear-gradient(to_right,transparent,#000_8%,#000_92%,transparent)]">
      <div
        ref={ref}
        className="flex w-max gap-8 animate-marquee will-change-transform
                   group-hover:[animation-play-state:paused] motion-reduce:animate-none"
      >
        <div className="flex shrink-0 gap-8">{children}</div>
        <div className="flex shrink-0 gap-8" aria-hidden>{children}</div>
      </div>
    </div>
  );
}
```

---

### Interacción del puntero

> **Aviso transversal**: botón magnético, tilt 3D y cursor custom son efectos de **landing**. En una app de trabajo diario introducen latencia percibida (el objetivo del click se mueve mientras apuntas — ley de Fitts en contra), rompen las affordances nativas del cursor, y no existen en táctil. Enciérralos en `@media (hover: hover) and (pointer: fine)`.

#### Botón magnético

```tsx
"use client";
import { useRef } from "react";
import { motion, useMotionValue, useSpring } from "motion/react";

export function MagneticButton({ children, strength = 0.35 }: { children: React.ReactNode; strength?: number }) {
  const ref = useRef<HTMLButtonElement>(null);
  const mx = useMotionValue(0), my = useMotionValue(0);
  const spring = { stiffness: 260, damping: 22, mass: 0.4 };
  const x = useSpring(mx, spring), y = useSpring(my, spring);

  return (
    <motion.button
      ref={ref}
      style={{ x, y }}
      onPointerMove={(e) => {
        if (!window.matchMedia("(hover: hover) and (pointer: fine)").matches) return;
        const r = ref.current!.getBoundingClientRect();
        mx.set((e.clientX - (r.left + r.width / 2)) * strength);
        my.set((e.clientY - (r.top + r.height / 2)) * strength);
      }}
      onPointerLeave={() => { mx.set(0); my.set(0); }}
      className="rounded-full bg-white px-6 py-3 font-medium text-neutral-900"
    >
      {children}
    </motion.button>
  );
}
```

#### Tilt 3D

`perspective` va en el **padre**; las rotaciones en el hijo. Sin `perspective` la rotación se ve plana.

```tsx
"use client";
import { useRef } from "react";
import { motion, useMotionValue, useSpring, useTransform } from "motion/react";

export function Tilt({ children, max = 8 }: { children: React.ReactNode; max?: number }) {
  const ref = useRef<HTMLDivElement>(null);
  const px = useMotionValue(0), py = useMotionValue(0);
  const s = { stiffness: 220, damping: 20 };
  const rx = useSpring(useTransform(py, [-0.5, 0.5], [`${max}deg`, `-${max}deg`]), s);
  const ry = useSpring(useTransform(px, [-0.5, 0.5], [`-${max}deg`, `${max}deg`]), s);

  return (
    <div ref={ref} style={{ perspective: 900 }}
      onPointerMove={(e) => {
        const r = ref.current!.getBoundingClientRect();
        px.set((e.clientX - r.left) / r.width - 0.5);
        py.set((e.clientY - r.top) / r.height - 0.5);
      }}
      onPointerLeave={() => { px.set(0); py.set(0); }}
    >
      <motion.div style={{ rotateX: rx, rotateY: ry, transformStyle: "preserve-3d" }}>
        {children}
      </motion.div>
    </div>
  );
}
```

#### Ripple al click

El único de esta familia defendible en producto: confirma que el tap **llegó**, y en móvil con red lenta eso vale oro.

```css
@theme {
  --animate-ripple: ripple 600ms cubic-bezier(0.22, 1, 0.36, 1) forwards;
  @keyframes ripple { to { transform: scale(14); opacity: 0; } }
}
```
```tsx
"use client";
import { useState } from "react";

export function RippleButton({ children, ...props }: React.ComponentProps<"button">) {
  const [drops, setDrops] = useState<{ id: number; x: number; y: number }[]>([]);
  return (
    <button
      {...props}
      className="relative isolate overflow-hidden rounded-lg bg-violet-600 px-4 py-2 text-white"
      onPointerDown={(e) => {
        const r = e.currentTarget.getBoundingClientRect();
        setDrops((d) => [...d, { id: Date.now(), x: e.clientX - r.left, y: e.clientY - r.top }]);
      }}
    >
      {drops.map((d) => (
        <span key={d.id} aria-hidden
          onAnimationEnd={() => setDrops((s) => s.filter((i) => i.id !== d.id))}
          className="pointer-events-none absolute size-3 -translate-x-1/2 -translate-y-1/2 rounded-full bg-white/40 animate-ripple motion-reduce:hidden"
          style={{ left: d.x, top: d.y }} />
      ))}
      {children}
    </button>
  );
}
```

#### Cursor custom

Úsalo solo en un portfolio o una landing de agencia. Rompe: el I-beam sobre texto, el cursor de resize, el `grab` de un mapa, el cursor de sistema en alto contraste. Y siempre va un frame detrás del real, porque el real lo dibuja el compositor del SO. Si lo haces: `cursor: none` **solo** dentro del contenedor del efecto, nunca en `body`.

---

### Transiciones de contenido

#### View Transitions API — same-document

```ts
export function withViewTransition(update: () => void) {
  const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
  if (reduce || !document.startViewTransition) { update(); return; }
  document.startViewTransition(update);
}
```

```css
::view-transition-old(root) { animation: 90ms cubic-bezier(0.4,0,1,1) both vt-fade-out; }
::view-transition-new(root) { animation: 210ms cubic-bezier(0,0,0.2,1) 60ms both vt-fade-in; }
@keyframes vt-fade-out { to { opacity: 0; } }
@keyframes vt-fade-in  { from { opacity: 0; } }

/* shared element: mismo nombre en origen y destino, ÚNICO por documento */
.thumb   { view-transition-name: var(--vt-name); }
.hero-img{ view-transition-name: var(--vt-name); }

::view-transition-old(hero),
::view-transition-new(hero) { object-fit: cover; overflow: clip; }
```

#### View Transitions — cross-document (MPA)

**Chrome 126+, Safari 18.2+, Firefox no.** El `<meta name="view-transition">` está **deprecado**: si lo ves en un tutorial, el tutorial es viejo y falla en silencio.

```css
@media (prefers-reduced-motion: no-preference) {
  @view-transition { navigation: auto; }
}
```
```js
window.addEventListener("pageswap", (e) => { if (!e.viewTransition) return; /* marcar nombres salientes */ });
window.addEventListener("pagereveal", (e) => { if (!e.viewTransition) return; /* marcar nombres entrantes */ });
```
Tres trampas documentadas: (1) el meta deprecado; (2) **timeout de 4 s** — si la página destino no es renderizable a tiempo, la transición muere sin error y la página aparece de golpe (fuentes bloqueantes e imágenes lentas son la causa habitual); (3) las capturas son **bitmaps**: con `object-fit: fill` por defecto, cualquier cambio de aspect ratio deforma la imagen.

#### `React ViewTransition` en Next.js

Sigue **experimental y solo en canary** a mediados de 2026. Next.js App Router sí sirve builds canary de React, así que el import funciona con la bandera puesta.

```ts
// next.config.ts
export default { experimental: { viewTransition: true } };
```
```tsx
import { unstable_ViewTransition as ViewTransition } from "react";
<ViewTransition name="hero"><img … /></ViewTransition>
```
**Recomendación para producción**: envía `document.startViewTransition` (API de navegador, estable) y reserva el componente de React para prototipos.

#### `@starting-style` + `allow-discrete` — animar la entrada/salida de `display: none`

```css
.panel {
  opacity: 1;
  translate: 0 0;
  transition: opacity 200ms, translate 200ms, display 200ms allow-discrete;
}
.panel[data-closed] {
  opacity: 0;
  translate: 0 -4px;
  display: none;
}
@starting-style {
  .panel { opacity: 0; translate: 0 -4px; }
}
```

> **El gotcha de Josh Comeau**: `@starting-style` obedece la especificidad normal de CSS (a diferencia de los keyframes, que viven en su propia cascada de alta prioridad). Si tu JS escribe estilos **inline** en el elemento — típico en partículas o posicionamiento dinámico — el inline gana y el starting style nunca se aplica: la transición simplemente no ocurre, sin error. Solución recomendada: usar un `@keyframes` con `from` en lugar de `@starting-style`.

#### `popover` nativo

`overlay` debe estar en la lista de `transition` con `allow-discrete`, si no el elemento sale del *top layer* al instante y la animación de salida se ve cortada.

```html
<button popovertarget="menu">Abrir</button>
<div id="menu" popover class="menu">…</div>
```
```css
.menu {
  opacity: 0; transform: scale(0.96);
  transition: opacity 150ms, transform 150ms,
              overlay 150ms allow-discrete, display 150ms allow-discrete;
}
.menu:popover-open { opacity: 1; transform: scale(1); }
@starting-style { .menu:popover-open { opacity: 0; transform: scale(0.96); } }
.menu::backdrop { background: rgb(0 0 0 / 0.4); backdrop-filter: blur(2px); }
```

#### `interpolate-size` para `height: auto`

**Chromium 129+ únicamente** (no Firefox, no Safari, a 2026). Necesita fallback sí o sí.

```css
:root { interpolate-size: allow-keywords; }

details::details-content {
  block-size: 0;
  overflow: clip;
  transition: block-size 250ms ease, content-visibility 250ms allow-discrete;
}
details[open]::details-content { block-size: auto; }
```

Fallback universal (`0fr → 1fr`), que funciona en todos los navegadores desde 2023 y es el que deberías usar por defecto:

```css
.collapse { display: grid; grid-template-rows: 0fr; transition: grid-template-rows 250ms cubic-bezier(0.22,1,0.36,1); }
.collapse > * { overflow: hidden; min-height: 0; }
.collapse[data-open] { grid-template-rows: 1fr; }
```

---

### Scroll

> **El gotcha que rompe páginas en Firefox**: si escribes `.reveal { opacity: 0; animation-timeline: view(); }` sin `@supports`, en un navegador sin scroll-driven animations el elemento se queda en `opacity: 0` **para siempre** — el contenido desaparece. Por eso el estado por defecto debe ser **el visible**, y la animación se añade solo dentro del `@supports`.

#### Barra de progreso de lectura (CSS puro, cero JS)

```css
@supports (animation-timeline: scroll()) {
  @media (prefers-reduced-motion: no-preference) {
    .reading-progress {
      position: fixed;
      inset: 0 0 auto 0;
      z-index: 50;
      height: 2px;
      background: var(--color-violet-500);
      transform-origin: 0 50%;
      animation: grow-progress linear both;
      animation-timeline: scroll(root block);
    }
    @keyframes grow-progress { from { transform: scaleX(0); } to { transform: scaleX(1); } }
  }
}
```

#### Reveal al entrar en viewport

```css
.reveal { opacity: 1; translate: 0 0; }   /* estado por defecto = visible */

@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .reveal {
      animation: reveal-in linear both;
      animation-timeline: view();
      animation-range: entry 10% cover 35%;
    }
    @keyframes reveal-in {
      from { opacity: 0; translate: 0 1.5rem; }
      to   { opacity: 1; translate: 0 0; }
    }
  }
}
```

Ventaja frente a `whileInView` de Motion: corre en el hilo del compositor (en Safari desde 26.4 también threaded), no dispara re-renders de React, y sobrevive al scroll rápido sin "saltar".

#### Sticky sections

```css
.chapter { position: sticky; top: 0; min-height: 100svh; display: grid; place-items: center; }
```
Usa `svh`, no `vh`: en móvil, `vh` no contempla la barra de URL que aparece y desaparece, y produce un salto de layout en pleno scroll.

#### Por qué el scroll-jacking está prohibido

Secuestrar el scroll (interceptar `wheel`/`touchmove` para mover N píxeles fijos) rompe, todo a la vez: la velocidad esperada por el usuario, la inercia del trackpad, la navegación por teclado (`Space`, `PgDn`, `Home`), el `Ctrl+F` del navegador, el *scroll anchoring*, los lectores de pantalla, y el botón "atrás" con restauración de posición. Además la métrica INP se dispara porque el handler corre en el hilo principal en cada evento. **Regla**: en producto, prohibido sin excepción; en landing, defendible solo en una narrativa lineal única, con salida obvia y con `prefers-reduced-motion` desactivándolo.

---

### Feedback y celebración

#### Confetti

**Cuándo NO** (que es casi siempre): cualquier acción que el usuario repite más de 3-5 veces al día; guardar un formulario; un éxito parcial con advertencias; nada que ocurra dentro de un flujo de trabajo. El confetti que se ve dos veces es encantador; el que se ve cuarenta es una interrupción. **Cuándo sí**: hitos únicos e irrepetibles — onboarding completado, primer envío creado, meta anual alcanzada.

`disableForReducedMotion` viene en `false` **por defecto** en `canvas-confetti`: si no lo pasas explícitamente, estás ignorando la preferencia del usuario.

```ts
import confetti from "canvas-confetti";

confetti({
  particleCount: 60,
  spread: 55,
  startVelocity: 32,
  ticks: 120,
  origin: { y: 0.7 },
  disableForReducedMotion: true,   // NO es el default
});
```
Además: el canvas debe ir con `pointer-events: none` y por encima de todo, pero nunca debe tapar el botón de "siguiente paso".

#### Success checkmark dibujado con `pathLength`

Motion normaliza `pathLength` a 0–1 sin importar la longitud real del trazo, así que no necesitas medir el path.

```tsx
"use client";
import { motion } from "motion/react";

export function SuccessCheck({ className = "size-8" }: { className?: string }) {
  return (
    <motion.svg viewBox="0 0 24 24" className={className} fill="none"
      stroke="currentColor" strokeWidth={2.5} strokeLinecap="round" strokeLinejoin="round"
      role="img" aria-label="Listo">
      <motion.circle cx="12" cy="12" r="10"
        initial={{ pathLength: 0 }} animate={{ pathLength: 1 }}
        transition={{ duration: 0.45, ease: "easeOut" }} />
      <motion.path d="m7.5 12.5 3 3 6-6"
        initial={{ pathLength: 0 }} animate={{ pathLength: 1 }}
        transition={{ duration: 0.28, delay: 0.35, ease: "easeOut" }} />
    </motion.svg>
  );
}
```

Versión CSS pura: pon `pathLength="1"` como atributo en el SVG y anima `stroke-dashoffset` de `1` a `0` con `stroke-dasharray: 1`.

#### Shake de error

Nunca como único canal: el shake acompaña al mensaje de texto, no lo sustituye. Y una sola vez — un shake que se repite es un tic.

```css
@theme {
  --animate-shake: shake 400ms cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  @keyframes shake {
    10%, 90%       { translate: -1px 0; }
    20%, 80%       { translate:  2px 0; }
    30%, 50%, 70%  { translate: -4px 0; }
    40%, 60%       { translate:  4px 0; }
  }
}
```
```tsx
<input aria-invalid={hasError} className="aria-invalid:animate-shake aria-invalid:border-red-500 motion-reduce:animate-none" />
```

#### Badge que pulsa

**Máximo uno visible por pantalla**, y solo si hay algo genuinamente en vivo detrás.

```html
<span class="relative flex size-2">
  <span class="absolute inline-flex size-full animate-ping rounded-full bg-emerald-400 opacity-75 motion-reduce:hidden"></span>
  <span class="relative inline-flex size-2 rounded-full bg-emerald-500"></span>
</span>
```

---

### Dónde copiar

| Colección | Licencia | Instalación | Enfoque real | Componentes estrella | Peso |
|---|---|---|---|---|---|
| **Magic UI** (magicui.design) | **MIT** limpio. Pro = $199 lifetime, solo *secciones y templates* | `pnpm dlx shadcn@latest add @magicui/<name>` | **Marketing**. Tailwind v4 + React 19 por defecto (v3 vive en `v3.magicui.design`) | `Border Beam`, `Marquee`, `Number Ticker`, `Animated Beam`, `Shimmer Button`, `Blur Fade`, `Text Animate` | `motion`. Por componente: `cobe`, `canvas-confetti`, `react-tweet`. Sin three.js ni GSAP |
| **Aceternity UI** (ui.aceternity.com) | ⚠️ **NO es MIT** — licencia propietaria propia (`/licence`). Puedes usarla en productos finales y venderlos; **prohibido** redistribuir los componentes como fuente o crear themes/templates para vender | Copy-paste (CLI de shadcn opcional). Sin paquete npm | **El más agresivamente landing** de todos. ~105 componentes gratis; los "200+ blocks" son de pago ($169/año, $199 lifetime, $1,590 team) | `Spotlight`, `3D Card Effect`, `Background Beams With Collision`, `Infinite Moving Cards`, `Text Generate Effect`, `Aurora Background`, `Glowing Effect`, `Tracing Beam` | `motion` + Tailwind v4. Varios con **three.js** (`GitHub Globe`, `Canvas Reveal Effect`, `Vortex`, `Dither Shader`) |
| **React Bits** (reactbits.dev) | ⚠️ **MIT + Commons Clause** — no es open source OSI. Uso comercial en tu producto: sí. Vender/redistribuir los componentes o una versión portada: **no** | `npx shadcn@latest add @react-bits/<Name>-TS-TW`, CLI `jsrepo`, o copy-paste. **4 variantes por componente**: `JS-CSS`, `JS-TW`, `TS-CSS`, `TS-TW` | Creativo/WebGL. 45.5k ★, el más popular. Ports a Vue y Svelte | `BlurText` (único confirmable: el sitio es SPA client-rendered y no expone el catálogo a fetch) | **El más pesado**: `three`, `gsap`, `ogl`, `@react-three/fiber`, `@react-three/drei`, `matter-js`, `lenis`, `motion`. Auditar componente por componente |
| **Motion Primitives** (motion-primitives.com) | **MIT**. Pro = $149 pago único | `npx shadcn@latest add "https://motion-primitives.com/c/<name>.json"` | ✅ **El único claramente orientado a producto/app.** 33 componentes, repo en beta | `Text Shimmer`, `Animated Background`, `Sliding Number`, `Morphing Dialog`, `Transition Panel`, `Border Trail`, `Progressive Blur`, `In View`, `Scroll Progress` | **El más ligero**: solo `motion` + Tailwind. Cero WebGL. *(Soporte Tailwind v4 no verificable: el sitio bloquea el acceso automatizado)* |
| **Animate UI** (animate-ui.com) | **MIT**, todo gratis, sin tier de pago | `npx shadcn@latest add @animate-ui/components-<name>` | ✅ **El mejor encaje para app**, por una razón distinta: **no reemplaza a shadcn, lo anima**. Publica cada componente en namespaces separados para **Base UI**, **Radix UI** y **Headless UI** | `Sliding Number`, `Liquid Button`, `Copy Button`, `Files`, `Management Bar`, `Notification List`; y versiones animadas de `Sidebar`, `Dialog`, `Tabs`, `Tooltip` | `motion` + la primitiva que elijas. Sin three.js ni GSAP |
| **Cult UI** (cult-ui.com) | **MIT**. Pro = $129 lifetime | `components.json` con `"@cult-ui": "https://cult-ui.com/r/{name}.json"` → `npx shadcn@beta add @cult-ui/<name>`. **Tiene servidor MCP propio** | Pivotó en 2026: el README ahora lidera con **"92+ AI agent patterns"**, no con animaciones. Menos librería de motion, más bloques para apps de IA | `Texture Card` (único confirmable; el índice devolvió 429 repetidamente) | `motion`, `recharts`, `zod`, `lucide-react` y **AI SDK** (`@ai-sdk/react`, `@ai-sdk/openai`) para los patrones de agente |
| **21st.dev** | ⚠️ **Marketplace comunitario, no colección de autor.** La MIT cubre **la plataforma**; los componentes de la comunidad **no declaran licencia general** y sus autores pueden venderlos. **Verificar caso por caso** | `npx shadcn@latest add "https://21st.dev/r/<autor>/<name>"`; integra Magic MCP | 12,000+ componentes de múltiples autores, calidad desigual por definición. Filtra por estado `featured` | Variable. Pipeline `on_review` → `posted` → `featured` | Variable. Base declarada: shadcn/ui + Radix + Tailwind. **Tailwind v4 no garantizado centralmente**. Navegar e instalar es gratis; se pagan los créditos de IA ($6–$15/mes) y las funciones de equipo |
| **tw-animate-css** | **MIT** | `npm i -D tw-animate-css` → `@import "tw-animate-css";` | Reemplazo CSS-first de `tailwindcss-animate` para Tailwind v4. **Ya viene por defecto en proyectos nuevos de shadcn** | Utilidades `duration-*`, `ease-*`, `delay-*`, `repeat-*`, `fill-mode-*`, `paused`; transformaciones blur/fade/zoom/spin/slide in-out; `accordion-down/up`, `collapsible-down/up`, `caret-blink` | **47 kB unpacked, cero dependencias**, CSS puro. v1.4.0 (sep-2025). ⚠️ Si usas la opción `prefix` de Tailwind, importa `tw-animate-css/prefix` |

**Advertencia explícita — efectos "de landing" que arruinan una app de trabajo diario.** Cualquiera de estos, metido en un dashboard que alguien usa 6 horas al día, degrada la herramienta: border beam permanente, aurora/mesh animado, marquee, meteors, retro grid, background beams, spotlight sobre cada card, tilt 3D en las filas, cursor custom, texto con gradiente en bucle, typewriter, count-up en métricas que se refrescan solas, confetti en acciones repetibles, y cualquier fondo WebGL. El criterio operativo: **si se mueve sin que el usuario haga nada, y sigue moviéndose después de 10 segundos, no va en producto.**

Orden de utilidad si el destino es una app interna: **Animate UI** (anima las primitivas que ya usas; su namespace Base UI está alineado con el cambio de default de shadcn de julio 2026) → **Motion Primitives** (dependencia mínima, catálogo genuinamente de producto) → **`@formkit/auto-animate`** (3 kB, resuelve listas y tablas reordenables hoy). Magic UI, Aceternity y React Bits: solo para la landing.

---

### Presupuesto de efectos

**La regla, concreta:**

1. **Una sola animación en bucle infinito visible al mismo tiempo por pantalla.** Dos o más y el ojo no sabe dónde posarse — es la definición operativa de "se ve a circo". Spinners y barras indeterminadas cuentan como bucle; por eso una tabla con 12 spinners es un error, no un detalle.
2. **Máximo un efecto "protagonista"** (el que la gente describiría al contarte la página: el hero con aurora, o el border beam de la card de pricing, o el marquee de logos — **uno**).
3. **Micro-interacciones disparadas por el usuario: sin límite.** Duran <200 ms, tienen causa evidente y no compiten por atención. Hover, focus, press, ripple, checkmark: pon todos los que quieras.
4. **En producto (dashboard, tabla, formulario): cero efectos ambientales.** El presupuesto de "protagonista" se gasta en el estado de carga, y nada más.

**Presupuesto de rendimiento (límites duros):**

- Cero animaciones que toquen layout: `width`, `height`, `top`, `left`, `margin`, `padding`. Solo `transform`, `opacity`, `filter`, `clip-path`.
- Máximo **3 capas** con `filter: blur()` grande simultáneas; **cero blur animado** (animar el radio del blur re-renderiza la textura cada frame).
- `backdrop-filter`: máximo dos elementos a la vez, y nunca sobre un contenedor con scroll.
- `will-change` solo sobre elementos que **de verdad** están animando; déjalo puesto en 30 cards y reservas 30 capas de GPU permanentes.
- Todo lo infinito que quede fuera del viewport, en pausa (`IntersectionObserver` → `animationPlayState`).

**Se apagan con `prefers-reduced-motion: reduce`** (paralaje, marquee, aurora/blobs, border beam, tilt, magnetic, ripple, confetti, typewriter, gradiente de texto en bucle, `animate-ping`, reveal por scroll, y el count-up — que debe mostrar el valor **final** de inmediato, no quedarse en cero).

**Se conservan**: spinners y barras indeterminadas (comunican estado del sistema; como mucho, ralentízalos), focus rings, cambios de color, y el fade de entrada/salida de modales y toasts reducido a ≤100 ms — quitar toda transición de un modal lo hace *aparecer de golpe*, que para muchos usuarios sensibles es peor que un fade corto.

```css
/* Base segura: acorta en vez de eliminar, y devuelve el movimiento a lo que lo necesita */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
  /* excepciones: sin esto, los indicadores de carga se congelan */
  .animate-spin, .animate-spin-slow, [role="progressbar"] > * {
    animation-duration: 1.2s !important;
    animation-iteration-count: infinite !important;
  }
}
```

Y en JS, un helper único para no repetir el `matchMedia` por todos lados:

```ts
export const prefersReducedMotion = () =>
  typeof window !== "undefined" &&
  window.matchMedia("(prefers-reduced-motion: reduce)").matches;
```

---
---

## Checklist de cierre — el filtro obligatorio

> Este checklist cruza las **dos partes** del curso (movimiento + diseño visual) y es la condición para dar por terminada **cualquier** pantalla. No es una lista de deseos: es el mínimo. Si un punto no aplica, se dice por qué; no se salta en silencio.

### A. Antes de escribir la primera línea

- [ ] ¿Qué **decide** el usuario en esta pantalla? Si no hay una decisión clara, la pantalla no está diseñada, está rellenada.
- [ ] ¿Cuál es **LA** acción primaria? Una, y una sola.
- [ ] ¿Qué densidad corresponde (compacta / default / cómoda) y por qué?
- [ ] ¿Existe ya un patrón para esto en el producto? Si existe, se copia; no se inventa una variante.

### B. Sistema (nada se decide dos veces)

- [ ] Ningún color, duración, curva, espaciado o radio **hardcodeado**: todo sale de un token.
- [ ] Ningún componente lee un primitivo (`--blue-9`); solo tokens semánticos.
- [ ] Máximo tres radios + `full`. Radios anidados con `R_int = R_ext − padding`.
- [ ] Bordes con alpha (6–12% claro / 8–15% oscuro). El borde de un **control** va aparte, a ~42% (3:1 real, WCAG 1.4.11).
- [ ] Tres niveles de color de texto. No ocho.
- [ ] Sombras en capas, con el hue del fondo. En dark mode, elevación por **color** (+3–4 L), no por sombra.
- [ ] Escala de `z-index` nombrada. Todo lo que flota va en **portal**.

### C. Jerarquía y ritmo

- [ ] Hay un elemento dominante (60–70% del peso visual).
- [ ] El espacio DENTRO de un grupo es ≥1.5× menor que el espacio ENTRE grupos. Prueba del blur.
- [ ] Lo secundario se apaga por **color y peso**, no se achica.
- [ ] Un solo botón sólido de acento visible. Cancelar nunca pesa más que Confirmar.
- [ ] Ninguna card anidada dentro de otra card.

### D. Cada control

- [ ] Los **7 estados**: default, hover, active, focus-visible, disabled, loading, selected.
- [ ] Press state (`scale 0.97`) en todo lo clickeable.
- [ ] Focus ring visible con `box-shadow`/`outline` + offset, en **todo** lo interactivo.
- [ ] Hit area ≥24×24 (ideal 44×44), con pseudo-elemento si el visual es menor.
- [ ] `<button>` de verdad (nunca `<div onClick>`), con `type="button"` si no envía.
- [ ] Icon-only con `aria-label`.
- [ ] El componente correcto según el árbol de decisión (switch ≠ checkbox ≠ radio ≠ select ≠ dropdown).
- [ ] Teclado completo del patrón: un radiogroup es **una** parada de Tab; `Esc` cierra y devuelve el foco; los menús tienen typeahead.

### E. Formularios

- [ ] Label visible arriba. Placeholder como ejemplo, nunca como instrucción.
- [ ] Ancho del campo proporcional al dato esperado.
- [ ] `name`, `autocomplete`, `inputmode`, `enterkeyhint` puestos.
- [ ] Validación: blur la primera vez, cada tecla una vez que hay error. Nunca mientras se escribe por primera vez.
- [ ] Mensaje que dice **qué pasó y cómo arreglarlo**, espejando el label. Color + icono + texto.
- [ ] El submit no se deshabilita por validación; al fallar, foco al primer error.
- [ ] Nunca se pierde lo capturado: ni al fallar, ni al recargar, ni al volver atrás.

### F. Movimiento (Parte A)

- [ ] Ninguna aparición en seco: todo cambio visual entra animado.
- [ ] UI <300ms; salidas más cortas que entradas; feedback inmediato ~100ms.
- [ ] Solo `transform` y `opacity` en interacciones.
- [ ] Nada nace de `scale(0)` ni del centro de la nada: origen en el trigger.
- [ ] Dirección con significado (adelante/atrás son espejo).
- [ ] Lo que se mueve entre estados usa `layoutId`, no aparece y desaparece.
- [ ] Acciones frecuentes y teclado: sin animación.
- [ ] Una sola animación en bucle visible por pantalla. Cero efectos ambientales en producto.

### G. Estados de la pantalla

- [ ] **Vacío de primer uso**, **vacío por filtros**, **error** y **carga**, los cuatro con copy real en español y con acción.
- [ ] Nada de loader por debajo de 300ms; skeleton que **calca** el layout real.
- [ ] Error de servidor con reintento que no pierde datos.
- [ ] Estado parcial/offline resuelto, o descartado explícitamente.

### H. Accesibilidad y dispositivo

- [ ] Contraste: 4.5:1 texto, 3:1 componentes y estados.
- [ ] El color **nunca** es el único canal. Prueba en escala de grises.
- [ ] `prefers-reduced-motion`: se **reemplaza** movimiento por fades — y los **spinners y barras de progreso siguen girando** (son estado del sistema, no decoración).
- [ ] Inputs ≥16px en móvil; safe areas en barras fijas; `h-dvh` en el shell.
- [ ] Zoom al 200% sin romper.
- [ ] Nada esencial vive solo en `hover` (en táctil no existe).
- [ ] `min-w-0` en todo lo que trunca; `tabular-nums` en toda cifra que cambia.

### I. Trampas comprobadas — cada una nos mordió de verdad

No son hipótesis: son errores que ya cometimos y que el usuario detectó antes que nosotros.

- [ ] **`cursor: pointer` en los botones.** Tailwind v4 lo **quitó de su preflight**: sin una regla base propia, todo lo clickeable se ve con la flecha de "aquí no pasa nada". Cubrir `button`, `select`, `a[href]`, `summary`, `[role=button]`, `[role=radio]`, y `not-allowed` en los deshabilitados.
- [ ] **El anillo de foco recortado.** Se dibuja 4px POR FUERA del control; cualquier contenedor con `overflow` se lo come. Todo contenedor con scroll necesita ≥4px de colchón horizontal. Se detecta tabulando, no mirando.
- [ ] **El `<select>` nativo delata al sistema operativo.** Y antes que el estilo, revisar el árbol de decisión: **2–5 opciones son radios o segmented control, no un select.** Un select de 3 opciones cuesta dos clics y esconde algo que cabía a la vista.
- [ ] **Ayuda y error NUNCA dentro del `<label>`.** Si van dentro, el lector de pantalla los lee como parte del nombre del campo. Van por `aria-describedby`.
- [ ] **`aria-pressed` no es `aria-checked`.** `pressed` es para toggles independientes; una selección única es `role="radio"` + `aria-checked` y **una sola parada de Tab**.
- [ ] **`<button>` sin `type` dentro de un form** envía el formulario. Es el bug #1 de React y no da ningún síntoma hasta que la página se recarga sola.
- [ ] **Una clase de Tailwind inexistente NO da error**: simplemente deja de pintar. Tras cualquier renombre de tokens hay que verificar que cada utilidad mapee a un token real.
- [ ] **`prefers-reduced-motion` congela los spinners** si el bloque global no los exceptúa — y un indicador de carga detenido hace creer que la app se colgó.
- [ ] **SMIL (`<animate>` en SVG) es invisible para `prefers-reduced-motion`**, que solo alcanza a las animaciones CSS.
- [ ] **El área táctil invisible de 44px solo sirve en controles aislados.** En grupos densos las áreas se solapan y un control roba los clics del vecino; ahí se cumple por tamaño real + separación.
- [ ] **La etiqueta de procedencia repetida campo por campo.** Marcar "sugerido / IA / borrador" en cada uno de los 4 campos de una fila, y con el color de acento, gasta el 10% de acento de la pantalla (§Color 9.1) en el dato menos importante y usa el canal de jerarquía más fuerte (§Tipografía 7) para metadato. **Se dice una vez, arriba, en L3.** Solo se marca campo por campo cuando la fila está MEZCLADA (unos del usuario, otros de la IA), que es el caso raro. Prueba: si la misma palabra aparece 4 veces en un bloque de 5 líneas, no informa — decora.
- [ ] **Convertir las columnas de un documento en pares `etiqueta: valor` apilados.** Es la traducción ingenua de una tabla de 6 columnas a pantalla angosta, y produce el bloque que nadie lee (§Layout 7). El patrón correcto es **lista de dos líneas**: identificador arriba, 2–3 datos abajo en gris, con la columna de etiquetas de ancho fijo para que los valores alineen.
- [ ] **Apagar lo secundario achicándolo en vez de bajándole el contraste.** Una etiqueta a 11px junto a un valor de 14px además desalinea las líneas base. **Mismo tamaño, color L3** (§Tipografía 7): se lee como secundario y alinea solo.

### J. Quién verifica qué

Buena parte de este checklist **se puede automatizar**, y lo que se automatiza deja de depender de la memoria de nadie. En `planeaciones-nem` vive como `npm run ui` (`scripts/verificar-ui.mjs`): comprueba colores huérfanos, valores hardcodeados, duraciones fuera del sistema, selects nativos, `type="number"`, SMIL, `aria-pressed`, botones sin `type`, anillos recortados y la presencia de las reglas base en el CSS. Un hallazgo **se corrige o se justifica por escrito** con un comentario `ui-ok:` — nunca se ignora en silencio.

Lo que **ningún script puede ver** y hay que mirar en pantalla:

- ¿Hay un elemento dominante, o todo pesa igual?
- Prueba del blur: ¿se distinguen los grupos?
- ¿Se ve la manita en todo lo clickeable?
- Tab por toda la pantalla: ¿el anillo se ve completo?
- ¿Un radiogroup es una sola parada de Tab y las flechas eligen?
- ¿Están diseñados los cuatro estados de pantalla?
- En escala de grises, ¿se sigue entendiendo?
- Zoom al 200%: ¿algo se corta?

Al reportar, **se declara explícitamente cuáles se verificaron con la herramienta y cuáles quedan pendientes de ojo humano.** Dar por bueno lo que no se vio es la forma más fácil de que el checklist no sirva para nada.

### K. La última pregunta

> Si al quitar el logo esta pantalla podría ser de cualquier SaaS, algo no está haciendo su trabajo.

---

### Fuentes consultadas

- [animation-timeline — MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/animation-timeline) — estado *limited availability*
- [CSS scroll-driven animations — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Scroll-driven_animations)
- [A guide to Scroll-driven Animations with just CSS — WebKit](https://webkit.org/blog/17101/a-guide-to-scroll-driven-animations-with-just-css/)
- [WebKit Features in Safari 26.0 — WebKit](https://webkit.org/blog/17333/webkit-features-in-safari-26-0/)
- [CSS scroll-triggered animations are coming! — Chrome for Developers](https://developer.chrome.com/blog/scroll-triggered-animations)
- [What's new in web UI (I/O 2026) — Chrome for Developers](https://developer.chrome.com/blog/new-in-web-ui-io26)
- [@property: Next-gen CSS variables now with universal browser support — web.dev](https://web.dev/blog/at-property-baseline)
- [View Transition API — MDN](https://developer.mozilla.org/en-US/docs/Web/API/View_Transition_API)
- [Cross-Document View Transitions: The Gotchas Nobody Mentions — CSS-Tricks](https://css-tricks.com/cross-document-view-transitions-part-1/)
- [The Big Gotcha With @starting-style — Josh W. Comeau](https://www.joshwcomeau.com/css/starting-style/)
- [transition-behavior — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/transition-behavior)
- [interpolate-size — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/interpolate-size)
- [popover global attribute — MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes/popover)
- [text-wrap-style — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/text-wrap-style) · [Better typography with text-wrap: pretty — WebKit](https://webkit.org/blog/16547/better-typography-with-text-wrap-pretty/)
- [mask-image — caniuse](https://caniuse.com/mdn-css_properties_mask-image) · [Apply effects to images with CSS mask-image — web.dev](https://web.dev/articles/css-masking)
- [offset-path — MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/offset-path)
- [sibling-count() and sibling-index() — caniuse](https://caniuse.com/wf-sibling-count) · [Advanced Tree Counting — Smashing Magazine](https://www.smashingmagazine.com/2026/05/mathematical-layouts-sibling-index-sibling-count/)
- [Grainy Gradients — CSS-Tricks](https://css-tricks.com/grainy-gradients/)
- [Creating an animated gradient border with CSS — ibelick](https://ibelick.com/blog/create-animated-gradient-borders-with-css)
- [Animation — Tailwind CSS docs](https://tailwindcss.com/docs/animation) · [Adding custom styles — Tailwind CSS docs](https://tailwindcss.com/docs/adding-custom-styles)
- [tw-animate-css — GitHub](https://github.com/Wombosvideo/tw-animate-css) · [npm](https://www.npmjs.com/package/tw-animate-css)
- [Motion & Framer Motion upgrade guide](https://motion.dev/docs/react-upgrade-guide) · [Scroll animations — Motion](https://motion.dev/docs/react-scroll-animations)
- [canvas-confetti — GitHub](https://github.com/catdad/canvas-confetti) · [issue #228: Honour prefers-reduced-motion by default](https://github.com/catdad/canvas-confetti/issues/228)
- [@formkit/auto-animate — GitHub](https://github.com/formkit/auto-animate) · [auto-animate.formkit.com](https://auto-animate.formkit.com)
- [Magic UI](https://magicui.design/docs/components) · [LICENSE.md](https://github.com/magicuidesign/magicui/blob/main/LICENSE.md)
- [Aceternity UI — Licence](https://ui.aceternity.com/licence) · [Pricing](https://ui.aceternity.com/pricing)
- [React Bits — GitHub + LICENSE.md](https://github.com/DavidHDev/react-bits)
- [Motion Primitives — GitHub](https://github.com/ibelick/motion-primitives) · [Animate UI — GitHub](https://github.com/animate-ui/animate-ui) · [Cult UI — GitHub](https://github.com/nolly-studio/cult-ui) · [21st.dev — GitHub](https://github.com/serafimcloud/21st) · [21st.dev pricing](https://21st.dev/pricing)
- [shadcn/ui — Base UI como primitiva por defecto (jul-2026)](https://ui.shadcn.com/docs/changelog/2026-07-base-ui-default)

---

**Notas sobre lo que no se pudo verificar** (dicho explícitamente en lugar de inventarlo): el soporte declarado de Tailwind v4 en **Motion Primitives** y **Animate UI** (ninguno lo documenta); el catálogo completo de **React Bits** (SPA client-rendered, solo `BlurText` es confirmable) y de **Cult UI** (429 repetido); y la licencia efectiva de cada componente comunitario de **21st.dev**, que requiere revisión caso por caso.
