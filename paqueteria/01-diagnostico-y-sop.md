# Diagnóstico y SOP — Proceso de Urgent Delivery

> **Contexto en el arsenal:** este es el **Proyecto 1 de la escalera del cliente** — el
> sistema de paquetería/rastreabilidad que Enkoras debe **entregar en ~1 mes** (sep-2026)
> y que abre la puerta a los proyectos Shein y Sells (esas carpetas se crean solo con luz
> verde). El documento de abajo es el diagnóstico que ya se entregó/preparó para el
> cliente — es la base del levantamiento y del scope del sistema.
> Fuente original: `Diagnostico paqueteria.docx` (convertido fiel a Markdown, 8-ago-2026).

---

**Proceso:** Urgent Delivery — Mex Urgent Delivery (TikTok) y Otros Clientes
**Preparado para:** Quality Control / Operaciones — **iMile México**
**Basado en:** diagrama de flujo, Reglamento de Proveedores v1.3 y carpeta de Drive "CAPACITACIONES / DOCUMENTOS PROVEEDORES"
**Fecha:** 5 de agosto de 2026 | **Versión:** 3.0 (incluye áreas de oportunidad tecnológica)

---

## 1. Resumen ejecutivo

La carpeta contiene tres archivos: el diagrama de flujo operativo de Urgent Delivery, el
Reglamento de Proveedores v1.3 (documento formal de evaluación con definiciones, SLA y
penalizaciones) y un video de capacitación que no pudo procesarse. Al cruzar el diagrama
con el Reglamento aparece el hallazgo más importante: **son dos documentos que deberían
estar alineados y no lo están.** El Reglamento ya define con precisión qué es un "paquete
perdido" (7 días sin movimiento), exige POD con evidencia específica, fija un límite de 2
horas para despachar paquetes en estación y establece multas por cada incumplimiento —
pero el flujo de Urgent Delivery no referencia ninguna de estas reglas. Además del SOP,
se identifica una **oportunidad mayor de digitalización**: hoy no existe un módulo
centralizado de incidencias, un tablero de KPIs generales ni visibilidad en tiempo real
de los repartidores. Este documento presenta el diagnóstico, esas áreas de oportunidad,
un SOP que integra ambas fuentes, y las preguntas pendientes para cerrar los vacíos
restantes.

## 2. Fuentes revisadas

| Archivo | Tipo | Uso en este análisis |
|---|---|---|
| TT MEX URGENT 1.jpg | Imagen (diagrama de flujo) | Base del proceso operativo de Urgent Delivery (TikTok y otros clientes) |
| 01 REGLAMENTO PROVEEDORES.docx.pdf | PDF (11 págs., v1.3, ene-2026) | Reglamento formal de evaluación operativa: definiciones oficiales, SLA, reglas de responsabilidad y penalizaciones |
| CAP. TIKTOK AGING.mp4 | Video (1.3 GB) | No pudo analizarse (sin capacidad de procesar video en ese entorno) |

> **Limitación:** el contenido de "CAP. TIKTOK AGING.mp4" no está reflejado en este
> análisis. Con una transcripción, notas clave o capturas se incorpora.

## 3. Diagnóstico del proceso actual

### 3.1 Qué se está haciendo bien

- Punto de decisión claro para clasificar el tipo de ticket (TikTok vs. otros clientes), permitiendo un trato diferenciado.
- Compromiso de tiempo de entrega explícito (24-48 horas) comunicado al cliente en el caso de TikTok.
- El flujo contempla un desenlace explícito para paquetes no localizados, evitando tickets sin resolución.
- Existe coordinación con proveedores/drivers después de sacar a ruta, conectando el proceso interno con la ejecución en campo.
- Ya existe una base normativa sólida y detallada — el Reglamento de Proveedores v1.3 — con definiciones oficiales, reglas de responsabilidad y penalizaciones claras. **Es un activo fuerte sobre el cual construir.**

### 3.2 Hallazgo principal: el flujo operativo y el Reglamento no están alineados

