---
tipo: identidad
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/PROYECTO-SISTEMA-COMERCIAL.md (2026-07-28)"
---

# La Caravana — Sistema Comercial de Propuestas y Página de Ruta Personalizada

Documento de contexto completo para trabajar este proyecto con cualquier asistente de IA.
Última actualización: 28 de julio de 2026.

---

## 1. Qué es La Caravana

Plataforma itinerante de contenido, cultura, turismo y entretenimiento que recorre Venezuela
a bordo de tres camionetas **Maxus D90**. Un ecosistema en movimiento de creadores de contenido,
narradores y talentos regionales. Lema: *"Venezuela sobre ruedas, contada por quienes la viven"*.

- Proyecto de **Diccionario Venezolano** (@diccionariovzla), producido por **Groove Creative Studio**
  y dirigido por **Caracas la Ciudad** (@caracaslaciudad).
- Cada edición: **15 días de recorrido**, **10 creadores seleccionados por convocatoria**,
  retos creativos diarios, integración orgánica de marcas patrocinantes.
- Alcance combinado (mediakit 2026):
  - Instagram @diccionariovzla: 1,5M seguidores · 12M visualizaciones · 2M cuentas alcanzadas
  - Instagram @caracaslaciudad: 293K seguidores · 7,7M visualizaciones · 800K cuentas alcanzadas
  - TikTok @diccionariovzla: 1,9M seguidores · 12,2M visualizaciones · 2,3M cuentas alcanzadas
  - TikTok @caracaslaciudad: 456,8K seguidores · 6,3M visualizaciones · 92,3K cuentas alcanzadas

### Las ediciones (rutas planificadas)

| Edición | Territorios | Enfoque |
|---|---|---|
| Ciudades de Venezuela | Caracas, Maracay, Valencia | Cultura urbana, gastronomía, comercio, emprendimiento |
| Playas de Venezuela | Margarita, Mochima, Chichiriviche, Lechería | Caribe, turismo, comunidades costeras |
| Llanos Venezolanos | Guárico y más estados | Identidad llanera, música, faena, tradiciones |
| Andes Venezolanos | Táchira, Mérida, Trujillo | Montaña, agricultura, aventura, tradición andina |
| Edición Especial | Cualquier región | Diseñada alrededor de una marca, categoría o propósito |

---

## 2. La plataforma web existente

Sitio en **PHP 7.4+ y MySQL puro, sin Composer ni Node**, desplegado en cPanel (hosting compartido).
Estilo de código: procedural en español, funciones globales, PDO con prepared statements.

### Convenciones técnicas clave

- Acceso a datos: `bd()` (PDO singleton), `fila()`, `filas()`, `tabla('nombre')` (aplica prefijo).
- Escape de salida: `e()`. Sanitización de entrada: `limpiar($valor, $maxLen)`.
- Roles del panel (`admin/guardia.php`): admin, **comercial**, productor, jurado.
  Funciones: `esAdmin()`, `esComercial()`, `exigirSesion()`, `exigirAdmin()`, `permitidasPorRol()`.
- Enlaces sin contraseña por token: patrón ya usado en creadores
  (`token` + `token_expira` 30 días, regenerable desde el panel).
- Páginas públicas componibles por **bloques** (`bloques/*.php`) administrables desde el panel.
- Imágenes: GD garantizado (no Imagick). Subidas en `assets/subidas` (escribible, 755).
- Instaladores puntuales en la raíz: patrón `instalar-*.php` (idempotentes, se borran tras usar).
- CSS del panel: `admin/admin.css` (clases `.tabla`, `.rejilla`, `.panel-plegable`, `.pestanas`,
  `.estado`, `.b .b-1/.b-2/.b-3`, `.op-*` para la vista comercial).

### Navegación administrativa

El menú evita una lista plana de herramientas y se organiza por intención:

- **Resumen** — entrada y visión general.
- **Comercial** — oportunidades, propuestas VIP, rotulador, patrocinantes y bandeja.
- **Producción** — edición en vivo, ediciones, itinerario, creadores, retos, jurado y finanzas.
- **Contenido** — páginas, módulos, formularios, medios, menú público y apariencia.
- **Sistema** — usuarios, permisos, ajustes, integridad y salud.

Los grupos respetan `puedeVer()` y los permisos por usuario; solo aparecen cuando contienen
al menos una pantalla accesible.

### Módulos comerciales previos

- `admin/oportunidades.php` — embudo comercial (leads del formulario VIP `propuesta-vip`):
  nuevo → calificar → contactar → reunión → propuesta enviada → negociación → ganado.
