# Hallazgos — carpeta CAPACITACIONES/DOCUMENTOS PROVEEDORES (iMile) — 8-ago-2026

## 1. 01 REGLAMENTO PROVEEDORES.docx.pdf (leído COMPLETO, 11 págs)
- Doc oficial: iMile Mexico PL【2025】No.0003, "Plan de Evaluación de Calidad Operativa v1.3", emitido por Tian Yi, 14-ene-2026, TRILINGÜE ES/EN/中文, confidencial. Vigencia 1 año o hasta nueva versión; interpretación = depto. operativo iMile México.
- Alcance: iMile México + subsidiarias + **CSP (Channel Service Partner)** + **DS (Directly Operated Station)**. → El cliente es proveedor CSP/DS.
- 15 ítems de evaluación con penalización:
  1. POD inválido (foto receptor + fachada + ID oficial o WhatsApp con autorización) → 30 pesos/ticket, máx 900/proveedor/día
  2. Entrega no reconocida → monto declarado + $1,000 MXN, máx $150 USD
  3. Paquete perdido → monto declarado, máx $150 USD
  4. Cobro ilegal → 100 pesos/ticket
  5. Tiempo de respuesta: queja 3 días (multa 10 pesos + presunción de responsabilidad + pasa a arbitraje), arbitraje 4 días (multa = valor declarado máx $150 USD)
  6. Desempeño: paquetes recibidos antes de las 12pm → salir a ruta en 2 HORAS
  7. 有发未到 (enviado pero no arribado): DS debe registrar "no ha llegado" en 48h del escaneo de salida HUB/CDC, si no → responsabilidad de DS; con registro → evidencia de ambas partes (CCTV)
  8. Paquete dañado HUB/CDC→DS: responsabilidad de DS tras la entrega
  9. Retraso Pick up: ≥72h sin movimiento → responsable la estación de recolección; 200 pesos/pza
  10. Retraso CDC: ≥72h (≥96h para NOROESTE: Sinaloa, Chihuahua, BC, BCS, Sonora, Durango — CDC sale cada 2 días, ~48h en tránsito); 200 pesos/pza
  11. Retraso DC: ≥48h; 200 pesos/pza
  12. DS: >10 días sin estado final tras arribo → penalización valor declarado SIN APELACIÓN; estados finales = NDR/firma/perdido/dañado en 10 días. Entrega dominical ≥60%/mes desde 1-sep-2025, multa 3% de tarifa del mes, máx 5,000 pesos
  13. Vehículos: mínimo 2 (TORNADO/KANGOO, ≥3.3 m³); solo motos si no hay capacidad
  14. Estación abierta hasta 17:30, entregar carga del día
  15. EXCLUSIVIDAD: no servir a competidores (J&T Express) → mín $100,000 MXN proporcional al volumen
- Ciclos de aceptación: ENR 14 días naturales desde entrega; perdido >7 días sin movimiento; cobro ilegal 14 días.
- Procesos: auditoría diaria de 10 PODs aleatorios por estación; archivo DIARIO de guías sin movimiento >21 días; captura automática de tiempos de respuesta.
- **NO HAY ninguna cláusula de API/acceso automatizado/sistemas en el Reglamento.**

