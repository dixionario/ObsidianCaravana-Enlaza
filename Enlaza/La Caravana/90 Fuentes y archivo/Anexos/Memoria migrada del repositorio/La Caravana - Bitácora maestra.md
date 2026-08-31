---
tipo: bitacora-maestra
proyecto: La Caravana
estado: activo
actualizado: 2026-08-27
entorno: PHP 8.1 + MariaDB + cPanel
---

# La Caravana — Bitácora maestra

## Hitos y filosofía

- [[Hitos - Capitulo 01 del lanzamiento a los Elegidos|Capítulo 01 — Del lanzamiento a los 10 Elegidos]]:
  memoria del período del 1 al 16 de agosto de 2026, con el embudo público de
  selección, los momentos de atención, el impacto acumulado en Instagram y los
  próximos capítulos declarados por el proyecto.
- [[Hito - Nacimiento espontaneo de la comunidad|Nacimiento espontáneo de la comunidad — 2026-08-26]]:
  antes de su anuncio formal, los creadores descubrieron el foro y comenzaron a
  compartir ideas, buscar colaboraciones regionales y proponer iniciativas por
  cuenta propia. El hito confirma que el propósito del Portal trasciende KE,
  rangos e insignias: busca potenciar capacidades, vínculos e impacto real.
- Esta filosofía orienta el producto, pero no autoriza nuevas funciones ni
  altera las prioridades vigentes del MVP.
- [[Plan - Roles moderacion y evolucion de la comunidad|Plan de roles, moderación y evolución de la comunidad — 2026-08-26]]:
  primera fase implementada en local con intenciones `Quiero`, tarjetas
  compactas, dos reacciones reversibles, videos permitidos y ciclo temporal
  completo de misiones; chat y permisos granulares continúan como fases
  posteriores. La revisión posterior definió la transición Pasaporte → Logros,
  notificaciones unificadas, identidad con nombre de usuario inmutable,
  distinciones compartibles, colaboraciones y misiones en pareja.
  La fase 1 posterior corrigió el selector duplicado y los retornos del foro,
  añadió límites antiabuso administrables y compactó la actividad reciente en
  una rotación accesible de cuatro segundos.
  El selector se refinó después con iconos, contadores y filtrado explícito;
  `Encontrar` pasó a `Dar ideas` con compatibilidad histórica. También se creó
  el rol Moderador de comunidad y la reclasificación auditada de publicaciones.
  La fase 2 añadió un centro de notificaciones que agrupa respuestas y
  reacciones, suma eventos y mensajes en la campana, enlaza al hilo exacto y
  permite administrar desde Comunidad qué avisos están activos. Su instalador
  es aditivo e idempotente y la prueba local se limpió al finalizar. El plan
  sirve como propuesta para delegar la operación del Portal mediante permisos por acción,
  moderación reversible y una evolución gradual desde el foro hacia
  colaboraciones y, solo si el uso lo justifica, mensajería entre creadores.
  Las fases 3 a 7 se implementaron después en local: Pasaporte pasó a Logros,
  el Perfil público recuperó su función, se añadieron identidad permanente y
  privacidad, invitaciones aceptables, credenciales explícitas, misiones con
  una entrega compartida y reparto real de KE, y cinco Distinciones con
  historial y tarjetas SVG. La administración permanece agrupada en Usuarios,
  Centro de creadores, Rangos y Misiones del Portal.
  Al preparar el despliegue se detectó que estos módulos solo aparecían en el
  actualizador técnico antiguo y no en la pantalla principal que usa el equipo.
  Se unificó la instalación en `Administración → Actualizaciones` mediante las
  versiones `2026.08.26.01` y `2026.08.26.02`; ambas rutas reutilizan ahora las
  mismas funciones idempotentes y no escriben sobre creadores ni postulaciones.
  Las credenciales públicas dejaron de ser píldoras textuales: ahora siguen el
  patrón compacto de las redes sociales y acompañan al autor en todos los
  contextos comunitarios. Creador usa check azul; Moderador, escudo turquesa;
  Consejo, estrella dorada; Administrador, rombo magenta; y Super Admin, corona
  con degradado especial. Las respuestas hechas durante una supervisión guardan
  el ID del usuario administrativo para acreditar al rol real, no al creador
  observado. La apariencia no sustituye los controles de permisos del servidor.
  Se añadió después un Modo Equipo independiente para resolver la limitación de
  participar únicamente mediante supervisión. Administradores y Super Admin
  entran desde el Centro de creadores; Moderadores, desde Comunidad. Un pase de
  un solo uso abre una sesión administrativa propia en el Portal, con nombre,
  rol y badge reales. Permite publicar, responder, mover, fijar, ocultar y
  restaurar conversaciones, dejando auditoría. No crea perfiles ficticios ni
  escribe en `cv_creadores` o `cv_applications`; Supervisión permanece como una
  herramienta separada para observar la experiencia de una persona concreta.

## Activos de comunidad

