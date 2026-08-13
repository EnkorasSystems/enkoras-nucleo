# ALTAM — Roadmap de Construcción

> Plan de construcción del sistema ALTAM (2 apps —móvil del chofer + web de
> supervisor— conectadas a las apps de iMile). Sigue al scope (board ALTAM / doc 06)
> y a la planeación (doc 07). Estructurado en **bloques enumerados**; cada bloque en
> partes numeradas que indican **qué va primero y qué después**. Sin fechas.
> Materializado también en Plaky (space "Enkoras Projects", board propio del roadmap).

## Cómo leer este roadmap
- **Orden:** los bloques van en orden de dependencia. B0 (fundación) y B1 (iMile) son
  la base; sin ellas no se sostiene lo demás. De B2 a B9 son los módulos funcionales.
- **B10 (Ciberseguridad) es TRANSVERSAL:** no se hace "al final", se aplica a lo largo
  de todos los bloques y se AUDITA en este bloque dedicado.
- **Vista de entrega por fases (MVP que enamora):** la primera versión demostrable =
  B0 + B1 + B2 + B3 + B4 + B6 + un B9 básico (el ciclo manifiesto → segregación →
  asignación → reparto con tracking → dashboard). B5, B7 y B8 agregan profundidad.
  B11 lanza y B12 opera/evoluciona.
- **Cada parte contempla sus casos límite** (mala señal, datos faltantes, caídas de
  iMile, fraude, GPS apagado, etc.) — descritos en el comentario de cada card.

## B0 — Fundación técnica

### 0.1 Repositorios y estructura del proyecto
Crear los repos: backend/panel web (Next.js + Supabase) y app móvil (React Native + Expo). Decidir monorepo vs. repos separados, convenciones, linting, formato y TIPOS COMPARTIDOS entre web y móvil (Zod). Es la primera piedra: sin esto nada se sostiene.

### 0.2 Supabase: proyecto y servicios base
Crear el proyecto Supabase y habilitar Postgres + PostGIS (geo), Auth, Storage y Realtime. Entornos separados de staging y producción. Aún sin datos ni features: solo la plataforma lista.

### 0.3 Esquema de datos definitivo (multi-tenant)
Diseñar TODAS las tablas desde el día 0 aunque su UI llegue en bloques posteriores: tenants, usuarios/roles, choferes, unidades, manifiesto, paquetes, bins, zonas, incidencias, entregas, ubicaciones GPS, combustible, mantenimiento y cola de eventos salientes. Cada tabla con tenant_id. Migrar esquema con datos en producción cuesta 10x — se define completo ahora.

### 0.4 Roles, RLS y aislamiento multi-tenant
Definir los roles (chofer, supervisor, gerencia, admin) y aplicar RLS en todas las tablas: cada quien ve SOLO lo suyo y cada cliente queda aislado del otro. Es la base de seguridad del sistema y se diseña junto con el esquema, no después.

### 0.5 CI/CD, entornos y despliegue
Pipeline de integración continua y despliegue: Vercel para el web, EAS para la app. Variables de entorno y secretos fuera del repo. Monitoreo (Sentry) y respaldos automáticos activados desde el inicio, no al final.

### 0.6 Capa de integración iMile (adaptador aislado)
Crear el módulo que aísla TODO lo de iMile en un solo lugar, arrancando con datos simulados (mock) para no depender de iMile para avanzar. Cambiar la fuente real (API o export) más adelante = tocar solo este archivo, no el sistema.

## B1 — Integración con iMile (Módulo 2.1)

### 1.1 Contrato de datos del manifiesto
Definir y documentar el modelo del manifiesto entrante (rutas, tracking numbers, dirección, geo). Depende de confirmar el formato real con iMile (pendiente que investiga Javi). Modelar flexible para absorber variaciones sin romper.

### 1.2 Ingestión del manifiesto
Construir la entrada del manifiesto según lo que dé iMile (API o archivo/export). Idempotente: si el mismo manifiesto llega dos veces, no se duplica. Validar datos: paquetes sin dirección, campos faltantes, formatos raros.

### 1.3 Normalización y geolocalización
Normalizar direcciones y obtener coordenadas (del manifiesto si vienen; si no, geocodificar con la base de SEPOMEX + Google como respaldo). Manejar el caso de direcciones incompletas o que no geocodifican.