- El Reglamento exige captura de **POD válido** (foto del receptor, foto de fachada, ID oficial o confirmación por WhatsApp) y penaliza su ausencia con 30 pesos/ticket — el flujo de Urgent Delivery **no menciona la captura de POD en ningún paso**.
- El Reglamento define "paquete perdido" como sin movimiento >7 días naturales en la misma estación — el flujo usa el mismo término para un paquete no localizado el mismo día, sin aclarar si es una bandera interna previa o si dispara el proceso formal (y su penalización).
- El Reglamento exige que los paquetes que llegan a estación **salgan a ruta dentro de 2 horas** (regla de "Desempeño") — el flujo no referencia este límite.
- El Reglamento fija plazos de respuesta a tickets (3 días quejas, 4 días arbitraje) con multas — el flujo solo indica "se contesta / no se contesta", especialmente riesgoso para los tickets de "otros clientes" que hoy **no se responden**.

### 3.3 Otros gaps operativos

- **Roles no definidos en el flujo**: el Reglamento asigna responsabilidad por nodo de red (DS, HUB/CDC, estación), pero el flujo no indica qué rol interno (Customer Service, almacén, dispatch) ejecuta cada paso.
- **Sin trazabilidad de escaneos**: el Reglamento basa sus reglas de responsabilidad en escaneos de Pick up / CDC / DC / DS; el flujo no indica en qué punto interviene el proceso urgente.
- **No se contemplan excepciones comunes**: dirección incorrecta, cliente no disponible, zona sin cobertura, ticket fuera de horario operativo (estación opera hasta 17:30).
- **No hay mecanismo de escalamiento** visible cuando un ticket está en riesgo de superar los plazos.
- **No existe visibilidad centralizada** de incidencias, KPIs ni ubicación de repartidores — la gestión depende de tickets y reportes manuales dispersos.
- El video de capacitación no analizado podría contener información operativa relevante (posiblemente manejo de "aging" para TikTok).

## 4. Recomendaciones de implementación (por impacto)

1. Alinear explícitamente el flujo de Urgent Delivery con el Reglamento v1.3: captura de POD, límite de 2 horas de despacho y plazos de respuesta (3/4 días).
2. Aclarar si el "paquete perdido" del flujo urgente es una bandera interna previa a los 7 días del Reglamento, o si debe redefinirse.
3. Definir un SLA de respuesta interno para tickets de "otros clientes" (evitar la multa automática de 10 pesos/ticket y el escalamiento a arbitraje).
4. Asignar responsables (matriz RACI) por etapa, mapeados a los nodos del Reglamento (DS, HUB/CDC, estación).
5. Implementar **registro digital de cada ticket con timestamps por etapa**, incluyendo los escaneos de red que determinan responsabilidad.
6. Formalizar el canal y protocolo de aviso a proveedores/drivers, con confirmación de recibido.
7. Documentar el manejo de excepciones (dirección incorrecta, cliente ausente, fuera de cobertura, fuera de horario).
8. Definir KPIs alineados a las metas que ya fija el Reglamento (entrega dominical ≥60%, despacho ≤2 horas, % POD válidos).
9. **Construir la plataforma de visibilidad operativa** (sección 5: incidencias, KPIs y mapa de repartidores).
10. Solicitar transcripción o resumen del video de capacitación.

## 5. Áreas de oportunidad — Plataforma de visibilidad operativa

> **Esta sección es el corazón del proyecto de Enkoras**: los tres módulos que pueden
> desarrollarse de forma independiente pero comparten la misma base de datos.

| Módulo | Objetivo | Complejidad |
|---|---|---|
| 1. Incidencias de no-entrega | Centralizar y visualizar todas las causas por las que un paquete no se entrega | Baja–Media (se apoya en datos de tickets existentes) |
| 2. KPIs generales de la empresa | Consolidar indicadores operativos y de negocio en un tablero único | Media (integrar varias fuentes de datos) |
| 3. Mapa de repartidores en tiempo real | Visualizar ubicación y movimiento de drivers en ruta | Alta (requiere GPS/app en dispositivos de proveedores) |

### 5.1 Módulo de incidencias de no-entrega

Centralizar todas las causas de no-entrega usando la taxonomía del Reglamento (POD
inválido, entrega no reconocida, paquete perdido, cobro ilegal, paquetes sin movimiento)
más las excepciones operativas (dirección incorrecta, cliente no disponible, sin cobertura).

