---
proyecto: La Caravana
area: sistema-tecnico
fecha: 2026-09-01
estado: implementado-local
---

# Estadísticas de Comunidad y cumplimiento de misiones

Se amplió la pestaña `admin/comunidad-salud.php`, sin crear un nuevo ítem de
menú, para medir la actividad comunitaria en cortes diario, semanal y mensual.

## Métricas

- Publicaciones, comentarios y reacciones del periodo.
- Publicaciones e interacción separadas en Colaborar, Ideas, Ayuda y Preguntas.
- Los tipos históricos `hallazgo` y `encontrar` se agrupan como Ideas;
  `pregunta` se agrupa como Preguntas.
- Cantidad de comentaristas, conversaciones que recibieron comentarios,
  comentarios por participante y tiempo hasta la primera respuesta.
- Cruce por creador entre actividad comunitaria del periodo y cumplimiento
  acumulado de misiones.

## Definición de cumplimiento

El universo son los creadores que publicaron, comentaron o reaccionaron durante
el periodo elegido. Para cada uno se consultan sus misiones aceptadas y se
calcula `entregadas / aceptadas`. «Al día» significa que no conserva ninguna
misión en estado aceptada sin entregar. Las personas sin misiones aparecen como
«Sin misiones» y no inflan el porcentaje.

El cruce es una lectura operativa y visible; no asigna puntuaciones ocultas ni
modifica perfiles, publicaciones, postulaciones o entregas.

## Verificación

- PHP 8.1 sin errores de sintaxis en la pantalla y la capa compartida.
- Consultas agregadas probadas contra `caravana_clon_local` en modo de solo
  lectura.
- Sin escrituras nuevas sobre `cv_creadores` ni `cv_applications`.
- El 2026-09-01 se regeneró el paquete acumulativo de producción
  `prueba/_paquetes/actualizacion-SITIO.zip` con el empaquetador oficial en modo
  producción. Incluye los cuatro archivos de métricas de misiones y Comunidad;
  la auditoría interna confirmó cero SQL, configuraciones locales, logs, notas
  o archivos `*-maquina.php`.

## Archivos

- `prueba/sitio/admin/comunidad-salud.php`
- `prueba/sitio/includes/salud-comunidad.php`