- `admin/patrocinantes.php` — marcas confirmadas por edición, con logo claro/oscuro y
  zonas del vehículo (texto libre).
- `bloques/vehiculo.php` — SVG del camión con 8 zonas numeradas por coordenadas.

---

## 3. El mediakit comercial (precios oficiales 2026)

Tres modalidades, todas "+ IVA por pago en edición, a tasa € BCV del día":

| Modalidad | Precio | Entregables |
|---|---|---|
| 1. Integración dentro de una Edición | €5.000 | 2 reels @diccionariovzla + 4 reels @caracaslaciudad + reels impulsados de creadores. Integración orgánica: experiencias y retos, contenido y territorio, visibilidad física en el convoy |
| 2. Experiencia de Contenido | €12.000 | 4 + 6 reels + creadores. Campaña coordinada con continuidad narrativa, formatos de valor, contenido propio para canales de la marca |
| 3. Edición Especial de Marca | €20.000 | 7 + 9 reels + creadores. La marca como motor de la expedición: identidad y ruta propia, acciones en terreno, entregables documentales |

### Espacios publicitarios en la Maxus D90 (rotulado físico, add-on independiente)

| # | Zona | Nivel |
|---|---|---|
| 1 | Puertas delanteras | Reservado (logo La Caravana, no vendible) |
| 2 | Puertas traseras | Exponsor Premium |
| 3 | Capó | Exponsor Premium |
| 4 | Retrovisores | Exponsor |
| 5 | Guardafango delantero | Exponsor Medio |
| 6 | Guardafango trasero | Exponsor Medio |
| 7 | Vidrio maleta | Vinil microperforado |
| 8 | Vidrio maleta lateral | Vinil microperforado |
| — | Camioneta completa | Patrocinio exclusivo |

Los precios individuales por zona no están en el mediakit: se definen desde el panel.

---

## 4. Sistema de Propuestas (construido — Fase 1 operativa)

Generador de propuestas comerciales personalizadas por marca, dentro del panel,
sección **Comercial** (Oportunidades · Propuestas · Patrocinantes).

### Filosofía

- La propuesta es un **constructor de líneas** (como cotización de agencia): cada línea tiene
  nombre, descripción/especificaciones, cantidad, precio unitario y total (sobrescribible).
- Los catálogos (modalidades y espacios) son **plantillas rápidas**, no restricciones:
  precargan una línea que luego se edita libremente.
- Esto cubre: venta de 1 plan, cotización multi-plan, paquetes multi-edición con descuento
  negociado, y ofertas a medida para pymes — sin lógica especial por caso.

### Tablas (prefijo del sitio + nombre)

- `planes_patrocinio` — catálogo de modalidades (nombre, descripción, precio, moneda, nota, orden, activo).
- `espacios_vehiculo` — catálogo de zonas del vehículo (nombre, nivel, precio, vendible, orden, activo).
- `propuestas` — cabecera: marca, contacto (nombre/cargo/correo/teléfono), edición,
  logos positivo/negativo, moneda, descuento (porcentaje o monto), nota de condiciones,
  nota personalizada, vigencia, token + expiración, estado, y campos de plantilla visual
  (antetítulo, título, diagnóstico, idea central, objetivos, forma de pago, siguiente paso,
  plantilla editorial/cinematográfica/minimal, tono, color de acento, imagen de portada,
  toggles mostrar_inversion / mostrar_resultados / mostrar_mockups / mostrar_contacto).
- `propuesta_items` — líneas de precio.
- `propuesta_mockups` — imágenes de mockups diseñados a mano (ruta + etiqueta), subidas por Medios.
- `propuesta_eventos` — registro de vistas (fecha + hash de IP, dedup 30 min).
- `propuesta_metricas` — cifras de redes editables para la prueba social.
- `propuesta_historial` — bitácora de eventos de la propuesta.

### Estados de propuesta

borrador → en revisión interna → enviada → vista por la marca → cambios solicitados →
aceptada / no interesa / vencida.

### Flujo

1. Admin crea la propuesta (marca + edición), llena datos, logos, líneas de precio,
   mockups manuales del diseñador y nota personalizada.
2. Se genera link único `propuesta.php?t=<token>` (expira 30 días, regenerable).
3. La marca abre el link: página personalizada (saludo con su nombre, diagnóstico, idea central,
   objetivos, servicios con o sin precios, mockups, métricas de alcance, CTA WhatsApp) y puede
   **responder desde la página**: aceptar / pedir cambios / rechazar (con nombre y comentario).
