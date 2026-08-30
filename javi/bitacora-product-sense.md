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
### 39. Retención de datos: generó, evaluó y descartó su propia idea
- **Contexto:** análisis del Resumen (M3); le preocupa datos = almacenamiento = dinero.
- **Petición:** (a) agregar al Resumen una card de MI actividad de licitaciones (creadas + participadas); (b) propuso y AUTO-DESCARTÓ la idea del backup trimestral por correo (ZIP con métricas + reporte Excel): "al hacerlo usar otra herramienta es ineficiente, cuando yo ya le estoy dando ese servicio incluido... mejor filtros por fecha en el dashboard y conservamos sus datos"; (c) preguntó cuánto tiempo resguardar, razonando jerarquía de criticidad: "los nuestros son solo métricas — importantes, pero no como inventarios o efectivo; un nivel más bajo".
- **Razonamiento:** ciclo completo de diseño en un solo mensaje: idea → evaluación contra el principio del producto (la visibilidad vive DENTRO del dashboard) → descarte argumentado. Y clasificó los datos por criticidad antes de decidir su retención, en vez de aplicar una regla pareja.
- **Término(s):** matar las ideas propias es diseño (kill your darlings) · el valor no debe salir del producto (el loop de retorno) · retención por criticidad del dato, no por regla uniforme.
### 40. "Cada cuenta solo ve lo suyo" — el aislamiento como requisito explícito
- **Contexto:** diseño del Resumen (M3) aprobado con realtime; antes de arrancar puso el requisito de seguridad.
- **Petición:** "probablemente ya lo sepas pero lo debo decir de igual forma: cada cuenta solo ve lo suyo — temas de IAM y de roles; no quisiera que tal empresa vea cosas de la mía ni yo de la suya".
- **Razonamiento:** verbalizar el requisito de seguridad aunque "probablemente ya esté" — los supuestos de seguridad no se asumen, se declaran y se verifican. Resultó que ya estaba blindado a nivel BD (RLS owner-only en company_events desde 008).
- **Término(s):** los requisitos de seguridad se declaran aunque parezcan obvios · aislamiento en la capa más profunda (BD), no en la UI.
### 41. El ancho se decide por el tipo de contenido
- **Contexto:** el Resumen (dashboard) heredó el ancho acotado max-w-3xl que él mismo pidió para los formularios — quedó una columna angosta con un tercio de pantalla vacío (lo marcó con un recuadro rojo).
- **Petición:** "estás haciendo todo el overview en una sola columna y eso puede provocar demasiado scroll, cuando al lado tienes espacio suficiente para acomodar el layout en 2 o 3 columnas — tienes espacio disponible, úsalo bien".
- **Razonamiento:** su regla anterior ("acomodado, no estirado") era para formularios; supo distinguir que un dashboard obedece otra regla (se escanea, no se llena) sin contradecirse. Las reglas de layout son POR TIPO de contenido, no globales.
- **Término(s):** densidad por propósito (formulario angosto, dashboard ancho) · el espacio vacío también es un bug · refinar la regla propia sin absolutizarla.
### 42. "Tenders" cazado al vuelo + preguntar qué muestra antes de aprobar
- **Contexto:** revisión visual del Resumen recién ensanchado.
- **Petición:** (a) "le pusiste tenders a la card cuando ya habíamos decidido bids, porque tenders es más europeo — se supone que habías guardado esa info"; (b) "la card de recent activity, ¿qué mostrará?".
- **Razonamiento:** detectó UNA palabra fuera del vocabulario decidido en una pantalla llena de elementos (el resto del app ya decía Bids y la card nueva lo rompió), y exigió que la decisión viva en memoria persistente, no en la conversación. Y antes de dar visto bueno a una card vacía, preguntó qué va a mostrar — no aprueba cascarones sin conocer el contenido.
- **Término(s):** vocabulario de producto como contrato (una sola voz en toda la app) · las decisiones se persisten o se repiten · no aprobar lo que no se entiende.
### 43. La actividad no crece: se desvanece
- **Contexto:** primera prueba en vivo del realtime del Resumen (los eventos entraban "just now").
- **Petición:** "¿cuántos rows se generan? No quiero que se generen infinitamente y el container se vaya agrandando — la idea es que se vayan desvaneciendo poco a poco los anteriores cuando salga uno nuevo". Y: "los clickeables de 7/30/90 días muévelos a la esquina superior derecha, al nivel del Overview".
- **Razonamiento:** ante una lista alimentada en vivo, su primera pregunta fue el límite (los contenedores tienen presupuesto de espacio) y propuso la metáfora visual correcta: lo viejo se desvanece por edad, no se apila. Y reubicó el control de rango a la jerarquía que le corresponde: manda sobre TODA la sección, así que vive en el header, no en el cuerpo.
- **Término(s):** presupuesto de espacio en listas vivas · recencia como opacidad · el alcance de un control define su posición.

