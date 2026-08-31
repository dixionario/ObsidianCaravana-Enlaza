---
tipo: plan-de-producto
proyecto: La Caravana
fecha: 2026-08-26
estado: en-refinamiento
---

# Roles, moderación y evolución de la comunidad

## Implementación local — primera fase (2026-08-26)

Ya está construida y verificada localmente la primera fase del refinamiento:

- Comunidad se organiza mediante la frase `Quiero:` y las pestañas Colaborar,
  Encontrar, Ayuda y Preguntar; cada filtro conserva una URL propia.
- Los tipos históricos Hallazgo y Pregunta se presentan como Encontrar y
  Preguntar sin migrar ni reescribir publicaciones existentes.
- Los títulos nuevos se limitan a 90 caracteres, los cuerpos a 1.200 y las
  respuestas a 600. Los textos largos usan `Leer más` y el feed enseña dos
  respuestas antes de ofrecer las restantes.
- Los únicos enlaces admitidos son Instagram, TikTok y YouTube. Se valida el
  host real en el servidor y se muestra un botón semitransparente que abre la
  red en otra pestaña, sin reproductores ni consultas externas.
- `Me sirve` ya puede retirarse. Se añadió `Me inspira`, también reversible y
  sin KE; la base existente permite ambas reacciones sin instalar otra tabla.
- Todas las misiones publicadas pueden tener apertura y cierre absolutos, o un
  cierre calculado desde una duración en horas. Antes de abrir muestran una
  cuenta regresiva sin brief; después del cierre conservan el registro pero el
  servidor rechaza aceptar, cancelar, editar o entregar.
- Se eligió la expresión `Plazo finalizado`, con las variantes `Entregada` y
  `No entregada · plazo finalizado` según la situación individual.

La prueba sembrada cubrió apertura futura, vencimiento, restauración del dato
original, enlaces válidos y engañosos, ausencia de errores PHP y móvil a 375 px
sin desbordamiento horizontal. Esta fase no crea todavía chat, mensajería entre
creadores, reportes ni el modelo nuevo de capacidades administrativas.

## Objetivo

Permitir que Alejandro delegue la operación cotidiana del Portal sin entregar
los poderes críticos del Super Admin. El sistema debe distinguir claramente
entre moderar conversaciones, atender mensajes, aplicar medidas comunitarias,
administrar accesos y controlar usuarios internos.

Este plan continúa la filosofía registrada en [[Hito - Nacimiento espontaneo de la comunidad|Nacimiento espontáneo de la comunidad]]: proteger y
potenciar la interacción orgánica sin convertir el Portal en una plataforma
sobredimensionada antes de validar su uso.

## Estado actual comprobado

- El panel ya tiene usuarios internos, roles, Super Admin, permisos por
  pantalla y registro general de acciones POST.
- La comunidad ya permite al equipo leer, buscar, ocultar/restaurar, fijar,
  cerrar ayudas, responder oficialmente, ocultar comentarios y borrar hilos.
- El Portal ya diferencia publicaciones y comentarios del equipo.
- Ya existen tablas aditivas para habilitar, suspender o bloquear el acceso de
  un creador y para auditar operaciones sin guardar estados técnicos en
  `cv_creadores`.
- La supervisión permite entrar como un creador y está correctamente reservada
  al Super Admin.
- Ya existe mensajería privada asíncrona entre cada creador y el equipo, además
  de avisos masivos. No existe mensajería directa entre creadores.
- La limitación actual es que los permisos son principalmente por pantalla:
  abrir la bandeja de comunidad todavía exige rol Administrador y las acciones
  sensibles no tienen capacidades independientes.

## Modelo recomendado de autoridad

### Super Admin

- Crea, edita y revoca usuarios internos; asigna roles y excepciones.
- Conserva en exclusiva la supervisión/impersonación, los instaladores, la
  gestión de permisos y el acceso técnico completo.
- Puede aplicar o revertir bloqueos permanentes y consultar toda la auditoría.
- Es el único que puede ejecutar borrado definitivo, cuando exista una razón
  legal u operativa documentada.

### Administrador del Portal

- Configura la experiencia del Portal y coordina moderadores.
- Puede moderar todo el contenido, responder como equipo, revisar reportes,
  emitir advertencias y aplicar suspensiones temporales.
- Puede administrar accesos ordinarios, pero no entrar como un creador, cambiar
  permisos internos, ejecutar instaladores ni eliminar cuentas o datos.

### Moderador de comunidad

- Lee la bandeja, busca, fija, oculta y restaura publicaciones o comentarios.
- Atiende reportes, marca ayudas como resueltas y responde identificado como
  integrante del equipo de moderación.
- Puede emitir advertencias y silenciar temporalmente la participación en
  comunidad; no puede bloquear todo el Portal ni cambiar etapas, identidad,
  KE, KR, rangos, misiones o datos del perfil.
- No puede reescribir silenciosamente las palabras de un creador. Ante un
  problema, oculta y explica; cualquier corrección excepcional conserva el
  original, la razón, el autor y la fecha.

### Soporte de creadores

- Atiende únicamente la bandeja privada creador ↔ equipo.
- Puede responder y escalar un caso, pero no moderar el foro, cambiar accesos ni
  consultar documentos de identidad.

Un mismo usuario interno puede recibir capacidades adicionales explícitas. El
rol aporta valores predeterminados; las excepciones por usuario permiten abrir
o cerrar una capacidad sin inventar roles nuevos.

## Permisos por acción

La autorización debe comprobarse en el servidor sobre cada operación, no solo
al mostrar u ocultar botones. Registro inicial de capacidades:

- `comunidad.ver`
- `comunidad.responder_equipo`
- `comunidad.fijar`
- `comunidad.ocultar`
- `comunidad.restaurar`
- `comunidad.editar_con_historial`
- `comunidad.borrar_definitivo`
- `reportes.gestionar`
- `creadores.advertir`
- `creadores.silenciar_comunidad`
- `creadores.suspender_portal`
- `creadores.bloquear_portal`
- `mensajes.atender`
- `mensajes.enviar_avisos`
- `creadores.supervisar`
- `usuarios.gestionar`

La implementación debe añadir un helper central equivalente a
`puedeAccion($capacidad)`, con valores predeterminados por rol y excepciones
guardadas por usuario. Los controles existentes por página se conservan para
navegación y compatibilidad, pero dejan de ser la única barrera.

## Moderación y medidas

- Priorizar siempre acciones reversibles: ocultar, restaurar, advertir y
  suspender temporalmente.
- Separar tres alcances: silencio en el foro, suspensión de mensajería y
  suspensión/bloqueo total del Portal.
- Cada medida registra creador, alcance, motivo interno, explicación visible
  para el afectado, duración, moderador, fecha y revocación.
- Añadir reportes de publicaciones y comentarios con categorías breves:
  acoso, spam, información privada, contenido peligroso, suplantación y otro.
- Evitar borrados físicos cotidianos. El contenido oculto sigue disponible
  para revisión y apelación; el borrado definitivo exige Super Admin,
  confirmación reforzada y auditoría.
- Las ediciones de contenido guardan la versión original y muestran que hubo
  intervención de moderación. Para el MVP se recomienda ocultar y solicitar
  corrección en vez de editar directamente lo escrito por una persona.
- Toda acción sensible queda en una bitácora específica de moderación, además
  del registro general del panel.

## Alternativas para ampliar la interacción

### 1. Foro mejorado y colaboraciones estructuradas — recomendado ahora

- Añadir reportes, menciones, notificaciones, seguimiento de hilos y solicitudes
  de colaboración con territorio, tema, necesidad y estado.
- Permitir que una persona manifieste interés y que el autor decida con quién
  intercambiar contacto; evita publicar datos personales o abrir mensajes no
  solicitados.
- Aprovecha el comportamiento que ya apareció, requiere menos moderación y
  mantiene conversaciones útiles visibles para toda la comunidad.

### 2. Mensajería directa asíncrona entre creadores — fase posterior

- Bandeja interna, sin teléfonos visibles, con aceptación previa, bloqueo,
  reporte, límites anti-spam y capacidad de cerrar conversaciones.
- Conveniente solo después de medir solicitudes de colaboración y contar con
  moderadores activos y reglas comunitarias claras.
- Puede reutilizar patrones visuales de la mensajería creador ↔ equipo, pero no
  debe compartir su tabla ni confundir mensajes privados con soporte oficial.

### 3. Chat en tiempo real — no recomendado para el MVP

- Aumenta inmediatez, pero también acoso, spam, expectativas de respuesta,
  almacenamiento, privacidad, presencia, notificaciones y carga de moderación.
- PHP/cPanel puede sostener sondeo periódico a pequeña escala, pero un chat
  realmente inmediato requeriría infraestructura adicional o un proveedor
  externo, con nuevos costos y dependencias.
- Solo reconsiderarlo cuando la mensajería asíncrona pruebe una necesidad clara
  de simultaneidad que el foro y las colaboraciones no puedan resolver.

## Refinamiento de experiencia del foro — 2026-08-26

### Principio de diseño

El feed sirve para descubrir conversaciones; el hilo sirve para profundizar.
Hoy ambos niveles aparecen completos en la misma tarjeta: una publicación puede
tener hasta 3.000 caracteres, un comentario hasta 2.000 y se muestran todas las
respuestas. El resultado crecerá en altura y contexto a medida que aumente la
participación.

La solución recomendada no es prohibir historias extensas, sino aplicar
divulgación progresiva:

- En el feed se muestran título, autor, tipo, fecha, una vista previa de cuatro
  líneas, métricas y acciones principales.
- `Leer más` expande el cuerpo en la propia tarjeta; `Abrir conversación` lleva
  al hilo cuando se necesitan respuestas y contexto completos.
- El feed muestra como máximo dos respuestas —una oficial o útil y la más
  reciente— y un botón `Ver las N respuestas`.
- Al abrir el hilo se muestra el contenido completo, todas las respuestas y el
  formulario para participar. El enlace debe poder compartirse internamente y
  devolver al mismo hilo.
- La expansión funciona sin JavaScript mediante `details` o una ruta dedicada;
  JavaScript solo mejora la transición y el estado visual.

### Límites editoriales recomendados para el MVP

- Título: máximo 90 caracteres; obliga a expresar la idea con claridad y ocupa
  como máximo dos líneas en móvil.
- Cuerpo: máximo 1.200 caracteres, con contador y orientación. En el feed solo
  se ven cuatro líneas hasta pulsar `Leer más`.
- Respuesta: máximo 600 caracteres.
- Los textos históricos que excedan estos límites siguen completos; la regla
  nueva se aplica a publicaciones futuras.
- El servidor mantiene el límite real. El contador del navegador es solo ayuda.

Los valores se validarán en el mundo sembrado con contenido realista. La vista
previa se limita por líneas CSS, sin cortar ni guardar versiones incompletas.

### Sistema mínimo de reacciones

No construir una fila de emojis genéricos. Cada interacción debe responder una
pregunta concreta y producir una señal comprensible:

1. **Me sirve:** conserva el significado actual de utilidad. Puede seguir
   aportando KE al autor dentro de los topes establecidos.
2. **Me inspira:** reconoce valor creativo o emocional, pero no entrega KE en
   el MVP para evitar intercambios de reacciones y puntuación inflada.

Una persona puede elegir una reacción de cada tipo y retirarla. Los contadores
son compactos y la interfaz destaca la selección propia. No se añaden `Me
gusta`, corazones o aplausos que dupliquen significado.

**Quiero colaborar** no es una reacción. Es una intención operativa con estado:
interés enviado, aceptado, cerrado y colaboración completada. Solo el autor
recibe inicialmente la identidad del interesado; los datos privados no se
publican.

### Interacciones con propósito

- `Responder` continúa como acción principal de preguntas y hallazgos.
- `Puedo ayudar` reemplaza `Responder` en peticiones de ayuda y abre el mismo
  formulario con un contexto más claro.
