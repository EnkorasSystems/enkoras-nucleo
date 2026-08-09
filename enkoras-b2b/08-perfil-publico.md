# Perfil público de empresa (rediseño) — bloque 1.G

Ruta `/empresa/[slug]` (ISR `revalidate 120` + disponibilidad en vivo por
Realtime). Rediseño completo con el design system — feedback directo de Javi:
el perfil heredado de la réplica "estaba muy mal".

## Estructura

- **Header** (card blanca protagonista): logo 20×20 (o inicial sobre negro),
  nombre + badge VERIFICADA (naranja, uppercase), giro, y la fila de datos
  duros: ubicación · rating (estrella) · tamaño ("51-200 empleados") ·
  "Miembro desde {año}". Chips de categorías clicables al pie.
- **Columna principal** (2/3): Disponibilidad EN VIVO primero (el
  diferenciador arriba), Acerca de (card con `whitespace-pre-line`),
  Catálogo de servicios (cards numeradas 01/02… estilo wizard, hover
  naranja), Galería (grid aspect-video con hover scale sutil).
- **Sidebar sticky** (1/3): Contacto ordenado POR CONVERSIÓN — WhatsApp
  primero (verde sólido, con mensaje precargado bilingüe vía `waPrefill`),
  Llamar (negro), Correo y Sitio web (fantasma). Card de Información:
  dirección completa y redes (LinkedIn/Facebook) como links silenciosos.
- Encabezados de sección uniformes: componente `Seccion` (icono en cuadrito
  naranja + título) — la disponibilidad usa el mismo patrón en verde.
- Cero estilos inline de color (tokens del design system en clases).
- Skeleton de `loading.tsx` alineado al layout nuevo.

## Criterio de salida de Fase 1 (verificado en vivo, 2-ago-2026)

Buscar ("uniformes para mi planta") → resultado con contexto ("Coincide:",
"Disponible ahora") → perfil premium → botón de WhatsApp con mensaje
precargado. El ciclo comprador-encuentra-proveedor completo, en ES y EN.