### 44. Reseñas sin respuesta: la esencia es mejorar, no litigar
- **Contexto:** revisando la sección Reseñas nueva (publicó una de prueba y la vio aparecer).
- **Petición:** "¿es necesario poder responderlas? Creo que las reseñas son para que los demás users las lean; interferir directamente no creo que sea buena idea... siento que responder es como querer enmendar el problema para que no afecte el negocio, pero pierde la esencia: verlas y mejorar en los puntos negativos, no estar respondiendo cada una... aunque sabemos que los clientes exageran y siempre habrá alguien inconforme".
- **Razonamiento:** analizó la feature desde su PROPÓSITO (las reseñas informan a terceros y disciplinan al negocio) y detectó que responder cambia el incentivo (de mejorar el servicio a gestionar la imagen). Consideró el contracaso honesto (la 1 estrella injusta a Tarimas) sin dejarse arrastrar por él. Dejó la puerta abierta al error propio ("o tal vez me equivoco").
- **Término(s):** diseñar por el propósito del mecanismo, no por el caso doloroso · los incentivos que una feature crea importan más que la feature · reversibilidad de la decisión (revisar con datos reales).

### 45. RFC fuera del wizard + "reportar reseña" a maduración
- **Contexto:** cierre de M4 — coherencia del principio "RFC en un solo lugar" y la válvula propuesta para reseñas injustas.
- **Petición:** "sobre lo del wizard sí, quitémoslo; sobre lo de reportar reseña, es buena idea pero debo considerarla más — así como está ahora es más que suficiente".
- **Razonamiento:** aceptó extender el principio hasta su consecuencia completa (si el RFC vive en Verificación, el wizard no lo pide — menos fricción de registro y cero datos fiscales flotando sin validar), y frenó una feature razonable no por mala sino por prematura: la deja madurar en vez de construirla por si acaso.
- **Término(s):** los principios se aplican hasta el final, no a medias · "buena idea" ≠ "constrúyela ya" · el backlog como sala de maduración.

### 46. El cartel importante no vive en la orilla
- **Contexto:** revisando el wizard tras quitar el RFC; el aviso naranja de IA vivía al pie de la columna lateral derecha.
- **Petición:** "los carteles naranjas son importantes; esa ubicación no es precisamente la mejor. Agrega un bloque contenedor en el top, arriba del wizard y de la columna de datos — que abarque el ancho de ambos. Quiero ver cómo queda".
- **Razonamiento:** jerarquía visual = jerarquía de importancia: si el mensaje es clave (la IA lee tu descripción), no puede vivir en la periferia donde el ojo llega al final o nunca; preside a lo ancho, arriba de todo. Y pidió verlo antes de opinar (juzga en pantalla, no en descripción).
- **Término(s):** posición como declaración de importancia · la periferia es donde mueren los mensajes · juzgar sobre el render, no sobre el plan.

### 47. Guardados entra al HQ antes de cerrar el bloque
- **Contexto:** a punto de commitear el cierre de M4, frenó: "no creo que aún — ¿recuerdas que tenemos una sección de favoritos?".
- **Petición:** integrar Guardados al panel y "rediseñar esa sección, hacerla bien de acuerdo a mis estándares de UI/UX y a la proyección futura, conforme al contexto de todo lo que llevamos".
- **Razonamiento:** antes de declarar terminado un bloque, barrió el mapa completo buscando la pieza olvidada — y la encontró: una sección pre-estándar (blueprint vivo, fuera del shell) que contradecía todo lo construido. "Terminado" significa que NADA quedó atrás, no que lo nuevo esté bonito.
- **Término(s):** cerrar un bloque = auditar el perímetro completo · la pieza olvidada delata el estándar · coherencia retroactiva (lo viejo se sube al nivel de lo nuevo).

### 48. El bug del bookmark mudo (reporte de QA de primera)
- **Contexto:** probando Guardados recién integrada al panel.
- **Petición:** "encontré otro bug: fui a search, le di al botón guardar en la card y no salió nada — ni una acción ni una confirmación — y en favoritos tampoco aparece. Revisa eso por favor".
- **Razonamiento:** reporte de bug perfecto en tres líneas: pasos exactos, comportamiento esperado implícito, y verificación cruzada (revisó el destino, no solo el origen). El diagnóstico resultó fino: el interceptor de clics del panel dual (fase de captura, anterior a React) se tragaba el clic del bookmark; como la card ya estaba seleccionada, re-seleccionarla no pintaba nada — silencio total.
- **Término(s):** QA con verificación en ambos extremos del flujo · los interceptores globales deben ceder ante los controles internos · el silencio es el peor modo de fallo.

