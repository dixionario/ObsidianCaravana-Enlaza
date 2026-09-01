---
proyecto: La Caravana
area: sistema-tecnico
fecha: 2026-09-01
estado: implementado-local
---

# Resumen de verificaciones de identidad

La pestaña Verificaciones de `admin/gestion-creadores.php` muestra ahora el
total de personas verificadas, pendientes, en corrección y rechazadas, junto
con el porcentaje aprobado entre quienes enviaron documento.

Cada persona cuenta una sola vez según su verificación más reciente. Los
reenvíos históricos no inflan los indicadores. La consulta es de solo lectura
y no modifica creadores, postulaciones ni documentos.

Verificado con PHP 8.1 y con una consulta agregada sobre
`caravana_clon_local`.

El 2026-09-01 se regeneró `prueba/_paquetes/actualizacion-SITIO.zip` en modo
producción. El paquete acumulativo incluye este resumen, el acceso directo a
conversaciones y las métricas anteriores. La auditoría del ZIP confirmó cero
SQL, logs, configuraciones locales, notas o archivos de máquina.
