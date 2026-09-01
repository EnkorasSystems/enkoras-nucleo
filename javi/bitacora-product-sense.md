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

### 57. "Cuando terminemos, explícamelo con un ejemplo"
- **Contexto:** maratón de construcción de 5.A (Bloque C en curso); los bloques técnicos se apilaban uno tras otro.
- **Petición:** "no estoy entendiendo bien los bloques — cuando terminemos necesito explicación práctica de cómo funcionaría, con un ejemplo".
- **Razonamiento:** no frenó el maratón (la construcción sigue), pero fijó el criterio de cierre: el trabajo no está terminado hasta que se pueda CONTAR como historia — una empresa real, una persona real, qué pasa cuando llega un mensaje. Es la misma vara con la que evalúa UI: si no se entiende desde el mostrador, no está listo.
- **Término(s):** el ejemplo vivido como criterio de aceptación · separar "constrúyelo ya" de "explícamelo después" — dos entregables, no uno.

### 58. La simetría de la propiedad: "¿y cuando la licitación la hacemos nosotros?"
- **Contexto:** recién entregada la explicación completa de 5.A, con "la puja con dueña" (lado proveedor).
- **Petición:** "¿también está lo contrario? cuando nosotros como empresa hacemos las licitaciones — solo el que la hace la puede controlar, a menos que la transfiera el owner a otro compañero, ¿no?"
- **Razonamiento:** detectó él solo el hueco de simetría del modelo: el claim protegía al equipo OFERENTE (dos no pujan a la vez) pero el lado CONVOCANTE seguía abierto por llaves — dos compañeros con 'ofertar' podrían mandar contraofertas contradictorias al mismo proveedor. La misma race condition que él predijo para los chats, aplicada al otro lado de la mesa.
- **Término(s):** simetría de propiedad como olfato de producto · el modelo mental "cada pieza de trabajo tiene UNA dueña" aplicado consistentemente antes que el código.

### 59. La liga sin contexto: "no hay correlación o concordancia"
- **Contexto:** dogfooding de la invitación al equipo (5.A·C) recién construida — mandó la liga por chat y la abrió sin sesión.
- **Petición:** la liga compartida se ve como texto plano, no como link; y al abrirla sin sesión aparece el login genérico "sin correlación o concordancia" — "debería ser una pantalla diferente, con textos dando más contexto de qué es esa liga; y te debería poner el iniciar sesión Y su contraparte el registrar — porque si ya tengo cuenta de nada sirve [el registro], solo si no tengo cuenta creada; ahora bien, también sería válido".
- **Razonamiento:** detectó en el primer uso real la ruptura del hilo narrativo del invitado (el momento de mayor fricción del funnel de equipo: un usuario NUEVO que jamás ha visto Enkoras) y diseñó la solución completa de pasada: landing con contexto + ambos caminos según tengas cuenta o no.
- **Término(s):** la continuidad del contexto como requisito del funnel · el invitado es un usuario frío, no uno logueado · dogfooding inmediato post-feature.
- **Corrección de rumbo (mismo día):** la primera implementación fue una tarjeta intermedia propia ("sosa") — Javi la rechazó y aclaró la intención original: "por lo general los links de invitación te redirigen a las pantallas de login o de sign up — eso es a lo que me refería, que fuera en la pantalla de login, no aquí dentro". El contexto vive DENTRO del auth (banner en login/registro), no en una pantalla extra. Lección: seguir las convenciones que el usuario ya conoce de otras apps antes que inventar pasos intermedios.

### 60. La cuenta exclusiva: "dueña O miembro, nunca las dos"
- **Contexto:** dogfooding del 5.A con dos cuentas reales — vio su cuenta invitada (con silla) mostrando TAMBIÉN su propia sección "Tu equipo" con botones de invitar.
- **Petición:** (1) para invitar, el dueño debe tener su empresa ya creada ("con eso ya puede enviar solicitudes"); (2) la cuenta invitada asume el rol de la invitación y ya no debe poder crear su propia empresa; (3) una cuenta con silla (admin/operador) no debe poder crear su propio team — "sería una ramita infinita de operadores o admins invitando y ellos invitando... no le veo sentido, además complicaría mucho las cosas"; (4) al salirte del team, "tu cuenta vuelve a ser normal, sin rol más que el rol owner".
- **Razonamiento:** cerró él solo la recursión del grafo de equipos (árboles infinitos de sub-equipos) con una regla de exclusividad simple y reversible — el estado de la cuenta es un interruptor (dueña ⟷ miembro) que se restablece al salir. La complejidad no se administra: se PROHÍBE por diseño.
- **Término(s):** exclusividad de rol por cuenta · cortar la recursión en el modelo, no en la UI · reversibilidad como válvula ("te sales y vuelves a ser normal").