4. Cada vista se registra; el panel muestra "vista el X, N veces" o "aún no vista".
5. Exportación a **.docx real** (`propuesta-docx.php`): OOXML mínimo generado con ZipArchive
   nativo (sin Composer), solo texto y tablas, para revisión interna de la marca.

### Archivos

- `instalar-propuestas.php` — instalador idempotente (sin transacción: CREATE TABLE hace commit implícito en MySQL).
- `includes/propuestas.php` — helpers: catálogos, totales, tokens, vistas, historial, respuesta.
- `admin/propuestas.php` — listado + constructor + catálogo editable.
- `admin/rotulador.php` — editor Canvas por capas para las vistas lateral y frontal de la
  Maxus D90. Permite cargar varios logos directamente sobre cada zona, reutilizarlos desde
  una biblioteca temporal, moverlos, escalarlos, rotarlos, deformarlos mediante cuatro
  esquinas, recortarlos dentro del espacio disponible y exportar el mockup final en PNG.
  Los retrovisores se editan exclusivamente desde la vista frontal: el ajuste se realiza
  sobre uno y se replica simétricamente en el otro para representar el par completo.
- `instalar-rotulador-propuestas.php` — migración idempotente para guardar composiciones
  por propuesta y controlar la disponibilidad de cada espacio publicitario por edición.

### Integración del rotulador con Propuestas

- El constructor incluye la etapa **Vehículos** entre Servicios e Inversión.
- Los logos subidos desde una propuesta se guardan en Medios; no dependen del navegador.
- La biblioteca, las capas y todas sus transformaciones se recuperan al volver al editor.
- Guardar una composición sincroniza las zonas seleccionadas como líneas de inversión.
- Cada precio corresponde a la aplicación idéntica en las tres camionetas.
- Una zona pasa por disponible → preseleccionada → reservada → confirmada según el estado
  de la propuesta. Rechazar, vencer o borrar una propuesta libera sus zonas.
- Las zonas ocupadas por otra propuesta no pueden seleccionarse.
- Las vistas lateral y frontal pueden guardarse como mockups dentro de la propuesta.
- La propuesta pública presenta los mockups en una galería cinematográfica a ancho completo:
  conserva la proporción panorámica de la vista lateral, evita recortes y adapta las vistas
  secundarias y frontales sin deformarlas.
- `propuesta.php` — página pública por token (plantillas visuales, responde la marca).
- `propuesta-docx.php` — exportador Word (usa `assets/subidas` como tmp: `sys_get_temp_dir()`
  suele estar fuera de open_basedir en cPanel).

### Lecciones técnicas aprendidas (errores ya resueltos)

- No envolver `CREATE TABLE` en `beginTransaction()` en MySQL → "There is no active transaction".
- `ZipArchive::open()` sobre `sys_get_temp_dir()` falla en hosting compartido → usar carpeta
  propia del sitio y verificar el código de retorno de `open()`.

---

## 5. Página de Ruta Personalizada (construida — pendiente de ejecutar el instalador en producción)

Página de venta emocional para la **Edición Especial de Marca** (la ruta 100% personalizada).
Objetivo único: que el gerente de marca se imagine su logo en la ruta antes de llegar al precio.

**Promesa central**: *"Durante una semana, Venezuela no habla de nosotros. Habla de ti."*

**Oferta narrada**: convoy rotulado con la identidad de la marca en las zonas contratadas +
7 días de conversación
nacional: 10 creadores + 2 reels @diccionariovzla + 4 reels @caracaslaciudad, cada día desde
un ángulo distinto (cultural, social, histórico, curioso).

### Reglas comerciales confirmadas

- La ruta comercial personalizada dura **7 días**. No se extiende a los 15 días de una edición
  regular porque la conversación de marca debe conservar ritmo y relevancia.
- La marca puede elegir cualquier ciudad, estado o combinación de estados.
- Puede contratar varias zonas publicitarias simultáneamente.
- El precio de cada zona cubre la rotulación idéntica en las **tres camionetas**.
- Pueden convivir varias marcas en un convoy. La propuesta VIP solo ofrece las zonas realmente
  disponibles y presenta únicamente la visualización correspondiente a la marca destinataria.
- La marca puede sugerir creadores y elegir entre convocatoria abierta o selección cerrada desde
  la base de creadores en espera. La aprobación final corresponde a La Caravana.
- La propuesta VIP pertenece a la última etapa del embudo: incluye precios, inversión, condiciones
  y vigencia desde el primer envío.