## 2. Proceso Tickets Proveedores - CSP - Julio 2026.pdf (leído COMPLETO)
- SISTEMAS: **DS = https://ds.imile.com/** (Service → Ticket Management → Ticket Workbench; o To-do list → My ticket) y **PCS = https://pcs.imile.com/#/Login** (Work Order Management → Ticket Workbench). También se menciona TMS en el doc hermano.
- 3 bandejas: Normal / Complaint / Arbitration — TODAS deben procesarse. Detalle del ticket: valor declarado, tipo, info de CS, processing record.
- Normal tickets: Client Feedback Order Delivered But Not Received (=ENR), Push For Delivery (=perdido), Stop Delivery (retornar), **MEX-Urgent delivery** (entrega máx 2 días; 4 plantillas de respuesta), urgent delivery (solo notificación, no responder), MEX-Delivery failure issues (problema mal registrado por mensajero).
- Quejas: ENR (hasta 14 días post-entrega), Queja Repartidor, Piezas Faltantes, Dirección Incorrecta (mal sorteo→retorno), Paquete Perdido (7 días sin movimiento).
- Arbitraje: Fake Delivered, Internals Are Short, Driver Complaint, lost (auto), tracking update (auto >14 días).
- Arbitraje DIRECTO A COBRO sin apelación: 2nd Fake Delivery, Invalid POD, no process (sin respuesta 24h → cobro 10 auto), Delivery Time Limit (no entregado/retornado en 7 días), lost/Abnormal Closed.
- Flujo respuesta: aceptar → processing → SI RESPONSABLE (sin evidencia, directo) / NO RESPONSABLE (comentarios + evidencia → valida iMile). Apelaciones: 1ª corazón agree/disagree; 2ª escudo Non-Appeal/Appeal. SOLO 2 APELACIONES (3 oportunidades totales).
- **SLA VERSIÓN CSP (más estricto que Reglamento): queja 24 HORAS o cierre automático como válida; ENR sin respuesta en 1 día = DIRECTO A COBRO (sin arbitraje); vida total queja+arbitraje = 5 días (1+4).**
- **EXPORT NATIVO** (clave para ingesta): Ticket Management → Arbitration Ticket → filtro Create Time → Search → **Export** → base con columnas: Ticket no, Related AWB (guía), Create Time, Handling Status, Responsible for Department (estación), Adjudicate Description, Penalty. Pagar = filtrar Handling Status ∈ {Financial Settlement Completed, Pending Financial Settlemented}.
- Evidencias correctas ENR: (a) foto paquete completo + guía visible + ID del titular; (b) paquete frente a domicilio con número de casa; (c) captura WhatsApp: teléfono coincide con DS/ticket + guía + confirmación del cliente.
- Perdido "enviado pero no recibido": escaneo en 48h + mínimo 3 evidencias (sellos, manifiesto, CCTV carga/descarga) → responsabilidad 50/50 si ambas partes correctas.
- FAQ: ticket se crea a la ÚLTIMA estación que escaneó; >1 mes = fuera de tiempo (cliente solo tiene 14 días); zonas rojas/ocurre requieren CP dados de alta por su network; guías ya cobradas NO se reapelan.
- Contactos QC: marco.d@imile.me (Team Leader), andy.yang@imile.me (Manager).

## 3. Proceso Tickets Proveedores-Julio.pdf (leído COMPLETO — versión DS/general, MENOS estricta)
- Mismo compendio pero con SLAs distintos: **queja 72h (3 días) / vida total 14 días** (7 queja + 4+3 arbitraje). Penalización tabla: SLA 72h→10 MXN; sin respuesta >14 días→valor declarado.
- Monitoring tickets AUTOMÁTICOS: Delivery Time Limit (10 días sin movimiento), No Tracking Update (14 días), Lost (declarado perdido).
- Arbitraje: overtime not process (72h), overtime not close (7 días), Delivery Time Limit (10 días), No Tracking Update (14 días).
- Export de Complaint Ticket: columnas Order Number, Associated Waybill, Create Time, Handling Status, **Ticket Sub Type**, Is responsable (From Adjudger), **Deduction**, Adjudicate Description, **Inventory Station**. Pendientes = Handling Status ∈ {Pending Accept, Processing} (tabla dinámica).
- Menciona TMS (además de DS). Paquete sin movimiento aquí: máx 10 días (vs 7 en CSP).
- Contacto: marco.d@imile.me + dore.hu@imile.me (Manager QC).
- **DISCREPANCIA CLAVE ENTRE DOCS: SLAs distintos según versión (Reglamento 3d/4d; CSP 1d/5d total; DS 3d/14d total) → el sistema DEBE parametrizar SLAs por tipo de proveedor/versión vigente.**