### 61. Una silla a la vez + el navbar que no sabe quién eres
- **Contexto:** cierre de la cuenta exclusiva (066); yo dejé abierta la puerta a que una cuenta-miembro tuviera sillas en DOS equipos distintos.
- **Petición:** (1) "no — que una cuenta sea miembro de dos equipos es demasiada complejidad y no es necesario para algo como esto"; (2) en una cuenta admin/operador el botón naranja del navbar "Publica tu empresa" es ilógico — "deberíamos cambiarlo por otro botón que lleve a las configuraciones de la empresa, en teoría".
- **Razonamiento:** recorta complejidad especulativa sin caso de uso real (YAGNI aplicado por instinto) y exige que el CTA principal refleje el ESTADO de la cuenta: al miembro no se le vende registrar empresa — se le abre la puerta del panel donde trabaja.
- **Término(s):** una silla a la vez · el CTA como espejo del estado de la cuenta · complejidad solo con caso de uso.
- **Adición (mismo hilo):** "Planes" en el navbar/panel también es innecesario y crearía fricción para una cuenta operador/admin — el plan es del Dueño (no delegable), así que a la cuenta miembro ni se le muestra.

### 62. "Pum, de repente ya ve que se cambió" — el dato viaja, no solo el candado
- **Contexto:** probó el candado de edición con dos pantallas (owner + operador en Ubicación): el bloqueo y el badge "la está editando X" llegaron en vivo, pero al GUARDAR el operador, el owner tuvo que refrescar para ver la ciudad nueva.
- **Petición:** "dijimos que es una de las cosas importantes, que los cambios se vean — que tal si le pidieron cambiar la dirección, va y lo hace, y la otra al entrar la ve bloqueada y se queda ahí viendo, y pum, de repente ya ve que se cambió: dice 'ah ok, ya la cambió mi compañera' y listo".
- **Razonamiento:** define el criterio de terminado del realtime con una ESCENA, no con un requisito técnico: el espectador congelado es un observador legítimo y el sistema le debe la recompensa de ver aterrizar el cambio. El candado sin el dato es media promesa.
- **Término(s):** el dato viaja con el candado · el espectador congelado como usuario de primera clase · escenas como specs.

### 63. "Javier Calixto editó: Ciudad" — el cambio con autor y campo
- **Contexto:** recién estrenado el panel en vivo (067) — el dato ya aterriza solo en la pantalla del espectador.
- **Petición:** "estaría bien que hubiera algo que dijera ahí mismo, si yo estoy en la misma pantalla, que el que está editando —tipo 'Javier Calixto editó campo: tal'— obviamente solo se mostrará si yo estoy en esa sección mientras la otra está editando".
- **Razonamiento:** cierra el ciclo de la colaboración visible: no basta que el dato cambie — el espectador necesita la ATRIBUCIÓN (quién) y el ALCANCE (qué campo) para confiar en lo que acaba de ver moverse solo. Es el patrón de Google Docs aterrizado al mostrador.
- **Término(s):** atribución del cambio en vivo · el campo como unidad de narración · feedback visual como confianza.

### 64. El freno por persona + el cronómetro que explica, y las sillas 3/5/7
- **Contexto:** le expliqué (con la escena del lunes de Luis) que el rate limit de 10 licitaciones/hora se reparte entre todo el equipo porque cuenta por EMPRESA.
- **Petición:** (1) "no sabemos cuántas licitaciones live u offline puede hacer una empresa — démosle margen: **10 por persona**"; (2) **ritmo de 5 minutos** entre una y otra, y **que salga un cronómetro contando los 5 minutos** hasta la siguiente, "y explicarle al user el por qué de eso"; (3) corrección del modelo de sillas: la base son **3** (owner = silla jefe + admin + operador) y los planes SUMAN extras: **premium 1 → 3+2 = 5**, **premium 2 → 3+4 = 7**.
- **Razonamiento:** convierte un límite defensivo (invisible hasta que muerde) en una regla de producto VISIBLE y explicada — el cronómetro no es un castigo, es ritmo con razón. Y ajusta el modelo de sillas hacia arriba al ver el uso real: la base ya no es el techo, es el piso sobre el que el plan suma.
- **Término(s):** el límite como ritmo explicado, no como castigo silencioso · cronómetro con porqué · sillas base + extras por plan (3/5/7, revisa el 4/6 anterior).

