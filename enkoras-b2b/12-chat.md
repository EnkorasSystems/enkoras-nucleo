# Chat 1-a-1 en tiempo real — bloque 2.C

Chat simple entre empresas (roadmap: nada de grupos, archivos ni features de
Slack). El esquema vive desde Fase 0 (008): `conversations` con par canónico
`company_a < company_b`, única por par + solicitud de contexto; `messages`
con `read_at` (grant fino); `messages` en la publicación Realtime.

## Piezas

- **Migración 014**: trigger `notify_new_message` — cada mensaje notifica al
  dueño receptor (tipo `message`, suena la campana de 2.D). Anti-spam: con
  una notificación SIN LEER de esa conversación, no duplica. No
  auto-notifica si ambas empresas comparten dueño (seed)
- **`app/actions/chat.ts`**: `iniciarConversacion` (par canónico,
  encuentra-o-crea por par+solicitud), `enviarMensaje` (1-4000), 
  `marcarLeidos` (solo mensajes de la contraparte)
- **`/mensajes`** (protegida en proxy y fuera de robots): split view —
  bandeja (contraparte con logo/verificada, contexto de solicitud en
  naranja, último mensaje, no-leídos en badge) + `HiloChat` (client):
  burbujas (mías negras a la derecha, suyas grises a la izquierda),
  suscripción Realtime por conversación (canal único por montaje), envío
  optimista deduplicado por id, Enter envía / Shift+Enter salto, leídos al
  ver, siempre al fondo
- **Entrada natural**: botón negro "Responder por chat" en el detalle de la
  solicitud (abre o retoma la conversación con contexto); WhatsApp queda
  como alternativa verde. Icono Mensajes en ambos navbars (con sesión)

## Pruebas

`tests/db/chat.test.ts` — 4 candados con fixtures autolimpiables:
participante crea y escribe, el trigger notifica al receptor con el dato
correcto, anti-spam no duplica, y un tercero no ve nada (RLS). El candado
de notificaciones de Fase 0 se ajustó a filtrar por tipo (los mensajes de
su fixture ahora generan notificaciones legítimas).