## 4. DOC DE DEVOLUCIONES.docx + FORMATO DE DEVOLUCIONES OF.xlsx
- Formato manual de devoluciones a HUB: proveedor, fecha, cantidad, tabla de 50 filas (No., ID/GUIA, motivo), firmas ENTREGA/RECIBE. Motivos: CÓDIGO INCORRECTO, FUERA DE RUTA, RHP/RETORNO A HUB. → Proceso HOY EN PAPEL/EXCEL = oportunidad de digitalización directa.

## 5. iMile - Carta Responsiva.docx
- Carta custodia de activos de iMile a su colaboradora Brisa Lizet Palma Cortez (Specialist QC Tijuana, jefe SHIYAN, 16-ene-2026): laptop Lenovo Yoga 7, celular Samsung A05. Cláusulas: solo actividades iMile, no instalar apps no autorizadas, no cambiar configuración, responsabilidad por robo/daño, devolución al terminar. **Aplica a DISPOSITIVOS PROPIEDAD DE iMILE, no a sistemas del proveedor.** (Plantilla de referencia, quizá para armar cartas responsivas propias.)

## 6. Capacitación - TT Aging - NORTHWEST.pptx (leído COMPLETO — es el contenido del video CAP. TIKTOK AGING.mp4)
- TT Aging = paquetes de sellers TikTok con demasiado tiempo en la región sin entrega/intentos.
- Resolución ANTES DE LAS 2:00 PM hora local. Problema a colocar: "Cancel — Already Cancelled With Seller".
- Ruta en DS: Services → Service Quality → **Problem Management** → agregar guías (multiguía) + problema Cancel/Return.
- Escenarios: paquete en almacén (agregar problema + devolver a HUB como devolución normal) vs en ruta (retornar a almacén, aplicar Back to DS, re-aplicar problema; problema de no-entrega se agrega de preferencia desde la APP DEL REPARTIDOR o PDA).
- SIEMPRE verificar estar en tu estación antes de operar.

## 7. PROCESO DE GESTION DE ENTREGAS Y PODs.pdf (46 láminas, LEÍDO)
- "Proceso de Gestión de POD estándar", 2025.10.10, por Santos Lui Enshuo (Control Tower Supervisor) + Luis Andrés Álvarez Vargas (QC); colaboración: Joshua Aarón Saucillo (Network Mgmt) + María Teresa López Polanco (Operations Mgmt).
- **APP iMile DRIVER** (com.imile.redelivery; Android 10+/iOS 14+; descarga vía ds.imile.com/hermes o App Store id6670469599). Login con cuenta de driver (formato D2101). Home: Tareas de entrega (Pending Delivery / Anormalidad / Completo / Problem), Operación de escaneo (Escaneo de entrega, Delivered Scan, Problem Scan, Añadir contactos), Búsqueda de Entrega; tabs Pickup / Entrega / **MAPA** / Más.
- Flujo de entrega en app: lista de tareas ordenable/filtrable **con distancia al destino** (ej. 0.691km) y accesos a mensaje/tel/WhatsApp por parada → escaneo AWB → Confirmar: "Firmado por" (categorías: otros/amigo/padres/hijo/vecino/en persona/puerta oficina/puerta casa/pabellón Baoan=caseta vigilancia), mín 2 fotos (paquete y fachada con número; hasta 5), "Firmado real" (nombre+apellido obligatorio), firma en pantalla → Entregado (upload con progreso Todo/Éxito/Fallado).
- **CADA ENTREGA REGISTRA GPS**: pantalla de Firma muestra Entrega{Escaneado, Tiempo, Firmado real, Datos escaneados (AWB), **Ubicación (lat,long)**, Nombre del conductor (ej. "IB - 18 Cesar Lozano")}; las FOTOS llevan marca de agua con AWB + Time + Location.
- PODs correctos/incorrectos: firma clara y reconocible (tache/garabato = inválida), lugar de entrega anotado si es punto de recepción, foto fachada+paquete JUNTOS (separados = inválido), cliente interactuando con paquete + guía legible, ID junto con paquete (no por separado).
- Excepciones: autorización por WhatsApp/SMS con captura (debe mostrar teléfono registrado + guía + autorización expresa) para: persona autorizada/punto de recepción, dejar en entrada/cochera, menores de edad, cliente se niega a dar ID (pedir ID es OBLIGATORIO; fotografiarla es OPCIONAL con autorización).
- **MEJORAS DE LA APP**: (1) **OFFLINE RECORD** — entregas sin señal quedan en cola local (Más → Registro sin conexión: Pickup/OFD/Delivered/Problem Scan) y se suben solas al volver la conexión; (2) **GEORREFERENCIACIÓN A ≤500m** — la entrega solo puede cerrarse dentro de un radio de 500m de la ubicación registrada; si se cierra a más de 500m con buena señal, la app EXIGE justificar motivo (domicilio mal ubicado en mapa / condominio-barrio cerrado-zona industrial / cliente pidió otra ubicación / sin internet). El mapa de la app muestra pins de entregas.

