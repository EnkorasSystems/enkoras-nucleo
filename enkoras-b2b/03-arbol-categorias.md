# Árbol de Categorías B2B — Completo

> Taxonomía de 3 niveles para la plataforma. Fuentes: SCIAN 2023 (INEGI), DENUE 2024-2025,
> ThomasNet (500K proveedores / 75K categorías), Kompass (67 sectores / 55K productos), B2BMAP México.
>
> **Estructura:** 14 sectores (nivel 1) → 94 subcategorías (nivel 2) → 410 sub-subcategorías (nivel 3).
> Cada nodo llevará en la BD: nombre ES, nombre EN, slug, parent_id, level.
> El árbol crece con el uso: si llega un proveedor que no cabe, se agrega su ramita.

---

## 1. Manufactura Industrial
*SCIAN 31-33 · ~660K establecimientos DENUE · 21.4% del PIB*

### 1.1 Metalmecánica y Fabricación de Metal
- 1.1.1 Estampado, troquelado y prensado
- 1.1.2 Fundición y forja
- 1.1.3 Maquinado CNC y mecanizado de precisión
- 1.1.4 Soldadura estructural y manufactura de ensambles
- 1.1.5 Tratamiento superficial (cromado, galvanizado, anodizado, pintura industrial)
- 1.1.6 Corte y doblez de lámina

### 1.2 Manufactura Automotriz y Autopartes
- 1.2.1 Componentes OEM (motores, transmisiones, chasis)
- 1.2.2 Arneses y sistemas eléctricos automotrices
- 1.2.3 Interiores y accesorios automotrices
- 1.2.4 Proveeduría Tier 1 / Tier 2
- 1.2.5 Estampado y componentes metálicos automotrices

### 1.3 Manufactura Aeroespacial y Defensa
- 1.3.1 Estructuras y componentes de aeronaves
- 1.3.2 Sistemas de navegación y aviónica
- 1.3.3 MRO aeroespacial (mantenimiento, reparación y overhaul)
- 1.3.4 Componentes de precisión aeroespacial

### 1.4 Manufactura Médica y Dispositivos
- 1.4.1 Dispositivos médicos de un solo uso
- 1.4.2 Equipos de diagnóstico
- 1.4.3 Implantes y prótesis
- 1.4.4 Ensamble de dispositivos médicos (cuarto limpio)
- 1.4.5 Empaque estéril y validación

### 1.5 Manufactura de Plásticos y Hules
- 1.5.1 Inyección de plástico
- 1.5.2 Extrusión de perfiles y láminas
- 1.5.3 Termoformado y soplado
- 1.5.4 Hule y elastómeros (moldeado, vulcanizado)
- 1.5.5 Moldes y herramentales

### 1.6 Manufactura de Madera, Muebles y Celulosa
- 1.6.1 Tableros y madera procesada
- 1.6.2 Muebles industriales y corporativos
- 1.6.3 Tarimas y embalaje de madera
- 1.6.4 Carpintería industrial y a medida

### 1.7 Maquinaria y Equipo Original (OEM)
- 1.7.1 Equipos de producción a medida
- 1.7.2 Líneas de ensamble automatizadas
- 1.7.3 Fabricación de maquinaria especial

### 1.8 Manufactura por Contrato y Maquila
- 1.8.1 Ensamble electrónico por contrato (EMS/CM)
- 1.8.2 Maquila de producto terminado
- 1.8.3 Etiquetado, kitting y subensamble
- 1.8.4 Shelter y administración de manufactura

---

## 2. Alimentos, Bebidas y Agroindustria
*SCIAN 311-312, 11 · sector #1 en exportación MX→USA*

### 2.1 Procesamiento de Alimentos
- 2.1.1 Cárnicos y embutidos
- 2.1.2 Lácteos y derivados
- 2.1.3 Frutas y verduras procesadas (congelados, deshidratados, enlatados)
- 2.1.4 Panadería, botanas y confitería
- 2.1.5 Salsas, condimentos y aderezos
- 2.1.6 Alimentos preparados y comida lista
- 2.1.7 Tortillería y derivados del maíz industrial