- `Quiero colaborar` aparece solo en publicaciones de colaboración.
- `Seguir conversación` activa avisos sin obligar a escribir una respuesta.
- El autor puede marcar `Esto resolvió mi pregunta` o cerrar una ayuda. Solo
  resultados confirmados deben alimentar reconocimientos o KE adicionales.
- Las notificaciones resumen actividad y llevan directamente al hilo; no se
  premia abrirlas ni reaccionar por reaccionar.

### Tipos y navegación

La primera fase traduce los tipos visibles a `Colaborar`, `Encontrar`, `Ayuda`
y `Preguntar`, conservando compatibilidad con `Hallazgo` y `Pregunta` en los
datos históricos. El flujo estructurado de aceptación de una colaboración
queda para una fase posterior; por ahora la intención abre una conversación
pública y usa una llamada a la acción específica.

Filtros iniciales: `Para ti` —edición y territorio, sin algoritmo opaco—,
`Recientes`, `Necesitan ayuda` y, después, `Colaboraciones`. No se añaden votos
negativos ni se ordena exclusivamente por reacciones: una petición nueva y
urgente no debe quedar invisible por no tener actividad todavía.

### Secuencia recomendada

1. Compactar tarjetas, añadir `Leer más`, abrir hilo dedicado y plegar
   respuestas. No cambia datos y resuelve primero la saturación visual.
2. Hacer `Me sirve` reversible y añadir `Me inspira` sin KE.
3. Añadir seguimiento y notificaciones por hilo.
4. Incorporar Reportar y la moderación definida en este plan.
5. Lanzar Colaboración como tipo estructurado con interés y aceptación.

### Métricas de éxito

- Publicaciones abiertas desde el feed.
- Respuestas por conversación y preguntas/ayudas resueltas.
- Relación entre `Me sirve`, `Me inspira` y respuestas reales.
- Conversaciones seguidas y retornos mediante notificaciones.
- Intereses de colaboración aceptados y colaboraciones confirmadas.
- Reportes y ocultamientos por cada cien publicaciones.

## Arquitectura de intenciones: «Quiero…» — 2026-08-26

La navegación principal de Comunidad se organizará alrededor de lo que la
persona quiere lograr, no de términos técnicos del foro:

- **COLABORAR:** encontrar a otra persona para crear o ejecutar algo juntos.
  Campos orientadores: qué propone, territorio/modalidad, perfiles que busca,
  fecha aproximada y qué aporta. Acción principal: `Quiero colaborar`.
- **ENCONTRAR:** localizar una historia, personaje, lugar, recurso, equipo,
  conocimiento o contacto. Campos: qué busca, categoría, territorio y para qué
  lo necesita. Acción principal: `Tengo una pista`.
- **AYUDA:** resolver un bloqueo concreto que ya está impidiendo avanzar.
  Campos: problema, qué intentó, urgencia y fecha límite opcional. Acción:
  `Puedo ayudar`; el autor puede marcarla como resuelta.
- **PREGUNTAR:** aprender de la experiencia colectiva sin que exista una
  urgencia operativa. Campos: pregunta y contexto breve. Acción: `Responder`;
  el autor puede marcar la respuesta que resolvió su duda.

La cabecera se leerá como una frase: `Quiero:` seguida de cuatro pestañas. Al
elegir una, se filtra el feed y el formulario de publicación cambia sus textos
y campos orientadores. La primera entrada puede ser `Todo para mí`, separada
visualmente de las cuatro intenciones, para no romper la frase ni obligar a
revisar cuatro pestañas una por una.

Cada pestaña conserva su URL (`?quiero=colaborar`, etc.) para volver al mismo
contexto, compartirlo y funcionar sin JavaScript. En móvil serán chips
horizontales desplazables o una rejilla 2 × 2; nunca cuatro palabras comprimidas
en una sola línea.

### Estados que cierran el ciclo

- Colaborar: `Abierta` → `Conversando` → `Colaboración acordada` → `Realizada`
  o `Cerrada`.
- Encontrar: `Buscando` → `Pistas recibidas` → `Encontrado`.
- Ayuda: `Necesita ayuda` → `Recibió respuestas` → `Resuelta`.
- Preguntar: `Abierta` → `Respondida` → `Respuesta confirmada`.

Los estados finales permanecen visibles: demuestran que la comunidad produce
resultados y evitan que otras personas sigan respondiendo innecesariamente.

### Enlaces de video en el foro

- Solo se aceptan URLs `http` o `https` cuyos hosts normalizados pertenezcan a
  Instagram, TikTok o YouTube, incluyendo sus subdominios y enlaces cortos
  oficiales necesarios (`youtu.be`, `vm.tiktok.com`, `vt.tiktok.com`).
- La validación se hace con el host interpretado por el servidor, no buscando
  palabras dentro del texto; dominios engañosos como
  `instagram.com.ejemplo.com` se rechazan.
- Se guarda la URL normalizada y la plataforma detectada. En el feed no se
  incrusta el reproductor: aparece un botón semitransparente `Video en
  Instagram`, `Video en TikTok` o `Video en YouTube`.
- El botón abre una pestaña nueva con `noopener nofollow`. No se descarga el
  contenido ni se solicitan metadatos externos en el MVP, evitando lentitud,
  rastreo, fallos de API y complejidad de derechos.
- Si el enlace no es admitido, el formulario conserva el texto escrito y
  explica exactamente cuáles plataformas acepta.
- Enlaces históricos no válidos no se rompen ni se convierten automáticamente;
  pasan por moderación o una migración explícita si fuera necesaria.

### Ideas posteriores, ordenadas por valor

1. **Seguir una conversación:** notificación agrupada cuando haya actividad.
2. **Afinidad territorial:** mostrar primero publicaciones de la edición o
   estados declarados por el creador, con opción de ver todo el país.
3. **Etiquetas pequeñas:** territorio y especialidad, administradas mediante un
   catálogo corto para evitar cientos de variantes escritas a mano.
4. **Respuesta confirmada:** el autor señala qué aporte le sirvió; mejor señal
   de calidad que acumular reacciones.
5. **Resumen semanal:** mejores hallazgos, ayudas abiertas y colaboraciones que
   todavía buscan personas; evita notificaciones constantes.
6. **Perfiles sugeridos:** al publicar una colaboración, sugerir creadores por
   territorio y capacidades declaradas, sin enviarles mensajes automáticos.
7. **Celebrar resultados:** una tarjeta compacta cuando una colaboración se
   confirma, sin revelar conversaciones ni datos privados.
8. **Borradores:** conservar localmente el texto del formulario si la sesión
   vence o falla la validación.

No se recomiendan para el MVP: tendencias basadas solo en popularidad, votos
negativos, reproducción automática, comentarios infinitamente anidados,
mensajes directos sin consentimiento o recompensas por cada clic.

## Programación y ciclo de vida de misiones — 2026-08-26

### Estado actual y brecha

Las misiones Radar y Concepto ya admiten `abre_en`, `fecha_limite`, estado
programado y reloj de cierre. Las misiones de video comparten `fecha_limite`,
pero todavía no aplican de forma completa `abre_en` ni bloquean todas las
acciones después del vencimiento. La programación debe convertirse en una
regla común a todos los tipos de misión.

### Ciclo visible recomendado

1. **Borrador:** solo el equipo la ve y edita.
2. **Próximamente:** el creador ve una tarjeta bloqueada con nombre, teaser,
   recompensa y cuenta regresiva hasta la apertura; el brief permanece oculto.
3. **Disponible:** al llegar `abre_en`, se revela el brief y comienza el plazo
   de entrega.
4. **Cierra pronto:** mismo estado operativo, con advertencia cuando entra en el
   umbral configurable —24 horas por defecto—.
5. **Plazo finalizado:** el contenido permanece como memoria, pero aceptar,
   editar o entregar queda bloqueado por el servidor.
6. **Entregada:** quien envió a tiempo conserva este resultado después del
   cierre y puede revisar su entrega, sin verla como fallida.
7. **No entregada · plazo finalizado:** etiqueta individual para quien aceptó
   pero no alcanzó a entregar.

`Plazo finalizado` se recomienda en lugar de `Caducada` o `Tiempo acabado`: es
claro, neutral y no sugiere que el contenido haya desaparecido o sea inválido.

### Configuración administrativa

- `Publicar ahora` o `Programar apertura` con fecha y hora de Caracas.
- Plazo expresado preferentemente como duración desde la apertura —minutos,
  horas o días—; el sistema calcula y guarda `fecha_limite` para disponer de un
  cierre absoluto y auditable.
- Alternativa avanzada: fijar manualmente apertura y cierre. La interfaz avisa
  si el cierre no es posterior a la apertura.
- Teaser previo opcional, umbral de `Cierra pronto` y decisión de si el resultado
  cerrado permanece visible.
- Acciones excepcionales: extender plazo o reabrir con motivo obligatorio y
  auditoría. La extensión aplica a todos; no se cambia silenciosamente para una
  persona.

### Reglas técnicas

- El reloj del navegador es visual; abrir el brief, aceptar, editar y entregar
  siempre vuelven a comprobar apertura y cierre con la hora del servidor.
- El cambio de estado puede calcularse al consultar, sin depender de un cron.
  Un cron se usará después para enviar avisos o consolidar cierres, no para
  garantizar la prohibición de entregar tarde.
- Todas las familias de misión usan una función única para determinar estado y
  permiso de participación.
- Al llegar a cero, la interfaz actualiza el mensaje y desactiva acciones; el
  siguiente POST confirma nuevamente el cierre en servidor.
- Una entrega registrada antes del límite conserva su hora original. No se
  alteran entregas ni estados de creadores durante la instalación.

### Avisos recomendados

- Misión programada: aparece en el Portal sin notificación invasiva.
- Apertura: aviso interno agrupado y acceso directo.
- Cierra pronto: un solo recordatorio dentro del umbral configurado.
- Cierre: sin notificación si no hubo participación; confirmación de resultado
  para quien entregó o participó.

## Entrega por fases

1. **Autoridad segura:** rol Moderador y Administrador del Portal, capacidades
   por acción, UI de asignación y auditoría; sin cambiar todavía la experiencia
   de los creadores.
2. **Moderación operable:** reportes, advertencias, silencios temporales,
   restauración de comentarios y cola de trabajo con estado y responsable.
3. **Colaboraciones estructuradas:** solicitud pública, manifestaciones de
   interés, aceptación y notificaciones internas, sin compartir datos privados.
4. **Mensajería directa opcional:** piloto limitado, con consentimiento,
   bloqueo, reporte y límites, solo si los datos de la fase anterior la
   justifican.
5. **Evaluación de tiempo real:** decisión basada en uso, tiempos de respuesta,
   incidentes y capacidad real de moderación.

## Reglas técnicas y de seguridad

- Todas las tablas son aditivas; ninguna medida de moderación escribe estados
  técnicos en `cv_creadores` ni modifica `cv_applications`.
- Todo POST conserva CSRF y valida la capacidad en el endpoint.
- Los bloqueos revocan sesiones activas o fuerzan una nueva comprobación de
  acceso en cada carga; ocultar botones no cuenta como seguridad.
- Las razones internas y la evidencia nunca se exponen a otros creadores.
- Los documentos de identidad quedan fuera del alcance de moderadores y soporte.
- Los instaladores son idempotentes y el despliegue no transporta filas locales.
- Antes de construir chat deben existir normas comunitarias, flujo de reportes,
  bloqueo entre usuarios, retención definida y responsable de incidentes.

## Indicadores para decidir la evolución

- Publicaciones, respuestas útiles y colaboraciones iniciadas/completadas.
- Reportes por cada cien interacciones y tiempo hasta su resolución.
- Reincidencia después de advertencias o silencios.
- Solicitudes de contacto aceptadas frente a ignoradas o reportadas.
- Tiempo que el equipo dedica semanalmente a moderar y atender mensajes.
- Casos donde el foro o la colaboración estructurada resultaron insuficientes y
  justificaron mensajería privada o tiempo real.

## Decisiones pendientes con Alejandro

