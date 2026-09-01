---
proyecto: La Caravana
area: sistema-tecnico
fecha: 2026-08-31
estado: implementado-local
---

# Panel de entregas compacto, paginado y KE

Se reorganizó `admin/misiones-portal.php?zona=entregas` para que el equipo pueda
revisar muchas entregas sin desplazamiento horizontal ni filas dominadas por
comentarios largos.

## Cambios

- Tabla de ancho fijo con seis grupos: creador, misión, entrega, estado,
  evaluación y KE, y acciones.
- Los comentarios muestran dos líneas y un control «Ver completo». Al abrirlos,
  el texto permanece dentro de un área limitada con desplazamiento vertical.
- Paginación real con 25 entregas por defecto y opciones de 50 o 100.
- Los filtros, la búsqueda y la página se conservan después de evaluar o cambiar
  una calificación.
- Cada fila muestra los KE realmente asentados en el libro mayor para
  `mision_entregada` y `mision_calificada`; no se muestra una recompensa teórica.
- En móvil cada registro se convierte en una tarjeta vertical.
- Al filtrar por misión, los indicadores se recalculan para mostrar cuántos la
  aceptaron (incluidas las personas que luego entregaron), cuántos entregaron,
  la tasa de cumplimiento y cuántas entregas contienen Instagram o TikTok.
- Instagram y TikTok se cuentan de forma no excluyente: una entrega con ambos
  enlaces aparece en ambos indicadores. También se pueden filtrar por red.
- La rapidez se mide desde `abre_en` (Disponible desde) hasta `enviado_en`. En
  misiones antiguas sin hora de apertura se usa `missions.creado` como respaldo
  explícito. Se muestran el mejor tiempo, el promedio y bandas de 0–1, 1–3,
  3–6, 6–12, 12–24 y más de 24 horas.
- Cada entrega muestra su tiempo desde el lanzamiento y los filtros de rapidez
  se conservan al paginar o evaluar.

## Verificación

- PHP 8.1 sin errores de sintaxis.
- Consulta del libro mayor probada contra `caravana_clon_local`.
- Consulta de aceptación, redes y duración probada contra
  `caravana_clon_local` sin modificar datos.
- La prueba HTTP completa quedó limitada porque el Apache local no arrancó con
  su configuración actual; no se cambió esa configuración como parte de este
  trabajo.
- Se generó el paquete acumulativo de producción
  `prueba/_paquetes/actualizacion-SITIO.zip`; contiene 188 archivos y fue
  comprobado para confirmar que incluye `admin/misiones-portal.php` y
  `admin/admin.css`, sin SQL, logs ni configuraciones de máquina.

## Archivos

- `prueba/sitio/admin/misiones-portal.php`
- `prueba/sitio/admin/admin.css`