### 65. "No trabajamos al mínimo ni con resultados mediocres"
- **Contexto:** con el 5.A completo y ya auditado dos veces, pidió una tercera revisión — pero con otra vara.
- **Petición:** "haz una nueva auditoría de toda la feature multiusuario porque esta es una de las features más pesadas, y no podemos entregar algo decente, ni más o menos, ni con fallas, sino algo EXCELENTE. No trabajamos al mínimo ni con resultados mediocres. Es muy importante la experiencia del user ya que de eso depende que regrese siempre y nos use y catalogue como una buena herramienta."
- **Razonamiento:** cambió el criterio de aceptación de "¿funciona?" a "¿lo recomendarían?" — y lo ató explícitamente a la retención. Es la diferencia entre auditar código y auditar PRODUCTO: la revisión resultante encontró cosas que ningún test detecta (el arrebato silencioso, el candado que bloquea a quien solo mira, la invitación artesanal, el switch de 20px imposible con el pulgar en una bodega).
- **Término(s):** la excelencia como criterio de entrega, no como lujo · la experiencia como motor de retención · auditar producto ≠ auditar código.

### 66. "Quiero ver el link", el correo, y las sillas en su propia columna
- **Contexto:** de los tres huecos de excelencia que le presenté, eligió dos (invitar por correo y presencia en línea) y descartó la bitácora de actividad.
- **Petición:** (1) las dos features; (2) reclamó que "el plan B del portapapeles" no se veía — yo lo había hecho invisible (solo aparecía al fallar) cuando lo que él quería era **ver el link siempre**: "¿a qué te referías, que iba a aparecer el link ahí mismo o cómo?"; (3) rediseño de /equipo en dos columnas: los integrantes en una, y **las sillas libres con su panel de invitar en la otra** ("que esa columna sea específica para las sillas; una vez agregado ya aparece en la otra card").
- **Razonamiento:** descartó la bitácora sin dudar (no la necesita = no se construye), y en lo del link corrigió una decisión mía de ingeniero: un fallback invisible no es una función, es una red que nadie ve. Ver lo que vas a mandar es parte de confiar. El layout de dos columnas separa el ESTADO (quién está) de la ACCIÓN (llenar sillas) — cada card con un solo trabajo.
- **Término(s):** el fallback invisible no cuenta como función · separar estado de acción por columna · descartar features sin ceremonia.

### 67. El panel maestro-detalle, el candado que se suelta y la llave que no debe existir
- **Contexto:** dogfooding intensivo de /equipo con dos cuentas abiertas.
- **Peticiones en cadena:** (1) rechazó dos layouts míos ("eso es horrendo, ¿por qué limitas el contenedor cuando tienes espacio libre?" y luego "quedó peor") hasta proponer él la solución correcta: **maestro-detalle** — lista a la izquierda, panel de lo elegido a la derecha, con el botón de silla libre **debajo de la card del dueño**; (2) 40/60 en vez de 50/50; (3) "el chip de administrar equipo nunca debe estar presente en el rol operador"; (4) al ver un `confirm()` del navegador: "sabes que eso es de las peores prácticas UI/UX"; (5) el candado: "escribo una letra, me arrepiento y la borro, pero el otro sigue viéndome editando — debería haber un botón de cancelar que deje todo como estaba y le avise al otro"; (6) "aunque le doy cancelar, en la otra cuenta sigue apareciendo… no hay realtime para eso, tengo que hacer refresh".
- **Razonamiento:** su ojo de layout es más rápido que mi razonamiento: propuso el patrón que la app ya usa en Mensajes y Licitaciones antes de que yo lo viera. Y sus dos observaciones de comportamiento destaparon un bug de infraestructura real (los DELETE no viajaban por realtime sin REPLICA IDENTITY FULL) que ningún test ni auditoría había encontrado.
- **Término(s):** dogfooding con dos cuentas como técnica de QA · rechazar dos veces hasta que el diseño sea el correcto · el usuario detectando un bug de replicación desde el síntoma visual.

---

