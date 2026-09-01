---
proyecto: La Caravana
area: sistema-tecnico
fecha: 2026-09-01
estado: implementado-local
---

# Acceso directo a conversaciones desde Mensajes

En `admin/mensajes.php`, la foto, el nombre y el adelanto de cada fila de la
bandeja ahora abren directamente el hilo con ese creador. Antes esa zona abría
la ficha emergente y obligaba a usar el botón «Abrir» para conversar.

La ficha sigue accesible desde «Abrir ficha completa» dentro del hilo. El cambio
no modifica mensajes ni datos; solo corrige el destino del enlace de la bandeja.

Verificado con PHP 8.1 sin errores de sintaxis.