- Confirmar quién podrá crear Administradores del Portal y Moderadores; la
  recomendación es que únicamente el Super Admin lo haga.
- Confirmar si los moderadores responderán con su nombre o con una identidad
  colectiva. Se recomienda mostrar nombre y rol para asegurar responsabilidad.
- Definir duraciones predeterminadas de silencios y suspensiones.
- Aprobar reglas comunitarias y proceso de apelación antes de habilitar medidas.
- Decidir si la primera función nueva será Reportar o Solicitar colaboración;
  por seguridad se recomienda Reportar primero y colaboración inmediatamente
  después.

## Arquitectura ampliada de identidad, logros y colaboración — 2026-08-26

### Hallazgos de la revisión

- La navegación `Quiero:` y el selector idéntico dentro del formulario duplican
  una misma decisión. Además, los enlaces de filtro vuelven al inicio de la
  página en lugar de llevar a la zona de creación.
- La campana actual cuenta mensajes del equipo; no existe aún un centro general
  para respuestas, reacciones, invitaciones, ascensos o logros.
- `Pasaporte` ya contiene rango, KE, KR, beneficios, insignias y sellos, mientras
  el perfil público vuelve a mostrar parte de esa información. La separación
  conceptual debe ser `Perfil` para identidad y trabajo, y `Logros` para
  progreso e historial.
- El panel permite crear Administradores y roles operativos, designar Super
  Admin y ajustar pantallas por usuario. No existen todavía Moderador ni
  Consejo como roles administrativos, ni permisos por acción.
- Consejo sí existe como consulta participativa a creadores, pero esto no debe
  confundirse con autoridad administrativa o insignia pública.

### Decisiones de producto recomendadas

1. **Un solo selector `Quiero`:** las pestañas filtran el foro y configuran el
   formulario. La pestaña activa muestra un CTA `Crear una publicación` que
   lleva a `#publicar`; se elimina el segundo grupo de cuatro botones.
2. **Antispam configurable:** 15 minutos entre publicaciones y máximo seis en
   24 horas como valores iniciales; 20 segundos entre respuestas y límites
   diarios separados. El servidor muestra el tiempo restante. El panel puede
   cambiar estos valores sin editar código.
3. **Actividad reciente rotativa:** una sola actividad visible, cambio cada
   cuatro segundos, pausa al tocar o enfocar y respeto a `prefers-reduced-motion`.
   Los elementos permanecen en el HTML para accesibilidad y como respaldo sin
   JavaScript.
4. **Centro de notificaciones independiente:** la campana suma eventos de
   comunidad, colaboraciones, misiones, rango, KE y logros; Mensajes continúa
   como conversación privada con el equipo. Cada aviso tiene tipo, actor,
   destino, enlace, fecha de lectura y clave de deduplicación.
5. **Pasaporte pasa a llamarse Logros:** conserva historial, tarjetas,
   beneficios, rangos y contenido compartible. Perfil queda libre para bio,
   trabajo, territorio, especialidad, redes autorizadas, colaboraciones y
   logros resumidos; los logros completos de otra persona viven dentro de su
   mismo perfil público.
6. **Jerarquía visual de éxitos:** Etapa, Rango, KE e Insignias no serán cuatro
   chips idénticos. Etapa es una credencial; Rango es la pieza principal con
   progreso; KE es un contador; Insignias es una colección. Comparten lenguaje
   visual sin competir por el mismo color o peso.

### Sistema de logros recomendado

No crear otro rango global `Creador Plata/Oro/Platino/Diamante`, porque
duplicaría Andariego, Trochero, Rutero y Baquiano y volvería ambiguo qué nivel
define al creador. Se recomienda llamar a la colección **Distinciones de
creador** y usar cinco categorías comprensibles:

- Creador de historias — misiones y calidad.
- Voz de la comunidad — aportes útiles.
- Constancia creadora — permanencia y actividad.
- Espíritu colaborador — ayudas y colaboraciones confirmadas.
- Explorador de rutas — participación y KR.

Cada distinción evoluciona en Bronce, Plata, Oro, Platino y Diamante. La
silueta aportada sirve como marco común de la tarjeta de La Caravana; dentro
cambian categoría, metal, número de nivel y progreso. Se evita depender de
Orquídea, Turpial, Araguaney, Arepa y Brújula en el nombre principal, aunque
pueden sobrevivir como detalles gráficos si la dirección decide conservar la
identidad venezolana.

Los hitos generan un aviso emocional —por ejemplo, `Tu voz ya mueve a otros
creadores`— y una imagen compartible con nombre de usuario, distinción, nivel,
fecha y marca de La Caravana. La tarjeta no contiene datos privados ni se
publica automáticamente.

### Nombre de usuario y verificaciones

- El creador elige una sola vez un nombre de usuario único, normalizado y
  reservado contra palabras oficiales o engañosas. Antes de confirmar se
  advierte claramente que no podrá cambiarse.
- Crear el nombre otorga KE una sola vez mediante el libro mayor idempotente.
- La marca sencilla confirma identidad de creador. Moderador, Consejo,
  Administrador y Super Admin son credenciales diferentes, con etiqueta y
  explicación accesible; el color por sí solo no comunica autoridad.
- Los roles internos del panel y las credenciales públicas permanecen
  separados. Si un integrante del equipo también tiene perfil creador, una
  vinculación explícita permite mostrar su credencial; no se infiere por nombre
  o correo.
- `Consejo` requiere una definición formal de membresía, duración y alcance;
  participar en una consulta no convierte automáticamente a alguien en miembro.

### Colaboraciones

- Desde un perfil se puede `Invitar a colaborar`, escribiendo propósito y
  misión opcional. El receptor recibe una notificación y puede ver al remitente,
  aceptar, rechazar, bloquear nuevas invitaciones o denunciar.
- Al aceptar aparece una sección privada `Mis colaboraciones` para ambos. En el
  perfil público puede mostrarse la relación, pero las redes no deben hacerse
  públicas automáticamente: se revelan entre las dos partes. Mostrar redes a
  toda la comunidad exige consentimiento separado y reversible.
- Una misión puede configurarse como individual, colaborativa opcional o
  colaborativa obligatoria. La pareja debe estar aceptada antes de entregar.
- Existe una sola entrega enlazada a ambos creadores. El total de KE configurado
  se reparte —50/50 por defecto— sin duplicarlo; ambos ven la entrega y reciben
  su historial. Cambiar integrantes después de enviar exige revisión del equipo.

### Administración propuesta

Agrupar las herramientas en pantallas existentes, mediante pestañas:

- `Usuarios` → Equipo, roles y vinculación con perfil creador.
- `Permisos` → capacidades por acción, plantillas de Moderador, Consejo,
  Administrador y Super Admin.
- `Comunidad` → Conversaciones, Reportes, Medidas y Límites antiabuso.
- `Portal del creador` → Notificaciones, Identidad pública y Privacidad.
- `Rangos` → Rangos, Distinciones, KE y Tarjetas compartibles.
- `Misiones del Portal` → Datos, Programación y Colaboración.
- `Creadores` → Perfil, Acceso, Credenciales y Colaboraciones del creador.

Cada pestaña incluirá una ayuda breve `Qué controla`, ejemplos y consecuencias
antes de guardar. Las operaciones sensibles tendrán vista previa, confirmación,
auditoría y reversión cuando sea posible.

### Secuencia de construcción

1. Corregir navegación duplicada, anclas, actividad rotativa y límites de
   publicación.
2. Crear centro de notificaciones y conectar respuestas/reacciones.
3. Renombrar Pasaporte a Logros, liberar Perfil y establecer la nueva jerarquía
   visual sin cambiar aún los cálculos.
4. Crear nombre de usuario inmutable, KE idempotente y credenciales públicas.
5. Implementar roles Moderador y Consejo con capacidades por acción y manuales
   contextuales.
6. Incorporar invitaciones, privacidad y directorio selecto de colaboraciones.
7. Añadir misiones colaborativas y reparto de KE.
8. Diseñar, probar y habilitar tarjetas compartibles de las Distinciones.

### Fase 1 implementada en local — 2026-08-26

- Se eliminó el selector duplicado dentro del formulario. Las pestañas `Quiero`
  ahora filtran, fijan la intención y aterrizan en `#publicar`; `Todo para mí`
  lleva al comienzo del foro.
- Publicar devuelve a `#foro`; responder y reaccionar devuelven a la
  publicación exacta. Los destinos incluyen margen para que la barra superior
  no tape el contenido.
- Comunidad incorpora `Límites de participación` dentro de la misma pantalla
  administrativa. Valores iniciales: 15 minutos entre publicaciones, seis en
  24 horas, 20 segundos entre respuestas y 60 respuestas en 24 horas.
- Los límites se comprueban en el servidor, no solo en la interfaz, y omiten la
  supervisión administrativa para que el equipo pueda atender el foro.
- Actividad reciente conserva hasta ocho registros en el HTML, pero muestra uno
  por vez y rota cada cuatro segundos. Tiene controles anterior/siguiente,
  pausa al interactuar y respeta la preferencia de movimiento reducido.
- La prueba sembrada creó una publicación temporal, rechazó la segunda por la
  espera configurada y eliminó después únicamente el registro de prueba.

### Refinamiento de filtros y Moderador — 2026-08-26

- El orden definitivo del selector es `Colaborar · Dar ideas · Ayuda ·
  Preguntar`. `Dar ideas` reemplaza visualmente a Encontrar; las publicaciones
  históricas `hallazgo` y `encontrar` se agrupan en el nuevo filtro sin migrar
  ni perder información.
- Cada opción es ahora una tarjeta-filtro con icono minimalista, propósito
  breve, contador y estado activo. El botón filtra el feed, configura el tipo
  del formulario y lleva a la zona de publicación.
- Se creó `Moderador de comunidad` como rol interno. Desde `Usuarios` el Super
  Admin o Administrador autorizado puede asignarlo; su acceso base se limita a
  Panel y Comunidad.
- El Moderador puede mover una publicación entre Colaborar, Dar ideas, Ayuda y
  Preguntar; también responder, ocultar/restaurar, fijar y resolver ayudas. No
  puede borrar definitivamente ni cambiar límites antiabuso.
- Mover conserva autor, contenido, respuestas, reacciones y fecha. La acción
  POST queda incluida en la auditoría transversal del panel con usuario,
  publicación y destino.
- El diseño usa cuatro columnas en escritorio, dos en pantallas medianas y una
  composición compacta por tarjeta por debajo de 390 px, sin depender del
  color para comunicar la intención.

### Fase 2 implementada en local — Centro de notificaciones — 2026-08-26

- La campana dejó de ser un acceso exclusivo a Mensajes y ahora abre un centro
  común. Su contador suma mensajes privados del equipo y eventos del Portal;
  Mensajes conserva su espacio y privacidad propios dentro del centro.
- Responder a una publicación y reaccionar con `Me sirve` o `Me inspira`
  generan avisos para el autor, nunca para quien interactúa con su propio
  contenido. Los avisos repetidos se agrupan por publicación y tipo, muestran
  la cantidad acumulada y conservan el actor más reciente.
- Cada aviso enlaza a la publicación exacta mediante su ancla, pertenece a un
  único creador y solo ese creador puede abrirlo o marcarlo como leído. También
  existe la acción `Marcar todo como leído` protegida con CSRF.
- La pantalla Comunidad del panel incorpora, sin sumar un nuevo ítem de menú,
  controles para activar o pausar avisos de respuestas y reacciones. Solo los
  administradores pueden cambiar estos ajustes; el Moderador mantiene sus
  capacidades operativas sin adquirir permisos de configuración.
- Se añadió el instalador idempotente `instalar-notificaciones-creador.php` al
  Actualizador. Crea únicamente la tabla aditiva de eventos y no escribe en
  `cv_creadores` ni `cv_applications`.
- La prueba funcional en el mundo sembrado confirmó el recorrido completo:
  reacción real, creación del aviso, contador en la campana, apertura del hilo
  exacto y cambio a leído. Después se eliminaron la reacción y el aviso de
  prueba, sin dejar datos artificiales.

