# Solicitudes de compra — bloque 2.A

La demanda también publica: una empresa registrada dice qué NECESITA con
vigencia; al vencer, la RLS pública la oculta sola (`open` + `expires_at >
now()` + empresa activa). El ruteo a proveedores llega en 2.B.

## Acciones (`app/actions/solicitudes.ts`)

- `publicarSolicitud`: valida (título 5-160, descripción ≥10, vigencia
  futura, ciudad⇒estado), **mismo pipeline del wizard**: embedding del texto
  + clasificación IA en dos pasos contra el árbol — ambos con falla suave
  (sin IA se publica igual, embedding null y sin categorías)
- `actualizarCategoriasSolicitud` (máx 6, mín 1), `cambiarEstadoSolicitud`
  (resolved/removed/open), `renovarSolicitud`, `eliminarSolicitud` — RLS
  como autoridad (owner_all)
- Nota: la clasificación reutiliza `clasificarEmpresa` mapeando
  giro=título y servicios=[la necesidad] — clasifica el bien/servicio
  requerido en las mismas ramas de proveedores (clave para el ruteo 2.B)

## UI

- **`/solicitudes`** — tablón split view (?sel=id, mismo modelo Indeed):
  `SolicitudCard` (título, empresa+verificada, ubicación o "Todo México",
  vence en verde, antigüedad) + `DetalleSolicitud` (empresa con link a su
  perfil, vence, chips de categorías, descripción, y CTA **Responder** por
  WhatsApp con mensaje precargado que cita la solicitud + llamar/correo);
  botón "Publicar solicitud" arriba de la lista
- **`/solicitudes/nueva`** — form con guardia (sesión + empresa; si no →
  login/registro): título, descripción con AvisoIA, estado/ciudad
  OPCIONALES ("Todo México"), vigencia de pastillas; al publicar redirige
  al tablón con la solicitud seleccionada
- **Mis solicitudes** (sección en /mi-empresa): lista con estado
  (vence/resuelta/vencida) y acciones — renovar 1 semana, marcar resuelta,
  eliminar en dos pasos; link a publicar
- Navbar: link "Solicitudes" en ambos idiomas

## Pruebas y demo

`tests/solicitudes/acciones.test.ts` — 7 candados mockeados: validaciones
sin gastar IA, publicación con embedding+categorías capturados, y falla
suave completa. Seed demo: 2 solicitudes cruzadas con la oferta del seed
(Agroexportadora pide transporte refrigerado 5.1.3 → lo ofrece Transportes
del Golfo; Electrónica Juárez pide corrugado 8.1.1 → lo ofrece Empaques
Monterrey) — la narrativa perfecta para probar el ruteo de 2.B.