### 68. "Ignora el 3.7 por favor"
- **Contexto:** caída de la IA del 31-ago. Le dije "Gemini está caído" y no lo aceptó como respuesta: *"¿en qué sentido, las API keys no responden o es por temas de modelos? Se supone que habíamos arreglado el tema de diferentes modelos y precisamente para que nunca hubiera una caída tenemos 5 API keys disponibles, analiza qué está pasando"*. A media investigación soltó la decisión: **"ignora el 3.7 por favor"**.
- **Petición:** (1) distinguir *la llave no responde* de *el modelo no responde* antes de declarar nada caído; (2) sacar al 3.7 de la cascada sin más análisis — ya lo había mordido el 28-ago (el alias `latest` se movió solo a 3.7 y colgaba >25s).
- **Razonamiento:** exigió exactamente la distinción que el código NO hacía: habíamos comprado redundancia (5 llaves) contra un riesgo (cuota) y la caída vino del otro eje (saturación del modelo) — redundancia en el eje equivocado es cero redundancia. Y un modelo con antecedentes no se re-litiga: se veta y se documenta el porqué.
- **Término(s):** ¿respaldo contra qué exactamente? · redundancia por eje de falla · veto con memoria (lo que ya mordió, fuera).

### 69. "Mismo maldito problema"
- **Contexto:** dogfooding con dos cuentas abiertas. Probó "Cancelar edición" y el aviso *"Esta sección la está editando Javier Calixto"* no desapareció del otro lado: *"otra vez cosas de realtime"*. Segunda vez que caza el MISMO síntoma — la primera derivó en la migración 076.
- **Petición:** que el realtime funcione de verdad, no que se vuelva a dar por cerrado.
- **Razonamiento:** yo lo daba por resuelto con la 076 y él no — y tenía razón: la causa real estaba en el cliente (`payload.new ?? payload.old`; en un DELETE Supabase manda `new: {}`, objeto VACÍO que no es nullish, y el evento se tiraba a la basura). Dos lecciones: un síntoma que reaparece significa que la causa no estaba encontrada, no que falte otra capa encima; y la forma de un payload se verifica con una sonda contra producción, no con lógica. Su dogfooding con dos pantallas encontró lo que tres auditorías y una jornada E2E de 41 pasos no vieron: las pruebas no miran dos pantallas a la vez.
- **Término(s):** síntoma repetido = reabrir la causa desde cero · sondear, no suponer · el dogfooding de dos pantallas.

### 70. "Esto ya te lo he confirmado miles de veces"
- **Contexto:** al repasar la cola de pendientes le listé como abierto el modelo de sillas 3/5/7. Corrección seca: *"esto ya te lo he confirmado miles de veces. Lo único que no he confirmado es los precios"*. De paso cerró tres cosas más de la misma lista.
- **Petición:** (1) sillas 3/5/7 CONFIRMADO (base 3; Premium 1 = 5; Premium 2 = 7) — solo faltan los precios; (2) trial = 1 MES: *"un mes es suficiente, de igual lo podemos cambiar luego"*; (3) las empresas demo NO se borran: *"son ejemplos que uso para pruebas, hasta que yo no lo diga no las borramos"*; (4) SEO al final, otra vez.
- **Razonamiento:** arrastrar como pendiente algo ya decidido le hace re-litigar y le quita autoridad a la lista entera — una cola que miente sobre su contenido deja de servir para priorizar. Y dos ítems estaban en jerga mía ("gating visual por switch") que no entendió: un pendiente que el dueño no puede leer no es un pendiente, es una nota mía.
- **Término(s):** decisión cerrada → CONFIRMADO en memoria con fecha y frase · la cola en lenguaje de producto, no de implementación · decidir sin agonizar, con puerta abierta a los datos.

### 71. "Para todo tienen que entrar a la app"
- **Contexto:** le expliqué dos pendientes que estaban en jerga mía y los resolvió en direcciones opuestas.
- **Petición:** (1) bloqueo por llave SÍ, pero discreto: *"bloquear los botones… algún mensaje que aparezca cuando le dé clic pero que no esté siempre… algo que no moleste pero que transmita que no tienes permiso… a veces menos es más, un candadito"*; (2) correo de notificaciones NO: *"la idea es que para todo tengan que entrar a la app, no que tengan que usar el correo, no tiene sentido. El correo será solo para problemas o cosas así, no para notificaciones"*.
- **Razonamiento:** del candado, tres reglas en una frase: el aviso se gana con el intento (no vive en la pantalla), la marca permanente es mínima, y se puede entrar a MIRAR — esconder secciones deja a la persona sin contexto. Del correo: lo que propuse como conveniencia él lo leyó como FUGA — un correo que resume la actividad le quita a la gente la razón de abrir Enkoras; la campana adentro obliga a entrar, y entrando se ve todo lo demás.
- **Término(s):** el aviso se gana con el clic · la escala mínima que comunica (candado < letrero < pantalla) · no construir conveniencias que compitan con el hábito de entrar.