### 2.2 Bebidas
- 2.2.1 Bebidas alcohólicas (cerveza, destilados, vino)
- 2.2.2 Bebidas no alcohólicas (jugos, aguas, refrescos)
- 2.2.3 Bebidas energéticas y funcionales
- 2.2.4 Café y té procesado

### 2.3 Ingredientes y Aditivos Alimentarios
- 2.3.1 Saborizantes, colorantes y conservadores
- 2.3.2 Almidones, proteínas y gomas industriales
- 2.3.3 Vitaminas y micronutrientes
- 2.3.4 Endulzantes y edulcorantes

### 2.4 Granos, Semillas y Commodities Agrícolas
- 2.4.1 Maíz, trigo, soya y oleaginosas
- 2.4.2 Azúcar y derivados de caña
- 2.4.3 Cacao y café verde
- 2.4.4 Semillas y material de siembra

### 2.5 Insumos Agrícolas y Ganaderos
- 2.5.1 Fertilizantes y nutrición de cultivos
- 2.5.2 Pesticidas, herbicidas y fungicidas
- 2.5.3 Equipos y maquinaria agrícola
- 2.5.4 Alimento balanceado para animales
- 2.5.5 Equipos de riego e invernaderos

### 2.6 Productos del Mar y Acuicultura
- 2.6.1 Pesca industrial y maricultura
- 2.6.2 Procesamiento de mariscos y pescados
- 2.6.3 Granjas acuícolas

### 2.7 Servicios de Alimentación Empresarial
- 2.7.1 Comedores industriales
- 2.7.2 Catering corporativo y banquetes
- 2.7.3 Máquinas expendedoras (vending)
- 2.7.4 Coffee break y servicios de cafetería

---

## 3. Química, Petroquímica y Materiales
*SCIAN 325-326 · ~45K establecimientos*

### 3.1 Química Industrial y Básica
- 3.1.1 Ácidos, bases y solventes
- 3.1.2 Cloro y derivados
- 3.1.3 Gases industriales (oxígeno, nitrógeno, argón, acetileno)
- 3.1.4 Reactivos y química fina

### 3.2 Petroquímica y Derivados del Petróleo
- 3.2.1 Resinas petroquímicas (polietileno, polipropileno, PVC)
- 3.2.2 Plastificantes y aditivos petroquímicos
- 3.2.3 Ceras y parafinas
- 3.2.4 Asfaltos y betunes

### 3.3 Plásticos y Polímeros (Materias Primas)
- 3.3.1 Pellets y compuestos plásticos
- 3.3.2 Masterbatch y concentrados de color
- 3.3.3 Biopolímeros y plásticos biodegradables
- 3.3.4 Resinas de ingeniería

### 3.4 Adhesivos, Selladores y Cintas
- 3.4.1 Adhesivos industriales (epóxicos, cianoacrilatos, hot melt)
- 3.4.2 Selladores estructurales y de construcción
- 3.4.3 Cintas adhesivas técnicas

### 3.5 Pinturas, Recubrimientos y Acabados
- 3.5.1 Pinturas industriales y anticorrosivas
- 3.5.2 Recubrimientos en polvo
- 3.5.3 Barnices, lacas y primers
- 3.5.4 Recubrimientos especiales (antiadherentes, cerámicos)

### 3.6 Lubricantes e Insumos Industriales
- 3.6.1 Aceites industriales y de motor
- 3.6.2 Grasas y lubricantes especializados
- 3.6.3 Fluidos de corte y mecanizado
- 3.6.4 Desengrasantes y limpiadores industriales

### 3.7 Farmacéutica y Cuidado Personal (Insumos)
- 3.7.1 Ingredientes farmacéuticos activos (API)
- 3.7.2 Excipientes y auxiliares farmacéuticos
- 3.7.3 Química cosmética y cuidado personal
- 3.7.4 Maquila farmacéutica y cosmética

---

## 4. Construcción y Bienes Raíces Industriales
*SCIAN 23 · +6.8% crecimiento 2025 · USD 5.83B inversión en parques industriales 2026*

### 4.1 Materiales de Construcción
- 4.1.1 Cemento, concreto y morteros
- 4.1.2 Acero de construcción (varilla, perfiles, lámina)
- 4.1.3 Block, tabique y materiales pétreos
- 4.1.4 Impermeabilizantes y membranas
- 4.1.5 Agregados (arena, grava, triturados)