- Tablero con desglose por causa, cliente (TikTok / SHEIN / Temu / otros), estación, proveedor y periodo.
- Filtros por severidad/costo usando las penalizaciones del Reglamento como referencia.
- Alertas automáticas cuando una causa supera un umbral.
- Vista de tendencia histórica (patrones por estación, proveedor, tipo de cliente).
- Trazabilidad del ciclo de vida de cada incidencia: apertura, investigación, responsable, resolución, penalización aplicada.

**Valor:** pasar de "reaccionar ticket por ticket" a identificar causas raíz sistémicas.

### 5.2 Tablero de KPIs generales de la empresa

- Los KPIs del SOP (sección 6.9): % dentro de SLA, tasa de perdidos, tiempo de respuesta, % POD válidos, entrega dominical, tiempo de despacho.
- Indicadores de negocio: volumen total, entregas al primer intento, costo total de penalizaciones por periodo, ranking por proveedor/estación, satisfacción del cliente.
- Comparativo entre clientes (TikTok vs. SHEIN vs. Temu).
- Vista ejecutiva (mensual) y vista operativa (diaria) según nivel de usuario.

**Valor:** visibilidad ejecutiva global; decisiones basadas en datos, no reportes manuales.

### 5.3 Mapa en tiempo real de repartidores

- Geolocalización en tiempo real de cada driver/proveedor en ruta (app móvil o GPS).
- Estatus visual por color: en ruta, en entrega, detenido, fuera de la ruta planeada.
- Cruce con tickets urgentes: qué guías trae asignadas cada driver y su hora estimada.
- Alertas por desvío significativo o detención prolongada (relevante para el límite de 2 horas).
- Histórico de rutas para auditoría (resolver disputas de "entrega no reconocida" con evidencia de presencia en el domicilio).

**Valor:** visibilidad en tiempo real, cumplimiento del SLA de 2 horas, evidencia para incidencias.

### 5.4 Consideraciones de implementación

- Los tres módulos comparten la necesidad de **captura de datos estructurados y en tiempo real** (tickets, escaneos, GPS) — hoy no centralizada.
- Evaluar si el sistema/CRM actual puede extenderse o si se requiere plataforma nueva (ver preguntas 7.3).
- Priorización sugerida: **incidencias primero** (datos existentes), luego KPIs (integración), al final el mapa (mayor inversión — depende de GPS/app en proveedores).

## 6. SOP propuesto — Manejo de Tickets de Entrega Urgente

### 6.1 Propósito
Estandarizar la atención, búsqueda, despacho y cierre de tickets urgentes, integrando el Reglamento de Proveedores v1.3, asegurando SLA y trazabilidad completa.

### 6.2 Alcance
Todos los tickets Urgent Delivery recibidos por Customer Service: Mex Urgent Delivery (TikTok) y Urgent Delivery de otros clientes (SHEIN, Temu y adicionales).

### 6.3 Roles y responsabilidades (propuesta, a confirmar con la operación)

- **Customer Service**: recepción y clasificación; comunicación con cliente final; control de plazos (3/4 días).
- **Almacén / Operaciones (estación)**: búsqueda física; cumplimiento del límite de 2 horas de despacho.
- **Dispatch / Coordinación de ruta**: asignación a ruta y aviso a proveedores/drivers.
- **Driver / Proveedor**: entrega y captura de POD válido conforme al estándar.
- **Supervisor de turno**: escalamiento y decisiones sobre paquetes no localizados.
- **Quality Control**: auditoría diaria de PODs, métricas y penalizaciones.

### 6.4 Definiciones oficiales (Reglamento Proveedores v1.3)

| Término | Definición oficial |
|---|---|
| POD inválido | El POD cargado no cumple los estándares (foto de receptor, foto de fachada e ID oficial, o WhatsApp con autorización) |
| Entrega no reconocida | El sistema muestra entregado, pero el cliente indica no haberlo recibido |
| Paquete perdido | Sin movimiento por más de 7 días naturales en la misma estación |
| Cobros ilegales | Cobro adicional no autorizado al cliente |
| Tiempo de respuesta | 3 días naturales tickets de queja, 4 días tickets de arbitraje |
| Paquetes sin movimiento | Guías sin actualización de estatus por más de 21 días; reporte diario automático |
| Tickets | Comprobante para documentar y dar seguimiento a una solicitud/problema hasta su resolución |
| OFD | "Out for Delivery" — en camino para su entrega |