### 72. "Es donde van a trabajar los usuarios"
- **Contexto:** construí el bloqueo por llave solo en el panel de Mi empresa y pregunté si replicarlo en las demás pantallas.
- **Petición:** *"pues es lo más importante, ya que en las diferentes pantallas es donde van a trabajar los usuarios, es obvio que estén los candados ahí también"*.
- **Razonamiento:** yo prioricé por donde él VIO el problema; él por donde pasa el trabajo real (chat y licitaciones — diario) contra el panel (una vez al mes). Un bloqueo a medias no es la mitad del valor: es inconsistencia, y la inconsistencia enseña a desconfiar de la señal. También rechazó mi pausa de "valida el patrón antes de replicar": con un patrón simple ya acordado, la validación intermedia es fricción — las pausas se reservan para lo que tiene forma discutible (layouts, jerarquías), no para repetir lo aprobado.
- **Término(s):** priorizar por frecuencia de uso real, no por dónde se reportó · consistencia o la señal muere · pausas solo para lo discutible.

### 73. "No vamos a ir de la mano diciéndole al user qué debería hacer"
- **Contexto:** defendí el pendiente "¿las pujas necesitan reloj?" con un escenario: una operadora toma una licitación, se enferma, no avisa, y cierra desierta con 4 ofertas buenas.
- **Petición:** matar el pendiente: *"es un escenario ficticio muy tonto y muy alejado de la realidad… alguien que tiene un trabajo real tiene un jefe y alguien a quien reportar… alguien con coherencia y lógica entra y asigna la bid a otro user, para eso están los jefes… no hay forma de que simplemente lo dejen. Nosotros no vamos a ir de la mano diciéndole al user qué debería hacer, y para prevenir cosas como esa es que creamos los roles principalmente"*.
- **Razonamiento:** tres reglas en un mensaje: no diseñar contra disfunciones organizacionales (un empleado que desaparece es problema de esa empresa, no hueco del producto); si una feature YA resuelve el caso (roles + arrebato), no inventar una segunda — la salida estaba en mi propio ejemplo y aun así lo traté como pendiente; y no tutelar al usuario: Enkoras da herramientas, no supervisa cómo la empresa administra a su gente.
- **Término(s):** ¿falla del producto o de la organización que lo usa? · el remedio ya existe = no hay pendiente · herramientas, no tutela.

### 74. "No estás poniendo atención"
- **Contexto:** tres veces en la misma sesión expliqué pendientes con premisas que contradicen decisiones YA tomadas y YA escritas: propuse "marcar empresas del equipo en el selector" (imposible: la cuenta exclusiva de la 066 la construimos ese mismo día) y expliqué el borrado de cuenta con "dos empresas, cada una con su Premium".
- **Petición:** *"ya habíamos definido ese tema hace rato en la sesión, no estás poniendo atención"* · *"no hay múltiples suscripciones por empresa de una sola cuenta, ya hablamos esto miles de veces: es UNA suscripción y esa cubre todas las empresas que tenga registrada esa cuenta; el límite era 2 en Premium 1 y 4 en Premium 2. Ya te lo dije muchas veces y lo has apuntado en las memorias"* · y la silla es por CUENTA: cubre todas las empresas de ese dueño con rol parejo.
- **Razonamiento:** la información no faltaba — estaba escrita, la escribí yo, y aun así expliqué desde el código sin cruzarlo con las decisiones. El código dice cómo está hoy; la memoria dice hacia dónde va y qué ya se decidió. Usar solo lo primero produce explicaciones técnicamente correctas y de producto FALSAS — y casi me pone a invertir en el flujo de cancelación "por empresa" que 5.F va a tirar.
- **Término(s):** el código es el presente, la decisión es el rumbo — la decisión manda · releer la memoria del tema ANTES de explicar el pendiente · trabajo sobre modelo condenado = trabajo a la basura.

### 75. "Un pendiente a la vez"
- **Contexto:** al repasar la cola fijó el método de trabajo.
- **Petición:** *"vamos de un pendiente a la vez y explícame el pendiente primero bien bien y con un ejemplo, y después qué haremos, o sea qué propones de solución, y vamos a ir de uno solo — cuando yo te diga pasamos al siguiente"*.
- **Razonamiento:** no quiere un lote de cambios hechos: quiere entender cada problema ANTES de que se construya, porque entendiendo detecta en la propuesta lo que a mí se me escapa — pasó literalmente con el reloj de las pujas (desarmó la premisa entera) y con eliminar cuenta (corrigió mi modelo de suscripción). Con ese ciclo salieron cuatro bugs de producción que ninguna auditoría había encontrado.
- **Término(s):** explicar con ejemplo → proponer → OK → construir → reportar y PARAR · nunca encadenar dos pendientes · su comprensión es parte del control de calidad.