### Fases 3 a 7 implementadas en local — 2026-08-26

- **Logros y Perfil:** `Pasaporte` pasa a ser `Logros`, conservando una
  redirección compatible para los enlaces antiguos. Rango, KE, KR e insignias
  tienen jerarquías visuales distintas. El Perfil público queda dedicado a la
  identidad, territorio, especialidad, privacidad y logros del creador.
- **Identidad pública:** el nombre de usuario vive en una tabla aditiva, es
  único, se confirma una sola vez y no se puede modificar. Su recompensa de 10
  KE usa una referencia idempotente. Las credenciales Creador, Moderador,
  Consejo, Administrador y Super Admin se comunican con texto además de color.
- **Colaboraciones:** se puede invitar desde un perfil indicando un propósito,
  revisar al remitente y aceptar o rechazar. Las redes siguen privadas hasta la
  aceptación, salvo consentimiento público separado. `Mis colaboraciones`
  reúne contactos aceptados.
- **Administración:** Consejo se incorpora como rol de gobernanza sin poderes
  de moderación. Moderador conserva Comunidad. Las suspensiones y bloqueos
  viven en tablas separadas, con motivo, reversión e historial auditado. Las
  credenciales públicas exigen vinculación explícita con el perfil creador.
- **Misiones colaborativas:** cada misión puede ser individual, colaborativa
  opcional u obligatoria. Existe una sola entrega con dos participantes; el KE
  configurado representa el total y se divide 50/50 sin duplicarse. La prueba
  temporal de 40 KE produjo exactamente 20 KE por creador y se limpió después.
- **Distinciones:** Historias, Comunidad, Constancia, Colaboración y Rutas
  evolucionan de Bronce a Diamante y no sustituyen los rangos. Sus nombres,
  explicaciones y umbrales se administran en `Rangos → Distinciones`.
- Cada Distinción desbloqueada queda en un historial, genera un mensaje
  emocional agrupado y ofrece una tarjeta SVG descargable y compartible basada
  en la silueta aportada. La tarjeta solo usa identidad pública.
- La arquitectura crea ocho tablas aditivas y no escribe en `cv_creadores` ni
  `cv_applications`. Las verificaciones cubrieron PHP 8.1, identidad inmutable,
  privacidad, invitación/aceptación, reparto de KE, SVG y layouts de 375 y 1440
  píxeles sin desplazamiento horizontal.

### Exploración del foro y Radar de ideas — 2026-08-27

- El foro dejó de depender de una lista fija de veinte publicaciones. Ahora
  muestra doce conversaciones inicialmente y permite cargar bloques sucesivos
  sin perder lo ya leído; el ancla aterriza en la primera conversación nueva.
- La búsqueda cubre título, texto, autor y contenido de respuestas visibles.
  Se combina con las cuatro intenciones y con los órdenes `Para ti`,
  `Recientes`, `Más activas` y `Sin respuesta`.
- `Para ti` mantiene primero las solicitudes de ayuda abiertas. `Más activas`
  usa respuestas y reacciones como señal; `Sin respuesta` permite descubrir
  conversaciones que todavía esperan participación.
- Comunidad incorpora `Radar de ideas` como puerta de descubrimiento. Cada
  tarjeta muestra estado, cantidad de propuestas, votos acumulados y votos que
  le quedan al creador. Proponer, votar y ver el resultado sigue ocurriendo en
  la misión oficial, evitando dos fuentes de verdad.
- Los cinco votos son por misión Radar, no un saldo global. La más votada recibe
  el reconocimiento de la comunidad; la decisión de producir una idea continúa
  en manos del equipo.
- La prueba local incluyó búsqueda en respuestas, cuatro órdenes, carga de 12 a
  14 publicaciones con continuidad de posición y una misión Radar temporal.
  Los tres registros temporales se retiraron al terminar. PHP 8.1 no reportó
  errores y la pantalla no tuvo desplazamiento horizontal a 375 px.

### Fusión de Distinciones e Insignias · Creador de ÉLITE — 2026-08-27

- Se retiró del Portal la segunda colección de Distinciones porque cuatro de
  sus cinco categorías repetían la misma métrica y el mismo recorrido de las
  insignias. Las tablas e historiales anteriores no se eliminan; simplemente
  dejan de actuar como sistema paralelo.
- Las insignias son la colección única: símbolo venezolano, progreso de Bronce
  a Diamante, bono KE, impulsos, tarjeta compartible e historial de ascensos.
- El historial visible se reconstruye desde los bonos `insignia_bono` del libro
  mayor, que ya conserva nivel, fecha y recompensa de manera idempotente.
- `Creador de ÉLITE` no es una insignia ni un rol manual. Se calcula cuando
  todas las insignias visibles alcanzan Diamante. Si se publica una insignia
  nueva, también deberá completarse para cumplir nuevamente la condición.
- La comunicación usa una pieza compacta `Camino a ÉLITE`: contador de
  diamantes, cinco señales visuales y una sola instrucción. Al completarse,
  cambia a una versión celebratoria, genera una notificación única, permite
  compartir el triunfo y aparece en el perfil público.
- El panel administrativo conserva un solo lugar de configuración:
  `Rangos → Insignias`. La antigua pestaña Distinciones redirige allí para no
  romper marcadores previos.
- La validación del perfil público descubrió y corrigió una dependencia previa:
  el Portal no cargaba `perfil-portal-creador.php`, por lo que un creador con
  redes visibles podía encontrar un error al normalizar sus enlaces. El helper
  se carga ahora desde el arranque compartido y el perfil vuelve a completar
  insignias y reconocimiento sin interrupciones.

### Bandeja unificada para equipo y colaboradores — 2026-08-27

- `Equipo La Caravana` dejó de usar la plantilla de chat antigua. Ahora comparte
  con las colaboraciones una misma estructura: listado de hilos, identidad del
  canal, cabecera contextual, burbujas, compositor y navegación móvil.
- El canal del equipo se distingue como oficial y privado. Los comunicados
  masivos conservan tratamiento visual propio para no confundirse con una
  respuesta personal.
- Los colaboradores aceptados aparecen en la misma lista con último mensaje,
  no leídos, credencial y acceso al perfil. El hilo comunica `Colaboración
  activa` sin fingir presencia en tiempo real.
- En móvil, la lista y la conversación funcionan como dos estados: al entrar a
  un hilo se oculta la lista y aparece un regreso explícito. El compositor queda
  al alcance sobre la navegación inferior, sin desplazamiento horizontal.
- Los hilos abren en el mensaje más reciente; el campo crece con el contenido y
  en escritorio Enter envía mientras Shift+Enter crea una nueva línea.
- Prueba local: canal del equipo con mensajes existentes y colaboración temporal
  con dos direcciones de mensaje. La colaboración, mensajes y lecturas de prueba
  se eliminaron al terminar. Sin errores PHP/JS ni desbordamiento a 375 px.
- Corrección móvil posterior: el botón Enviar heredó por error `grid-row:1/3`
  y ocupaba también la fila de emoticonos. Ahora es un control compacto de
  48×48 junto al campo; la etiqueta permanece accesible y en móvil se presenta
  solo la flecha. Validado a 375 px sin desbordamiento.
## Tarjetas sociales de logros y avatar del equipo — 2026-08-27

- Las cinco insignias generan tarjetas PNG de 1080 × 1350 con nombre, nivel y
  progreso del creador; comparten una firma visual basada en el isotipo.
- Élite no es una sexta insignia: su tarjeta aparece solo al completar todas en
  Diamante y siempre lleva el nombre dinámico del creador.
- El flujo prioriza `navigator.share()` con archivo y descarga el PNG como
  alternativa en navegadores sin soporte.
- La fotografía del equipo vive en `cv_usuarios.foto`, nunca en
  `cv_creadores`; se carga desde la gestión del usuario interno.
- Migración idempotente: `2026.08.27.02 · Fotografías de los usuarios internos`.

### Centro de Expedición y previsualización real de tarjetas — 2026-08-27

- El Portal no se bifurca en dos productos. Cuando la postulación activa del
  creador tiene estado `elegido`, Inicio incorpora automáticamente un `Centro
  de Expedición`; el resto de los creadores conserva su experiencia habitual.
- El centro reutiliza Misiones de Ruta y sus permisos existentes: resume
  pendientes, entregas en revisión, aprobadas, compañeros de edición y la
  próxima misión KR. Da acceso directo a Ruta KR, la edición y la mensajería.
- Una misión KR con fecha futura permanece visible como programación, pero su
  formulario queda bloqueado hasta la apertura y comunica una cuenta regresiva.
  Al vencer, mantiene el antecedente y comunica `Tiempo acabado` sin permitir
  nuevas entregas.
- Se resolvió la confusión entre el medallón compacto de progreso y la tarjeta
  social completa. `Ver y compartir tarjeta` abre ahora una previsualización
  grande de 1080 × 1350 dentro del Portal, con símbolo, metal y nombre del
  creador, antes de ofrecer Compartir o Descargar PNG.
- La tarjeta ÉLITE utiliza el mismo flujo de vista previa. Su nombre continúa
  siendo dinámico y solo se habilita cuando la colección completa alcanza
  Diamante.
- Validación local: PHP 8.1 y JavaScript sin errores; Inicio, Logros y Ruta KR
  respondieron por HTTP sin avisos; un Elegido vio el centro y un creador no
  elegido no lo recibió. La vista previa abrió el lienzo de 1080 px sin
  desbordamiento horizontal.
- Corrección de fidelidad final: se descartaron tanto el canvas simplificado
  como los recortes del tablero. Se produjeron 25 renders originales e
  independientes de 1254 × 1254 (Orquídea, Turpial, Araguaney, Arepa y
  Brújula × Bronce, Plata, Oro, Platino y Diamante), optimizados a WebP sin
  cambiar su resolución. La tarjeta ÉLITE conserva el arte aprobado y deja su
  placa disponible para el nombre dinámico.
- La composición social final presenta el render a sangre en 1080 × 1080, con
  placa inferior integrada y modal oscuro sin patrón cuadriculado. Se validó a
  375 px sin scroll horizontal.
- Reinicio visual acordado el 2026-08-28: cada una de las cinco insignias usa
  una única composición que evoluciona estrictamente en Bronce, Plata, Oro,
  Platino y Diamante. No se escribe el nombre del creador sobre estas piezas.
  La culminación es la tarjeta Creador de ÉLITE, desbloqueada al completar las
  cinco insignias en Diamante. No es una tarjeta de Elegido ni una sexta
  insignia. La propuesta de Elegido producida durante la exploración se descarta.
- ÉLITE no combina símbolos, gemas ni colores de las cinco insignias. Su lenguaje
  propio se simplifica a solo el isotipo sin ®, con acabado de diamante, borde
  dorado y el título `CREADOR DE ÉLITE`. No contiene placa, nombre, subtítulo ni
  símbolos adicionales. El concepto requiere aprobación antes de integrarse.

### Implementación definitiva de tarjetas y celebración — 2026-08-28

- Se integraron 25 tarjetas originales cuadradas de 1254 × 1254 y la pieza
  independiente de Creador de ÉLITE; el concepto ya no está pendiente.
- Ninguna tarjeta lleva el nombre del creador. El Portal muestra, comparte y
  descarga el WebP maestro sin redibujarlo ni estirarlo mediante canvas.
- El nuevo logro abre una celebración automática una vez por ascenso y creador,
  con halo, destello, partículas y entrada de la tarjeta. Puede repetirse desde
  `Ampliar y compartir` y respeta la preferencia de movimiento reducido.
- La progresión pública contiene exclusivamente Orquídea, Turpial, Araguaney,
  Arepa y Brújula. Las cinco en Diamante desbloquean ÉLITE; una fila adicional
  o histórica del catálogo no altera ese requisito.
- QA local: PHP 8.1 y JavaScript válidos, imagen natural 1254 × 1254, modal
  cuadrado, cero errores de consola y cero desbordamiento a 375 px.