### 4.2 Estructuras Metálicas y Prefabricados
- 4.2.1 Naves industriales prefabricadas
- 4.2.2 Estructuras de acero y aluminio
- 4.2.3 Muros y paneles prefabricados
- 4.2.4 Cubiertas y techados industriales

### 4.3 Instalaciones Eléctricas
- 4.3.1 Instalaciones eléctricas industriales y comerciales
- 4.3.2 Tableros eléctricos y distribución
- 4.3.3 Iluminación industrial y comercial
- 4.3.4 Pararrayos y tierras físicas

### 4.4 Instalaciones Hidráulicas, Sanitarias y HVAC
- 4.4.1 Tuberías (PVC, CPVC, acero, cobre)
- 4.4.2 Válvulas, conexiones y accesorios
- 4.4.3 Aire acondicionado y ventilación industrial (HVAC)
- 4.4.4 Sistemas contra incendio (instalación)
- 4.4.5 Plomería industrial y comercial

### 4.5 Acabados y Especialidades
- 4.5.1 Pisos industriales (epóxicos, concreto pulido)
- 4.5.2 Vidrio arquitectónico y cancelería
- 4.5.3 Puertas y portones industriales
- 4.5.4 Pintura y acabados de obra
- 4.5.5 Tablaroca y plafones

### 4.6 Parques Industriales y Naves
- 4.6.1 Parques industriales (clase A y B)
- 4.6.2 Naves a medida (build-to-suit) y en renta
- 4.6.3 Administración de inmuebles industriales
- 4.6.4 Terrenos industriales

### 4.7 Ingeniería y Gestión de Obra
- 4.7.1 Ingeniería civil y estructural
- 4.7.2 Gestión de proyectos (EPC, EPCM)
- 4.7.3 Inspección y supervisión de obra
- 4.7.4 Arquitectura industrial y diseño
- 4.7.5 Estudios de suelo y topografía

### 4.8 Equipos y Renta para Construcción
- 4.8.1 Renta de maquinaria pesada
- 4.8.2 Andamios y cimbra
- 4.8.3 Herramienta y equipo ligero
- 4.8.4 Grúas y maniobras

---

## 5. Logística, Transporte y Almacenamiento
*SCIAN 48-49 · ~220K establecimientos · driver crítico del nearshoring*

### 5.1 Transporte Terrestre de Carga
- 5.1.1 Carga completa (FTL) — tráiler, caja seca
- 5.1.2 Carga consolidada (LTL)
- 5.1.3 Transporte refrigerado
- 5.1.4 Materiales peligrosos (HAZMAT)
- 5.1.5 Carga sobredimensionada y maniobras especiales
- 5.1.6 Última milla y reparto local
- 5.1.7 Transporte transfronterizo (cruce MX-USA)

### 5.2 Transporte Marítimo y Portuario
- 5.2.1 Líneas navieras y agentes marítimos
- 5.2.2 Operadores portuarios y terminales
- 5.2.3 Estiba y manejo de contenedores
- 5.2.4 Fletamento y carga proyecto

### 5.3 Transporte Aéreo y Mensajería
- 5.3.1 Carga aérea (air freight)
- 5.3.2 Mensajería y paquetería express
- 5.3.3 Envíos urgentes y temperatura controlada

### 5.4 Transporte Ferroviario e Intermodal
- 5.4.1 Carga ferroviaria
- 5.4.2 Servicios intermodales (rail-truck)
- 5.4.3 Transferencia y patios intermodales

### 5.5 Almacenamiento y Distribución (3PL/4PL)
- 5.5.1 Almacenes generales y bodegas
- 5.5.2 Fulfillment y pick & pack
- 5.5.3 Cross-docking y transferencia
- 5.5.4 Logística dedicada (3PL/4PL)
- 5.5.5 Almacén fiscal y depósito fiscal

### 5.6 Comercio Exterior y Aduanas
- 5.6.1 Agencias aduanales
- 5.6.2 Corretaje aduanal USA (customs brokers)
- 5.6.3 Cumplimiento IMMEX, PROSEC y anexos
- 5.6.4 Clasificación arancelaria y consultoría de origen
- 5.6.5 Entries y pedimentos
- 5.6.6 Programas de certificación (OEA, CTPAT)

