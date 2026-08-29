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


---

## directorio-b2b · 28-ago-2026 — sesión Bloque M (panel Mi Empresa)

### 23. El blueprint que sobrevivió: "big no"
- **Contexto:** primer vistazo al panel con sidebar nuevo; el contenido seguía envuelto en el fondo blueprint decorativo.
- **Petición:** "el hecho de que todavía exista esa parte de blueprint que se supone que habíamos eliminado ya es un big no".
- **Razonamiento:** una decisión de diseño ya tomada (eliminarlo) debe ejecutarse COMPLETA en todos lados; el residuo delata falta de cuidado.
- **Término(s):** consistencia de ejecución · deuda visual (el detalle olvidado erosiona la calidad percibida).

### 24. Contenedores estirados sin razón
- **Contexto:** las secciones del panel (p. ej. Servicios) se estiraban a todo el ancho disponible junto al sidebar.
- **Petición:** "ese container es super largo sin razón aparente... yo siempre he preferido tener el layout bien acomodado y consistente".
- **Razonamiento:** el ancho debe justificarse por el contenido, no por el espacio disponible; detectó además la brecha de calidad entre el sidebar (que le encantó) y las secciones "rústicas" — todo debe estar al mismo nivel.
- **Término(s):** ancho de contenido intencional · paridad de calidad entre piezas del mismo sistema.

