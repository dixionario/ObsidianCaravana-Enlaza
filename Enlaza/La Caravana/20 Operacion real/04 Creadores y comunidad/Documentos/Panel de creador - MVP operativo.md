---
tipo: plan
estado: PROPUESTO
actualizado: 2026-08-22
fuente: "La Caravana/PLAN-PANEL-CREADOR-MVP.md (2026-08-19)"
---

# Panel de creador — MVP operativo

## Objetivo

Convertir el enlace personal del creador en un centro de trabajo durante toda la edición: saber qué hacer, entregar el contenido, recibir retroalimentación y mantenerse conectado con el equipo.

El panel no debe convertirse todavía en una red social completa. La interacción debe servir a la producción de la edición.

## Principios de producto

- Un solo enlace personal; no crear usuario ni contraseña para el MVP.
- La pantalla principal siempre responde: qué está pasando, qué debo hacer y cuándo vence.
- Separar claramente contenido obligatorio (retos) de contenido voluntario (iniciativas).
- La entrega debe poder editarse antes del cierre y mostrar su estado después de la revisión.
- Toda acción importante debe quedar vinculada a creador, edición, reto y fecha.
- El diseño usa el lenguaje visual actual: negro, azul eléctrico, amarillo y tipografía grande.
- Mobile-first: el creador trabajará principalmente desde el teléfono.
- Cada vista importante debe tener una imagen editorial real: no usar paneles vacíos ni tarjetas monocromas como superficie principal.

## Dirección visual épica

La interfaz debe combinar operación clara con una capa visual cinematográfica:

- Inicio: imagen de la ruta o del destino en el encabezado, con degradado oscuro para conservar legibilidad.
- Reto: imagen o fotograma de referencia del territorio junto a la consigna.
- Comunidad: miniaturas de hallazgos, retratos y publicaciones de los creadores.
- Perfil: retrato grande del creador y una tira de sus mejores piezas.
- Logística: mapa de ruta simplificado y una fotografía del próximo destino.

Reglas:

- Una imagen dominante por vista, no una galería saturada.
- El texto siempre queda sobre una capa de contraste.
- No usar imágenes genéricas de stock.
- Mantener formato responsive: `16:9` en escritorio y recorte `4:5` o `1:1` en móvil.
- Añadir texto alternativo y carga diferida.
- Comprimir WebP/AVIF y conservar una versión de respaldo.

## Vistas que sí entran en el MVP

### 1. Inicio del creador

Debe mostrar:

- Nombre y edición.
- Día actual de la expedición.
- Kilómetros aprobados.
- Reto prioritario.
- Cuenta regresiva para el cierre.
- Estado de los últimos retos.
- Acceso al perfil y a la logística esencial.

CTA principal: `Ver reto del día`.

### 2. Detalle y entrega del reto

Debe mostrar:

- Día y kilómetros.
- Título y consigna.
- Qué busca el equipo creativo.
- Reglas y formatos.
- Plataformas aceptadas.
- Hora de cierre.
- Enlace de Instagram.
- Enlace de TikTok.
- Comentario opcional.
- Estado de la entrega.
- Retroalimentación del equipo.

Estados: `Pendiente`, `Borrador`, `Entregado`, `En revisión`, `Aprobado`, `Requiere ajuste`, `Cerrado`.

### 3. Comunidad de edición

Interacción mínima y útil:

- Avisos del equipo creativo.
- Hallazgos de otros creadores.
- Solicitudes de apoyo.
- Comentarios y reacciones simples.
- Destacados del día.
- Próximo destino y aviso logístico.

No incluir chat privado ni feed algorítmico en esta fase.

## Herramientas de soporte

### Perfil del creador

Mantener el formulario actual, pero ordenar en pasos:

1. Datos personales.
2. Bio y especialidad.
3. Redes sociales.
4. Documentos y contrato.
5. Vista previa pública.

Mostrar una barra de completitud y los pendientes obligatorios.

