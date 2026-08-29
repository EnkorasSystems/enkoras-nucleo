# Bitácora de product sense — Javi

> Registro de cada petición espontánea de Javi durante el desarrollo — las
> que NO vienen del roadmap sino de su instinto de usuario (dogfooding).
> Propósito: auto-evaluación — conocer con datos reales su forma de diseñar.
> Formato por entrada: contexto → petición → razonamiento → término(s).
>
> Se apunta EN EL MOMENTO de cada petición. Arrancó el 28-ago-2026 con
> directorio-b2b; las apps hermanas (CV maker + bolsa) continuarán aquí.

---

## directorio-b2b · 28-ago-2026 — sesión de rediseño de cards y search

### 1. Del avatar grande al logito inline
- **Contexto:** rediseño de la card de resultados de búsqueda.
- **Petición:** quitar el logo circular de 48px y usar "el logito con el título que hay en las de bids".
- **Razonamiento:** "ya tenemos un diseño que funciona y es muy bueno, el de las cards de bids" — reusar el patrón propio probado en vez de inventar.
- **Término(s):** consistencia de sistema de diseño · reconocimiento sobre memoria (el usuario ya aprendió ese lenguaje en otra parte de la app).

### 2. La odisea del badge de verificada (4 posiciones)
- **Contexto:** ubicar el sello de verificación en la card.
- **Petición(es):** esquina pegada al borde → "se ve raro, ponlo al nivel del featured" → al pie junto a la estrella → "que vivan siempre a la derecha de la ubicación".
- **Razonamiento:** iteró posiciones MIRANDO el resultado real cada vez, hasta que el ojo dijo "ahí".
- **Término(s):** dogfooding iterativo · diseño por ensayo visual (no especifica de antemano: converge viendo).

### 3. El color del sello: ni negro ni naranja → azul
- **Contexto:** el badge VERIFICADA era píldora negra.
- **Petición:** "el negro no es la mejor opción... tampoco naranja... pensemos otra alternativa".
- **Razonamiento:** el negro pesa demasiado, el naranja se confunde con la marca; aceptó el azul como color universal de verificación.
- **Término(s):** semántica de color (cada color = un significado) · convenciones aprendidas de plataforma.

### 4. Insignia sin palabra
- **Contexto:** el sello azul decía "VERIFICADA" en texto.
- **Petición:** "que diga con texto verified también es un big no. Hay cosas en las que minimalismo es mejor".
- **Razonamiento:** la palomita azul YA es el símbolo; el texto es redundancia (referencia: Computrabajo lo hace igual).
- **Término(s):** minimalismo funcional · confianza en convenciones ("los users son inteligentes").

### 5. Jerarquía por tamaño
- **Contexto:** estrella e insignia juntas al lado de la ubicación.
- **Petición:** "los dos más grandes, y el de verified un poquito más grande que el de estrellas".
- **Razonamiento:** el sello de confianza debe mandar sobre la opinión (rating).
- **Término(s):** jerarquía visual deliberada.

### 6. Bookmark + chips para las cards "pelonas"
- **Contexto:** cards sin featured/verified se veían vacías.
- **Petición:** botón de guardar como Indeed + badges de categorías "para que las cards que no tengan featured y verified pues igual se vean vivas".
- **Razonamiento:** toda card necesita textura y una acción, tenga los datos que tenga.
- **Término(s):** degradación elegante (piso visual mínimo) · benchmark de competidores bien digerido (no copiar: traducir).

### 7. Chips completas o nada
- **Contexto:** las chips de categoría salían cortadas ("Structural & constructio…").
- **Petición:** abajo del todo como Indeed, cuadraditas no redondas, y "que no aparezcan incompletos — de ser el caso reduzcámoslo a 1 + el número".
- **Razonamiento:** texto mutilado se ve roto; mejor menos información pero íntegra.
- **Término(s):** legibilidad > densidad · integridad del contenido.

### 8. El layout adaptable... y su reversa
- **Contexto:** pidió que los elementos "subieran" a llenar filas vacías según los datos de cada card.
- **Petición:** primero la adaptividad compleja (con la regla "solo si el título no se corta"); al verla: "sabes que no, regrésalo — el cluster va SIEMPRE a la derecha de la ubicación".
- **Razonamiento:** al ver la heurística en acción prefirió la regla fija y predecible.
- **Término(s):** simplicidad > cleverness · matar a tu propio darling sin ego (señal de madurez de diseño).

### 9. El chevron corona la esquina
- **Contexto:** la flechita de hover flotaba centrada y chocaba con otros elementos.
- **Petición:** "siempre en la esquina superior derecha, pero dinámica: si hay featured vive en esa fila, si no, al nivel del título".
- **Razonamiento:** una posición ancla fija (la esquina) que se adapta a la anatomía real de cada card.
- **Término(s):** affordance consistente · microinteracción.

### 10. "El botón de favoritos no tiene hover"
- **Contexto:** el bookmark solo mostraba tooltip.
- **Petición:** agregarle hover.
- **Razonamiento:** todo lo clickeable debe SENTIRSE clickeable antes del click.
- **Término(s):** feedback de interacción (doctrina de microinteracciones) · su regla de casa "nada en seco".

### 11. Criterio selectivo en el duelo de diseños
- **Contexto:** comparación de su card vs la propuesta alternativa (artifact).
- **Petición:** "tomaré lo del (33) y también lo del giro; lo del fallback de servicios lo descarto, no veo razón".
- **Razonamiento:** adopta por valor concreto, no por paquete — y rechaza sin miedo lo que no le convence.
- **Término(s):** juicio de producto selectivo · evidencia sobre completitud (el conteo de reseñas como prueba de la estrella).