### 1.4 Cola de eventos salientes a iMile (tiempo real)
Construir la cola que envía a iMile cada evento (asignación, entrega, incidencia) EN TIEMPO REAL. Garantizar orden, reintentos ante fallo y que ningún evento se pierda.

### 1.5 Resiliencia de la integración
Manejar caídas de iMile: si su API no responde, los eventos se encolan y se reintentan, y el sistema sigue operando en local. Dedupe, idempotencia y alertas cuando la integración falla. Confirmar si iMile acepta tiempo real o exige batch.

### 1.6 Pruebas con datos reales de iMile
Probar ingestión y envío con datos reales una vez que iMile dé acceso. Ajustar el adaptador a la realidad (formatos, tiempos, límites).

## B2 — Catálogos: Choferes y Unidades (Módulo 2.3)

### 2.1 Modelo y login de chofer
Modelo de chofer (perfil, historial de rutas e incidencias) + autenticación para la app móvil. Login seguro, recuperación de acceso, y que un chofer dado de baja NO pueda entrar.

### 2.2 CRUD de choferes (web)
Pantalla del supervisor para dar de alta, editar y desactivar choferes, y ver su historial. La asignación de ruta la sigue haciendo el líder a mano; el catálogo solo liga GPS/fotos/incidencias/desempeño de forma persistente.

### 2.3 Modelo de unidad
Modelo de unidad usando placas/número económico existente, más tipo, modelo, rendimiento de fábrica (km/litro), intervalos de mantenimiento y foto de la unidad.

### 2.4 CRUD de unidades (web)
Pantalla para administrar unidades y asociar la unidad al chofer del día, para poder ligar GPS, combustible e incidencias a cada unidad.

## B3 — Recepción y Segregación / Bins + Zonas (Módulo 2.2)

### 3.1 Modelo bin-ruta y zonas
Modelo de bins/gaylords, asociación bin-ruta, y modelo de zonas (automáticas por CP). Incluir la referencia geográfica de CPs (base SEPOMEX + coordenadas del manifiesto o enriquecidas una vez).

### 3.2 App: escaneo/alta de bin
Pantalla móvil para escanear o dar de alta un bin y asociarlo a una ruta al iniciar la segregación.

### 3.3 App: escaneo de paquete con verificación
Escanear cada paquete al segregar, verificando contra el manifiesto: correcto / faltante / sobrante, con feedback inmediato. Manejar los casos: paquete que no está en el manifiesto (sobrante) y paquete esperado que no llegó (faltante).

### 3.4 Lógica de zonas automáticas
Al llegar el manifiesto, agrupar los CPs de cada ruta en zonas automáticas por cercanía, separadas por ciudad (Tijuana/Rosarito/Ensenada). Nombrar cada zona por su colonia/CP principal para que el chofer la reconozca.

### 3.5 Web: saturación de rutas en tiempo real
Vista que muestra desde temprano cuántos paquetes trae cada ruta/zona y marca en semáforo las sobrecargadas (umbral configurable que define Beto), para pedir refuerzos a tiempo.

## B4 — Asignación de Ruta y Carga (Módulo 2.4)

### 4.1 App: escaneo único de gaylord
El chofer escanea UNA vez la etiqueta del gaylord y el sistema auto-asigna todos los tracking numbers de ese gaylord a él. Elimina el escaneo redundante paquete por paquete.

### 4.2 Auto-asignación + trigger a iMile
Al escanear el gaylord, asignar los trackings al chofer y disparar el estatus a iMile en tiempo real. Manejar reasignaciones y correcciones.

### 4.3 Checkpoints de tiempo (KPI)
Registrar timestamps por checkpoint (llegada, escaneo de gaylord, salida a ruta) para medir y reducir los 45 minutos promedio actuales.

### 4.4 Web: reporte de tiempos por chofer
Vista del tiempo de almacén por chofer para el supervisor.

## B5 — Reparto y Captura de Incidencias (Módulo 2.5)

### 5.1 Catálogo de incidencias configurable
Modelo de catálogo de incidencias EXTENSIBLE (persona no salió, dirección no encontrada, inaccesible, etc.), al que se pueden agregar categorías con el tiempo.

### 5.2 App: foto de entrega (POD)
Captura de foto de comprobante al entregar, con COMPRESIÓN en el celular antes de subir (clave para el costo de storage). Manejar mala señal: guardar local y subir después.