- [[Activos - Pack de emoticonos de comunidad|Pack de emoticonos de comunidad — 2026-08-27]]: 71 piezas integradas en publicaciones, respuestas y mensajería mediante un carrusel horizontal; catálogo administrable, validación móvil completada y actualización diferencial para cPanel preparada.

## Arquitectura

- Sitio público y panel administrativo: `lacaravanave.com`.
- Portal del creador: `creadores.lacaravanave.com`.
- Desarrollo oficial: `prueba/sitio/` y `prueba/portal/`.
- El espejo `La Caravana/` es de solo lectura; la raíz histórica no se usa.
- Los datos de producción nunca se reemplazan con datos locales. A producción
  viajan código e instaladores idempotentes, nunca dumps.

## Finanzas, Tesorería y bot de Telegram

- Se construyeron los módulos de Finanzas y Tesorería con instaladores para cPanel.
- El bot recibe fotografías de facturas, valida chats autorizados, pone cada
  documento en cola, extrae proveedor, fecha, total y categoría, y solicita
  confirmación mediante botones de Telegram.
- El procesamiento diferido requiere un cron configurado manualmente en cPanel;
  registrar el webhook no crea el cron.
- Las facturas confirmadas entran inicialmente al flujo pendiente y se administran
  desde Finanzas/Tesorería según sus estados.
- La configuración privada vive en `includes/config.php` de producción. Nunca
  registrar aquí tokens, claves de API, secretos de webhook ni credenciales.
- Hubo exposición visual accidental de credenciales durante la configuración;
  deben considerarse rotables y permanecer fuera de documentación y paquetes.
- Referencia técnica: `prueba/BOT-TELEGRAM-FINANZAS-CPANEL.md`.

## Migración local a producción

- La migración se diseñó para superponer código sin perder la base real.
- Los medios locales importantes deben auditarse por separado porque imágenes,
  videos y subidas se excluyen de los paquetes diferenciales normales.
- El empaquetador oficial es `prueba/empaquetar-actualizacion.py`.
- Para producción se ejecuta con `--zip --produccion`.
- El empaquetador bloquea configuraciones locales, archivos `*-maquina`, SQL,
  dumps y escrituras de instaladores sobre creadores/postulaciones.

## Edición Ciudades de Venezuela

- Se recuperó el hero nuevo de la edición y su navegación actualizada.
- El hero depende de paradas estructuradas, no solo del texto libre de la ruta.
- Ruta provisional configurada: Barquisimeto, Valencia, Maracay y Caracas.
- Se recuperó el acceso superior al Portal de Creadores.
- Los perfiles públicos requieren dos condiciones distintas: postulación
  `elegido` y creador publicado. Elegir no publica automáticamente.
- La votación se muestra según el estado operativo de la edición y sus permisos.

## Misiones, Radar y Concepto

- Existen misiones de video, Radar y Concepto.
- Radar/Concepto permiten propuestas, votación comunitaria, propuesta ganadora y
  confirmación de realización.
- Las definiciones creadas en bases locales no viajan como datos; deben recrearse
  mediante el panel o semillas explícitas dentro de un instalador auditado.
- El sistema requiere la columna `missions.tipo` y la tabla `mission_votos`.
- La gestión vive en el panel de Misiones de Cantera/Portal.

## Sistema KR y KE

- KR representa kilómetros de Ruta para Elegidos.
- KE representa experiencia acumulada por acciones de plataforma y comunidad.
- Rangos de Cantera: Andariego, Trochero, Rutero y Baquiano.
- La corona Leyenda corresponde al Elegido con más KR de una edición.
- Los valores KE son configurables en `admin/rangos.php?t=ke` mediante base,
  dificultad, tope diario y tope semanal.
- Cambiar valores afecta adjudicaciones futuras; no reescribe lo ya ganado.
- Radar y Concepto pueden tener premios específicos por misión.

## Comunidad del Portal

- “Actividad reciente” usa datos reales de entregas de retos y misiones.
- El texto “hace N horas/días” se calcula desde las fechas guardadas.
- El bloque de participantes se cambió a **CONOCE A LOS ELEGIDOS**.
- Solo muestra postulaciones con estado `elegido` de la misma edición.
- El bloque fue destacado visualmente con tratamiento dorado/azul.

## Oportunidades y gobernanza

- La pantalla pública del Portal es `gobernanza.php` aunque el menú diga
  Oportunidades.
- Se corrigieron rutas compartidas que funcionaban localmente pero fallaban en
  cPanel; ahora usan `RUTA_SITIO_PRINCIPAL`.
- El instalador de participación crea solicitudes de oportunidades, consultas
  del Consejo y respuestas, sin modificar creadores.

## Bios y multilinks

- `/bio` conserva la Bio oficial de La Caravana.
- Cada creador usa `/bio/nombre-personalizado`, sin `.php` visible.
- Clasificados y Elegidos desbloquean `Recursos → Mi Bio`.
- El creador puede personalizar slug, biografía, foto pública, redes y botones.
- El sistema usa foto, biografía y redes del perfil como valores predeterminados.
- Nunca publica teléfono, WhatsApp, correo, cédula, fecha de nacimiento ni documentos.
- El botón **VOTAR** aparece primero en Bios de creadores y no es editable.
- El equipo administra Bios institucionales y de creadores desde
  `Contenido → Recursos → Gestor de Bios`.