### Identidad pública del equipo y experiencia sonora — 2026-08-28

- ÉLITE presenta candado, cinco hitos y barra `n/5`; al alcanzar 5/5 usa una
  celebración especial y una firma sonora ascendente.
- Bronce, Plata, Oro, Platino y Diamante tienen sonidos cortos diferenciados,
  generados con Web Audio. Fallar o bloquear el audio nunca impide el logro.
- El creador puede silenciar o reactivar los sonidos y el Portal conserva la
  preferencia. Movimiento reducido implica una experiencia silenciosa.
- Los usuarios internos poseen perfiles públicos separados de los creadores,
  con fotografía, biografía, redes, enlaces y logros configurables por el
  Super Admin. La asignación registra quién la realizó.
- El nombre del equipo en Comunidad enlaza su perfil si está publicado. El
  Moderador solo recibe las zonas Inicio y Comunidad del panel y participa sin
  suplantación mediante Modo Equipo.

## Implementado — 2026-08-28

- Creador de ÉLITE comunica el avance con candado, barra `n/5` y cinco hitos; se desbloquea al completar las cinco insignias en Diamante.
- Cada nivel tiene una firma sonora breve y ÉLITE una secuencia propia. El sonido es opcional, persistente y nunca bloquea la animación.
- Super Admin, Administradores, Consejo y Moderadores pueden tener perfil público independiente con fotografía, biografía, redes, enlaces y logros asignados.
- El Moderador continúa limitado a Inicio y Comunidad y participa mediante Modo Equipo, sin suplantar creadores.
## Ajuste visual del check verificado — 2026-08-29

- El check azul del creador verificado se hizo más luminoso y definido para conservar legibilidad junto al nombre en móvil.
- El ajuste afecta únicamente la credencial `creador`; las formas especiales de Moderador, Consejo, Administrador y Super Admin no cambian.
- La verificación sigue siendo manual: la mejora visual no altera quién recibe la credencial.
- Validación local: la regla compartida carga en el Portal, no aparecen errores PHP y el clon no muestra checks porque no posee verificaciones documentales aprobadas.
- Se usó una ficha QA local con el mismo HTML y CSS para comprobar el acabado sin modificar datos de creadores.

## Familia Destello Caravana — 2026-08-29

- Se aprobó e implementó una silueta de destello propia para todas las credenciales públicas del ecosistema.
- Creador verificado: destello azul y check blanco.
- Moderador: destello turquesa y escudo con check.
- Consejo: destello dorado y estrella blanca.
- Administrador: destello magenta y rombo con check.
- Super Admin: destello iridiscente oro–magenta–violeta y corona blanca, ligeramente mayor que los demás.
- La familia se implementó en la capa compartida del Portal, por lo que aparece junto al nombre en perfiles, comunidad, respuestas, rankings, colaboraciones, bandeja y Modo Equipo.
- El tooltip accesible continúa mostrando el significado al tocar o enfocar cada badge.
- Esta decisión es visual y no modifica la lógica de asignación ni el requisito de verificación manual para creadores.

### Corrección de contraste sobre el fondo real — 2026-08-29

- La primera versión del Destello de Super Admin resultó demasiado pequeña y opaca sobre las tarjetas azul oscuro del Portal.
- Se corrigió en la capa compartida: 21 px, corona blanca sólida y halo oro–magenta–violeta con borde de luz blanco.
- Las cinco credenciales se probaron juntas sobre el mismo azul y borde dorado de Comunidad, además de una vista móvil de 375 px sin desbordamiento horizontal.
- El criterio de aceptación para futuras credenciales incluye obligatoriamente probarlas en el fondo real y al tamaño final de uso, no únicamente en una lámina aislada.

### Estados aspiracionales del Inicio — 2026-08-29

- La bandera de estado evoluciona con el proceso: Convocado usa plata luminosa y Clasificado conserva el amarillo convertido en oro metálico.
- Ambos estados comparten estrella oscura, destello blanco y profundidad inspirada en las tarjetas de logros, sin confundirse con una insignia coleccionable.
- La tarjeta de estado tenía una etiqueta de cierre HTML incorrecta (`</section>` en lugar de `</a>`); se corrigió porque provocaba reconstrucciones y desplazamientos impredecibles en navegadores móviles.
- Bandera, texto, cifra y acceso permanecen dentro de una retícula flexible. La prueba aislada a 375 px confirmó las dos tarjetas sin desbordamiento horizontal.
- Elegido se presenta como una categoría claramente superior: esmalte azul cobalto, marco y texto dorados, brillo ceremonial y laureles reales formados por tallos curvos y hojas individuales alrededor de la estrella.
- La validación sobre un Elegido real del mundo local confirmó 10 hojas, símbolo y título dentro de la tarjeta, sin errores PHP ni desbordamiento a 360 px.

### Perfil público del fundador y continuidad de navegación — 2026-08-29

- El perfil público del equipo deja de presentar al fundador como un usuario administrativo genérico. La condición se deriva del rol real de Super Admin y se comunica mediante identidad, origen, responsabilidad, propósito, enlaces y reconocimientos.
- Para Diccionario Venezolano, el relato central expresa su condición de creador y fundador de La Caravana, su vínculo con Venezuela y su propósito de inspirar y abrir caminos a otros creadores.
- Los datos comprobables se separan del relato: cantidad de logros y enlaces se calculan desde el perfil; no se inventan métricas de impacto.
- Los logros se muestran como colección expansible y Creador de ÉLITE recibe tratamiento de distinción máxima.
- Un creador que visita el perfil conserva la cabecera y la navegación inferior compartidas del Portal. En Modo Equipo aparece una navegación móvil propia hacia Inicio, Comunidad y Mi perfil.
- QA local a 375 px: sin desplazamiento horizontal, navegación inferior visible y HTML sin avisos de PHP.
- Se generó `actualizacion-PORTAL.zip` para cPanel mediante el empaquetador diferencial; contiene el perfil y la capa compartida del Portal, sin configuraciones locales, SQL ni datos de creadores.

## Ruta oficial de cierre del Panel — temporada actual · 2026-08-29

Alejandro decidió cerrar el Panel del creador para esta temporada mediante seis pasos consecutivos:

1. Seguridad y moderación: reportes, permisos, advertencias, silencios, suspensiones, bloqueos, apelación e historial.
2. Perfiles del equipo y Consejo: identidad pública, facultades, duración, nombramiento y retiro.
3. Panel de Elegidos: misiones KR, agenda, logística, avisos, retroalimentación e historial de expedición.
4. Centro de actividad y trazabilidad: movimientos de KE/KR, verificaciones, colaboraciones, misiones, logros y medidas.
5. Métricas y salud de la comunidad: actividad, interacción, Radar, colaboraciones, reportes y tiempos de atención.
6. Pulido y cierre integral: responsive, accesibilidad, textos, rutas limpias, pruebas por rol, documentación y paquetes finales.

Esta ruta queda aprobada como compromiso de producto para el cierre de temporada. Su ejecución está pausada antes del paso 1 para resolver primero una preocupación todavía no especificada por Alejandro. No se deben iniciar estas seis fases ni ampliar su alcance hasta aclarar y resolver ese asunto previo.

### Incidente previo al cierre: Mi Bio / multilinks — 2026-08-29

- Producción mostró un error 500 al abrir `Recursos → Mi Bio`.
- La prueba local confirmó que la pantalla y el acceso para Clasificados y Elegidos funcionan. La brecha identificada es de despliegue y compatibilidad: el paquete acumulado del Portal podía activar `bio.php` antes de que el dominio principal recibiera `includes/bio-creador.php` y el instalador correspondiente.
- La detección anterior consideraba el sistema instalado con solo encontrar las dos tablas; no comprobaba que una instalación antigua tuviera todas las columnas requeridas.
- Se blindó el Portal para explicar un complemento ausente o una Bio pendiente de actualización sin caer en error genérico.
- `biosInstaladas()` valida ahora la estructura completa y el instalador repara columnas faltantes de forma aditiva e idempotente.
- QA: PHP 8.1 válido, dos corridas consecutivas sin duplicados, 2 perfiles y 5 enlaces conservados, editor y navegación cargados por HTTP con un Elegido real, sin escrituras sobre `cv_creadores` ni `cv_applications`.
- Para producción deberán viajar juntos Sitio y Portal y luego se debe pulsar `Reinstalar` en `Actualizaciones → Bios y multilinks de creadores`.

### Rediseño aprobado: multilink como extensión del perfil — 2026-08-29

- La Bio de un creador no será un segundo perfil. Foto, nombre, biografía, territorio, especialidad y redes se leerán siempre desde su perfil canónico.
- El creador únicamente personaliza el slug, abre la vista previa y copia o comparte su enlace. Los datos faltantes se completan desde Editar perfil.
- Instagram, TikTok y YouTube tendrán prioridad visual sobre los botones institucionales.
- Existirá una plantilla maestra administrada una sola vez desde Recursos. Toda edición de estructura, textos, orden y botones predeterminados se reflejará automáticamente en todos los multilinks de creadores, sin copiar filas ni editar cada Bio.
- Los botones institucionales podrán segmentarse por etapa: todos, Clasificados o Elegidos. `Vota por mí` resolverá dinámicamente la edición del Elegido; `Postúlate a La Caravana` utilizará la convocatoria vigente.
- Las únicas variaciones individuales serán los datos canónicos del perfil, el slug y los destinos dinámicos derivados de la edición o estado. Para el MVP no habrá excepciones visuales por creador.
- Las Bios institucionales de campañas, aliados o La Caravana conservarán edición completa separada; no se confunden con las Bios automáticas de creadores.

#### Implementación y QA local

- Se creó la tabla aditiva `cv_bio_botones_globales`. Cada regla tiene título, texto breve, función dinámica o URL, audiencia, orden, estado y comportamiento de apertura.
- Las reglas iniciales son `Vota por mí` para Elegidos y `Postúlate a La Caravana` para todos. El instalador es re-ejecutable y no duplica reglas, perfiles ni enlaces.
- `Mi Bio` quedó reducido a slug, guardar, abrir y copiar. Explica que la identidad se hereda y señala qué campos faltan con acceso a Editar perfil.
- La Bio pública ignora las copias históricas de identidad cuando pertenece a un creador: lee nombre artístico, foto, bio y redes directamente desde `cv_creadores`. Las filas anteriores se conservan por compatibilidad, sin mostrarse como una segunda verdad.
- Instagram, TikTok y YouTube se presentan como botones principales antes de la sección institucional. Votar solo aparece para Elegidos; Clasificados no lo reciben.
- El administrador dispone de `Recursos → Bios → Plantilla compartida`. Una edición real mediante POST confirmó el mensaje de propagación global.
- QA con un Elegido y un Clasificado: ambos heredaron `Postúlate`, solo el Elegido recibió `Vota por mí`; un cambio temporal de texto apareció simultáneamente en ambos y luego se restauró.
- QA móvil a 375 px: cero desbordamiento horizontal, redes antes de botones globales y página pública sin errores. La Bio institucional `/bio/caravana` conserva su editor y botones propios.
- No se escribieron filas de `cv_creadores` ni `cv_applications`. Para producción deben viajar Sitio y Portal juntos y luego reinstalar Bios.
### Entrega coordinada de multilinks — 2026-08-29

- Se generaron dos paquetes acumulativos coordinados para cPanel: uno para la raíz del sitio público y administrativo y otro para la raíz del Portal de creadores.
- Orden de despliegue: primero **SITIO**, después **PORTAL** y finalmente ejecutar en Administración → Actualizaciones el instalador **Bios y multilinks de creadores**.
- La entrega no requiere importar SQL ni contiene bases de datos, datos de creadores o configuración local.
- Los multilinks toman identidad, biografía y redes del perfil canónico; la plantilla global administrable añade enlaces comunes a todos y puede segmentar acciones para Elegidos.
### Criterio visual de multilinks — 2026-08-29

