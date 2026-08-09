# Verificación RFC — bloque 1.F

El sello de confianza del directorio: RFC + constancia de situación fiscal →
revisión del admin → badge **Empresa verificada** en perfil y cards, y
prioridad en el ranking de búsqueda (bono +0.10 cableado desde 1.E).

## Flujo

1. **Dueño** (`SeccionVerificacion` en /mi-empresa): captura RFC (validación
   en vivo), sube su constancia al bucket **privado** `company-docs` (se
   guarda la RUTA, no URL pública) y solicita. Estados: sin solicitar /
   rechazada (reintenta) → en revisión (ámbar) → verificada (verde).
2. **Servidor** (`app/actions/verificacion.ts`): `solicitarVerificacion`
   revalida el RFC, guarda RFC+ruta (columnas del grant del dueño) y llama la
   RPC `submit_company_verification` — que vive en la BD y exige dueño + RFC
   + constancia (migración 006, SECURITY DEFINER).
3. **Admin** (`NegociosTable`): columna Verificado consciente del estado —
   pendiente muestra **Ver constancia** (URL firmada de 10 min vía
   `urlConstancia`; RLS decide el acceso: solo admin y dueño) + Aprobar /
   Rechazar (`admin_set_company_verification`). Rechazada se marca; el dueño
   puede volver a solicitar.

## Validador (`lib/rfc.ts` — puro)

Formato del SAT: moral 3 letras + AAMMDD + homoclave (12), física 4 letras
(13); acepta Ñ y &; valida la fecha interna (mes/día reales, feb 29 ok).
**Deliberadamente laxo en homoclave**: rechazar homoclaves raras bloquearía
empresas reales — el falso negativo es el peor error en verificación. La
autenticidad real la da la constancia revisada por el admin, no el regex.

## Pruebas

`tests/verificacion/rfc.test.ts` — 12 candados puros: morales/físicas
válidas, Ñ/&, normalización, longitudes, estructura cruzada (12 con 4
letras), caracteres inválidos y fechas imposibles (mes 13, día 32, 31-feb).

## Nota operativa (2-ago-2026)

La cuota diaria gratuita del modelo de chat se agotó con el seed demo; la
clasificación IA del wizard degrada a "elige categorías a mano" hasta el
reset (medianoche Pacífico). Los embeddings tienen cuota aparte y siguen
funcionando. Es exactamente el escenario para el que existe la falla suave.