- Se añadió al menú lateral de Contenido el acceso **Recursos y Bios**; dentro
  de esa pantalla están las pestañas Biblioteca y Gestor de Bios.
- Tablas aditivas: `cv_bio_perfiles` y `cv_bio_enlaces`.
- Instalador: `admin/instalar-bios.php`, reejecutable sin duplicados.

### Incidente de guardado corregido — 2026-08-24

- Síntoma en producción: después de guardar aparecía “Bio no encontrada”, la
  información no parecía actualizarse y la foto no subía.
- Causa: el gestor administrativo trataba la foto como una ruta textual y su
  formulario no enviaba archivos. Además, al cambiar el slug o dejar la Bio en
  borrador, abrir la dirección anterior produce correctamente un 404, pero el
  panel no señalaba con suficiente claridad la nueva URL vigente.
- Corrección: subida multipart real para Portal y administración, validación
  JPG/PNG/WebP hasta 8 MB, almacenamiento en `assets/subidas/bios/`, conservación
  de la foto anterior, aviso explícito de publicación y enlace canónico después
  de guardar.
- Prueba real completada: cambio de slug, biografía, foto y estado publicado;
  la URL limpia respondió HTTP 200 mostrando los nuevos datos.

### Rediseño de la Bio pública — 2026-08-24

- En este contexto, **Bio** se refiere al resumen de la historia personal del
  creador.
- La identidad se compactó: foto y nombre tienen una escala más equilibrada.
- Redes, Votar, enlaces personalizados, Compartir y Copiar aparecen antes del
  resumen personal.
- La historia vive en una tarjeta breve **Sobre mí**; si excede el espacio,
  ofrece **Ver más / Ver menos** sin recortar ni modificar el texto guardado.
- El cierre incluye el logotipo blanco de La Caravana y “Hecho en Venezuela”.
- No cambiaron el gestor, las URLs limpias, la lógica de voto ni los datos.
- Segunda pasada visual inspirada en un pasaporte móvil: marco compacto azul
  noche, acentos dorados, marca superior, aro dorado en la foto y enlaces más
  finos.
- La cabecera muestra dinámicamente y sin duplicar datos el estado real
  **Clasificado/Elegido**, la edición y el rango vigente del creador.
- Se añadió el cierre enlazado **Parte de la red de creadores de La Caravana**.
- No se añadieron seguidores ficticios, contenido reciente inventado ni datos
  privados. La prueba móvil a 375 px quedó sin desplazamiento horizontal.

## Recursos del Portal — actualización visual

- El bloque de accesos rápidos conserva textos, rutas, lógica y colores.
- Se sustituyó la iconografía 3D por SVG lineales propios, sin dependencias.
- Iconos de una sola capa, 46–48 px, borde fino y glow sutil.
- Se eliminó el punto azul decorativo.
- Inspiración usa Play; Misiones usa ClipboardCheck; Pasaporte usa credencial.
- Tarjetas con iluminación atmosférica, foco visible, hover de 1 px y feedback
  táctil discreto.
- Existe arquitectura CSS para badges reales; no aparecen con valor cero.
- Revisado a 390 px y con `prefers-reduced-motion`.

## Despliegue a producción

- Paquetes oficiales:
  - `prueba/_paquetes/actualizacion-SITIO.zip`
  - `prueba/_paquetes/actualizacion-PORTAL.zip`
- Sitio se extrae en la raíz de `lacaravanave.com`.
- Portal se extrae en la raíz de `creadores.lacaravanave.com`.
- Después del módulo Bio, ejecutar `admin/instalar-bios.php`.
- No subir archivos de configuración locales ni bases locales.

## Pendientes vigentes

- Terminar el contenido real de retos de Ciudades de Venezuela.
- Decidir retos amarrados a paradas, mecánica Navideña y desbloqueos por rango.
- Terminar auditoría responsive de páginas interiores.
- Revisar y publicar la Bio oficial desde el gestor.
- Configurar el cron del bot financiero si aún no existe en cPanel.
- Confirmar en producción permisos de `assets/subidas/` y reglas de rewrite de `/bio`.
- Mantener sincronizada esta nota después de cada sesión relevante.

## Incidente de acceso al panel — 2026-08-25

- Tras conceder acceso de cPanel a Hermes, el login administrativo comenzó a
  mostrar el contenido de `admin/guardia.php` en pantalla.
- La captura evidencia conversión del código PHP a entidades HTML: aparecen
  literalmente `&lt;?php`, `=&gt;` y `&amp;&amp;`. El indicio principal es corrupción
  del archivo, no un fallo de credenciales ni de base de datos.
- Las copias locales autorizadas `prueba/sitio/admin/guardia.php` y el espejo de
  producción abren correctamente con `<?php`; todavía falta obtener inventario,
  fechas y hashes del hosting para delimitar todos los archivos alterados.
