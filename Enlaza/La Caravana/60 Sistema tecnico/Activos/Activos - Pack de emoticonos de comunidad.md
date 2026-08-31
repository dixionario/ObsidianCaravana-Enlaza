---
tipo: activo-visual
proyecto: La Caravana
estado: empaquetado-para-cpanel
actualizado: 2026-08-27
---

# Pack de emoticonos de comunidad

El 27 de agosto de 2026 se preparó el primer pack instalable de emoticonos
propios para la comunidad. Reúne dos familias visuales aportadas como
referencia por Alejandro:

- **La Caravana:** 30 iconos basados en el símbolo de la marca.
- **Diccionario Venezolano:** 41 iconos del personaje y símbolos editoriales.

Cada pieza usa un slug corto, lienzo transparente de 256 × 256 px, conserva
su color original y lleva un borde blanco sutil para responder bien sobre
fondos claros y oscuros. El entregable incluye PNG, WebP lossless,
`manifest.json`, instrucciones y una previsualización HTML comparativa.

El 27 de agosto quedó integrado localmente en el Portal como catálogo de 71
WebP. El selector permanece cerrado hasta tocar “Emoticonos” y entonces abre
un carrusel horizontal, pensado para el pulgar y sin desplegar la colección
completa en la pantalla. Está disponible al publicar, responder en el foro,
escribir a un colaborador y escribir al equipo.

Los textos guardan tokens estables, por ejemplo `:lc-feliz:`, y solo al
mostrarse se transforman en imágenes de 32 px. Esto evita guardar HTML de
usuarios y permite retirar un icono del selector sin romper conversaciones
históricas. En **Administración → Comunidad y moderación → Emoticonos de la
comunidad**, administradores y superadministradores pueden activar o
desactivar las 71 piezas y añadir PNG o WebP propios. Los moderadores pueden
usar y moderar la comunidad, pero no alterar el catálogo.

La validación responsive se hizo a 375 px y escritorio: el documento no
produce desplazamiento horizontal, las 71 imágenes cargan sin roturas y la
bandeja mide 294 px dentro del móvil. La colección existe una sola vez en una
plantilla reutilizable y se materializa únicamente al abrir un selector; así
una conversación con muchas respuestas no repite cientos de botones ocultos.
La barra visual del navegador se oculta, pero se conserva el desplazamiento
horizontal táctil y por rueda.

El mismo 27 de agosto se sustituyó la primera extracción: los recortes por
cuadrícula podían incluir fragmentos de piezas vecinas o cortar stickers
anchos. La versión 1.0.1 usa aislamiento por componentes y fue revisada en dos
láminas de control que reúnen visualmente las 71 piezas sobre fondos claros y
oscuros.

Ubicación local: `ENTREGABLES-CPANEL/emoticonos-la-caravana-foro.zip`.

Código integrado: `prueba/portal/assets/emoticonos/`,
`prueba/sitio/includes/emoticonos-comunidad.php` y la pestaña plegable de
`prueba/sitio/admin/comunidad.php`.

El 27 de agosto de 2026 se generó la actualización diferencial para cPanel.
Son dos paquetes porque el catálogo y su interfaz viven en el subdominio del
Portal, mientras la configuración administrativa compartida vive en el sitio
principal. La auditoría confirmó 71 WebP, ningún archivo local/configuración,
ningún SQL y ninguna escritura nueva sobre creadores o postulaciones. El
despliegue en producción continúa pendiente de subida y prueba humana.