### 5.7 Cadena de Frío
- 5.7.1 Almacenes refrigerados y congelados
- 5.7.2 Transporte de cadena de frío
- 5.7.3 Logística farmacéutica de temperatura controlada

### 5.8 Servicios Logísticos Complementarios
- 5.8.1 Embalaje de exportación y aseguramiento de carga
- 5.8.2 Fumigación y tratamiento fitosanitario (NOM-144/NIMF-15)
- 5.8.3 Custodia y seguridad de carga
- 5.8.4 Seguros de carga
- 5.8.5 Renta de contenedores y equipos de transporte

---

## 6. Tecnología, IT y Automatización
*SCIAN 51, 54 · ~180K establecimientos · driver Industria 4.0*

### 6.1 Software Empresarial
- 6.1.1 ERP y sistemas de gestión
- 6.1.2 CRM y automatización de ventas
- 6.1.3 SCM y gestión de cadena de suministro
- 6.1.4 MES y software de manufactura
- 6.1.5 Nómina, contabilidad y facturación electrónica

### 6.2 Infraestructura IT y Cloud
- 6.2.1 Servidores y almacenamiento
- 6.2.2 Redes corporativas (LAN, WAN, SD-WAN)
- 6.2.3 Servicios cloud (IaaS, PaaS, SaaS)
- 6.2.4 Centros de datos y colocation
- 6.2.5 Equipo de cómputo y periféricos (venta/renta)

### 6.3 Automatización, Robótica e Industria 4.0
- 6.3.1 PLCs y sistemas SCADA
- 6.3.2 Robots industriales y cobots
- 6.3.3 Visión artificial y sensórica
- 6.3.4 IIoT y monitoreo remoto
- 6.3.5 Integración de celdas automatizadas

### 6.4 Ciberseguridad
- 6.4.1 Seguridad de redes (firewalls, SIEM)
- 6.4.2 Seguridad industrial OT/IT
- 6.4.3 Gestión de identidades y accesos
- 6.4.4 Auditorías y pentesting

### 6.5 Telecomunicaciones Empresariales
- 6.5.1 Telefonía empresarial y VoIP
- 6.5.2 Conectividad dedicada (fibra, MPLS, 5G)
- 6.5.3 Radiocomunicación industrial
- 6.5.4 Enlaces satelitales y GPS/telemetría

### 6.6 Desarrollo de Software y Consultoría IT
- 6.6.1 Desarrollo de software a medida
- 6.6.2 Desarrollo web y apps móviles
- 6.6.3 Integraciones y APIs
- 6.6.4 Consultoría de transformación digital
- 6.6.5 Inteligencia artificial y analítica de datos
- 6.6.6 Soporte técnico y mesa de ayuda

---

## 7. Maquinaria y Equipo Industrial
*SCIAN 333 · ~55K establecimientos · MRO crítico para manufactura*

### 7.1 Maquinaria de Producción
- 7.1.1 Tornos, fresadoras y centros de maquinado
- 7.1.2 Prensas hidráulicas y mecánicas
- 7.1.3 Equipos de corte (láser, plasma, waterjet)
- 7.1.4 Inyectoras y equipos para plástico

### 7.2 Equipos de Soldadura
- 7.2.1 Soldadoras (MIG, TIG, arco, punto)
- 7.2.2 Equipos de soldadura automatizada
- 7.2.3 Consumibles de soldadura

### 7.3 Compresores, Neumática e Hidráulica
- 7.3.1 Compresores de aire (tornillo, pistón)
- 7.3.2 Componentes neumáticos (cilindros, válvulas)
- 7.3.3 Sistemas hidráulicos de potencia
- 7.3.4 Mangueras y conexiones industriales

### 7.4 Bombas y Manejo de Fluidos
- 7.4.1 Bombas centrífugas e industriales
- 7.4.2 Ventiladores y extractores industriales
- 7.4.3 Mezcladores y agitadores
- 7.4.4 Sistemas de dosificación