### 76. "Ya sabes cómo soy con el backend"
- **Contexto:** encargo del design system del admin: *"la sección de admin es más que nada para mí el desarrollador, y aún hay muchas cosas que quiero cambiar de ahí, agregar, etc., pero sería bueno ya tener la base del design system, así que empecemos. Divídelo en bloques y hazlo correctamente, y por favor revisa que tanto el frontend y backend sean perfectos, ya sabes cómo soy"*.
- **Petición:** (1) la BASE del sistema antes que las 9 sombras sueltas que yo iba a arreglar; (2) por bloques, no en lote; (3) "perfecto" incluye el backend aunque el encargo parezca de estilos.
- **Razonamiento:** sabe que va a construir encima y una base torcida multiplica el error en cada cosa nueva — cimientos antes que síntomas. Y su instinto del backend tenía razón: el listado de usuarios iba a mentir con +1000 usuarios, seis acciones fallaban en silencio y la gráfica de sectores nunca había pintado una barra — nada de eso "se veía de estilos".
- **Término(s):** la base antes que el síntoma cuando piensa extender · su "perfecto" nunca es solo lo que se ve · bloques con el hallazgo de backend por delante.

### 77. "Que refleje lo que realmente es hoy"
- **Contexto:** al cerrar 5.B le pasé los pendientes; el primero era qué hacer con la sección "Mis solicitudes" del panel ahora que Solicitudes se retiró. Yo lo planteé binario: ¿se borra o se queda?
- **Petición:** *"creo que se debería cambiar y reflejar lo que realmente es hoy, las bids y cotizaciones, ¿no crees?"*.
- **Razonamiento:** rechazó mis dos opciones y eligió la tercera, que era la correcta: se TRANSFORMA. La sección no sobraba — sobraban su nombre y su contenido, que describían un producto que ya no existe. Retirar una feature no es apagar sus rutas: es que todo lo que la nombraba deje de mentir. Mismo instinto de la 74: mira el producto entero, no el pendiente aislado.
- **Término(s):** retirar = inventariar TODO lo que la nombra y decidir en qué se convierte cada cosa · no encerrar sus decisiones en dos opciones: la tercera mejor existe y la va a encontrar · la app se explica sola.

### 78. "Se quedan la original y las 2 correcciones — y eso es muy grave"
- **Contexto:** dogfooding de 5.B recién construido, con sus dos cuentas. Corrigió su cotización dos veces y miró el tablero del lado convocante: las TRES versiones apiladas. Minutos después, la generalización: *"lo mismo pasa en las bids live"*.
- **Petición:** *"al corregir una cotización, en vez de que se redibuje la original crea otra, y así al final si usas las 2 correcciones se quedan la original y las 2 correcciones — y eso es muy grave"* — y que en vivo sea igual: una sola tarjeta por proveedor.
- **Razonamiento:** vio dos cosas que yo no: la FUGA (la convocante veía precios que el proveedor ya retiró — munición para presionarlo, cuando el trato del sobre cerrado es "mandas un número y ese es el que compite") y que el arreglo es del COMPONENTE, no del síntoma — extendió la regla a los dos modos sin que nadie se lo pidiera. Encima, era una decisión que él YA había tomado ("sobre historial de correcciones, solo la vigente") y que yo no implementé en el bloque C: la cazó usando el producto, no leyendo mis reportes.
- **Término(s):** una fila por proveedor · corregir REEMPLAZA, no acumula · el precio retirado no se enseña · el dogfooding caza los requisitos perdidos.

### 79. "Para mí bids es eso: live y quotes"
- **Contexto:** tras la tanda de arreglos de 5.B pidió barrer todos los datos de prueba de la feature.
- **Petición:** *"elimina todas las bids de pruebas — las ganadas, las cerradas, canceladas, etc. — junto con sus archivos que tengan attached o cotizaciones, y lo mismo con las quotes o requests anteriores… también las notificaciones y etc. que haya quedado guardado de esa feature, para dejar todo limpio"*. Y fijó el vocabulario: *"cuando hablo de la feature de bids me refiero a live y quotes"*.
- **Razonamiento:** antes de probar en serio quiere el terreno en CERO — los datos demo sucios esconden bugs (el "History 6" fantasma nació de ahí) y contaminan el dogfooding. Y "limpiar" para él no es borrar filas: son también los archivos de Storage y las notificaciones — todo lo que la feature dejó regado. Fijar el término compartido ("bids" = la feature entera, sus dos modos) evita otra ronda de malentendidos de vocabulario.
- **Término(s):** vocabulario compartido antes que nada · terreno limpio para el dogfooding · limpiar = filas + archivos + notificaciones, no solo la tabla.