- La Bio pública prioriza exclusivamente Instagram, TikTok y YouTube; el sitio web personal deja de mostrarse para evitar dispersión.
- Cada red usa un icono reconocible y conserva prioridad sobre las acciones globales de La Caravana.
- La firma “Parte de la red de creadores de La Caravana” funciona como acceso a la portada institucional.
- La dirección visual combina azul profundo, dorado luminoso, bordes finos y destellos contenidos para comunicar pertenencia, exclusividad y aspiración sin perjudicar la lectura móvil.
### Entrega del multilinks premium — 2026-08-29

- Se generó el paquete acumulativo `La-Caravana-Multilinks-Premium-SITIO-2026-08-29.zip` para desplegar en la raíz de `lacaravanave.com`.
- Incluye la Bio pública con redes sociales, iconos y tratamiento aspiracional azul/dorado. No requiere actualizar el Portal ni importar SQL.
- Ajuste visual posterior: los iconos SVG de redes quedan centrados mediante la regla específica del contenedor y el botón Postúlate utiliza el isotipo vectorial de La Caravana, sin depender de una letra o imagen rasterizada.
### Paso 1 completado — Seguridad y moderación · 2026-08-29

- Se construyó un Centro de convivencia para que cada creador consulte medidas, duración, alcance, motivo e historial y pueda apelar una vez.
- Las publicaciones y respuestas visibles permiten reportes privados por categorías; reportar nunca oculta ni sanciona automáticamente.
- El panel incorpora `Comunidad → Reportes y medidas`, con cola, contexto, asignación, resolución, historial y apelaciones.
- Facultades: Moderador puede advertir y silenciar dentro de Comunidad; Administrador puede suspender o bloquear el Portal, revocar medidas y resolver apelaciones.
- Las decisiones generan notificaciones y permanecen trazables. Las tablas son aditivas y no escriben sobre creadores ni postulaciones.
- QA local: instalador ejecutado dos veces sin duplicación; reporte, silencio y apelación probados; Portal y centro administrativo sin errores PHP; auditoría de escrituras prohibidas en cero.

### Paso 2 completado — Perfiles del equipo y Consejo · 2026-08-29

- Los perfiles públicos de Super Admin, Administradores, Consejo y Moderadores incorporan una capa institucional: cargo público, facultades, fecha de nombramiento, vigencia y estado del mandato.
- El cargo visible y el permiso técnico permanecen separados. Editar cómo se presenta una responsabilidad no amplía el acceso real del usuario.
- Las facultades se escriben una por línea y se muestran como piezas breves en el Portal, para explicar el alcance sin acumular un texto difícil de leer.
- Los estados disponibles son En funciones, En pausa y Retirado. Un retiro puede conservar su motivo y no elimina la identidad ni la trayectoria pública.
- Cada nombramiento, actualización relevante o retiro deja un movimiento inmutable en `cv_equipo_nombramientos`, con cargo, rol, periodo, facultades, motivo, responsable administrativo y fecha de registro.
- La gestión continúa agrupada en `Usuarios → Gestionar → Perfil público en el Portal`; no se añadió una pantalla dispersa al menú.
- El instalador fue ampliado de forma aditiva y se ejecutó dos veces sobre el mundo sembrado sin duplicados. La prueba funcional guardó un nombramiento temporal, verificó historial y perfil público por HTTP y retiró luego todos sus datos de ensayo.
- No hubo escrituras sobre `cv_creadores` ni `cv_applications`. En producción el actualizador mostrará esta ampliación como pendiente mediante el centinela `perfiles_equipo.titulo_publico`.

### Paso 3 completado — Panel exclusivo de Elegidos · 2026-08-29

- Se consolidó el Centro de Expedición exclusivo para quienes tengan estado Elegido en la edición activa. Los demás estados son redirigidos y no acceden a la información operativa.
- El Inicio del Elegido conserva la misión KR prioritaria, resumen de pendientes, revisión y aprobaciones, y añade accesos directos a Agenda, Logística y Avisos.
- `Expedición` reúne cuatro pestañas: Agenda, Logística, Avisos e Historial. No reemplaza ni duplica `retos`: las entregas KR continúan en su sistema canónico.
- Agenda y Logística se publican mediante `cv_expedicion_eventos`; el equipo decide de forma explícita qué elemento es visible y qué información práctica compartir.
- Los avisos oficiales viven en `cv_expedicion_avisos`. Al publicar uno visible, la campana notifica únicamente a los Elegidos de esa edición y abre directamente la sección de Avisos; no se mezcla con la mensajería estática.
- El Historial combina entregas KR y contenido libre. Presenta estado, fecha, KR obtenido y las notas de retroalimentación que el equipo ya guarda al aprobar o rechazar.
- La administración se agrupó en una pestaña de `Misiones de Ruta · KR`, con formularios separados para programación operativa y avisos, sin crear un nuevo ítem del menú.
- QA local: instalador ejecutado dos veces; Agenda, Logística, Avisos e Historial respondieron por HTTP con un Elegido real y sin errores PHP; el POST administrativo fue probado y un aviso temporal generó exactamente 10 notificaciones para los 10 Elegidos. Todos los datos QA y la credencial temporal fueron retirados o restaurados.
- La implementación no escribe sobre `cv_creadores` ni `cv_applications`. Las tablas nuevas deben nacer vacías en producción.

### Paso 4 completado — Actividad y trazabilidad · 2026-08-29

- Se creó `Mi actividad`, una cronología personal que reúne KE, KR, misiones, verificación, colaboraciones, logros y medidas de convivencia.
- La cronología es una vista de solo lectura sobre las fuentes canónicas. No existe una tabla duplicada de eventos ni un proceso que copie información: cada movimiento conserva la fecha y estado de su registro de origen.
- Los filtros muestran conteos reales por categoría y la paginación conserva el filtro. La interfaz agrupa por fecha y presenta estado, razón o devolución, valor ganado y acceso al contexto cuando existe.
- La verificación se resume únicamente como proceso y estado. Nunca se consultan ni exponen número cifrado, últimos cuatro dígitos, tipo documental o archivo privado.
- El menú de cuenta incorpora `Mi actividad` y la tarjeta de actividad semanal del Inicio abre directamente esta cronología.
- El expediente administrativo muestra los últimos 20 movimientos trazables con identificador `tabla#id`, permitiendo investigar el origen sin editarlo desde esa vista.
- QA local: perfiles con 14, 8, 3 y 94 movimientos; fueron comprobadas todas las familias disponibles, filtros y segunda página. Portal y expediente administrativo respondieron sin avisos PHP y la credencial temporal de acceso quedó restaurada.
- Este paso no requiere migración y no escribe sobre creadores, postulaciones ni ninguna fuente consultada.

### Paso 5 completado — Métricas y salud de la comunidad · 2026-08-29

- Se creó `Comunidad → Salud`, una lectura agregada para Administradores y Moderadores con periodos comparables de 7, 30 y 90 días.
- La vista mide creadores activos, publicaciones, respuestas, reacciones, tiempo hasta la primera respuesta y proporción de ayudas resueltas. Cada creador activo se cuenta una sola vez aunque haya intervenido de varias maneras.
- Radar conserva su significado preciso: únicamente reúne propuestas y votos de misiones Radar o Concepto; nunca mezcla las ideas con publicaciones generales del foro.
- Colaboración muestra solicitudes, aceptaciones, proporción de aceptación, tiempo para decidir y cantidad de mensajes posteriores. No consulta ni expone el contenido de la mensajería privada.
- Convivencia muestra reportes nuevos, casos cerrados, tiempo de resolución y cola actual. La interfaz recuerda explícitamente que reportar abre una revisión y no constituye culpabilidad automática.
- Cada indicador se compara con el periodo inmediatamente anterior de la misma duración. Cuando no existe base anterior se comunica `Nuevo`, sin fabricar porcentajes infinitos.
- La sección `Lectura del momento` traduce reglas sencillas y visibles en prioridades operativas: conversación débil, ayudas abiertas, cola de convivencia o Radar sin propuestas. No utiliza puntuaciones ocultas, perfilado ni inteligencia artificial.
- La pantalla es de solo lectura y calcula las cifras desde las fuentes canónicas al abrirse; no crea tablas, eventos duplicados ni contadores persistentes.
- El Moderador puede abrir Conversaciones, Reportes y medidas y Salud, pero conserva su límite de acceso al resto del panel. Una recomendación de Radar lo dirige a Comunidad y no a la administración de misiones.
- QA local: los tres periodos respondieron por HTTP sin avisos PHP; a 30 días el mundo sembrado reflejó 10 creadores activos, 34 interacciones y 17,6 horas promedio hasta la primera respuesta. El acceso de Moderador fue probado con credencial temporal y luego restaurado.
- QA responsive a 375 px: KPIs y bloques en una columna, pestañas con desplazamiento contenido y ancho documental menor que el viewport, sin desplazamiento horizontal global.
- No se escribieron filas de `cv_creadores`, `cv_applications` ni de las fuentes métricas. El paso no requiere instalador.

### Paso 6 completado — Cierre integral del Panel · 2026-08-29

- Se cerró la ruta de seis pasos de la temporada con una auditoría transversal de sintaxis, navegación, permisos, accesibilidad, responsive, seguridad de datos y despliegue.
- Los 492 archivos PHP de los árboles de trabajo Sitio y Portal pasaron PHP 8.1 sin errores de sintaxis.
- El recorrido HTTP cubrió 13 pantallas con Convocado, Clasificado y Elegido: 39 cargas correctas, sin avisos PHP ni enlaces visibles terminados en `.php`. Expedición permaneció exclusiva del Elegido y los demás estados regresaron a Inicio.
- La URL antigua `inicio.php` redirigió a `/inicio`. El último destino JavaScript que todavía escribía `inicio.php` se normalizó a la ruta limpia.
- La matriz administrativa fue comprobada: Moderador accede a Conversaciones, Reportes y Salud y no a Usuarios/Misiones; Consejo accede a su laboratorio y no a Comunidad/Usuarios; Administrador accede a Salud, Usuarios y Misiones. Las credenciales y roles temporales fueron restaurados.
- La revisión visual a 375 px cubrió Inicio, Comunidad, Logros, Perfil, Bandeja, Actividad y Expedición. No hubo desbordamiento horizontal ni errores de consola; la navegación inferior permaneció fija.
- Se corrigió accesibilidad en los reportes de respuestas: motivo y contexto tienen nombres accesibles. El barrido final de Comunidad encontró cero imágenes sin `alt`, controles sin nombre, enlaces vacíos o campos sin etiqueta accesible.
- Se añadió una variante `.htaccess` exclusiva de producción para el Portal. Conserva HTTPS, URLs limpias, compresión, cabeceras de seguridad y caché de activos, pero nunca transporta el `noindex` de la zona local.
- El empaquetador oficial fue ampliado para incluir el instalador del Centro de Expedición y el `.htaccess` seguro del Portal. Sus cerrojos rechazaron configuraciones locales, SQL, logs, documentación, ZIP internos y escrituras prohibidas en instaladores.
- Se generaron paquetes coordinados: SITIO con 183 archivos y 790.726 bytes; PORTAL con 166 archivos y 10.298.396 bytes. Orden obligatorio: Sitio, Portal y luego Actualizaciones pendientes de arriba hacia abajo.
- SHA-256 SITIO: `348A00E1EB6E97B529399DC59B9F928ACA026FE6B7554BB71B07F28134461E63`.
- SHA-256 PORTAL: `E417B5B6B9E35D975BA0CE950D7C762A3774B2B71C5FC4FFA33A5D95E64C663F`.
- No se transportan creadores, postulaciones, agenda local, avisos, reportes, medidas, credenciales ni datos de ensayo.
### Paquete maestro de cierre · 2026-08-29