- No se ha reparado ni modificado producción. Antes de sustituir archivos se debe
  conservar una copia forense del estado dañado y comparar contra
  `prueba/sitio/`, sin tocar datos ni configuraciones privadas.
- Reparación completada: se preservó `admin/guardia.php.danado-20260825`, se
  revirtieron las dos capas de `htmlspecialchars` en una copia temporal y se
  comprobó que el resultado coincidía byte por byte con la fuente oficial local
  (40.807 bytes; SHA-256 `48e56f6acb2898f3c6e60dbcf92ebdd9a2f105ceeecca777165164d4d1852fe5`).
- El archivo recuperado pasó `php -l`, se instaló con permisos `0644` y el login
  respondió HTTP 200 sin `Fatal error`, `Warning`, `Deprecated` ni PHP escapado.
- Pendiente inmediato: barrer el dominio completo en busca de otros `.php`
  convertidos a entidades HTML y probar el inicio de sesión como usuario real.
- El login volvió a verse y funcionar después de restaurar `admin/admin.css`
  desde `prueba.lacaravanave.com`. El CSS dañado de producción tenía solo
  14.472 bytes; se conservó como `admin/admin.css.danado-20260825`.
- La restauración visual es operativa pero provisional: la copia remota usada
  tiene 73.670 bytes y la fuente oficial local más reciente tiene 74.219 bytes.
  Queda sincronizar esta última mediante el flujo normal y auditar el resto de
  archivos modificados por Hermes.
- Auditoría forense cerrada: la ventana de intervención señaló cuatro artefactos
  atribuibles a Hermes/Aragorn: `admin/guardia.php` codificado dos veces,
  `admin/admin.css` sustituido por una versión truncada, y los archivos nuevos
  huérfanos `admin/admin_modern.css` y `admin/MODERNIZACION_PANEL_ADMIN.md`.
- No se encontraron referencias activas a los dos archivos añadidos ni otros PHP
  con la misma firma de codificación. Los cuatro artefactos dañados/originales se
  trasladaron fuera de la raíz pública a
  `/home/lsxdahut/hermes-evidencia-20260825/`, con permisos de directorio `0700`.

## Pieza audiovisual de Juandi — 2026-08-24

- Se analizó la carpeta editorial local de Juandi: 189 archivos, compuestos por
  159 imágenes y 30 videos con 28,7 minutos de metraje.
- Decisión narrativa: pieza inspiradora de máximo 2:30, conducida por narración
  y con intervenciones breves de Juandi, su madre y su padre.
- Eje: antes de contar Venezuela, Juandi ya preservaba desde niño los recuerdos
  de su familia; la vocación actual es la continuación de ese impulso.
- Clímax: espera y reacción familiar al ser elegido. Cierre: «Las mejores
  historias están aquí, esperando ser contadas».
- Guion, tiempos, archivos fuente y criterios de montaje documentados en
  `prueba/GUION-JUANDI-2M30.md`.
- Los originales de `Documentos/Juandi` se mantuvieron intactos. Herramientas,
  transcripciones y hojas de contacto permanecen solo en `prueba/_local/`.
- Se recibió `juandi.mp3` (78,02 s) y se creó una primera edición vertical
  1080 × 1920 a 30 fps, de exactamente 2:30, como FCPXML importable en DaVinci.
- Entregables: `prueba/_entregables/JUANDI_2M30_VERTICAL_DAVINCI.fcpxml`, mapa
  de cortes y guía de importación. Contiene 24 cortes de video, seis bloques de
  narración, testimonios/reacción con sonido y siete marcadores editoriales.

## Equipo de La Caravana — estructura editorial — 2026-08-25

- Alejandro Liendo — Creador y Director General.
- Carlos Coronado — Cocreador y Productor General.
- Ramsés Guevara — Director Creativo.
- María del Pilar Sakkal — Coordinadora de Producción; periodista como
  credencial secundaria.
- Denncys Pasos — Coordinador de Dirección Creativa; creador de 0294 Digital y
  abogado como credenciales secundarias.
- Sabrina Tori — Asistente de Producción; turismo como credencial
  secundaria.
- Luisana Contreras — Directora de Arte; diseñadora gráfica como credencial.
- Elí García — Diseñador Gráfico / Motion Designer.
- Wilson Marques — Filmaker; Director de Just Media Pro como
  credencial secundaria.
- Ángeles Pereira — Filmaker; Creadora de Contenido como credencial secundaria.
- Carlos Romero — Filmaker; Editor como credencial secundaria.
- José González — Editor Audiovisual.
- Presentación acordada: Dirección del proyecto, Producción, Dirección creativa
  y arte, y Equipo audiovisual. Profesiones, empresas y proyectos externos se
  muestran como credenciales, no mezclados con el cargo principal.
- Implementación local terminada en `prueba/sitio`: fuente canónica en
  `includes/equipo-caravana.php`, render compartido en
  `bloques/equipo-caravana.php` y estilos en `assets/css/caravana.css`.