### 5.3 App: no-entrega (foto + nota + motivo)
Captura de foto + nota + motivo del catálogo cuando no se puede entregar un paquete.

### 5.4 Reprogramación al día siguiente
Job que reprograma automáticamente los paquetes no entregados para el siguiente día.

### 5.5 Web: corrección de ruta por supervisor
Pantalla para que el supervisor corrija paquetes mal segregados (ruta equivocada) ANTES del reintento, para no repetir el mismo error indefinidamente.

### 5.6 Reasignación de paquete
Endpoint y flujo para reasignar un paquete a otra ruta/gaylord cuando fue mal clasificado.

## B6 — Tracking en Tiempo Real, Verificación y Offline (Módulo 2.6)

### 6.1 App: GPS en background
Servicio de tracking GPS continuo desde el celular del chofer, en segundo plano. Manejar permisos, consumo de batería y el caso de que el chofer apague el GPS o la app.

### 6.2 Ingestión de ubicación
Endpoint que recibe y guarda las ubicaciones, con frecuencia configurable (~20-30 s en ruta).

### 6.3 Verificación geo de entrega
Capturar la geolocalización al momento de la foto de entrega y compararla contra la dirección esperada, usando el accuracy del GPS para no dar falsos positivos cuando la señal viene mala.

### 6.4 App: botón descanso/comida
Estatus formal de "en descanso / comida" que reemplaza el aviso informal por mensaje.

### 6.5 Web: mapa en tiempo real
Mapa de choferes en vivo con su estatus (en ruta / entregando / detenido / en descanso). El dolor central resuelto: saber dónde está cada quien y poder verificar entregas.

### 6.6 Modo offline (cola local + sync)
La app funciona SIN señal: escaneos, entregas y fotos se guardan localmente y se suben solos al recuperar conexión, sin perder datos. Imprescindible en reparto.

## B7 — Optimización y Predicción de Rutas (Módulo 2.7)

### 7.1 Orden de zonas por cercanía
Ordenar las zonas de cada ruta por cercanía al almacén (punto de salida) y decirle al chofer por cuál zona empezar. Es matemática propia sobre PostGIS, sin costo de mapas.

### 7.2 Orden fino dentro de zona
Dentro de cada zona, ordenar las paradas por dirección con un motor de ruteo (Google Routes en v1; VROOM/OSRM self-hosted como plan B por costo). Como se calcula por zona y no por paquete, el costo es bajo.

### 7.3 Predicción de sobrecarga
Predecir qué rutas vienen sobrecargadas desde el manifiesto matutino, con umbral configurable, para pedir más choferes a tiempo.

### 7.4 Web: vista de ruta sugerida
Mostrar la zona/orden sugerido al supervisor y al chofer.

## B8 — Combustible y Mantenimiento (Módulo 2.8)

### 8.1 App: captura tanque/odómetro + litros
Foto de odómetro al inicio y fin del día por unidad, y registro de litros cargados (del ticket o tarjeta de flotilla). Guía para que las fotos salgan claras.

### 8.2 OCR del odómetro
Leer los dígitos del odómetro con visión (Gemini). Respaldo manual (teclear el número) si una foto no se puede leer.

### 8.3 Cálculo de eficiencia
Calcular km del día (odómetro final menos inicial) y rendimiento (km/litro). Comparar cada unidad contra SU propio normal, no contra el ideal de fábrica, para detectar gastos raros.

### 8.4 Mantenimiento por km
Acumular km y avisar mantenimientos según el intervalo configurado por unidad. Detectar consumos anómalos (posible uso indebido de la unidad fuera de ruta).

### 8.5 Web: mantenimientos próximos
Vista de mantenimientos próximos y consumos por unidad para el supervisor.

## B9 — Dashboard de Resultados (Módulo 2.9)

### 9.1 KPIs de reparto
Dashboard en tiempo real: entregas vs. meta 85%, tiempo de almacén por chofer, incidencias abiertas. Reemplaza el "logrado/no logrado" actual.

### 9.2 KPIs de flotilla
Combustible, mantenimientos próximos y rutas sobrecargadas en un solo tablero.

### 9.3 Tiempo real y vistas por rol
Vista ejecutiva (gerencia, resumen) y vista operativa (supervisor, detalle diario), actualizadas en vivo.