- Se generó `La-Caravana-Cierre-Panel-MAESTRO-2026-08-29.zip` como contenedor de entrega.
- Incluye los paquetes coordinados de SITIO y PORTAL, el manifiesto de archivos y la guía de instalación.
- El ZIP maestro no se extrae directamente en un dominio: SITIO va a `lacaravanave.com` y PORTAL a `creadores.lacaravanave.com`.
- Orden de despliegue: respaldo, SITIO, PORTAL y luego Actualizaciones del panel de arriba hacia abajo.
- SHA-256 del maestro: `F44DF953C3534DE89420D3128302D02C57DC19EE2CF2C12E402669307E2ADB2F`.

#### Incidente 404 y corrección del paquete

- Tras la primera instalación, Usuarios, Actualizaciones e Instalar sistemas devolvieron 404: el menú nuevo estaba presente, pero no todas sus puertas habían quedado disponibles en el hosting.
- El empaquetador ahora fuerza como unidad reparable `usuarios.php`, `actualizaciones.php`, `actualizador.php`, la navegación, la guardia y la consolidación, aunque alguno coincida con el espejo usado para calcular diferencias.
- Se generó `REPARACION-404-PANEL-2026-08-29.zip` para extraer exclusivamente en la raíz de `lacaravanave.com`.
- Se regeneró el ZIP maestro. Nuevo SHA-256: `03FF4518D2619F19FCC4E28C0C2DF98FC64AF43FBFCAF1B32E0D0177C597628B`.

#### Segunda causa observada: URLs del panel sin extensión

- En producción se observó que las tres direcciones terminaban como `/admin/usuarios`, `/admin/actualizaciones` y `/admin/actualizador`, aunque el menú fuente conserva `.php`.
- Se añadió una regla interna en `admin/.htaccess` que acepta ambas formas y solo resuelve archivos PHP existentes dentro del panel.
- El reparador específico es `REPARACION-RUTAS-PANEL-2026-08-29.zip`; al instalarlo hay que comprobar en cPanel, con archivos ocultos visibles, que `admin/.htaccess` exista.
- El maestro se regeneró otra vez con SHA-256 `4D6C977BD83356D5E5FAEE3C41B58F6D06C099519FE935FABA70CF66960B46AE`.
### Rediseño de la experiencia de moderación · 2026-08-29

- Comunidad se ordenó como un recorrido visible de tres pasos: Conversaciones, Reportes y medidas, y Salud.
- Reportes dejó de desplegar formularios dentro de cada tarjeta: ahora usa una cola estable a la izquierda y un solo expediente activo a la derecha.
- El expediente separa contexto, evidencia, asignación y dos cierres excluyentes: aplicar medida o cerrar sin medida.
- Apelaciones e historial quedaron como áreas secundarias posteriores, evitando que compitan con la decisión actual.
- En móvil, seleccionar una conversación lleva deliberadamente al detalle y ofrece regreso explícito a la lista.
- En Mensajes del equipo, el nombre y avatar del creador enlazan a su expediente; también lo hace su nombre dentro de la conversación.
- No se generó paquete de producción en este punto; primero corresponde revisar la experiencia completa y luego empaquetar cuando el director lo pida.

#### Paquete de instalación

- A solicitud del director se generó `ACTUALIZACION-UX-MODERACION-2026-08-29.zip` para instalar exclusivamente en la raíz de `lacaravanave.com`.
- Contiene cinco archivos: Comunidad, Reportes y medidas, Salud, Mensajes y la capa compartida `admin.css`.
- SHA-256: `495BB933FB662F147D8C902EF80122A6C27AE8E70CA667F272732A359C2A1618`.
### Visibilidad de reportes en el Portal · 2026-08-29

- La captura de producción confirmó que Reportar publicación no estaba visible, aunque la rama de trabajo contenía el formulario mezclado en la fila de reacciones.
- Se movió a un menú de tres puntos en el encabezado de cada publicación ajena; abre una hoja estable en móvil y un panel contextual en escritorio.
- No aparece en publicaciones propias ni en comunicaciones oficiales del equipo.
- Se generó `ACTUALIZACION-REPORTAR-POST-PORTAL-2026-08-29.zip` para instalar exclusivamente en `creadores.lacaravanave.com`.
- SHA-256: `15695D0EF4D896DB223F2708CB674EF05C8945028F75749A322F560BB4FB2186`.
### Corrección de ficha desde Mensajes · 2026-08-29

- Los enlaces del nombre y avatar en Mensajes abrían el expediente administrativo, pero el objetivo correcto es la ficha completa de Cantera y perfiles.
- Ahora abren `creadores.php?ver=<id>` en una pestaña nueva para conservar la conversación.
- Se generó `CORRECCION-FICHA-MENSAJES-2026-08-29.zip` para `lacaravanave.com`.
- SHA-256: `A7179EEBA056490EA15BFCBB804CD046B42DF20B3D5FF1776B06DCD57DA3CB90`.

### Corrección de iconos en Herramientas del Portal · 2026-08-29

- Los iconos quedaban en la esquina superior izquierda porque una regla heredada para el último `span` de las tarjetas reemplazaba el `display:grid` del contenedor.
- Se acotó esa regla para excluir `.pn-rec-ico` y `.pn-rec-badge`, conservando el centrado compartido horizontal y vertical en todas las herramientas y en móvil.
- Se empaquetó `ACTUALIZACION-FICHA-E-ICONOS-2026-08-29.zip` como contenedor de dos paquetes separados por dominio: sitio administrativo y Portal de creadores.
- SHA-256 del paquete maestro: `840F4D15778C1F60A6E0EB2109B4CC966E9CBBCACC1DA4620ED22D45F4F064CF`.

### Reporte visible y marco público de Elegido · 2026-08-29

- La acción para reportar publicaciones ajenas dejó de depender del menú ambiguo de tres puntos: ahora muestra bandera y texto `Reportar` en el encabezado. El formulario sigue siendo privado y no aparece en publicaciones propias ni avisos oficiales.
- Los creadores con `applications.status = elegido` reciben un retrato ceremonial compartido: aro azul, marco de oro, laureles, estrella, destello y placa `ELEGIDO`.
- El marco se muestra en publicaciones de Comunidad, la vitrina de elegidos, el perfil público visto por compañeros y el perfil propio; es SVG/CSS escalable y respeta movimiento reducido.
- Se probó por HTTP en Comunidad, Perfil y perfil público sin warnings, deprecations ni errores fatales. Aún no se generó paquete de producción.
- A solicitud del director se generó `ACTUALIZACION-PORTAL-REPORTES-Y-ELEGIDOS-2026-08-29.zip` para extraer exclusivamente en la raíz de `creadores.lacaravanave.com`.
- El paquete contiene siete archivos del Portal y excluye `arranque.php`, configuración local, SQL y escrituras sobre `creadores` o `applications`.
- SHA-256: `EB5D20FBC9C3FB2D5BC14F3177588A48B586FEA3429D1CF3F9D6F67AD288F467`.

### Ficha contextual dentro de Mensajes · 2026-08-29

- Se retiró la apertura de la ficha en otra pestaña, porque interrumpía la conversación y cambiaba el contexto de trabajo.
- Nombre, avatar y enlace `Abrir ficha completa` abren ahora un popup sobre Mensajes con la ficha real de `creadores.php` dentro de un marco aislado.
- El popup conserva los formularios y edición reales de la ficha, permite cerrar desde la equis, el fondo, el cierre interior o Escape, y devuelve el foco al enlace original.
- Escape prioriza cerrar la ficha antes que cerrar el hilo de mensajes. En móvil el popup ocupa la pantalla completa sin perder la conversación que queda debajo.
- PHP 8.1 y balance de CSS verificados; todavía no se empaquetó para producción.

### Ajuste de Reportar junto a reacciones · 2026-08-29

- `Reportar` se movió desde el encabezado de la publicación a la fila de acciones, inmediatamente después de `Me inspira`.
- Se corrigió una colisión CSS: la regla general de formularios de reacción reemplazaba el layout del formulario de reporte.
- El reporte abre ahora una ventana privada centrada en escritorio y una hoja inferior en móvil, con cierre propio y todos los motivos disponibles.
- La salida HTTP confirmó el orden `Me inspira` → `Reportar` → formulario, sin warnings ni errores fatales. Pendiente empaquetar esta corrección cuando lo solicite el director.

### Aportes publicados en el perfil del creador · 2026-08-29

- El perfil público de cada creador muestra el total real de publicaciones visibles y un botón `Ver sus aportes`.
- La vista incluye exclusivamente filas de `comunidad_publicaciones`; no consulta ni mezcla comentarios del creador.
- Cada aporte conserva tipo, fecha relativa, título, texto completo, reacciones, respuestas y acceso a la publicación en Comunidad.
- Se pagina de 12 en 12 para mantener un perfil ágil incluso con historiales extensos y adapta la rejilla a una columna en móvil.
- Se validó por HTTP con un perfil que tenía aportes reales: conteo, botón y listado presentes, sin warnings ni errores fatales. Pendiente empaquetar.

### Paquete consolidado de Comunidad y Mensajes · 2026-08-29

- Se generó `ACTUALIZACION-COMUNIDAD-Y-MENSAJES-2026-08-29.zip` con dos paquetes internos separados por dominio.
- `01-SITIO-MENSAJES-FICHA-POPUP.zip` instala la ficha contextual dentro de Mensajes en `lacaravanave.com`.
- `02-PORTAL-COMUNIDAD-PERFILES.zip` instala Reportar funcional, marco de Elegido y aportes del perfil en `creadores.lacaravanave.com`.
- La auditoría excluyó configuración local, SQL y escrituras sobre `creadores` o `applications`; todos los PHP pasaron lint 8.1 y las tres hojas CSS conservaron llaves balanceadas.
- SHA-256 maestro: `5A36B8757895FA7EBCFEE98468533BEE2A0842429B4D47D5A1E3C2F02EF3E5DC`.

### Refinamiento del marco Elegido y recuperación de Mi Bio · 2026-08-29

- El marco de Elegido tenía el SVG ornamental por encima de la foto; los laureles invadían el rostro y la pieza parecía una calcomanía.
- Se reordenaron las capas: aro ceremonial y laureles detrás, fotografía por delante, placa y destello en primer plano. El retrato deja más aire y suma doble aro azul/oro.
- `bio.php` ahora captura incompatibilidades del motor compartido, verifica que existan sus funciones y reemplaza el error genérico por una recuperación segura.
- `includes/bio-creador.php` amplía la detección de fallos de instalación a cualquier `Throwable`, dejando registro técnico sin exponer datos al creador.
- Bio, perfil propio y perfil de compañero se probaron por HTTP sin warnings ni errores fatales. El siguiente paquete debe actualizar Portal y sitio principal a la vez.
### Paquete acumulativo de perfiles y Mi Bio · 2026-08-29

- Se generó `ACTUALIZACION-ACUMULATIVA-PERFILES-BIO-2026-08-29.zip` con dos instalables separados: sitio principal y Portal de creadores.
- Incluye el marco ELEGIDO refinado, aportes públicos, reporte de publicaciones, ficha emergente desde Mensajes, corrección de iconos de Recursos y recuperación compatible de Mi Bio.
- No requiere instalador ni contiene datos, SQL o configuración local. Auditoría PHP 8.1 y escrituras sobre `cv_creadores`/`cv_applications`: correcta.
- SHA-256 maestro: `90BB2BA9AE4D7F2785A68387E18D2B00F8098D0E1E91D6C050EE7F9FEE9EC665`.
### Mi Bio simplificada y reporte móvil · 2026-08-29

- Mi Bio queda disponible para todo creador con acceso al Portal. No duplica el perfil: foto, nombre, biografía y redes se leen del perfil canónico; el creador solo cambia el `slug`, ve el enlace y lo copia.
- Los botones institucionales siguen siendo globales y administrados una sola vez para todos los multilinks.
- Se detectó una instalación parcial: `instalar-bios.php` reparaba perfiles y enlaces antiguos, pero no las columnas de `bio_botones_globales`. Ahora repara las tres tablas sin borrar ni sobrescribir configuraciones, y el Actualizador usa `bio_botones_globales.nueva_ventana` como centinela real.
- El formulario Reportar se rehízo como hoja inferior móvil contenida. Verificado a 375 × 812: ancho 351 px, sin desbordamiento horizontal y por encima de la navegación inferior.
### Aportes del perfil en escritorio · 2026-08-29

