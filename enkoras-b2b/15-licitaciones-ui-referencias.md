# Licitaciones — catálogo de referencias UI/UX (bloque 5)

> Investigación web (ago 2026) sobre herramientas reales de e-sourcing y patrones
> adyacentes, para que la sala en vivo (fases L1/L2 del doc 14) se diseñe sobre
> mecánica PROBADA y no sobre ocurrencias. Cada patrón viene calibrado
> [adoptar] / [adaptar] / [evitar] para el design system de la casa (glass +
> springs + naranja #FF6803 sobre grises neutros).
>
> Síntesis: **mecánica de Ariba/Scanmarket, piel de exchange premium.** Ninguna
> herramienta de procurement real tiene buen diseño visual — todas parecen ERP
> de 2012. Ahí está el diferenciador.

---

## A. Vista compradora — tablero de monitoreo y adjudicación

- **A1. Consola de dos niveles: gráfica arriba, tabla abajo** (SAP Ariba Bid
  Console). [adoptar] — dos glass panels apilados: la caída del precio en vivo
  + la tabla de ofertas. Layout canónico del lado comprador.
  https://learning.sap.com/learning-journeys/introducing-sap-ariba-guided-sourcing-projects/monitoring-events-and-reviewing-bids
- **A2. Gráfica precio-vs-tiempo con auto-refresh** (Oracle Sourcing "Event
  Price Chart", refresh 5 s; Bid History solo para el creador). [adoptar] —
  una línea por proveedor en gris, el líder en naranja.
  https://docs.oracle.com/cd/E41948_01/fscm92pbh1/eng/fscm/mssg/task_ViewingtheBidHistory-c9f3e4.html
- **A3. Columnas que importan** (Ariba award panel): rank, precio unitario,
  Δ vs líder, cobertura de cantidad, ahorro vs presupuesto privado. [adoptar]
- **A4. Escenarios de adjudicación como tarjetas comparables** (Ariba scenario
  cards). [adaptar] — versión simple: 2-3 tarjetas ("Todo al mejor precio",
  "Repartido 60/40", "Mi selección") con total y ahorro; la optimización con
  restricciones es overkill.
  https://learning.sap.com/courses/project-monitoring-and-event-administration-within-sap-ariba-guided-sourcing/exploring-the-award-scenarios-panel
- **A5. Split award con validación dinámica del 100%** (Ariba "Allocate by" %
  o cantidad; no deja pasar de 100%). [adoptar] — barra de cobertura: "70 t
  asignadas / 30 t restantes" llenándose al asignar.
  https://help.sap.com/docs/strategic-sourcing/event-management/how-to-award-single-items-or-lots-to-multiple-suppliers
- **A6. Tabs de sala: Análisis / Historial / Mensajes** + acciones de control
  (remover puja errónea, deshabilitar proveedor). [adoptar]
  https://medium.com/design-bootcamp/designing-a-b2b-e-auction-platform-a-ui-ux-case-study-a0084e7954c4
- **A7. Identidades visibles SOLO para la compradora** (anti-colusión; Ariba y
  el case study coinciden). [adoptar] — su board: nombres + badges; el board
  del proveedor: nada de nombres. (Coincide con el candado RLS ya construido.)

## B. Vista proveedor — consola de puja

- **B1. Semáforo "Leading Bid" + posición** (Scanmarket: verde=líder,
  rojo=perdiendo; Market Dojo añade ámbar). [adoptar] — hero card gigante
  "Vas #2" con borde semántico; el naranja se reserva al CTA de pujar.
  https://www.jabil.com/dam/jcr:e2ef4736-a105-4dbf-a97d-d55c5a998415/scanmarket-e-sourcing-supplier-guide-07-20.pdf
- **B2. "Next Bid" sugerido** (Scanmarket precalcula el siguiente precio
  válido). [adoptar] — botón de un tap "Pujar $X" + campo libre; mata el 90%
  de errores de captura bajo presión.
- **B3. Validación de decremento explícita** ("tu puja debe ser ≤ $X") antes
  de enviar, con shake sutil. [adoptar] — jamás rechazar en silencio.
- **B4. Confirmación de puja irreversible** (Scanmarket "Please confirm your
  bid of…"). [adaptar] — glass modal ligero, monto en tipografía enorme, y
  con < 2 min restantes hacerlo de un solo tap grande.
- **B5. Feedback rank + distancia al mejor** (Coupa/Market Dojo lo hacen
  configurable). [adaptar] — mostrar ambos: "#2 · a $120/ton del líder".
  https://docs.coupa.com/en/supplier-documentation/coupa-for-suppliers/the-coupa-supplier-portal-or-csp/features-and-processes-in-the-coupa-supplier-portal/sourcing/participate-in-a-sourcing-event
- **B6. Estados del countdown: lobby → en vivo** (Scanmarket: "Time until
  event start" con controles en gris → "remaining time" activo). [adoptar]
- **B7. Extensión anti-sniping narrada como JUSTICIA** (Scanmarket/Jaggaer
  mecánica; Catawiki microcopy: "para que todos tengan oportunidad justa").
  [adoptar] — banner glass "Subasta extendida +5 min — entró una nueva
  oferta" + pulso del timer; la regla se explica ANTES del evento.
  https://www.catawiki.com/en/help/bidding-basics/why-are-bidding-times-for-some-lots-occasionally-made-longer
- **B8. Puja automática con tope (proxy bid)** (Scanmarket "My Proxy Bid":
  el sistema baja por ti hasta tu mínimo). [adaptar, v1.1] — "piso
  automático"; gran diferenciador premium pero añade complejidad de estado.
- **B9. Cantidad parcial como campo editable** (Scanmarket "My Quantity").
  [adoptar] — requisito núcleo nuestro: stepper "Puedo surtir: 40/100 t"
  junto al precio; dejar claro que compite por su tramo.
- **B10. Feedback inmediato post-puja** (eBay prioriza outbid notifications).
  [adaptar] — toast "Oferta aceptada — vas #1"; push "te superaron" SOLO si
  no está en la sala.

## C. Datos en vivo (motion, reorden, timers)

- **C1. Flash-on-change sutil y direccional** (trading/sportsbooks): flash
  verde translúcido 400 ms en LA CELDA + flecha ↓. Nunca la fila completa.
- **C2. FLIP para reordenar el ranking** (= layout animations de Motion): la
  fila del que sube a #1 se DESLIZA con spring — una sola animación cuenta
  toda la historia. https://www.joshwcomeau.com/react/animating-the-unanimatable/
- **C3. Regla NN/g contra change blindness: UN cambio a la vez.** Si llegan 3
  ofertas en ráfaga → stagger ~150 ms, no explotar todo junto. Presupuesto
  estricto de motion. https://www.nngroup.com/articles/change-blindness/
- **C4. Timer por estados HONESTOS**: gris > 10 min, ámbar < 5, naranja con
  pulso < 2 min — que coincide con la ventana de auto-extensión (la urgencia
  máxima ocurre donde ofertar extiende el reloj: eso es honesto porque la
  regla se anunció antes).
- **C5. Carga progresiva**: mejor precio + rank + timer renderizan PRIMERO;
  gráfica e historial después; "reconectando…" visible si el socket cae —
  números congelados sin aviso = confianza rota.
- **C6. Mini-histograma de ofertas por rango** (inspirado en depth charts de
  exchanges) para resumir un split award de un vistazo. [adaptar] — el
  orderbook literal de dos lados NO aplica (solo un lado baja).

## D. Anti-patrones

- **D1. Cierre duro sin extensión** (el sniping de eBay). El cierre dinámico
  es obligatorio en subasta inversa: maximiza el ahorro.
- **D2. Proveedores viéndose entre sí** → colusión. Solo rank + mejor precio
  anónimo. (Ya es candado de BD.)
- **D3. Urgencia fabricada** (timers falsos). En B2B la credibilidad ES el
  producto: toda urgencia mapea a una regla publicada.
- **D4. Todo animándose a la vez** (NN/g). Presupuesto de motion estricto.
- **D5. Cambios instantáneos SIN transición** — por change blindness el
  proveedor no nota que lo superaron. "Sin animación" no es opción aquí.
- **D6. Copiar Dribbble como producto probado** — sirven para dirección
  visual (jerarquía, countdown, dark glass), jamás para flujos: no resuelven
  errores, extensiones, empates ni parciales.

---

**Fuentes completas:** SAP Learning (Bid Console / Award Scenarios), SAP Help
(Split Award), guía de proveedor Scanmarket/Jabil (PDF), Scanmarket eAuction
Features, Coupa Docs (Sourcing Events), Jaggaer Supplier Handbook (PDF),
Market Dojo Guide to eAuctions (PDF), Oracle Bid History, Catawiki Help,
NN/g (Change Blindness / Distracting Animations), Josh Comeau (FLIP),
case study B2B e-Auction (Medium), Lollypop Trading App Design, Altenar
Sportsbook UX. URLs inline arriba.