- Los colores y la identidad visual de cada marca se reservan para su propuesta VIP; no se aplican
  a la página pública general.

### Arquitectura editorial confirmada

- Existe una página pública maestra que explica la experiencia de una edición 100% para una marca.
- La misma estructura se reutiliza en modo VIP dentro de cada propuesta, con campos sustituidos por
  los datos, colores, ruta, creadores, mockups, inversión y condiciones de la marca.
- Las propuestas enviadas deben congelar una copia de su contenido para que cambios posteriores en
  la página pública no alteren silenciosamente una oferta ya presentada.

### Implementación local terminada

- URL pública: `pagina.php?p=edicion-personalizada`.
- Instalador idempotente: `instalar-pagina-edicion-personalizada.php`.
- Ocho módulos independientes en `Páginas → Edición personalizada`.
- Nuevo módulo reutilizable: `Experiencia de marca · portada interactiva`.
- Simulador público de nombre y color exclusivamente visual: no almacena datos ni sustituye al rotulador VIP.
- Los módulos `Edición especial · ruta personalizada` se conectan automáticamente con esta página.
- La página no se añade al menú principal; se abre desde las tarjetas de edición especial para mantener limpio el recorrido comercial.

### Estructura (6 secciones)

1. **Hero cinematográfico** — "UNA SEMANA. UNA RUTA. TU MARCA." + convoy vectorial en loop.
2. **Interactivo "Píntala de tu marca"** — el visitante escribe su marca y elige un color:
   la camioneta SVG se rotula en vivo, el mapa de ruta cambia de acento y el titular se
   reescribe con su nombre. Corazón de la página.
3. **Timeline de 7 días** — tarjetas animadas: llegada, lo histórico, lo social, lo cultural,
   el reto de creadores, lo curioso, el cierre.
4. **Aritmética del impacto** — contadores animados al scroll: 10 creadores · 2 + 4 reels ·
   7 días · 4M+ audiencia combinada. Sin precio (el precio va en la propuesta personalizada).
5. **Comparativa** — publicidad tradicional vs. ser la historia (reusa bloque existente).
6. **CTA doble** — "Quiero mi semana" → formulario VIP con campo oculto
   `oportunidad=ruta-personalizada` (cae en el embudo de Oportunidades) + WhatsApp.

Todo con recursos ya existentes (convoy SVG, mapa de Venezuela, comparativa, formulario VIP,
contadores `data-cuenta`), JS vanilla, cero librerías.

### Decisiones pendientes

- ¿Página independiente (`ruta-personalizada.php`, más libertad de animación) o bloque del
  sistema de Páginas (editable desde el panel)?
- ¿El interactivo permite subir logo (más impacto, más fricción) o solo nombre + color (fluido)?

---

## 6. Convención de nombres para ediciones de marca

La preposición comunica el nivel de inversión (escalera que vende sola):

| Nivel | Fórmula | Qué comunica |
|---|---|---|
| Integración en una edición (€5.000) | *Daka **en** La Caravana* | Está presente en el viaje |
| Experiencia de contenido (€12.000) | *La Caravana **con** Daka* | Viaja como aliado protagonista |
| Edición Especial (€20.000) | ***Edición Daka** de La Caravana* | La expedición existe por y para ella |

Reglas:
- Evitar "by" (anglicismo, choca con la identidad venezolana del proyecto).
- Evitar "La Caravana de [Marca]" como nombre de plataforma (diluye la marca propia;
  el posesivo se lo lleva la *edición*, no la plataforma).
- Lockup visual para piezas gráficas: **La Caravana × [Marca]**.
- Frases narrativas del universo verbal propio: *"La Caravana rueda con [Marca]"*,
  *"[Marca] se sube a La Caravana"* (juega con el "¿Te subes?" del mediakit).

---

## 7. Roadmap pendiente

- **Página de Ruta Personalizada** (construida localmente; falta instalar y verificar en producción).
- **Motor de mockups automático** (Fase 2, opcional): calibrador de 4 esquinas por foto +
  warp del logo por mapeo inverso en GD + máscara de sombras en multiply, para generar
  mockups sin diseñador. Hoy los mockups se suben hechos a mano (decisión de MVP).
- **Correo**: `mail()` de PHP cae en spam; configurar SPF/DKIM con Brevo/Resend/Zoho
  antes de enviar propuestas por email.
- Dashboard de estadísticas agregadas (tasa de apertura/aceptación de propuestas).
- Recordatorios automáticos si la marca no ha visto la propuesta.