### 25. Partir Ubicación y Contacto
- **Contexto:** la sección "Ubicación y contacto" concentraba 12 inputs (mapa + canales + 5 redes).
- **Petición:** "location y contact deberían ser separados... es muy abrumadora los 12 inputs"; adelantó que tiene un mejor diseño para los inputs de contacto que hará después — "por ahora separémoslo".
- **Razonamiento:** dividió por carga cognitiva, no por modelo de datos (en la BD siguen siendo columnas de la misma tabla); y secuenció: primero el corte estructural barato, después el rediseño fino.
- **Término(s):** chunking (Miller) · divide por percepción, no por esquema · priorización incremental (separar hoy, embellecer mañana).
### 26. Glifos de marca dentro del input
- **Contexto:** los inputs de redes (Instagram, LinkedIn, TikTok...) usaban un icono genérico de link.
- **Petición:** "que el icono estuviera de Instagram, de LinkedIn, de TikTok... al igual que los de correo, teléfono o sitio web, y estaría cool que viviera en el input así como están ahorita".
- **Razonamiento:** el icono genérico no comunica nada; el glifo de la marca identifica el campo de un vistazo sin leer el label. Y pidió mantener la posición ya establecida (dentro del input) — extender el patrón, no inventar otro.
- **Término(s):** reconocimiento sobre lectura (scanning por iconos) · consistencia del patrón existente · lenguaje visual del perfil público reutilizado en el panel.
### 27. ¿"Fotos" o "galería"? — naming por contexto
- **Contexto:** la sección del panel se llama "Logo y fotos"; el perfil público muestra esas mismas imágenes bajo "Galería".
- **Petición:** "¿no sería mejor logo y galería? porque sí son fotos de su empresa, pero el término no sé cuál sea el adecuado en el contexto de esta página" — reconoció abiertamente no tener el conocimiento y pidió criterio.
- **Razonamiento:** detectó que un mismo contenido puede tener dos nombres válidos y que la elección depende del contexto de la página, no del gusto. Preguntar antes de decidir en terreno desconocido también es product sense.
- **Término(s):** naming por perspectiva (dueño que sube vs visitante que ve) · humildad epistémica (saber cuándo pedir el criterio en vez de adivinar).
### 28. El límite de fotos se dice, no se adivina
- **Contexto:** la sección Logo y fotos tenía un contador "2/6" tan sutil que ni él lo había visto.
- **Petición:** "si hay un límite de fotos sería bueno que lo dijeras en esa sección".
- **Razonamiento:** las reglas del sistema (máximo 6) deben ser visibles ANTES de toparse con ellas, no descubrirse al fallar. Que el dueño del producto no hubiera notado el contador existente probó que no comunicaba.
- **Término(s):** visibilidad del estado del sistema (Nielsen #1) · prevención sobre corrección · si el que lo diseñó no lo ve, el usuario menos.
### 29. La última categoría no se puede borrar
- **Contexto:** en la sección Categorías se podía quitar la única chip y dejar la empresa "en el limbo" visual (el servidor sí bloqueaba guardar en cero, pero el usuario no lo sabía hasta intentar).
- **Petición:** "hacer un bloqueo que si solo tienes una categoría, no te deje eliminarla a menos que agregues primero 1 extra, para que la empresa nunca se quede sin categoría". Además pidió que el máximo fuera "más comunicativo, texto más grande más vivo" y el aviso naranja de la IA explicando por qué importan las categorías.
- **Razonamiento:** razonó el caso límite completo (equivocarse → borrar la única → quedar invisible en el ruteo aunque el nombre aún la encuentre) y eligió prevenir en el momento del error, no regañar al guardar. Y pidió elevar la comunicación de las reglas (límite + beneficio) en el mismo lugar donde se decide.
- **Término(s):** prevención de errores (Nielsen #5) · invariantes de producto (nunca cero categorías) · comunicar el porqué junto al qué (el aviso de IA como motivación, no como regla seca).
### 30. El aviso naranja como sistema, no como parche
- **Contexto:** el aviso naranja de IA acababa de entrar a Categorías y funcionó.
- **Petición:** "ese aviso naranja lo deberíamos replicar en varias secciones: contacto y redes, datos (creo que ahí ya hay), servicios, mis solicitudes, disponibilidad... y donde tú creas que sea necesario. Y si ya están, pues mejorarlas".
- **Razonamiento:** al ver un patrón de comunicación funcionar una vez, lo elevó a sistema para todo el panel — con auditoría de los existentes incluida ("si ya están, mejorarlas") y delegando el criterio de dónde no ponerlo.
- **Término(s):** patrón → sistema (consistencia deliberada) · auditoría de lo existente al extender · delegación con criterio propio ("donde tú creas").
### 31. La verificación cambió de rol — y la sección debe contarlo
- **Contexto:** la sección Verificación se mejoró con beneficio + pasos, pero solo hablaba del badge.
- **Petición:** "más que nada contexto y diseño: la verificación ahora es muy diferente de la del principio, te abre las puertas a otras features y es un requisito cuando compras planes más altos — ¿recuerdas lo de los nuevos planes?".
- **Razonamiento:** detectó que la UI comunicaba el rol VIEJO de una feature cuya estrategia ya cambió en la mesa de socios (de cosmética a credencial-requisito). La interfaz debe contar la historia actual del producto, no la de cuando se construyó — y al revisarlo salió que la regla ni siquiera estaba escrita en el roadmap.
- **Término(s):** deuda narrativa (UI desalineada de la estrategia) · la conversación no es registro: lo hablado se escribe o se pierde · verificación como gate, no como adorno.
### 32. Explorar el árbol, no adivinar el nombre
- **Contexto:** para agregar categorías solo había una barra de búsqueda: tenías que saber escribir lo que buscabas.
- **Petición:** "es algo difícil elegir qué categorías poner con solo un input de search... las categorías vienen de un sector, luego categoría, luego subcategoría — 3 niveles ¿no? Que en base al sector al que pertenece le aparezcan sus categorías y subcategorías para elegir ahí, pero que NO lo limite: que esas salgan primero, en base a lo que la IA le asignó la primera vez".
- **Razonamiento:** distinguió reconocer vs recordar (buscar exige recordar el nombre; explorar solo reconocerlo), dedujo la jerarquía del catálogo por su cuenta, y diseñó la priorización sin encierro: lo probable primero (sus sectores según la IA), todo lo demás accesible.
- **Término(s):** reconocimiento sobre recuerdo (Nielsen #6) · defaults inteligentes sin candado (sugerir ≠ restringir) · usar la señal de la IA como ordenamiento, no como filtro.
### 33. Rechazo del acordeón: "es demasiado"
- **Contexto:** el explorador de categorías se construyó como acordeón de 14 sectores con contadores, etiquetas y despliegues.
- **Petición:** "no me gusta el diseño... muy horrendo, muy difícil de usar, de ver, de manejar, ES DEMASIADO, no es eficiente, no es agradable" — rechazo total, sin pedir un diseño concreto de reemplazo.
- **Razonamiento:** la idea (explorar por sector) era suya y sigue en pie; lo que tumbó fue la ejecución con demasiada maquinaria visual. Confía en que el rediseño salga de iterar, no de especificarlo él. Se reemplazó por UN select de sector (control que ya existe en la app) + chips del sector elegido.
- **Término(s):** economía de interfaz (cada control se gana su lugar) · rechazar ejecución ≠ rechazar la idea · reusar controles conocidos antes que inventar widgets.
### 34. Categorías: que el usuario hable y la IA clasifique
- **Contexto:** tras dos iteraciones de UI de selección manual (acordeón muerto, select+chips), cuestionó la premisa entera de la sección.
- **Petición:** "elegir categorías puede abrumar y el user se puede equivocar... mejor dejar que la IA lo haga: que escriba una descripción adicional de por qué quiere más categorías y la IA trabaje con lo que ya tiene más eso. Y que le diga: en base a todos tus datos, estas son tus categorías y viven en este árbol — a qué árbol perteneces y en qué ramas apareces y en cuáles no". Pidió validar la idea antes de codear.
- **Razonamiento:** la taxonomía de 500+ nodos es trabajo de bibliotecario, no de usuario: el usuario es experto en SU empresa (que lo diga en sus palabras), la IA es experta en el árbol (que mapee). Y convirtió el resultado en mapa mental: "si filtran por Cuero y Piel, ahí NO aparezco". Además pidió parar y validar en vez de construir — control del ciclo.
- **Término(s):** división del trabajo usuario-IA (expresar vs mapear) · el árbol como mapa de visibilidad, no como formulario · validar antes de construir.
### 35. ¿Servicios mínimos obligatorios: 1 o 2?
- **Contexto:** el diagnóstico del search mostró que una empresa sin servicios es invisible para la búsqueda semántica (la demo de Uniformes tenía cero).
- **Petición:** "quiero que en el wizard servicios sea súper obligatorio, mínimo 1 sin poder avanzar... ¿o qué dices tú, que sean mínimo 2 para poder registrar tu empresa?".
- **Razonamiento:** conectó el hallazgo del search con la causa raíz (datos de entrada) y fue directo a blindar el origen. Y en vez de imponer el número, abrió la decisión a debate técnico (1 vs 2). Resultó que el sistema ya exigía 1 en las tres capas (wizard, servidor atómico, candado anti-borrar-el-último): su instinto era correcto y ya estaba construido.
- **Término(s):** arreglar la causa en el origen del dato, no en el síntoma · fricción de registro vs calidad de datos · validar reglas contra lo construido antes de construirlas.
### 36. La descripción no puede ser opcional si la IA vive de ella
- **Contexto:** venía de decidir el mínimo de servicios; siguió auditando el wizard campo por campo.
- **Petición:** "lo que me está molestando es que creo que el campo de descripción en el paso 1 no es obligatorio, y es de los más importantes ¿no?".
- **Razonamiento:** detectó la incoherencia entre el mensaje del producto (el aviso naranja dice "tu descripción es clave, la IA la lee") y sus reglas (el wizard la dejaba vacía). Si un dato es insumo del sistema, su obligatoriedad no es opinión: es consecuencia. Al implementarlo salió además el hueco espejo (el panel permitía BORRARLA después) — cerrados ambos con piso de 30 caracteres en cliente y servidor.
- **Término(s):** coherencia mensaje-reglas (lo que el producto dice importante debe exigirlo) · auditar el ciclo completo del dato (nacer obligatorio no basta si puede morir después).
### 37. El sistema reacciona al momento, no "luego"
- **Contexto:** planeando la reclasificación por IA, precisó dos cosas antes de arrancar.
- **Petición:** (a) "estamos hablando de la sección Categorías del PANEL, no la vayas a confundir con la del navbar"; (b) "cuando se agregue un servicio nuevo, aparte de clasificar, que le cree su embedding de una vez y no esperar — como hace el wizard, ¿no?".
- **Razonamiento:** desambiguar el alcance antes de construir, y exigir inmediatez en las reacciones del sistema (el dato debe quedar operativo al momento del alta, no en un batch nocturno). Resultó que ya funcionaba así: crearServicio embebe inline y el cron es solo red de seguridad para caídas de Gemini.
- **Término(s):** precisión de alcance (nombrar la pantalla exacta) · inmediatez como expectativa de calidad · el batch como respaldo, nunca como camino principal.
### 38. Umbral por evidencia + nudge aprobado con comprensión
- **Contexto:** cierre del tema reclasificación/embeddings: umbral semántico, nudge y pantalla de revisión del wizard sobre la mesa.
- **Petición:** "el umbral, en base a tu conocimiento y la proyección futura, ¿qué crees que sea mejor?"; "me interesa lo del nudge pero ocupo que me lo expliques bien"; "lo del wizard no lo entiendo, explícame qué pantalla es"; y corrigió el dato del cron ("son la 1:47 AM apenas"). Tras las explicaciones: "estoy de acuerdo con todo y lo comprendí, empecemos".
- **Razonamiento:** delega decisiones técnicas al criterio experto pero NO firma nada que no entienda — pidió explicación completa de cada pieza antes del go. Y verificó el detalle del cron contra su propio reloj: no acepta afirmaciones sin cuadrar los hechos.
- **Término(s):** delegar la decisión ≠ delegar la comprensión · aprobar por entendimiento, no por confianza ciega · umbral calibrado con mediciones reales (0.62 entre banda de ruido 0.57-0.62 y relevantes 0.65+).