## B10 — Ciberseguridad y Endurecimiento (transversal)

### 10.1 Autenticación y sesiones
Auth robusto en móvil (tokens con refresh y expiración) y web (sesiones). Bloquear cuentas dadas de baja. MFA para roles admin/gerencia. Poder revocar el acceso de un chofer al instante.

### 10.2 RLS y aislamiento multi-tenant auditado
Auditar que cada tenant y rol vea SOLO lo suyo: un chofer no ve datos de otro, un cliente no ve datos de otro. Pruebas explícitas de fuga cross-tenant.

### 10.3 Cifrado y manejo de secretos
TLS en tránsito y cifrado en reposo. Las llaves (iMile, Google, Gemini) en variables de entorno/vault, NUNCA en el repo. Rotación de llaves y principio de mínimo privilegio.

### 10.4 Protección de datos personales (LFPDPPP)
Las direcciones de los clientes finales son datos personales. Minimización, política de retención de fotos de entrega, borrado seguro y cumplimiento con la ley mexicana de protección de datos.

### 10.5 Seguridad de la API
Validar TODA entrada, rate limiting, protección contra inyección, idempotencia y verificación de firma en lo que venga de iMile. Nunca confiar en el cliente.

### 10.6 Seguridad de la app móvil
Almacenamiento local cifrado (la cola offline), permisos mínimos, certificate pinning y ofuscación. El celular del chofer se puede perder o robar: el acceso debe ser revocable y los datos locales protegidos.

### 10.7 Auditoría, logs y no repudio
Registrar quién hizo qué y cuándo (entregas, cambios, accesos) para resolver disputas y detectar abusos. Logs sin datos sensibles, con trazabilidad completa.

### 10.8 Anti-fraude del chofer
Reforzar la verificación (GPS + foto + geo + timestamp) y detectar patrones sospechosos: accuracy malo justo al "entregar" lejos, entregas imposiblemente rápidas, GPS apagado. El sistema debe ser difícil de engañar.

### 10.9 Respaldos y recuperación ante desastre
Backups automáticos, plan de recuperación y prueba real de restauración. No perder datos operativos ni de facturación.

## B11 — Pruebas, QA y Lanzamiento

### 11.1 Pruebas unitarias
Probar los motores puros (zonas, verificación geo, cálculos de combustible, umbrales) sin red y deterministas.

### 11.2 Pruebas de integración
Probar contra la BD real con RLS y contra el mock de iMile, verificando el aislamiento multi-tenant.

### 11.3 Pruebas E2E
Flujo completo de punta a punta: manifiesto → segregación → asignación → reparto → entrega/incidencia → estatus a iMile → dashboard, en ambas apps.

### 11.4 Piloto de campo
Prueba real con 1-2 choferes en ruta con datos reales, ANTES del rollout completo. Ajustar tolerancias de GPS y umbrales con lo observado.

### 11.5 Publicación de la app
Publicar en Google Play (Android primero): cuenta de desarrollador, ficha, permisos. iOS después si se decide.

### 11.6 Capacitación y documentación
Capacitar a choferes (app) y supervisores (web) con manual breve. Documentar el sistema (como se hace con cada pantalla en Enkoras).

### 11.7 Rollout gradual
Lanzamiento por etapas (unos choferes, luego todos), monitoreando errores en Sentry y ajustando sobre la marcha.

## B12 — Operación, Multi-tenant y Evolución

### 12.1 Monitoreo en producción
Vigilar errores, rendimiento y costos (storage de fotos, mapas, OCR) con alertas.

### 12.2 Iteración con datos reales
Calibrar tolerancias de GPS, umbrales de sobrecarga y de combustible con los datos reales de las primeras semanas.

### 12.3 Onboarding de nuevos clientes (renta)
Aprovechar el multi-tenant: dar de alta futuros clientes sobre la MISMA plataforma (el modelo construir-y-rentar de Enkoras). Costo marginal casi cero por cliente.

### 12.4 iOS y optimización de costos
Publicar iOS si se justifica. Migrar fotos a Cloudflare R2 y ruteo a self-hosted si el volumen dispara el costo mensual.

### 12.5 Evolución del producto
Nuevas features según el uso, y evaluar si nuestra app reemplaza del todo la app del teléfono de iMile (decisión que se toma con la práctica).