### 6.5 Procedimiento

| Paso | Actividad | Responsable (propuesto) | Detalle |
|---|---|---|---|
| 1 | Generación del ticket | Customer Service | Ticket urgente en sistema: cliente y número de guía |
| 2 | Clasificación | Customer Service | Tipo de cliente (TikTok/otros); queja o arbitraje conforme al Reglamento |
| 3a | Respuesta al cliente (TikTok) | Customer Service | Siempre: "La guía [XXX] se entrega en 24 o 48 horas, según pueda salir a ruta" |
| 3b | Respuesta al cliente (otros) | Customer Service | Hoy NO se responde. Riesgo: el Reglamento exige responder quejas en 3 días (multa 10 pesos/ticket) |
| 4 | Búsqueda física | Almacén / Operaciones | Si el paquete ya está en estación: salir a ruta dentro de 2 horas (regla de Desempeño) |
| 5 | Despacho a ruta | Dispatch / Coordinación | Localizado → a ruta y entrega el mismo día |
| 6 | Captura de POD | Driver / Proveedor | Foto de quien recibe + paquete en fachada + ID oficial (o WhatsApp con confirmación). POD inválido = 30 pesos/ticket, máx. 900/día |
| 7 | Aviso a proveedores/drivers | Dispatch / Coordinación | Notificación por [definir canal], con confirmación de recibido |
| 8 | Cierre del ticket | Customer Service | Registra POD y cierra en sistema |
| 9 | Manejo de no localizado | Supervisor de turno | "Posible pérdida"; a los 7 días sin movimiento → Paquete Perdido formal + penalización |

### 6.6 Manejo de excepciones

- **Paquete no localizado**: escalar a supervisor; investigación (últimos movimientos, almacén, CCTV); a los 7 días → Paquete Perdido formal.
- **Dirección incorrecta/incompleta**: contactar al cliente final antes de reprogramar o cancelar.
- **Cliente no disponible**: protocolo de reintento (p. ej. hasta 2 intentos adicionales) y notificación.
- **Zona sin cobertura**: escalar a coordinación de rutas (proveedor externo, ajuste de SLA).
- **Ticket fuera de horario** (estación hasta 17:30): definir siguiente turno o guardia de urgencias.

### 6.7 Escalamiento

Quejas: responder en 3 días naturales o multa de 10 pesos/ticket + escala automática a
arbitraje. Arbitraje: responder/apelar en 4 días o multa = valor declarado (máx. $150
USD). Recomendación: alerta a Customer Service al 70% de cada plazo; notificación a
Quality Control si un ticket está por vencer sin respuesta.

### 6.8 Penalizaciones aplicables (resumen)

| Incidencia | Estándar / Umbral | Penalización |
|---|---|---|
| POD inválido | Falta foto de receptor, fachada, ID o WhatsApp | 30 pesos/ticket, máx. 900/proveedor/día |
| Entrega no reconocida | Figura entregado sin evidencia válida | Monto declarado + $1,000 MXN, máx. $150 USD/proveedor |
| Paquete perdido | Sin movimiento >7 días en la misma estación | Monto declarado, máx. $150 USD/proveedor |
| Cobro ilegal | Cobro no autorizado sin evidencia que lo desmienta | 100 pesos/ticket |
| Respuesta a queja | Sin respuesta en 3 días naturales | 10 pesos/ticket + responsabilidad automática + arbitraje |
| Respuesta a arbitraje | Sin respuesta/apelación en 4 días | Multa = valor declarado, máx. $150 USD |
| Desempeño (despacho) | Salir a ruta en 2 horas | Según reglas de retraso por etapa (hasta 200 pesos/pieza) |
| Entrega dominical | ≥60% mensual por estación (desde sep-2025) | 3% de la tarifa de reparto del mes, máx. 5,000 pesos |
| Sin movimiento | >21 días sin actualización | Revisión y asignación de responsabilidad |
| Exclusividad | Servir a competidores de iMile | Mínimo $100,000 MXN, proporcional al volumen |