### 49. Mi Cuenta: revisar que funcione ANTES de embellecer
- **Contexto:** tras cerrar Guardados, siguió barriendo el perímetro: "la sección Mi Cuenta también está muy mal en diseño, layout, opciones — necesito que la revises que REALMENTE funcione y la mejoremos".
- **Petición:** auditoría funcional primero, rediseño después — en esa orden.
- **Razonamiento:** no pidió "hazla bonita": pidió verificar el funcionamiento real y LUEGO mejorar. La estética sobre un botón roto es maquillaje. Y "opciones etc" delató su instinto de completitud: una página de cuenta sin cambiar correo ni cerrar sesiones se siente amateur aunque se vea bien.
- **Término(s):** funcionalidad antes que estética · completitud de expectativas (lo que toda página de su tipo debe tener) · el barrido del perímetro continúa.

### 50. Persona ≠ empresa hasta en el copy + matriz por tipo de acceso
- **Contexto:** revisión del rediseño de Mi Cuenta.
- **Petición:** (a) "'así te ves en el resto de la plataforma' va a confundir: van a decir '¿entonces no me ven como mi empresa?' — que diga el nombre de tu CUENTA, no de tu perfil público, que ese es tu empresa/anuncio"; (b) "cambiar contraseña y correo imagino que solo aplican a cuentas creadas con correo; la gente que entra por Gmail, esas opciones no deben estar disponibles ¿correcto?"; (c) "la zona de peligro sí funciona en los dos tipos ¿no?".
- **Razonamiento:** leyó el copy con los ojos del usuario confundible (la ambigüedad persona/empresa que él mismo institucionalizó en el sidebar) y luego auditó las opciones como MATRIZ: cada opción × cada tipo de acceso — la pregunta que atrapa los huecos que el caso feliz esconde.
- **Término(s):** el copy también respeta el modelo persona≠empresa · auditar features como matriz (opción × tipo de usuario) · preguntar "¿correcto?" para verificar supuestos en vez de asumirlos.

### 51. El riel de contexto: regrésalo
- **Contexto:** para desapachurrar los hints de Mi Cuenta se probó un riel lateral (patrón del wizard) con la card "Tú y tu empresa" + consejo de seguridad, adelgazando los hints de las cards.
- **Petición:** "No, quedó horrible — en especial ese cartel de tú y tu empresa. Estaba mucho mejor antes lo que decía dentro de las cards de perfil y de contraseña".
- **Razonamiento:** la explicación pertenece al LADO del control que explica, no a un panel aparte que obliga a conectar dos lugares. El riel didáctico funcionó en el wizard (usuario nuevo, flujo guiado) pero en una página de ajustes estorba: ahí se viene a hacer, no a aprender. Revert limpio, sin defender el intento.
- **Término(s):** la ayuda vive junto a lo que ayuda (proximidad de Gestalt) · el mismo patrón no sirve en todos los contextos (wizard ≠ settings) · regrésalo sin ego.

### 52. El apachurre tenía nombre: la sangría del hint
- **Contexto:** tras el revert del riel, siguió mirando hasta aislar la molestia real.
- **Petición:** "ya sé qué es lo que me molesta: los textos de información empiezan en la primera letra del título de la card, y no es así como quiero — necesito que empiecen en el principio del icono y terminen al final del largo de los inputs. Trata a ver cómo queda".
- **Razonamiento:** no se conformó con "algo se ve mal": iteró hasta DIAGNOSTICAR el pixel exacto (la sangría de 2.625rem alineada al título comprimía el párrafo). La solución era una línea, pero encontrarla requirió dos intentos fallidos de terceros y su ojo. El fix aplicó a TODAS las cards del panel de un golpe (vivía en el componente compartido).
- **Término(s):** diagnosticar la molestia hasta el pixel · arreglar en el componente compartido = arreglar en todos lados · el proceso de aproximación (probar → rechazar → aislar) es el método, no el desperdicio.

### 53. Dudar de la feature propia aunque los socios la quieran
- **Contexto:** definiendo 5.A (asientos y roles) sobre el papel, antes de construir.
- **Petición:** afinó la jerarquía con un caso real (dueño=manager define proyecto con el supervisor/admin → "pon una licitación para este contrato" → el operador ejecuta bajo supervisión: "los jefes supervisan, enseñan y guían; el operador ejecuta"), abrió verificación a los 3 roles ("el operador será alguien de puesto alto, no le veo sentido restringirla")... y luego el freno: "mis socios decían que puede ser muy bueno, pero sinceramente yo le veo como ¿para qué complicarse tanto? Roles los entiendo en Plaky, en un WMS, en contabilidad — ¿pero aquí? Para definir si es necesario, innovador o solo complicarnos, primero hay que ver qué ofrece actualmente mi app: el funcionamiento completo, su propósito, y de ahí definir. Dale con el análisis a fondo full, sin nada a medias, revisa todo el código".
- **Razonamiento:** modeló los roles desde una obra real (no desde el software), y ante el desacuerdo con sus socios no impuso ni cedió: exigió EVIDENCIA — el inventario completo del producto como base de la decisión. La feature se juzga contra lo que la app ES, no contra lo que otras apps hacen.
- **Término(s):** los roles se copian de la obra, no del software ajeno · disentir con datos, no con jerarquía · el inventario funcional como juez de las features.