- La cabecera de Aportes se cambió de flex a una cuadrícula estable: resumen flexible y acción de ancho contenido. La regla global `pn-btn-linea { width:100% }` comprimía el texto del resumen en escritorio.
- Se conserva el apilado móvil. Verificado en 1280 px (texto 832 px, botón 148 px) y 375 px (ambos 302 px), sin desbordamiento horizontal.
### Entrega acumulativa corregida · 2026-08-29

- ZIP maestro: `ACTUALIZACION-CORRECCIONES-PERFILES-BIO-REPORTES-2026-08-29.zip`.
- Contiene dos raíces separadas: sitio principal (5 archivos) y Portal (9 archivos), más instrucciones.
- Después de subir ambos hijos se debe ejecutar o reinstalar una vez “Bios y multilinks de creadores” desde Actualizaciones.
- Auditoría: PHP 8.1 limpio, CSS equilibrado, sin SQL/config local y sin escrituras sobre `cv_creadores` o `cv_applications`.
- SHA-256 maestro: `DE3844CD06262393D595FE552A859A066B962880F23828FE198A43B4CDFB5298`.
### Regreso desde Mi Bio y reporte en capa superior · 2026-08-29

- “Ver mi Bio” añade una señal temporal de procedencia y la página pública muestra “Volver a mi panel”; el enlace que el creador copia o comparte permanece público y limpio.
- El destino de regreso se toma de `URL_PORTAL_CREADORES`, sin aceptar URLs aportadas por el visitante.
- El reporte de publicaciones dejó de depender del flujo y de las capas internas de cada post. Ahora usa un `dialog` modal nativo, centrado en la capa superior del navegador, con fondo atenuado, cierre explícito, cierre al pulsar fuera y adaptación a móvil.
- PHP 8.1 y `git diff --check` pasaron sin errores. La prueba HTTP local quedó impedida por el Apache local, que no permaneció escuchando tras advertencias de extensiones PHP; no es un defecto de estas pantallas.
### Paquete de regreso desde Bio y reporte modal · 2026-08-29

- ZIP maestro: `ACTUALIZACION-BIO-REGRESO-Y-REPORTE-MODAL-2026-08-29.zip`.
- Contiene `01-SITIO-REGRESO-DESDE-BIO-2026-08-29.zip` y `02-PORTAL-BIO-Y-REPORTE-MODAL-2026-08-29.zip`; deben instalarse en sus dominios respectivos.
- No requiere instalador ni cambios de base de datos. Auditoría sin SQL, configuración local, datos ni escrituras sobre `cv_creadores` o `cv_applications`.
- SHA-256 maestro: `A92FC045D23AA05BB872ED3AB9B913F073FC01017A40E4C18A6AA3A38DAC0D9F`.
### Incidente de dominios cruzados y ruta canónica de Mi Bio · 2026-08-29

- Producción mostró pantallas del Portal tanto en `lacaravanave.com` como en `creadores.lacaravanave.com`, mientras `/bio/` fallaba solo en el subdominio. La evidencia corresponde a una instalación cruzada: archivos del Portal quedaron también en el sitio principal y una carpeta pública `bio/` puede estar interceptando el gestor en el Portal.
- El gestor del creador pasa a la ruta inequívoca `/mi-bio`; Recursos ya enlaza allí. `/bio` y `/bio/` en el Portal redirigen temporalmente a `/mi-bio`, incluso si existe una carpeta física con ese nombre.
- `includes/arranque.php` incorpora un cerrojo previo a sesiones y datos: si una pantalla del Portal se ejecuta bajo `lacaravanave.com` o `www`, redirige al mismo camino en `creadores.lacaravanave.com`. Los POST erróneos usan 303 para no retransmitir formularios entre dominios.
- Pendiente operativo: instalar el próximo parche en ambos destinos indicados y retirar del sitio principal los archivos exclusivos del Portal después de inventariarlos; no borrar a ciegas porque algunos nombres pueden coincidir con páginas públicas.
### ZIP de recuperación de dominios · 2026-08-29

- ZIP maestro: `RECUPERACION-DOMINIOS-MI-BIO-Y-REPORTES-2026-08-29.zip`.
- Contiene instrucciones y dos paquetes inequívocos: `01-SITIO-PRINCIPAL-PROTECCION-Y-BIO.zip` para `lacaravanave.com` y `02-PORTAL-CREADORES-RUTAS-Y-MODAL.zip` para `creadores.lacaravanave.com`.
- El Portal incluye `.htaccess`, por lo que cPanel debe mostrar y sobrescribir archivos ocultos. No requiere instalador ni base de datos.
- Auditoría: 0 entradas prohibidas, 0 escrituras nuevas sobre `cv_creadores` o `cv_applications`, PHP 8.1 limpio y CSS con llaves equilibradas.
- SHA-256 maestro: `B59F7AFAAF805DC2AFE4B4D1E7D1591E952FF857DC6B70F7F29CA8B752351E59`.
### Recuperación Bio V2: slug bajo PATH_INFO · 2026-08-29

- La captura de `/bio/juan-perez` con “Bio no encontrada” confirmó que cPanel ejecutaba `bio/index.php` mediante `PATH_INFO`, pero el código solo leía `$_GET['slug']`.
- La Bio ahora resuelve el identificador por `GET`, `PATH_INFO` o `REQUEST_URI`; una prueba CLI sobre `/bio/alejandro-bio-prueba` cargó correctamente “Alejandro Martínez”.
- `asegurarBioCreador()` repara de forma idempotente una Bio existente con slug vacío o `publicado=0`; solo actualiza `cv_bio_perfiles`, nunca `cv_creadores` ni `cv_applications`.
- El sitio ahora recibe también `bio/.htaccess`, omitido en el paquete anterior.
- ZIP que sustituye al anterior: `RECUPERACION-DOMINIOS-MI-BIO-Y-REPORTES-V2-2026-08-29.zip`. SHA-256: `4352B001B939BE429F0AECEB85EC349045BCE84F0128098742B3105C1CA9D82C`.
### Bio canónica independiente de la tabla auxiliar · 2026-08-29

- La V2 seguía mostrando “Bio no encontrada”. El clon confirmó que un creador Clasificado puede existir correctamente aunque `cv_bio_perfiles` no exista o todavía no contenga su fila.
- La página pública intenta primero `bio_perfiles` y luego reconstruye la Bio desde `cv_creadores` cuando el creador está publicado o tiene una postulación Clasificado/Elegido. Es lectura canónica: no duplica perfiles ni expone postulantes no habilitados.
- La prueba reproducible sobre el clon sin `cv_bio_perfiles` cargó título, fotografía y datos reales del perfil solicitado, sin caer en “Bio no encontrada”.
- Parche exclusivo del sitio principal: `CORRECCION-BIO-CANONICA-LACARAVANAVE-2026-08-29.zip`. No se instala en el Portal y no requiere instalador. SHA-256: `EBF354B42283BF89C2D138C0AFAFE5CBC1FCAD65BF4203376BBC8278AECC4EC4`.
### Moderación alineada con las categorías del Portal · 2026-08-29

- La navegación antigua mezclaba 296 conversaciones visibles y ocultas: las cuatro categorías visibles sumaban 280 y las 16 ocultas completaban 296. “Todas” ahora significa únicamente visibles y “Ocultas” permanece separada.
- Conversaciones adopta las categorías canónicas Colaborar, Dar ideas, Ayuda y Preguntar, con los mismos colores e iconos del Portal. Añade estados Todas, Pendientes, Resueltas y Ocultas.
- Cualquier conversación puede marcarse resuelta o reabrirse; al trabajar desde Pendientes, la selección avanza al siguiente elemento disponible.
- La pantalla muestra una tarjeta persistente con el número de reportes abiertos y un badge en cada publicación reportada, enlazando directamente al expediente formal.
- Se detectó un centinela incompleto: Actualizaciones consideraba Supervisión instalada por `pases_supervision`, aunque podía faltar `comunidad_reportes`. El nuevo centinela es `comunidad_reportes.resolucion` y el instalador repara columnas faltantes de forma idempotente.
- Cada reporte devuelve ahora un número de caso. Los reintentos del mismo creador sobre el mismo contenido conservan un solo expediente abierto; publicaciones diferentes generan casos distintos.
### Salto exacto desde notificaciones a Comunidad · 2026-08-29

- Varias personas confirmaron que la campana podía abrir el muro general en vez de la conversación indicada. No era un problema de mouse: el destino guardaba únicamente `#publicacion-ID` y el navegador no encontraba el ancla cuando esa publicación quedaba fuera de las primeras 12 cargadas.
- Comunidad acepta ahora `ver=ID` y carga expresamente esa publicación visible antes de aplicar el ancla. El enlace ignora el filtro de categoría para que una selección anterior no esconda el destino.
- Las respuestas y reacciones nuevas generan el destino estructurado `comunidad.php?ver=ID#publicacion-ID`.
- `notificacion-abrir.php` reconstruye el mismo destino usando `entidad_tipo=publicacion` y `entidad_id`, de modo que también corrige las notificaciones antiguas sin modificar filas ni ejecutar una migración. Como respaldo, reconoce las URLs históricas que solo tenían el ancla.
- PHP 8.1 y `git diff --check` pasaron; no se añadieron escrituras sobre `cv_creadores` ni `cv_applications`. La prueba HTTP quedó impedida porque el Apache local no permaneció escuchando debido a advertencias de extensiones PHP del entorno.
- Parche exclusivo del Portal: `CORRECCION-NOTIFICACIONES-COMUNIDAD-2026-08-29.zip`. Se extrae en la raíz de `creadores.lacaravanave.com`, no requiere instalador ni cambios de datos. SHA-256: `0F6B47D695791D14D095EEF956402A2D65872403CDD687A46A1265C00181E996`.
### Centro de control del Modo Equipo · 2026-08-29

- El registro actual no es una cronología en tiempo real de toda la actividad del creador. `cv_operaciones_creadores` audita acciones sensibles del equipo —supervisión, accesos, etapas, identidad y moderación—, mientras publicaciones, respuestas, reacciones, entregas, notificaciones y accesos permanecen en sus tablas canónicas. El Panel muestra hasta 250 operaciones en Centro de creadores → Historial de medidas.
- El Modo Equipo dejó de tener un corte fijo de 60 publicaciones. Ahora pagina todo el histórico en bloques de 20 y muestra total histórico, visibles, pendientes, oficiales y ocultas.
- Se incorporó búsqueda por título, texto, autor y contenido de respuestas; filtros combinables por categoría, visibilidad, autoría y resolución; orden por actividad, participación o antigüedad.
- Cada publicación permite desplegar la conversación completa, incluidas respuestas ocultas identificadas. Desde la misma ficha el equipo puede responder, mover, resolver/reabrir, fijar/desfijar y ocultar/restaurar.
- Los filtros, la página y la posición se conservan después de cada acción administrativa. La redirección de retorno solo admite rutas internas del Modo Equipo.
- La interfaz usa la capa compartida `eq-` de `portal/assets/panel.css`, con adaptación a móvil. PHP 8.1, consultas locales, llaves CSS y `git diff --check` pasaron; no hay escrituras nuevas sobre `cv_creadores` ni `cv_applications`.
- Pendiente de decisión: construir una cronología operativa unificada de solo lectura para Moderación, compuesta desde las fuentes canónicas y sin duplicar los eventos de negocio.
- Paquete exclusivo del Portal: `MEJORA-MODO-EQUIPO-CENTRO-DE-CONTROL-2026-08-29.zip`. Se instala en la raíz de `creadores.lacaravanave.com`, no requiere instalador ni cambios de datos. SHA-256: `3294855130DFEBE6821064FB6BBF6F327196C3484D9B53E76BC1F6F28F637F1A`.