### 12. El avatar de letra, naranja
- **Contexto:** empresas sin logo mostraban inicial sobre negro.
- **Petición:** "que sea anaranjado en vez de negro".
- **Razonamiento:** más vivo y más de la marca.
- **Término(s):** identidad de marca en los detalles.

### 13. El CTA habla el color de la casa (y con su vida)
- **Contexto:** botón "Enviar mensaje" del perfil en negro.
- **Petición:** "¿el negro no es la mejor opción, no crees?" → naranja; y al verlo: "pues si ibas a ponerlo naranja, hubieras puesto la animación que usamos en los botones naranja".
- **Razonamiento:** un CTA principal usa EL color de acción de la casa, con TODO su lenguaje (incluido el btn-vivo) — consistencia total o nada.
- **Término(s):** consistencia de sistema (color + movimiento van juntos) · ojo de auditoría visual.

### 14. Los contornos que se pierden
- **Contexto:** las secciones del anuncio se fundían con el fondo de la columna.
- **Petición:** "el contorno es demasiado finito casi blanco... en cambio las cards de la columna 1 sí se diferencian".
- **Razonamiento:** comparó dos zonas de la MISMA pantalla y exigió el mismo recorte visual.
- **Término(s):** percepción de contraste · consistencia intra-pantalla.

### 15. Las redes juntas
- **Contexto:** LinkedIn quedaba en la fila de la dirección, separado de las demás redes.
- **Petición:** "deberían estar en la misma row con los demás".
- **Razonamiento:** las cosas de la misma familia viven juntas.
- **Término(s):** ley de proximidad (Gestalt) — agrupación por afinidad.

### 16. "Dice que tengo 23 reviews pero ¿dónde están?"
- **Contexto:** el anuncio mostraba promedio/conteo cosmético sin reseñas reales.
- **Petición:** entender la inconsistencia; decidió NO sembrar reseñas falsas ("de igual ya vimos que sí funcionan").
- **Razonamiento:** dogfooding de DATOS, no solo de UI — y preferir la honestidad del sistema sobre el maquillaje.
- **Término(s):** integridad de datos percibida · escepticismo sano.

### 17. Del parpadeo a la marquesina
- **Contexto:** el carrusel de disponibilidades hacía fade discreto.
- **Petición:** "que en vez de parpadear sea continuo, como si los títulos estuvieran dentro de un círculo y conforme rota van apareciendo" + referencias concretas (ReactBits scroll-velocity / carousel).
- **Razonamiento:** el movimiento continuo comunica "flujo vivo" mejor que el cambio discreto; llegó con referencias visuales, no solo palabras.
- **Término(s):** dirección de arte con referencias · metáfora de movimiento (ticker).

### 18. Quitar el "+N" y la etiqueta "Disponible ahora"
- **Contexto:** la línea verde de la card cargaba etiqueta y contador.
- **Petición:** fuera el "+N"; y "esa palabra está arruinando esto... o sin esa palabra y solo el puntito verde. Los users son inteligentes, no es necesario marcarle todo".
- **Razonamiento:** el puntito verde pulsando + texto verde YA es el mensaje; el resto es ruido.
- **Término(s):** minimalismo funcional · confianza en convenciones · reducción de ruido.

### 19. "¿Es malo que las cards no se actualicen en vivo?"
- **Contexto:** sembró disponibilidades y el panel se actualizó en vivo pero las cards no.
- **Petición:** entender si era bug o decisión; aceptó el trade-off (foto estable > lista que se baraja sola) al oír el razonamiento.
- **Razonamiento:** planteó el escenario del usuario que "pasó de largo y no se enteró" — pensó el caso límite desde el usuario.
- **Término(s):** análisis de trade-offs desde el usuario · apertura a cambiar de marco con argumentos.

### 20. Filtrar no ocupa lupa
- **Contexto:** los selects de estado/ciudad requerían presionar buscar.
- **Petición:** "cuando queramos usar filtros SIN texto, que se haga en automático nomás terminar de seleccionar; cuando ya escribimos, ahí sí la lupa".
- **Razonamiento:** distinguió dos intenciones distintas (filtrar vs buscar) y les asignó fricciones distintas — "si elegir ya es la orden, no cobres otro click".
- **Término(s):** principio del menor esfuerzo · modelo mental de intenciones.

### 21. "Todos los estados" sin perder la búsqueda
- **Contexto:** para des-filtrar el lugar había que darle a la X... que borraba también el texto buscado.
- **Petición:** una opción "ver todos" en los selects "que resetee a que muestre todo, pero SIN que me borre la búsqueda".
- **Razonamiento:** vivió el flujo completo (buscar cartón → filtrar BC → Mexicali → querer volver) y encontró el callejón sin salida.
- **Término(s):** principio de menor sorpresa · acciones quirúrgicas vs bomba (recovery granular).

### 22. Restraint: los filtros que NO quiso
- **Contexto:** se le propusieron filtros extra (disponibilidad, verificadas, antigüedad).
- **Petición:** disponibilidad "sería muy redundante siendo que el sistema trabaja por ranking"; verificadas "hasta que vea que realmente lo ocuparán"; "tampoco quiero abarrotar de filtros innecesarios".
- **Razonamiento:** cada filtro debe ganarse su lugar con uso real, no con "podría servir".
- **Término(s):** anti-feature-creep · diseño por sustracción · decisiones basadas en datos futuros (esperar el lanzamiento).