*(El Reglamento completo incluye 15 puntos de evaluación; aquí solo los aplicables a este flujo.)*

### 6.9 Métricas / KPIs

| Indicador | Definición | Meta / Referencia | Frecuencia |
|---|---|---|---|
| % de entregas dentro de SLA | Entregadas en 24/48h vs. total urgentes | A fijar (no definida en Reglamento) | Semanal |
| Tasa de paquetes perdidos | >7 días sin movimiento vs. total urgentes | Mínimo posible | Mensual |
| Tiempo de respuesta a tickets | Días hasta primera respuesta | ≤3 (queja) / ≤4 (arbitraje) | Semanal |
| % de POD válidos | PODs válidos sobre muestra auditada | Auditoría diaria de 10 aleatorios | Diaria |
| Tasa de entrega dominical | Éxitos domingo vs. pendientes al sábado | ≥60% | Semanal |
| Tiempo de despacho | Llegada a estación → salida a ruta | ≤2 horas | Diaria |

### 6.10 Documentos relacionados

- Diagrama de flujo original: TT MEX URGENT 1.jpg
- Reglamento Proveedores v1.3 (vigente desde enero 2026)
- Video de capacitación: CAP. TIKTOK AGING.mp4 (pendiente de revisión)

### 6.11 Control de documento

- Versión: 3.0 — incorpora áreas de oportunidad tecnológica
- Fecha: 5 de agosto de 2026
- Preparado por: Consultoría / Quality Control
- Próxima revisión: [definir]

## 7. Preguntas clave para completar el levantamiento

### 7.1 Alineación flujo–Reglamento (prioritarias)
- ¿El "paquete perdido" del flujo urgente es la clasificación de 7 días del Reglamento o una bandera interna previa? ¿Cómo se conectan?
- ¿Por qué el flujo no incluye la captura de POD? ¿Se captura hoy y solo no está documentada?
- ¿En qué nodo de la red (Pick up, CDC, DC, DS) ocurre el proceso urgente? ¿Aplica la regla de 2 horas?
- ¿Los tickets urgentes son de queja (3 días) o arbitraje (4 días)? ¿Aplica igual a TikTok y a otros?
- ¿Qué contiene el video "CAP. TIKTOK AGING.mp4"? ¿Debería incorporarse al SOP?

### 7.2 SLA y clientes
- ¿SLA exactos de entrega por cliente (TikTok, SHEIN, Temu, otros)?
- Si no se responde a "otros clientes", ¿cómo se evita la multa del plazo de 3 días?
- ¿Existen otros tipos de cliente o ticket urgente no reflejados?

### 7.3 Roles y sistema
- ¿Cómo se mapean los roles internos a los nodos de responsabilidad (DS, HUB/CDC, estación)?
- **¿Qué sistema o CRM se usa hoy para tickets y escaneos?** (clave para decidir extender vs. plataforma nueva)
- ¿Existe supervisor de turno o punto de escalamiento definido?
- ¿Existe tablero de BI o todo son hojas de cálculo y reportes manuales?

### 7.4 Plataforma de visibilidad (los módulos propuestos)
- ¿Los drivers cuentan con app o dispositivo GPS, o se implementa desde cero?
- ¿El sistema/CRM actual permite módulos nuevos o se requiere plataforma independiente?
- ¿Qué prioridad y presupuesto tiene el desarrollo frente a los ajustes de proceso?
- ¿Quién sería el dueño/administrador de cada módulo (Operaciones, QC, TI)?

### 7.5 Operación y desempeño
- ¿Desempeño actual vs. metas del Reglamento (dominical, 2 horas, % POD)?
- ¿Cómo se maneja hoy dirección incorrecta o cliente no disponible?
- ¿Qué pasa con un ticket urgente después de las 17:30?
- **¿Volumen diario/semanal de tickets urgentes por tipo de cliente?** (clave para dimensionar)

### 7.6 Documentación existente
- ¿Existen reglamentos o SOPs regionales/departamentales del proceso?
- ¿Hay capacitación formal más allá del diagrama y el video?