### 80. El diferenciador que no se debe perder
- **Contexto:** corrigió su cotización cambiando solo el precio; la corrección llegó SIN el diferenciador que había escrito en el envío anterior.
- **Petición:** *"tal vez el user solo quiere cambiar el precio y dejar su diferenciador; estaría bien que apareciera precargado por si lo quiere modificar, y si no lo modifica pues que se publique con el mismo que ya tenía. No sé si este mismo problema también pasa en las bids a la hora de mejorar oferta, pero deberíamos checarlo y corregirlo también"*.
- **Razonamiento:** anticipó el error silencioso: un campo marcado "opcional" que en realidad BORRA lo anterior si se deja vacío — porque cada envío reemplaza la oferta entera y el usuario no tiene forma de saberlo. Editar debe arrancar de lo vigente, nunca de cero. Y otra vez el instinto de generalizar: "¿pasa también en live?" — sí: era el mismo componente. Piensa en el gemelo de cada bug sin saber cómo está construido por dentro.
- **Término(s):** editar parte de lo VIGENTE · un campo vacío no significa "igual que antes" · buscar el gemelo de cada bug (mismo componente, otro modo).

### 81. La campana sabe quién eres
- **Contexto:** con la cuenta convocante (golfo) tocó la notificación "te ofertaron" y aterrizó en la pestaña Invitations — como si lo hubieran invitado a ofertar en su propia licitación — con el selector marcando Live sobre una cotización.
- **Petición:** *"me lleva a invitations, lo cual no tiene sentido — ¿por qué me enviaría a invitations si yo soy quien creó la licitación?… otra cosa: me lleva a la tab de live en vez de la de quotes… necesito que vuelvas a analizar por completo la feature de bids, las live y quotes, porque hay muchos bugs"*.
- **Razonamiento:** dos señales: la notificación debe aterrizar según TU RELACIÓN con la cosa (quien convocó llega por "te ofertaron" → a Mías; el invitado → a Para ti) y en la pestaña del modo real — la pantalla no puede contarse dos historias. Y ante tres síntomas no pidió tres parches: pidió el análisis COMPLETO de la feature — olfato de causa común, y la había (la URL sin normalizar).
- **Término(s):** aterrizar según el rol, no según un default · la URL no miente (selector, pestaña y sala cuentan la misma historia) · varios síntomas juntos = buscar la causa común, no parchar de a uno.

### 82. La sala de espera también es producto
- **Contexto:** al ir a publicar su segunda bid de prueba topó con el cronómetro del ritmo de publicación (los 5 minutos entre una y otra).
- **Petición:** *"este anuncio sale muy pegado arriba, está mal en UI/UX, aparte le falta diseño"*.
- **Razonamiento:** una pantalla de espera carga más fricción emocional que casi cualquier otra — te frenó cuando venías a HACER algo — y justo por eso merece diseño: el centro real de la pantalla, un anillo que dice cuánto falta sin leer nada, y una salida útil (ver tus publicadas) para que la espera no sea una celda. Coherente con su doctrina: las microinteracciones y las pantallas "menores" son señal de primera clase, no sobras.
- **Término(s):** las pantallas "menores" no existen · la espera se diseña (progreso visible + salida útil) · el reloj sin porqué es castigo; con porqué es ritmo.

### 83. "Esa es una fuga"
- **Contexto:** dogfooding en la sala de la cotización con su cuenta secundaria: la puja la llevaba su otro usuario (el chip lo decía), el formulario de ofertar estaba cedido… y el botón "Adjuntar cotización" seguía vivo.
- **Petición:** *"si se supone que la puja la está llevando mi otro compañero, no debe ser posible que yo pueda subir cotización — debe estar bloqueado también. Esa es una fuga"*.
- **Razonamiento:** definió el alcance del claim mejor que yo: llevar la puja no es "ofertar" — es TODO el trabajo sobre esa puja, y la cotización formal (subirla, reemplazarla, borrarla) es trabajo sobre la puja. Y la palabra exacta: "fuga", no "falta un candadito" — intuyó que si la pantalla lo permitía, el hoyo era de fondo. Lo era: el candado del titular (061/064) vivía solo en `bids`; las policies de `tender_quotes` pedían la llave y nada más, alcanzable por PostgREST directo. Salió la 082.
- **Término(s):** el claim cubre TODO el trabajo de la puja, no solo la acción principal · cada acción hermana hereda los candados de su acción principal · "fuga" = sospechar del fondo cuando la pantalla permite de más.