- Diseño aprobado como base: fondo negro, jerarquía por cuatro áreas, retratos
  4:5 en blanco y negro con color al interactuar, azul reservado al acento y
  credenciales secundarias en menor jerarquía tipográfica.
- Se prepararon doce derivados web optimizados en `assets/img/equipo/`; los
  originales de `Fotos equipo/` permanecen intactos. `Armando.png` fue
  identificado por el director como la fotografía de José González.
- Verificación local: 12 fichas, cuatro áreas en el orden acordado, sin imágenes
  rotas visibles, sin errores PHP y sin desbordamiento horizontal a 375 px.
- El director pidió el segundo paso y se generó el paquete vertical
  `prueba/_paquetes/equipo-la-caravana-cpanel-2026-08-25.zip` para extraer en
  la raíz de `lacaravanave.com`.
- El paquete contiene 15 archivos: bloque, fuente canónica, CSS aislado y 12
  retratos. No incluye la hoja global, datos, configuraciones, instaladores,
  SQL, logs ni escrituras sobre `cv_creadores` o `cv_applications`.
- Se eligió un paquete vertical porque el empaquetador general detectó otros
  182 archivos pendientes entre sitio y Portal, y además excluye medios de
  `assets/img`; mezclar ese conjunto no correspondía a este despliegue.

## Memoria operativa de Ciudades — 2026-08-25

- Se creó `OBSIDIAN/La Caravana/Ediciones/Ciudades de Venezuela/` como espacio
  canónico de la primera edición.
- Se incorporaron los resúmenes del Meet de producción y de los 1 a 1 del 24 de
  agosto de 2026.
- La memoria separa estrategia narrativa, tablero de decisiones y perfiles de
  creadores.
- Perfiles completos disponibles para Mafe y Juan Argento; quedan ocho 1 a 1
  pendientes. No se usaron los nombres del clon local porque no están validados
  por estos documentos.
- Los detalles personales o funcionales sensibles permanecen en las fuentes
  restringidas; Obsidian conserva solo implicaciones creativas y operativas.

## Meet de producción y 1 a 1 de Gabriel Molina — 2026-08-28

- Se incorporó la transcripción del 27 de agosto, que reúne el 1 a 1 con Gabriel
  Molina y el Meet posterior de producción audiovisual.
- Gabriel queda registrado como creador de mirada cinematográfica y edición,
  con aspiración de construir una carrera de viajes; producción debe equilibrar
  su búsqueda de material propio con entregables diarios y preparar una laptop
  de contingencia compatible con Premiere.
- La estrategia de Ciudades consolida tres flujos editoriales: documental,
  cobertura social y POV; dos familias de retos: evaluables y expres; y una
  dirección documental única con cámaras distribuidas.
- Se fijan como riesgos prioritarios el volumen de material, roles difusos,
  fatiga, evaluación sesgada por audiencia, integración comercial invasiva y
  uso de dron sin permiso nacional suficiente.
- El arco central queda formulado como transformación profesional y humana:
  resolver bajo presión, contar el territorio, ejercer influencia con humildad
  y sostener calidad sin convertir la exigencia en maltrato.

## Rediseño del bloque de equipo en Nosotros — 2026-08-26

- Objetivo del director: que la zona del equipo se vea de élite, con tipografía
  más legible, y corregir dos datos erróneos.
- Datos corregidos en `prueba/sitio/includes/equipo-caravana.php`: el apellido
  de Wilson pasa a **Marquez** con z, y el cargo **Filmmaker** queda bien
  escrito en las tres personas que lo tenían como "Filmaker". El archivo de la
  foto sigue llamándose `wilson-marques.jpg`; solo cambió el nombre mostrado.
- Diagnóstico: los doce retratos vienen de sesiones distintas (estudio oscuro,
  fondo blanco, exterior). El gris puro no unificaba nada, y al no haber hover
  en teléfono el equipo se veía sin color para siempre en la mitad del tráfico.
- Tratamiento elegido: gris de base más lavado azul plano y viñeta en el marco.
  Se probó y se descartó el duotono por cadena de filtros (sepia más
  hue-rotate): el giro de tono deriva según la luz de cada foto, así que los
  fondos claros salían rosados y los oscuros azules.
- Tipografía: cargo de 12,5 a 14,7 px y detalle de 11,5 a 13,3 px; el nombre
  pasa de tracking negativo a positivo, que es lo que pedía Oswald condensada
  en mayúsculas. Se reservan dos líneas de alto para el nombre, de modo que los
  cargos de una misma fila queden siempre a la misma altura.
- Estructura: la numeración 01 / 02 / 03 se eliminó. Estaba rota (mostraba
  01, 04, 06, 09 porque contaba personas, no áreas) y las áreas no son una
  secuencia. En su lugar va el número real de integrantes de cada área.
- Se añadió un cierre con el total del equipo y enlace a `convocatoria.php`,
  única nota amarilla de la sección.
