# 13 — Suscripción Premium (Stripe, $499 MXN/mes)

Fase 3 · construido el 3-ago-2026. Primer bloque de monetización real: el
dueño de una empresa mejora a Premium con Stripe Checkout y administra su
suscripción desde el portal de facturación de Stripe.

## Modelo

- **Free**: todo lo esencial (perfil, catálogo, búsqueda, disponibilidad,
  solicitudes, chat, verificación). La liquidez del marketplace es sagrada.
- **Premium ($499 MXN/mes, solo mensual por decisión de Javi)**: ventaja
  competitiva — bono ACOTADO de ranking (+0.08, ya existía en 3.A), franja
  ⚡ DESTACADO en cards, aparición preferente. El comprador no paga nunca.
- Cuenta Stripe propia "Enkoras" (`acct_1U0ZEIEjn1KFAmsN`, MXN, descriptor
  `ENKORAS.COM`, payout semanal). Producto `prod_V0bAbNNfF0kmLW`, precio
  `price_1U0ZyoEjn1KFAmsNDzMeCvSj`.
- El checkout acepta códigos promocionales (`allow_promotion_codes`) — listo
  para el cupón FUNDADOR del lanzamiento sin tocar código.

## Flujo

1. `/planes` muestra la comparativa con precio real; el CTA depende de quién
   mira: visitante → login, sin empresa → registro, free → Checkout,
   premium → su panel (`CtaPremium`).
2. `iniciarCheckout` (app/actions/stripe.ts): crea/reutiliza el customer de
   Stripe (persistido vía service role — la columna es protegida), abre la
   sesión de Checkout en el idioma del usuario y regresa la URL.
3. Stripe cobra y dispara el **webhook** `/api/stripe/webhook` (runtime
   nodejs, firma verificada SIEMPRE):
   - `checkout.session.completed` → `plan='premium'` + guarda customer y
     subscription id (por `metadata.company_id`).
   - `customer.subscription.updated/deleted` → plan según la regla pura de
     `lib/suscripcion.ts` (busca por subscription id, respaldo por metadata).
4. El regreso (`/mi-empresa?premium=exito#plan`) muestra banner de éxito
   mientras el webhook aterriza; la SeccionPlan lee `empresa.plan`.
5. Premium activo → botón "Administrar suscripción" abre el portal de Stripe
   (cancelar al fin del periodo, cambiar tarjeta, historial de recibos —
   configuración `bpc_1U0a7hEjn1KFAmsNdYHUwtx2`, default del modo live).

## Regla suscripción → plan (lib/suscripcion.ts, pura)

- `active`, `trialing`, `past_due` → **premium** (past_due da gracia mientras
  Stripe reintenta la tarjeta varios días).
- Todo lo demás (`canceled`, `unpaid`, `incomplete*`, `paused`, estados
  futuros desconocidos) → **free**. Falla cerrada: nunca regala premium.
- Suscripción eliminada → free SIEMPRE, gane quien gane.

## Candados

- `plan`, `stripe_customer_id` y `stripe_subscription_id` son columnas
  protegidas: el grant de update de authenticated (006) no las incluye; solo
  el webhook y las acciones (service role) las escriben. Migración 017 agrega
  las columnas + índices de búsqueda del webhook.
- Webhook sin firma o con firma inválida → 400, sin tocar BD.
- `iniciarCheckout` rechaza sin sesión y si ya es premium.
- tests/planes/suscripcion.test.ts: 9 candados de la regla pura (150 total).

## Operación

- Env (local y Vercel Production): `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`,
  `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `STRIPE_PREMIUM_PRICE_ID`.
- Endpoint del webhook en Stripe: `we_1U0a7dEjn1KFAmsNPpb7DVX6` →
  https://enkoras.com/api/stripe/webhook (3 eventos suscritos).
- Cambiar el precio = crear precio nuevo en Stripe y actualizar la env var
  (los suscritos existentes conservan el suyo).
- Premium de cortesía (aliados del lanzamiento): `update companies set
  plan='premium'` a mano — el webhook no lo pisa porque esas empresas no
  tienen suscripción.