### 84. "No podemos ser condescendientes en esto"
- **Contexto:** encontró la race condition del claim: dos compañeros reciben la misma invitación, entran a la vez, la ven libre (el claim solo nacía al PUBLICAR), ambos redactan su oferta 10 minutos… y al segundo la BD le tira el trabajo con "la lleva otra persona". Propuso el diseño completo: la zona operable de la sala nace BLOQUEADA con una capa y un botón al centro ("Asignarme / Reclamar"); el sistema resuelve la carrera asignando al primero; a los demás les avisa con nombre y el realtime les actualiza la pantalla.
- **Petición:** (1) *"el de reclamar solo funciona una vez que ya publicaron, y debería ser desde el momento que tiene la intención"*; (2) ante mi pregunta de si el jefe entra directo sin reclamar: *"que sea parejo para todos, no podemos ser condescendientes en esto. No importa si es jefe o lo que sea: al final es una acción que puede afectar el trabajo de todos. Los permisos de nivel altos no son para querer abarcar o imponer o favoritismo — son para protegernos. No porque sea admin o dueño quiere decir que con solo entrar ya se le asigna"*.
- **Razonamiento:** dos principios. Primero: el claim de hoy protegía el DATO (nunca dos ofertas del mismo equipo) pero no el TRABAJO (los 10 minutos del que pierde la carrera) — la asignación debe ocurrir cuando nace la intención, no cuando termina el esfuerzo. Segundo, y es doctrina: **los permisos altos son poder de corrección, no privilegio de paso** — el rango vive en las acciones deliberadas y visibles (arrebatar, transferir), jamás en saltarse el flujo normal; si el dueño tomara la puja con solo entrar, MIRAR tendría efectos secundarios y espiar "¿cómo va esto?" le robaría el trabajo a la operadora. Es el mismo ADN de "mirar no es editar" del panel: entrar no es tomar; tomar es tomar.
- **Término(s):** la intención se declara, no se infiere · proteger el trabajo, no solo el dato · permisos altos = poder de corrección, no privilegio de paso · flujo parejo + poder auditable.

### 85. "Se escucha muy fácil: haz que tenga multiusuario"
- **Contexto:** reflexión suya al cierre de la tanda de 5.B, viendo cuántas piezas ha exigido el multiusuario (sillas, llaves, claims, candados, vistas por rol, la 082 y la 083 del mismo día).
- **Petición:** no es una petición — es la doctrina, dicha completa: *"una cosa tan simple que salió en una conversación: simplemente dijeron 'que haya multiusuario'. Se escucha muy fácil, pero la realidad es que es muy difícil… claro, tú puedes programarlo, pero la LÓGICA es lo más difícil: pensar en qué puede pasar, en cómo debe ser correctamente, en qué puede que el usuario falle… aparte las diferentes vistas, las opciones, el cómo se verá para cada user… Llevamos demasiadas cosas, pequeñas y grandes, para que el multiuser funcione correctamente. Si no fuera por multiuser, creo que ya hubiéramos terminado antes jaja. Yo siempre pienso en qué errores pueden pasar, en el qué pasaría si hacen esto, qué no debería pasar, cómo lo puedo prevenir — y al final, el cómo debe funcionar. Ya con todas esas preparaciones, ya no debe fallar: ya está bien blindado para que no se rompa. Necesitamos ver siempre los diferentes puntos de vista y situaciones, por más locas que parezcan. Obviamente sin irnos al extremo, como querer prevenir cosas internas de empresa, como coordinación."*
- **Razonamiento:** tiene el diagnóstico exacto de por qué "multiusuario" es la palabra más cara del producto: no SUMA trabajo, lo MULTIPLICA — cada pantalla deja de ser "qué ve el usuario" y pasa a ser "quién de los N, con qué llave, con qué claim, en qué modo, mientras otro hace qué". Y su método es, con nombre técnico, diseño por invariantes + análisis de modos de falla: (1) enumerar qué puede salir mal desde cada punto de vista, (2) decidir qué NO debe pasar jamás, (3) prevenirlo en el modelo (BD), no en la pantalla, (4) y solo entonces definir el funcionamiento feliz — que a esas alturas "ya no debe fallar". Con la frontera de la 73 intacta: se blinda el producto, no la coordinación interna de la empresa cliente.
- **Término(s):** multiusuario multiplica estados, no suma features · primero qué no debe pasar, luego cómo funciona · blindar en el modelo, no en la pantalla · todos los puntos de vista, por locos que parezcan — hasta la frontera de la tutela.