- El marco de los retratos reutiliza la esquina cortada del hero, en vez de
  inventar un recurso nuevo para esta pantalla.
- Verificado en local: `php -l` sobre los dos archivos, la página sin avisos de
  PHP, y a 375 px sin scroll horizontal.
- Pendiente: subir a producción. El cambio vive en el árbol de trabajo
  `prueba/sitio/`, no en `La Caravana/`.

## Documentos relacionados

- `AGENTS.md`
- `prueba/CHECKLIST-MIGRACION.md`
- `prueba/EDICIONES-RUTA-MAESTRA.md`
- `prueba/GOBERNANZA-CREADORES-DECISIONES.md`
- `prueba/BOT-TELEGRAM-FINANZAS-CPANEL.md`
- `PLAN-PANEL-CREADOR-MVP.md`
- `PLAN-SISTEMA-OPERATIVO-Y-FINANCIERO.md`
## 2026-08-26 — URLs limpias en el sitio y el Portal

Se implementó y validó localmente una capa de compatibilidad para ocultar `.php`
en las direcciones públicas del sitio y del Portal. Los enlaces históricos siguen
funcionando mediante redirección canónica, mientras los formularios POST, el panel
administrativo, la API, los recursos estáticos y las Bios quedan protegidos de
reescrituras incompatibles. El despliegue se separa en dos paquetes mínimos de
`.htaccess`, uno por dominio, para no transportar las reglas `noindex` del entorno local.

## 2026-08-26 — Verificación pública manual

Se corrigió la credencial pública para que crear un usuario o pertenecer a la
comunidad no otorgue automáticamente el check azul. Un creador común solo lo
recibe cuando su documentación figura como aprobada mediante revisión manual.
Moderador, Consejo, Administrador y Super Admin muestran directamente la
credencial propia de su función. La insignia se redujo y ahora explica su
significado al tocarla, enfocarla o pasar el cursor.

## 2026-08-26 — Bandeja separada y mensajería colaborativa

El Portal separa definitivamente eventos y conversaciones: la campana reúne
interacciones, KE, verificación, colaboraciones, misiones y logros; el sobre
reúne mensajes del equipo y conversaciones privadas entre creadores. Un hilo
entre creadores solo nace después de aceptar una colaboración y funciona de
forma asincrónica, no como chat en vivo. La aceptación avisa al solicitante y
lo lleva al hilo; cada mensaje posterior enciende únicamente la bandeja para
evitar señales duplicadas. El motor central de KE informa cantidad y motivo,
y la revisión documental notifica aprobación, corrección o rechazo.

## 2026-08-27 — Comunidad explorable y Radar de ideas

