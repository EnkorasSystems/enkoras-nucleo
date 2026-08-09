# Disponibilidad en tiempo real — bloque 1.D

El diferenciador del producto: el dueño publica qué tiene disponible **ahora**
(espacio, capacidad, unidades, citas) con una **vigencia**; al vencer desaparece
sola del directorio. Ningún competidor B2B en México ofrece esto.

## Modelo (tabla `availability`, migración 008; Realtime en 011)

- `kind`: `units` | `volume` | `capacity` | `slots` (iconos en `components/disponibilidad/util.ts`)
- `title` (3-160), `quantity` (> 0 opcional), `unit`, `notes`, `expires_at`
- **La vigencia es RLS, no cron**: la política pública es `expires_at > now()` —
  lo vencido deja de existir para el público sin ningún proceso de limpieza.
  El dueño sí ve sus vencidas (para renovarlas con un clic).
- Migración 011 agrega la tabla a la publicación `supabase_realtime`.

## Panel del dueño (`SeccionDisponibilidad` en /mi-empresa)

- Lista ordenada: vigentes primero (las que vencen antes arriba), vencidas al final en gris.
- Editor: tipo (Select), título, cantidad+unidad, notas y **vigencia de pastillas
  rápidas** (24h / 48h / 3 días / 1 semana / 2 semanas).
- Al editar una vigente aparece la pastilla "Mantener la actual" (payload
  `expiresAt: null` = no tocar `expires_at`); al abrir una vencida se preselecciona
  1 semana — reabrir + guardar = renovar.
- Eliminar con confirmación en dos pasos, igual que servicios.

## Perfil público (`DisponibilidadEnVivo`)

- Client component con datos iniciales del servidor (la página es estática,
  `revalidate 120`) + suscripción Realtime `postgres_changes` filtrada por
  `company_id`. **El refresh inicial ocurre al confirmarse la suscripción**
  (`SUBSCRIBED`) — datos frescos sin ventana ciega entre render y suscripción.
- Cualquier evento (insert/update/delete) → refetch anónimo (RLS filtra a
  vigentes de empresa activa). Un tic por minuto deja caer lo que vence en vivo.
- Header con badge "En vivo" (punto verde pulsante). Cards: icono por tipo,
  título, chip de cantidad, notas y "Vence {relativo}".
- El reloj entra por prop (`ahoraInicial`) desde el server component —
  `tiempoRelativo`/`esVigente` son funciones puras (regla del React Compiler).

## Acciones (`app/actions/disponibilidad.ts`)

`crearDisponibilidad` / `actualizarDisponibilidad` / `eliminarDisponibilidad` —
sesión del usuario + RLS como autoridad. Validación de servidor: tipo del
catálogo, título 3-160, cantidad > 0 o null, vigencia futura (obligatoria al
crear, opcional al editar = mantener). Sin IA: esto es puro dato fresco.

## Pruebas

`tests/mi-empresa/disponibilidad.test.ts` — 10 candados mockeados: no-auth,
validaciones (tipo/título/cantidad/vigencia pasada/creación sin vigencia),
inserción correcta, edición que respeta "mantener vigencia", y eliminación.