### Retroalimentación del equipo

Cada entrega puede recibir una nota interna visible para el creador:

- `Aprobada`.
- `Ajuste solicitado`.
- `No aprobada`.

La nota debe incluir responsable y fecha. No usar mensajes ambiguos.

### Logística esencial

El creador solo necesita consultar:

- Punto de encuentro.
- Próxima salida.
- Transporte asignado.
- Hospedaje.
- Contacto operativo.
- Avisos urgentes.

La edición de esta información permanece en el panel administrativo.

## Cambios de datos mínimos

Reutilizar `creadores`, `ediciones`, `retos`, `entregas` e `iniciativas`.

Añadir solo si no existe un equivalente:

- `creador_avisos`: aviso, edición, creador o audiencia, prioridad, leído, fecha.
- `entrega_comentarios`: entrega, autor, comentario, visible_para_creador, fecha.
- `comunidad_publicaciones`: creador, edición, texto, enlace opcional, estado, fecha.
- `comunidad_reacciones`: publicación, creador, tipo, fecha.

No duplicar estados ni crear un segundo sistema de perfiles.

## Funciones administrativas necesarias

En `Retos`:

- Publicar, cerrar y reabrir reto.
- Revisar entregas.
- Solicitar ajustes.
- Aprobar con kilómetros.
- Enviar aviso a todos o a un creador.

En `Edición en vivo`:

- Publicar aviso operativo.
- Ver pendientes del día.
- Ver entregas que requieren revisión.
- Ver incidencias y próximo destino.

En `Creadores`:

- Ver completitud de perfil.
- Ver último acceso.
- Ver entregas pendientes.
- Abrir portal personal.

## Orden recomendado de construcción

### Sprint 1 — Claridad y mobile

- Rediseñar `portal.php` y `assets/css/portal-creador.css`.
- Crear jerarquía Inicio → Reto → Historial.
- Mejorar estados, cuenta regresiva y formularios.
- Probar enlaces personales caducados y enlaces válidos.

### Sprint 2 — Operación editorial

- Añadir estado `Requiere ajuste`.
- Añadir comentario visible del equipo.
- Añadir avisos de edición.
- Integrar pendientes en `admin/edicion-viva.php`.

### Sprint 3 — Comunidad controlada

- Publicaciones breves de hallazgos.
- Reacciones simples.
- Moderación administrativa.
- Aviso logístico destacado.

## Criterios de aceptación

- Un creador abre su enlace en móvil y entiende su siguiente acción en menos de 10 segundos.
- Puede enviar o actualizar un reto sin abandonar el portal.
- Sabe si su entrega está pendiente, en revisión, aprobada o requiere ajuste.
- El equipo puede revisar, comentar, aprobar y notificar desde el panel.
- Un aviso urgente aparece en el portal y queda registrado.
- Ninguna vista nueva rompe el acceso actual por token.
- Todos los módulos nuevos tienen espejo editable en el panel.

## Prompt de trabajo para Claude

> Trabaja sobre el proyecto local La Caravana. Evoluciona el portal del creador como un MVP operativo mobile-first. Conserva el acceso por enlace personal y reutiliza las tablas y funciones existentes antes de crear nuevas. Implementa primero Inicio del creador, Detalle/Entrega del reto y Comunidad controlada. Usa el lenguaje visual existente: negro, azul eléctrico, amarillo y tipografía grande. No construyas una red social completa, chat privado, pagos, cuentas de usuario ni feed algorítmico. Cada función visible debe tener gestión administrativa, permisos, estados claros y registro de actividad. Antes de modificar, inspecciona `portal.php`, `assets/css/portal-creador.css`, `admin/retos.php`, `admin/edicion-viva.php`, `admin/creadores.php`, `includes/funciones.php` y el esquema de base de datos. Entrega cambios por sprint, con migraciones reversibles, pruebas de móvil y criterios de aceptación verificables.
