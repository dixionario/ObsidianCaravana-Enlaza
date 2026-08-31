# Consola de moderación de Comunidad — 2026-08-31

## Decisión

La bandeja `admin/comunidad.php` se convirtió en una consola operativa dentro de la pantalla existente, sin crear otro elemento de menú.

- El orden cronológico dejó de estar forzado por “ayuda pendiente” y “fijada”. El moderador elige: lo más actual, mayor interacción, más comentado o lo más antiguo.
- Se añadieron filtros combinables por texto, estado/categoría, ventana de actividad, presencia o ausencia de respuestas y creador.
- Se incorporó un ranking de actividad por 7, 30, 90 días o histórico. Su índice es únicamente una ayuda de moderación: publicación ×3, comentario ×2 y reacción ×1. No concede ni modifica Kr o Ke.
- Se incorporó una vista de presencia en vivo usando el sistema existente de latidos. “En línea” significa actividad en los últimos dos minutos; los visitantes públicos se cuentan de forma anónima.
- Se añadieron atajos para conversaciones sin respuesta durante más de 24 horas, conversaciones abiertas con al menos 10 interacciones y reportes pendientes.
- Se corrigió en la capa compartida del panel el salto al inicio al usar filtros, pestañas o formularios que recargan la misma pantalla. La posición se conserva por ruta; los enlaces con ancla y la navegación a otra pantalla mantienen su comportamiento propio.

## Verificación

- PHP 8.1: sintaxis válida.
- Las vistas Conversaciones, Ranking y En línea se ejecutaron contra `caravana_pruebas_local` sin avisos, errores SQL ni errores fatales.
- No hubo escrituras sobre `cv_creadores` ni `cv_applications`.
- La prueba visual por HTTP quedó impedida por el arranque local de Apache: faltan/cargan mal las extensiones PHP `curl` e `intl` en Laragon. No es un defecto de esta pantalla.

## Siguientes mejoras sugeridas

- Alertas por palabras sensibles y enlaces repetidos.
- Historial de decisiones del moderador por conversación.
- Etiquetas internas y asignación de casos a un integrante del equipo.
- Detección de reincidencia por autor y tipo de reporte.
- Objetivo de tiempo de primera respuesta y tablero semanal de cumplimiento.