Se implementó la siguiente etapa de crecimiento del foro: búsqueda sobre
publicaciones y respuestas, filtros por intención, órdenes por relevancia,
recencia, actividad o falta de respuesta, y carga progresiva en grupos de doce
sin devolver al creador al inicio. `Radar de ideas` muestra dentro de Comunidad
las misiones Radar oficiales con su estado, propuestas, votos y saldo personal;
la propuesta y la votación permanecen concentradas en Misión. Detalle y pruebas
en [[Plan - Roles moderacion y evolucion de la comunidad#Exploración del foro y Radar de ideas — 2026-08-27]].

## 2026-08-27 — Mensajería incorporada al actualizador versionado

Se corrigió una desconexión de despliegue: la Bandeja tenía un instalador
idempotente dentro de `Instalar sistemas`, pero no figuraba en la pantalla
canónica `Actualizaciones del sistema`. Esto hacía que producción mostrara
`0 pendientes` mientras el Portal advertía que faltaba completar la
actualización. La mensajería queda registrada como `2026.08.27.01`; al
ejecutarla crea conversaciones, mensajes y lecturas sin modificar creadores ni
postulaciones.

## 2026-08-27 — Insignias unificadas y Creador de ÉLITE

Se eliminó de la experiencia visible la duplicación entre Insignias y
Distinciones. Las insignias concentran ahora progreso, beneficios, historial y
posibilidad de compartir. La culminación de la colección es `Creador de ÉLITE`,
estado automático reservado para quien alcanza Diamante en todas las insignias
visibles. Se comunica mediante una ruta visual compacta y, al desbloquearse,
produce una notificación única y reconocimiento en el perfil público. Véase
[[Plan - Roles moderacion y evolucion de la comunidad#Fusión de Distinciones e Insignias · Creador de ÉLITE — 2026-08-27]].

## 2026-08-27 — Bandeja visual unificada

Las conversaciones con el equipo y con colaboradores aceptados comparten ahora
la misma experiencia visual y de navegación. La Bandeja reúne hilos, no leídos,
último mensaje, identidad y compositor; el canal oficial diferencia avisos
generales de respuestas privadas. En móvil, lista e hilo se presentan por
separado para reducir ruido y mantener el compositor accesible. Detalle en
[[Plan - Roles moderacion y evolucion de la comunidad#Bandeja unificada para equipo y colaboradores — 2026-08-27]].

## 2026-08-27 — Tarjetas aspiracionales y rostro del equipo

Los logros de Orquídea, Turpial, Araguaney, Arepa y Brújula conservan su
progresión Bronce–Diamante, pero al compartir generan una tarjeta vertical en
alta resolución con el material de su nivel, el isotipo abierto de La Caravana
y el nombre real del creador. `Creador de ÉLITE` genera también su propio PNG:
reúne las cinco gemas, muestra la colección completa y escribe dinámicamente el
nombre del titular. En móviles se usa la hoja nativa de compartir y, si no está
disponible, se descarga el archivo.

Los usuarios internos del panel —Super Admin, administradores, moderadores,
Consejo y demás roles— pueden tener fotografía propia sin reutilizar ni alterar
la ficha de un creador. La fotografía se administra dentro de `Usuarios`, se
muestra en la navegación interna y vuelve a iniciales cuando se retira.

### Corrección posterior al primer despliegue

La primera entrega cambió únicamente la exportación social y reinterpretó el
diseño con un medallón genérico; no trasladó a la colección visible el lenguaje
aprobado. Se corrigió la fuente compartida: cada insignia muestra el contorno
abierto del isotipo, conserva su símbolo propio y cambia solamente el material
entre Bronce, Plata, Oro, Platino y Diamante. El mismo tipo de insignia viaja al
generador social para evitar iniciales genéricas. La lección queda fijada:
aprobar una tarjeta implica tanto su representación dentro de Logros como la
pieza exportable, y ambas deben verificarse juntas.

### Centro de Expedición y tarjetas finalmente visibles

Los diez Elegidos disponen ahora de una experiencia adaptativa en Inicio: el
`Centro de Expedición` resume misiones KR, entregas, equipo y accesos operativos
sin crear otro portal ni otra cuenta. Las misiones programadas se ven pero no
admiten entrega antes de abrir. En Logros, la pieza social completa dejó de ser
un resultado invisible del botón Compartir: se previsualiza dentro del Portal
y desde allí puede compartirse o descargarse. Detalle en [[Plan - Roles moderacion y evolucion de la comunidad#Centro de Expedición y previsualización real de tarjetas — 2026-08-27]].

### Corrección de fidelidad visual de las tarjetas

La reconstrucción inicial por canvas no respetaba el acabado aprobado y el
segundo intento usó recortes del tablero de referencia, que tampoco constituían
activos originales de calidad. Ambos enfoques fueron retirados. La colección
final contiene 25 renders independientes de 1254 × 1254: cinco símbolos por
cinco materiales, creados individualmente con una geometría visual compartida.
Cada creador ve directamente en Logros la tarjeta correspondiente a su nivel;
las aún no ganadas muestran la versión Bronce atenuada. La ampliación y la
exportación reutilizan el mismo render, por lo que pantalla y pieza compartida
no divergen. Creador de ÉLITE conserva su tarjeta vertical aprobada con placa
limpia y nombre dinámico superpuesto por el Portal.

### Composición social integrada de las insignias

La pieza exportable usa el render original a sangre y lo funde con
una placa inferior oscura para mostrar logro, nivel, avance y creador. También
se retiró el fondo cuadriculado del modal. Tras comprobar que el formato vertical
seguía percibiéndose alargado, la salida se corrigió a 1080 × 1080. Fue
verificada en escritorio y a 375 px sin desbordamiento horizontal.

### Reinicio del sistema visual de insignias — 2026-08-28

Se decidió rehacer desde cero la familia visual tomando como referencia la
energía ceremonial que funcionó en la tarjeta de Clasificado, sin recortar ni
copiar su composición. La progresión oficial e invariable es **Bronce → Plata
→ Oro → Platino → Diamante**. Orquídea, Turpial, Araguaney, Arepa y Brújula
conservarán símbolo, composición y geometría durante toda la evolución; solo
cambiarán material, color, luz e intensidad. Estas tarjetas no llevarán el
nombre del creador. La pieza culminante de esta familia es **Creador de ÉLITE**,
que se desbloquea automáticamente al alcanzar Diamante en las cinco insignias;
no corresponde al estatus Elegido ni constituye una sexta insignia. La propuesta
visual generada por error para Elegido quedó descartada y no se integra al Portal.

La identidad visual de Creador de ÉLITE tampoco mezclará las cinco insignias ni
sus cinco colores. Tendrá un único emblema propio: portal ceremonial de obsidiana
y oro. La dirección se simplificó después: la tarjeta contendrá únicamente el
isotipo de La Caravana sin ®, tratado como diamante con borde dorado, y el título
`CREADOR DE ÉLITE`. No llevará placa, nombre, subtítulo ni símbolos adicionales.
El maestro visual se mantiene como propuesta pendiente de aprobación antes de
reemplazar activos.

### Colección visual v2 integrada y celebración de logro — 2026-08-28

La propuesta quedó aprobada e integrada. El Portal sirve 26 obras terminadas:
25 tarjetas cuadradas independientes de 1254 × 1254 —Orquídea, Turpial,
Araguaney, Arepa y Brújula en Bronce, Plata, Oro, Platino y Diamante— y una
tarjeta exclusiva de Creador de ÉLITE. ÉLITE usa únicamente el isotipo sin ®,
acabado de diamante, luz azul, borde dorado y el título; no mezcla las otras
insignias, no lleva nombre ni se recompone en el navegador.

Pantalla, ampliación, descarga y compartir reutilizan exactamente el mismo
WebP optimizado. Se eliminó del flujo la composición por canvas que podía
deformar o degradar las imágenes. Cuando el historial registra un ascenso, el
Portal presenta una celebración una sola vez por creador y logro: entrada de la
tarjeta, halo, destello y partículas sutiles. El botón `Ampliar y compartir`
permite volver a verla. `prefers-reduced-motion` desactiva el movimiento sin
ocultar el reconocimiento.

La colección pública y la condición ÉLITE quedaron fijadas a exactamente cinco
insignias. Filas históricas o experimentales del catálogo administrativo no se
cuentan ni aparecen en esta progresión. La prueba local confirmó `2/5`, camino
ÉLITE `0 de 5`, activos naturales de 1254 × 1254, ausencia de errores de consola
y vista móvil a 375 px sin desbordamiento horizontal.

### Progreso ÉLITE, firma sonora y perfiles públicos del equipo — 2026-08-28

La tarjeta ÉLITE bloqueada comunica ahora su condición mediante candado, cinco
marcadores y una barra `n/5`, sin acumular explicaciones. Al completar las cinco
insignias en Diamante se activa una celebración propia, más luminosa que los
ascensos ordinarios, y la tarjeta queda disponible para ampliar, compartir y
descargar. Cada metal posee una firma sonora sintetizada corta; ÉLITE usa una
secuencia ascendente exclusiva. El sonido nunca bloquea la interfaz, respeta
movimiento reducido y dispone de control persistente 🔊/🔇.

Super Admin, Administradores, Consejo y Moderadores pueden tener un perfil
público independiente de `cv_creadores`: fotografía del usuario interno,
biografía, redes, enlaces y logros asignados manualmente. Sus intervenciones en
Comunidad enlazan ese perfil y muestran su fotografía y credencial. El
Moderador conserva su alcance estricto de panel —Inicio y Comunidad— y opera
en el Portal mediante Modo Equipo, sin suplantar a ningún creador. La migración
aditiva crea `cv_perfiles_equipo` y `cv_logros_equipo`; no toca creadores ni
postulaciones.

## Consolidación ejecutiva de producción — 2026-08-28

- Se incorporó el resumen estratégico entregado por Dirección a la memoria
  canónica de Ciudades de Venezuela, evitando duplicar lo ya extraído de la
  transcripción del Meet.
- Quedó explícita la secuencia operativa de cada jornada: apertura, ruta, reto,
  comida y pausa, edición y entrega, cierre, respaldo y preparación.
- Se registró el mapa de responsabilidades: Alejandro concentra alineación y
  reglas; Wilson dirige documental y técnica; Carlos consolida marcas y
  logística; María/Maripili coordina operación; Ramsés, Densis y Armando
  desarrollan retos, gamificación y cobertura social.
- Se documentó el estado estratégico de Grupo Sindoni, Óptica Caroní, Cashea,
  Diablitos, Rolda, BNC y hoteles, distinguiendo integración propuesta de
  compromiso confirmado.
- Los en vivo quedan como capacidad condicionada a reunión técnica y prueba de
  YoloBox, cámaras, audio, energía, temperatura, conectividad y plan alterno.
- El tablero suma un documento maestro único para equipo y creadores, una matriz
  comercial por estado y asignaciones inmediatas por responsable.

## Elenco canónico de los diez Elegidos — 2026-08-28

- Se recibieron y procesaron las fichas integrales de los diez Elegidos de la
  primera edición, Ciudades de Venezuela.
- El elenco confirmado es: Carla Mariné García Peña, Carlos José Molina Jaimes,
  Daril José Chacón Jiménez, Gabriel Leonardo Molina Díaz, Génesis Marcelys
  Ysasis Lobatón, Juan Diego Argento Baissari, Marcos Antuare, María Fernanda
  Monsalve, Rosangela Inés Medina Rondón y Yorlenis Blanco.
- Se creó una ficha estratégica por persona y un índice canónico del elenco en
  [[00 - Índice de creadores]].
- La memoria conserva territorios, fortalezas, lenguajes, oportunidades de
  activación, arcos posibles y pendientes funcionales. No copia teléfonos,
  correos, fechas personales, salud, documentos, equipos identificables ni
  detalles privados del expediente.
- La diversidad del grupo cubre Lara, Mérida, La Guaira, Miranda/Caracas,
  Sucre, Zulia y Portuguesa, con perfiles de actuación, documental regional,
  periodismo, cine, turismo, fotografía, humor, UGC y storytelling.
- Ser Elegido está confirmado; llamada, expediente o validación operativa
  incompletos se registran aparte y no degradan ni contradicen esa condición.