### 7.5 Medición, Instrumentación y Calidad
- 7.5.1 Instrumentos de proceso (presión, temperatura, flujo)
- 7.5.2 Equipos de metrología y control de calidad (CMM)
- 7.5.3 Calibración de equipos e instrumentos
- 7.5.4 Equipos de laboratorio y análisis

### 7.6 Mantenimiento Industrial (MRO)
- 7.6.1 Rodamientos y transmisión de potencia
- 7.6.2 Bandas, cadenas y poleas
- 7.6.3 Sellos, empaques y juntas
- 7.6.4 Herramienta de corte y abrasivos
- 7.6.5 Refacciones industriales generales
- 7.6.6 Servicios de mantenimiento predictivo y correctivo

### 7.7 Manejo de Materiales
- 7.7.1 Montacargas (venta, renta y servicio)
- 7.7.2 Grúas puente y polipastos
- 7.7.3 Bandas transportadoras y conveyors
- 7.7.4 Racks y estantería industrial
- 7.7.5 Equipos de almacén (patines, apiladores)

---

## 8. Empaque y Envase
*SCIAN 322, 326 · ~90K establecimientos · correlacionado con alimentos y farma*

### 8.1 Empaque de Cartón y Papel
- 8.1.1 Cajas de cartón corrugado
- 8.1.2 Cartón plegadizo y estuches
- 8.1.3 Papel para empaque (kraft, encerado)
- 8.1.4 Empaque de lujo y presentación
- 8.1.5 Exhibidores y material punto de venta (POP)

### 8.2 Envases Plásticos Rígidos
- 8.2.1 Botellas y frascos (PET, HDPE, PP)
- 8.2.2 Cubetas, bidones y contenedores
- 8.2.3 Tapas, cierres y dosificadores
- 8.2.4 Charolas y clamshells termoformados

### 8.3 Empaque Flexible
- 8.3.1 Bolsas stand-up pouch y doypack
- 8.3.2 Películas y films (stretch, shrink, BOPP)
- 8.3.3 Laminados multicapa
- 8.3.4 Bolsas industriales y de vacío

### 8.4 Envases de Vidrio y Metal
- 8.4.1 Botellas y frascos de vidrio
- 8.4.2 Ampolletas y viales farmacéuticos
- 8.4.3 Latas de aluminio y hojalata
- 8.4.4 Tambores y contenedores metálicos (IBC)

### 8.5 Empaque Industrial y Logístico
- 8.5.1 Tarimas (madera, plástico, metal)
- 8.5.2 Flejes, esquineros y películas de emplaye
- 8.5.3 Espumas y protección (EPE, EPS, burbuja)
- 8.5.4 Empaque retornable y contenedores plegables

### 8.6 Etiquetas e Impresión
- 8.6.1 Etiquetas autoadheribles en rollo
- 8.6.2 Etiquetas térmicas y de código de barras
- 8.6.3 Mangas termoencogibles (sleeves)
- 8.6.4 Impresión flexográfica y offset de empaque
- 8.6.5 Ribbons y consumibles de impresión térmica
- 8.6.6 Etiquetas RFID y trazabilidad

### 8.7 Maquinaria de Empaque
- 8.7.1 Llenadoras y dosificadoras
- 8.7.2 Selladoras y termoformadoras
- 8.7.3 Etiquetadoras e impresoras industriales
- 8.7.4 Líneas de empaque automatizadas
- 8.7.5 Emplayadoras y flejadoras

---

## 9. Metales, Minerales y Materias Primas
*SCIAN 21, 331-332 · México: 3er productor mundial de plata*

### 9.1 Acero y Productos Siderúrgicos
- 9.1.1 Lámina (rolada en caliente, en frío, galvanizada)
- 9.1.2 Perfiles, vigas y estructurales
- 9.1.3 Varilla y alambrón
- 9.1.4 Acero inoxidable
- 9.1.5 Tubería de acero (con y sin costura)
- 9.1.6 Placa y aceros especiales

### 9.2 Aluminio y Metales No Ferrosos
- 9.2.1 Aluminio (lingote, perfil, lámina)
- 9.2.2 Cobre y aleaciones (bronce, latón)
- 9.2.3 Zinc, plomo y estaño
- 9.2.4 Titanio y metales especiales

