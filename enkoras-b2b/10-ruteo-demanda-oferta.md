# Ruteo demanda→oferta — bloque 2.B

El corazón de la Fase 2: al publicarse una solicitud, el sistema encuentra a
los proveedores coincidentes y les escribe su notificación — nadie busca nada.

## Motor (migración 013, `route_request_notifications`)

Función SQL **SECURITY DEFINER** (los usuarios no pueden insertar
notificaciones ajenas; la función blindada sí). Dos capas unidas por UNION:

1. **Categorías por prefijo** (ambas direcciones): solicitud en 5.1.3
   coincide con proveedores en 5.1.3, en su rama, o en un ancestro
2. **Semántica**: `embedding` de la solicitud vs vectores de TODOS los
   servicios, con el MISMO corte relativo del buscador (piso 0.55 + brecha
   0.09 contra el mejor coseno) — sin spam a proveedores irrelevantes

Reglas: nunca a la empresa solicitante; un aviso por dueño; dedupe por
`link` en re-ruteos; autorización = dueño de la solicitud, admin o sistema.
Se guarda DATO, no prosa (title = título de la solicitud, body = empresa
solicitante, link = `/solicitudes?sel=id`) — la campana de 2.D redacta en el
idioma del receptor.

## Enganche

`publicarSolicitud` llama la RPC tras insertar categorías, con **falla
suave**: si el ruteo falla, la solicitud queda publicada igual (el tablón
público sigue siendo el match manual).

## Verificación

- En vivo: las 2 solicitudes demo rutearon a sus proveedores exactos
  (transporte refrigerado → Transportes del Golfo; corrugado → Empaques de
  Monterrey), 1 notificación por dueño
- `tests/db/ruteo.test.ts` — fixtures autolimpiables con 3 usuarios/empresas
  reales: notifica al coincidente, jamás al solicitante ni al no
  relacionado, re-ruteo no duplica, y un tercero no puede rutear solicitud
  ajena (candado de la RPC). Limpia también las notificaciones que el
  fixture le genere al seed
