# Campana de notificaciones — bloque 2.D

La cara visible del ruteo demanda→oferta (docs/10): las notificaciones que
el sistema escribe en BD llegan a la campana del navbar **en vivo**.

## `components/notificaciones/Campana.tsx` (client)

- Visible solo con sesión, en el Navbar público (desktop + móvil) y en el
  DashboardNav del panel
- Badge naranja con no-leídas (9+ como tope visual; count exacto por query)
- **Realtime**: suscripción a INSERT en `notifications` — RLS filtra a las
  propias; la carga inicial ocurre al confirmarse SUBSCRIBED (sin ventana
  ciega). Una notificación de ruteo aparece sin recargar la página
- Popover con las últimas 10, redactadas EN EL IDIOMA DEL RECEPTOR: la BD
  guarda dato (type + título de solicitud + empresa + link) y la campana
  compone la prosa (`Notifs.requestMatch` / `message` / `system`)
- Clic: marca leída (update optimista + `read_at`, único grant de update) y
  navega al link (`/solicitudes?sel=...` — cae en el split view con la
  solicitud abierta); "Marcar todas" disponible con no-leídas
- El reloj del tiempo relativo se fija al ABRIR el popover (handler, no
  render — regla de pureza)

## Cobertura

Sin suite nueva: la campana es UI sobre el ruteo ya blindado en
tests/db/ruteo.test.ts (quién recibe, dedupe, candados) y la paridad i18n
cubre sus claves. La verificación E2E del ciclo completo es el criterio del
bloque 2.E.