### 9.3 Minerales Industriales
- 9.3.1 Sílice, caolín y arcillas
- 9.3.2 Caliza, dolomita y yeso
- 9.3.3 Mineral de hierro y concentrados
- 9.3.4 Sales y minerales químicos

### 9.4 Materias Primas Agroindustriales
- 9.4.1 Madera en rollo y aserrada
- 9.4.2 Fibras naturales (algodón, henequén)
- 9.4.3 Biomasa y biocombustibles

### 9.5 Reciclaje y Materiales Secundarios
- 9.5.1 Chatarra metálica (ferrosa y no ferrosa)
- 9.5.2 Papel y cartón reciclado
- 9.5.3 Plástico reciclado (molido, pellet)
- 9.5.4 Reciclaje electrónico (e-waste)

### 9.6 Distribución de Metales (Centros de Servicio)
- 9.6.1 Corte a medida y procesamiento de metales
- 9.6.2 Distribución de aceros al detalle
- 9.6.3 Metales para maquinado (barras, placas)

---

## 10. Electrónica y Electricidad
*SCIAN 334-335 · hubs: Tijuana, Ciudad Juárez, Monterrey, Guadalajara*

### 10.1 Componentes Electrónicos
- 10.1.1 Semiconductores y circuitos integrados
- 10.1.2 Componentes pasivos (resistencias, capacitores)
- 10.1.3 Conectores, terminales y cables de datos
- 10.1.4 Circuitos impresos (PCB) y ensamble PCBA
- 10.1.5 Displays y componentes optoelectrónicos

### 10.2 Equipos Eléctricos de Potencia
- 10.2.1 Transformadores de potencia y distribución
- 10.2.2 Tableros y centros de control de motores (CCM)
- 10.2.3 Interruptores, contactores y protecciones
- 10.2.4 Plantas de emergencia y UPS
- 10.2.5 Motores eléctricos y reductores

### 10.3 Automatización y Control Eléctrico
- 10.3.1 PLCs y controladores
- 10.3.2 HMIs y paneles de operador
- 10.3.3 Variadores de frecuencia (VFD)
- 10.3.4 Sensores industriales
- 10.3.5 Instrumentación de control de procesos

### 10.4 Cables y Conductores
- 10.4.1 Cable de potencia y construcción
- 10.4.2 Cable de control e instrumentación
- 10.4.3 Fibra óptica y cable estructurado
- 10.4.4 Cable automotriz y arneses

### 10.5 Iluminación
- 10.5.1 Iluminación industrial LED (alta bahía)
- 10.5.2 Iluminación comercial y de oficinas
- 10.5.3 Iluminación exterior y vial
- 10.5.4 Iluminación de emergencia

### 10.6 Material Eléctrico y Distribución
- 10.6.1 Distribuidores de material eléctrico
- 10.6.2 Canalización (tubería conduit, charola)
- 10.6.3 Clavijas, contactos e instalación

---

## 11. Textil, Cuero y Confección
*SCIAN 313-316 · concentración en Jalisco, Guanajuato, CDMX, Puebla*

### 11.1 Fibras, Hilos y Telas
- 11.1.1 Hilos (algodón, poliéster, nylon)
- 11.1.2 Telas de punto y planas
- 11.1.3 Telas no tejidas (nonwoven)
- 11.1.4 Textiles técnicos e industriales (geotextiles, filtros, lona)

### 11.2 Uniformes y Ropa de Trabajo
- 11.2.1 Uniformes corporativos y ejecutivos
- 11.2.2 Ropa industrial y de trabajo
- 11.2.3 Ropa de protección (retardante de flama, alta visibilidad)
- 11.2.4 Uniformes médicos y de servicio

### 11.3 Confección y Maquila Textil
- 11.3.1 Maquila de confección
- 11.3.2 Corte y costura industrial
- 11.3.3 Manufactura de calzado

### 11.4 Cuero y Piel
- 11.4.1 Cuero curtido y acabado
- 11.4.2 Cuero sintético (PU, PVC)
- 11.4.3 Artículos de piel B2B (fundas, correas, marroquinería)

### 11.5 Personalización y Promocionales Textiles
- 11.5.1 Bordado computarizado
- 11.5.2 Serigrafía y sublimación
- 11.5.3 Parches, etiquetas y emblemas textiles

