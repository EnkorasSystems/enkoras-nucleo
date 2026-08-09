# Pantalla: Mi empresa (panel del dueño) — bloque 1.C

Ruta: `/mi-empresa` (protegida por proxy; sin sesión → login; sin empresa → `/registro`).
El dueño administra su empresa publicada sin re-registrarse. El admin (dueño de varias) ve la más reciente.

## Estructura

- **Header** — logo (o placeholder), nombre, giro y botón fantasma "Ver perfil público" → `/empresa/[slug]`.
- **Riel lateral izquierdo** (sticky, solo desktop) — anclas a las 5 secciones; el scroll es suave (`scroll-behavior: smooth` global + `scroll-mt-24` en cada card).
- **5 secciones** como cards blancas independientes, cada una guarda por su cuenta con el patrón *Guardar cambios → Guardando... → Guardado ✓* y botón deshabilitado si no hay cambios (detección dirty contra snapshot).

| Sección | Archivo | Acción de servidor |
|---|---|---|
| Datos | `components/mi-empresa/SeccionDatos.tsx` | `actualizarDatosEmpresa` |
| Ubicación y contacto | `SeccionUbicacion.tsx` | `actualizarContactoEmpresa` |
| Servicios | `SeccionServicios.tsx` | `crearServicio` / `actualizarServicio` / `eliminarServicio` |
| Logo y fotos | `SeccionFotos.tsx` | `actualizarLogoEmpresa` / `agregarFotosEmpresa` / `eliminarFotoEmpresa` |
| Categorías | `SeccionCategorias.tsx` | `actualizarCategoriasEmpresa` (reusada del wizard) |

Acciones en `app/actions/mi-empresa.ts` — todas corren con la sesión del usuario: **RLS es la autoridad** (el dueño solo toca lo suyo; columnas protegidas — plan, verificación, rating — fuera del grant de update).

## Reglas de negocio

- **Re-embedding solo si el texto cambió**: `actualizarServicio` compara nombre+descripción contra la BD; texto idéntico = ni update ni llamada a la IA. Falla suave: si la IA no responde, el servicio se guarda con `embedding: null` (fuera de la búsqueda semántica hasta regenerarse).
- **El último servicio no se elimina** — una empresa publicada necesita catálogo (error `ultimo`, la UI lo explica).
- **Fotos**: máximo 6 en galería; al eliminar foto o reemplazar logo se borra también el archivo de Storage (mejor esfuerzo). Subida client-side al bucket `company-photos/{companyId}/…` (política de Storage: solo el dueño escribe en su carpeta).
- **Categorías**: máx 6, mín 1; guardan con `assigned_by: "owner"`. Solo nivel fino — la jerarquía superior viene gratis por prefijo de `code` en las queries.

## UI compartida

`components/mi-empresa/form.tsx` — tokens calcados del wizard (inputs FAFAFA con anillo naranja al foco, `campo(err)`, `MsgCampo`, `AvisoIA`, `Valido`) + `BotonGuardar` (3 estados) + `SeccionCard` (card con ancla e icono). El editor de servicios replica el patrón del wizard: mini-cards 4 por fila + un editor + Guardar servicio (negro) / Cancelar; eliminar con confirmación en dos pasos.

## Pruebas

`tests/mi-empresa/acciones.test.ts` — 8 candados con Supabase e IA mockeados: no-auth, validaciones sin gastar IA, re-embedding solo con texto cambiado, falla suave con embedding null, y protección del último servicio.