### 54. "Soy una calculadora": el alcance legal dibujado con una analogía
- **Contexto:** el análisis de multiusuario describió la app como "sistema donde se mueve dinero"; frenó a corregir el encuadre.
- **Petición:** "quiero dejar claro: esta app no hace transacciones de dinero ni las hará, más que la compra del plan por Stripe. En licitaciones se oferta dinero por contrato pero eso ya es ajeno a nosotros — tenemos la constancia PDF donde ELLOS acuerdan un precio y Enkoras solo es testigo, no doy validación de que fuera de la plataforma lo respeten. Soy una calculadora: me dicen cuánto es 4×2, digo 8; si luego deciden que será 4×3, es asunto suyo. Soy un cable conector: conecto empresas y les doy herramientas, no les manejo su dinero ni soy responsable de ellos". También precisó que admin/semillero son EXCLUSIVOS de su equipo, jamás de clientes.
- **Razonamiento:** detectó que un encuadre técnico impreciso ("maneja dinero") tiene consecuencias legales y comerciales reales, y lo corrigió con una analogía que cualquier abogado o socio entiende. El posicionamiento (herramienta/testigo, no fintech) es una decisión de producto tan dura como cualquier feature.
- **Término(s):** el encuadre define la responsabilidad legal · posicionamiento como constraint de diseño (nunca escrow, nunca settlement) · analogías que fijan alcance.

### 55. Los permisos los reparte el dueño, no el software
- **Contexto:** revisando la matriz fija de permisos propuesta para 5.A (Dueño/Admin/Operador con celdas predefinidas).
- **Petición:** "el operador no es una persona de bajo rango — tiene responsabilidades; darle pocos permisos me frena. Adjudicar sí debe tenerla (es parte de operación: los jefes le dicen 'elige a este' o le dan carta libre con variables que ellos definen), y el perfil también ('agrega otro servicio, borra ese que ya no damos'). Mejor: que el DUEÑO elija qué permisos da a su equipo — al admin más si quiere, al operador según se requiera. Los managers y supervisores mandan evaluando todo y son responsables de las decisiones; el operador ejecuta lo que le asignan. Es muy reactivo, cambiante y dinámico — no podemos definir los usuarios como nosotros queramos y dejarlos así para siempre: en producción es cuando se ven las debilidades, y no quiero que esta sea una".
- **Razonamiento:** rechazó la rigidez del diseñador a favor de la realidad laboral: la jerarquía existe (y "está bien que a los jefes les guste mandar — para eso estudiaron"), pero QUÉ delega cada jefe varía por empresa y por semana. Los roles pasan de cajas fijas a PLANTILLAS con permisos ajustables por el dueño — el software da defaults, la empresa decide.
- **Término(s):** roles como plantillas, permisos como switches del dueño · la jerarquía es real pero su contenido es dinámico · diseñar para producción, no para el diagrama.

### 56. El sistema de trabajo en equipo, diseñado desde el mostrador
- **Contexto:** definición final de 5.A — de "roles" a un sistema completo de propiedad del trabajo.
- **Petición:** sillas fijas por plan (2 de por vida: Dueño y Admin; el resto sin etiqueta, el dueño las define) matando el $/asiento del artifact por precios fijos ("esto no es Plaky — muchos users no es el caso aquí"); claim de chats ("el que lo agarre primero, el sistema se lo asigna — pero lo puede soltar o transferir"); chats de bids nacen asignados a la creadora de la bid; candado de edición con reloj ("le das editar y se te asigna hasta que guardes, con cambios en realtime sin refresh"); y el keep-alive de soporte: "como cuando te dicen 'permítame tantito' y al minuto 'sigo con usted' — que conserve el chat mientras detecte movimiento y le avise al operador antes de soltarlo".
- **Razonamiento:** diseñó concurrencia, asignación y locks sin saber sus nombres técnicos (claim & lease, pessimistic locking, heartbeat) — puro razonamiento desde el mostrador de una empresa real y las apps de soporte que ha usado como cliente. Y detectó él solo la race condition del chat compartido antes de que existiera el código.
- **Término(s):** claim & lease diseñado por intuición · el reloj como válvula anti-secuestro · la experiencia de CLIENTE de otras apps como material de diseño.