---

## 12. Servicios Profesionales y Empresariales
*SCIAN 54-56 · ~42% de los establecimientos DENUE*

### 12.1 Consultoría de Negocio
- 12.1.1 Consultoría estratégica y de operaciones
- 12.1.2 Lean manufacturing y mejora continua
- 12.1.3 Consultoría de comercio exterior
- 12.1.4 Consultoría de calidad (ISO, IATF)

### 12.2 Contabilidad, Fiscal y Financiero
- 12.2.1 Despachos contables
- 12.2.2 Auditoría financiera y fiscal
- 12.2.3 Precios de transferencia
- 12.2.4 Financiamiento empresarial y arrendadoras
- 12.2.5 Seguros y fianzas empresariales

### 12.3 Legal y Cumplimiento
- 12.3.1 Derecho corporativo y mercantil
- 12.3.2 Derecho laboral
- 12.3.3 Propiedad intelectual y marcas
- 12.3.4 Cumplimiento regulatorio (NOM, FDA, COFEPRIS)
- 12.3.5 Migratorio y visas de trabajo

### 12.4 Recursos Humanos y Talento
- 12.4.1 Reclutamiento y headhunting
- 12.4.2 Servicios especializados REPSE
- 12.4.3 Maquila de nómina y administración de personal
- 12.4.4 Capacitación y desarrollo
- 12.4.5 Seguridad e higiene laboral (consultoría NOM-035, STPS)

### 12.5 Marketing, Diseño y Comunicación
- 12.5.1 Agencias de marketing digital
- 12.5.2 Diseño gráfico e identidad corporativa
- 12.5.3 Producción audiovisual y fotografía
- 12.5.4 Impresión comercial y gran formato
- 12.5.5 Artículos promocionales
- 12.5.6 Stands, ferias y eventos empresariales

### 12.6 Certificación, Pruebas y Laboratorios
- 12.6.1 Organismos de certificación (ISO 9001, IATF 16949, AS9100)
- 12.6.2 Laboratorios de pruebas y ensayos
- 12.6.3 Certificación NOM y unidades de verificación
- 12.6.4 Inspección de calidad y auditoría de proveedores

### 12.7 Servicios Generales para Empresas
- 12.7.1 Limpieza industrial y corporativa
- 12.7.2 Seguridad privada y vigilancia
- 12.7.3 Jardinería y mantenimiento de exteriores
- 12.7.4 Control de plagas
- 12.7.5 Mensajería local y gestoría
- 12.7.6 Renta de oficinas y coworking
- 12.7.7 Mobiliario y equipo de oficina
- 12.7.8 Papelería y consumibles corporativos

### 12.8 Traducción e Idiomas
- 12.8.1 Traducción técnica y legal
- 12.8.2 Interpretación empresarial
- 12.8.3 Capacitación en idiomas para empresas

---

## 13. Energía y Medio Ambiente
*SCIAN 22, 562 · el nearshoring exige confiabilidad energética*

### 13.1 Energía Eléctrica Industrial
- 13.1.1 Generación distribuida y cogeneración
- 13.1.2 Subestaciones y media tensión
- 13.1.3 Plantas de emergencia (venta, renta, mantenimiento)
- 13.1.4 Calidad de energía y corrección de factor de potencia

### 13.2 Energías Renovables
- 13.2.1 Sistemas solares fotovoltaicos
- 13.2.2 Energía eólica
- 13.2.3 Almacenamiento de energía (baterías, BESS)
- 13.2.4 Biogás y biomasa

### 13.3 Gas y Combustibles
- 13.3.1 Gas natural (distribución y estaciones)
- 13.3.2 Gas LP industrial
- 13.3.3 Diésel y combustibles a flotillas

### 13.4 Eficiencia Energética
- 13.4.1 Auditorías energéticas
- 13.4.2 Sistemas de gestión de energía (ISO 50001)
- 13.4.3 Retrofit de iluminación y motores

### 13.5 Agua Industrial
- 13.5.1 Plantas de tratamiento de agua y PTAR
- 13.5.2 Ósmosis inversa y purificación
- 13.5.3 Suavizadores y acondicionamiento
- 13.5.4 Pipas y suministro de agua