## 8. Invoicing 2025.pdf (24 láminas, LEÍDO) — manual del sistema PCS (facturación del proveedor)
- **PCS = pcs.imile.com** — portal financiero del proveedor (login por Email/Account o supplier code; registro disponible; cambia a español). Home: Bill mensual con Importe total de factura / por confirmar / liquidación; Gestión de facturas y de solicitudes de pago; entradas pendientes de confirmación/apelación; centro de notificaciones.
- Gestión financiera → Gestión de facturas: filtros código/fecha/tipo/estado/liquidación/país/estación. **Tipos de factura**: "Factura de tarifas" = **CORTE SEMANAL**; "Ajustar la factura" = ajustes al corte (positivo=bono, negativo=deducción); "Arbitration Bill" = por ahora NO se toma en cuenta. **Estados**: Audited (aprobada por finanzas) / Rejected / Beupload (pendiente de subir). Estado de liquidación: Liquidado / Sin resolver. Ciclo de facturación por fila (periodos semanales).
- **EXPORT NATIVO FINANCIERO**: "Crear archivo de exportación" → descarga EXCEL con el detalle de guías consideradas para pago.
- Conciliación multi-estación: seleccionar PZDs de TODAS las estaciones con el mismo ciclo → "Confirmación de la conciliación" → factura POR EL IMPORTE TOTAL (corte − deducciones, ya con IVA; ya no se requiere nota de crédito) → subir archivo de factura (CFDI) → Confirmar → Decisión. (Ejemplos reales muestran estaciones XAL-VER-Tuzamapan y TGZ Tuxtla — el material circula entre CSPs multi-estación.)

## 9. TT MEX URGENT 1-4.jpg (diagramas del cliente, LEÍDOS)
- #1 Diagrama de flujo Urgent Delivery (la base del diagnóstico): CS genera ticket → ¿tipo? → TikTok (responder "24 o 48 horas") / Otros clientes (NO se contesta) → búsqueda física → localizado (a ruta, entrega mismo día) / no localizado (perdido) → avisar a proveedores/drivers → fin.
- #2 Puntos importantes: PRIORIDAD TikTok (si ya está en ruta, avisar al driver que apure); **seguimiento antes de la 1:00 pm — si no se ha entregado, re-notificar porque debe entregarse antes de las 4:00 pm**; evidencias obligatorias de no-entrega (foto domicilio, foto fraccionamiento sin acceso, captura de llamada no contestada, captura WhatsApp/mensaje); no localizado tras búsqueda física → estatus "EXTRAVIADO".
- #3 Dos tipos de tickets urgentes: MEX Urgent Delivery (TikTok — prioridad alta, SÍ se responde dentro del SLA) vs Request Ticket - Urgent Delivery (Temu/SHEIN/otros — NO se responde, se gestiona internamente).
- #4 FAQ del flujo (16 preguntas): quién genera (CS), dónde llega (Ticket Workbench), tiempos límite (1pm/4pm), evidencias, quién autoriza declarar perdido (responsable de operación), **KPIs mencionados: % entregas mismo día, tiempo de respuesta, tickets contestados TikTok, paquetes extraviados, satisfacción del cliente**.