### 13.6 Gestión Ambiental y Residuos
- 13.6.1 Recolección y disposición de residuos industriales
- 13.6.2 Manejo de residuos peligrosos (CRETIB)
- 13.6.3 Consultoría y auditoría ambiental (SEMARNAT, impacto ambiental)
- 13.6.4 Remediación de suelos
- 13.6.5 Reciclaje y economía circular

---

## 14. Seguridad Industrial, Salud y EPP
*SCIAN 562, 325 · obligatorio en manufactura y construcción*

### 14.1 Equipo de Protección Personal (EPP)
- 14.1.1 Protección respiratoria
- 14.1.2 Protección visual y facial
- 14.1.3 Protección auditiva
- 14.1.4 Guantes industriales
- 14.1.5 Calzado de seguridad
- 14.1.6 Protección contra caídas (arneses, líneas de vida)

### 14.2 Señalización y Seguridad en Planta
- 14.2.1 Señalización industrial (NOM-026, NOM-003)
- 14.2.2 Barreras, guardas y control de acceso
- 14.2.3 Candadeo y etiquetado (LOTO)
- 14.2.4 Detección de gases

### 14.3 Protección contra Incendios
- 14.3.1 Extintores (venta y recarga)
- 14.3.2 Sistemas fijos (rociadores, hidrantes)
- 14.3.3 Supresión especial (CO2, agentes limpios)
- 14.3.4 Equipo para brigadas
- 14.3.5 Alarmas y detección de humo

### 14.4 Salud Ocupacional
- 14.4.1 Servicios médicos empresariales y exámenes ocupacionales
- 14.4.2 Botiquines, primeros auxilios y DEA
- 14.4.3 Ergonomía industrial
- 14.4.4 Ambulancias y atención de emergencias empresariales

### 14.5 Seguridad Electrónica
- 14.5.1 CCTV y videovigilancia
- 14.5.2 Control de accesos y biometría
- 14.5.3 Alarmas de intrusión
- 14.5.4 Monitoreo remoto

---

## Resumen numérico

| Nivel | Cantidad |
|-------|----------|
| Sectores (nivel 1) | **14** |
| Subcategorías (nivel 2) | **94** |
| Sub-subcategorías (nivel 3) | **410** |

## Datos de contexto por sector

| # | Sector | SCIAN | Est. DENUE | Nota |
|---|--------|-------|-----------|------|
| 1 | Manufactura Industrial | 31-33 | ~660K | 21.4% del PIB |
| 2 | Alimentos, Bebidas y Agro | 311-312, 11 | ~800K+ | #1 exportación MX→USA |
| 3 | Química y Petroquímica | 325-326 | ~45K | Crecimiento sostenido |
| 4 | Construcción | 23 | ~300K | +6.8% en 2025 |
| 5 | Logística y Transporte | 48-49 | ~220K | Driver del nearshoring |
| 6 | Tecnología e IT | 51, 54 | ~180K | Industria 4.0 |
| 7 | Maquinaria y Equipo | 333 | ~55K | MRO crítico |
| 8 | Empaque y Envase | 322, 326 | ~90K | Correlación food+pharma |
| 9 | Metales y Materias Primas | 21, 331-332 | ~30K | 3er productor Ag mundial |
| 10 | Electrónica y Electricidad | 334-335 | ~40K | Hub Tijuana/Juárez/MTY |
| 11 | Textil y Confección | 313-316 | ~120K | Jalisco, Gto, CDMX |
| 12 | Servicios Profesionales | 54-56 | ~500K+ | 42% del DENUE |
| 13 | Energía y Medio Ambiente | 22, 562 | ~20K | Demanda por nearshoring |
| 14 | Seguridad Industrial y EPP | 562, 325 | ~35K | Obligatorio en industria |

**Notas de diseño:**
- Sectores con más proveedores en MX: Servicios Profesionales > Alimentos > Manufactura > Construcción
- Sectores con más volumen B2B: Manufactura > Metales > Química > Logística
- Sectores críticos para el corredor MX-USA: Automotriz, Aeroespacial, Electrónica, Logística/Aduanas, Empaque
- Sectores con menor oferta digital hoy (mayor oportunidad): Seguridad Industrial, MRO, Materias Primas, Energía