## 10. PROCESO DE OFD Y BACK TO DS 1-2.jpg (LEÍDOS — es el contenido del video homónimo de 1.6GB)
- Ciclo de entrega **HASTA 3 INTENTOS**: carga a ruta (estatus OFD) → intento de entrega → ¿recibe? SÍ=entregado / NO=**registrar evidencia válida** (foto calle si dirección incompleta, llamada, WhatsApp, SMS, correo, foto acceso a fraccionamiento + intento de contacto) → regreso al almacén con **escaneo obligatorio Back to DS** → fin del ciclo de visita → ¿ya son 3 intentos? NO=nueva carga a ruta / SÍ=**generar RHP (Return to Hub Process)**: reetiquetado y envío a CDC México para retorno.
- Puntos clave: todo intento fallido con evidencia válida; todo no-entregado regresa con Back to DS; sin evidencias y sin escaneo el ciclo queda incompleto; RHP SOLO tras 3 intentos documentados. Lema: "Evidencia correcta, escaneo correcto, entrega completa."

---

# SÍNTESIS ESTRATÉGICA PARA EL SISTEMA ENKORAS

1. **El universo iMile del cliente tiene 3 sistemas**: DS (ds.imile.com — operativo: tickets, problem management), PCS (pcs.imile.com — financiero: cortes semanales, conciliación, facturas) y la app iMile Driver (entregas, PODs, GPS). El cliente OPERA DENTRO de estos sistemas; nuestro sistema es la capa de visibilidad/gestión ENCIMA.
2. **La ingesta puede arrancar 100% con exports nativos que iMile MISMO enseña a usar**: base de Complaint Tickets, base de Arbitration (paquetes a pagar), y Excel de guías a pago del PCS. Columnas ya conocidas. → El debate del API pierde urgencia: hay camino legal e indiscutible desde el día 1 (el usuario descarga, el sistema ingiere). API = optimización futura.
3. **El GPS ya existe dentro de iMile** (coordenada por entrega, marca de agua en fotos, geocerca de 500m, tab Mapa) — pero es dato DE iMile. Pregunta clave del lunes: ¿el mapa de repartidores que quiere el cliente es sobre SUS drivers con app propia, o le basta explotar las coordenadas por entrega que iMile registra?
4. **El Reglamento es la spec del motor de penalizaciones**: 15 ítems con montos exactos → el módulo de incidencias puede calcular PESOS PERDIDOS por causa/estación/cliente (el argumento de venta más fuerte: "esto te está costando X al mes").
5. **Los SLAs son INCONSISTENTES entre documentos** (Reglamento: 3d/4d; CSP jul-2026: 24h, vida 5d; DS jul: 72h, vida 14d; urgentes: 1pm/4pm mismo día; aging: 2pm) → el sistema DEBE parametrizar SLAs por tipo de ticket/cliente/versión; y esa confusión es en sí un dolor que el sistema resuelve (semáforos de vencimiento).
6. **Quick win de papel**: el formato de devoluciones (hoja impresa de 50 filas con firmas) es digitalizable de inmediato.
7. Reglas operativas duras para el motor de alertas: despacho ≤2h (antes de 12pm), urgente TikTok mismo día (1pm aviso / 4pm límite), aging antes de 2pm, 3 intentos + Back to DS + RHP, dominical ≥60%, cierre de estado en DS ≤10 días (sin apelación), sin movimiento 21 días (reporte diario automático de iMile).
