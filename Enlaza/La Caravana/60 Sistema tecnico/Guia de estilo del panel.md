---
tipo: tecnico
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/La Caravana/GUIA-DE-ESTILO-PANEL.md (2026-08-13)"
---

# Guía de estilo del panel

Cómo se construye cualquier pantalla del panel de La Caravana, desde el 13 de agosto de 2026.
Esta guía no es una sugerencia: es el estándar. Toda pantalla nueva se hace así, y toda pantalla
vieja que se toque se acerca a esto.

---

## Por qué existe

El panel tenía **catorce prefijos de clase distintos** (`.tb-`, `.prop-`, `.itin-`, `.age-`,
`.mrc-`, `.mk-`, `.env-`, `.cr-`, `.op-`, `.hoy-`, `.prep-`, `.pag-`...) haciendo lo mismo de
catorce maneras. Cada pantalla, mirada sola, estaba bien. El conjunto se veía improvisado.

La causa no era falta de gusto: era falta de vocabulario compartido. Los colores, sombras y
radios ya estaban bien definidos como tokens; lo que faltaba era la capa de arriba, los
componentes. Eso es lo que se añadió, con el prefijo `ui-`, al final de `admin/admin.css`.

---

## Las siete reglas

### 1. Lo operativo primero, la configuración plegada

Una pantalla abre mostrando lo que el usuario vino a hacer. Direcciones de feeds, tokens,
instrucciones de conexión y formularios de "crear nuevo" van dentro de `<details class="panel-plegable">`
cerrado, o detrás de un botón.

**Nada se abre desplegado por gusto.** Si algo está abierto al cargar, tiene que haber una razón:
o es el contenido principal, o el usuario pidió expresamente abrirlo.

### 2. Un solo botón principal por pantalla

La acción que define la pantalla va arriba a la derecha del encabezado, con `.b .b-1`. Todo lo
demás es `.b .b-2` (secundario), `.b .b-3` (destructivo) o `.ui-a` (discreto, para acciones de
fila que no deben competir con el contenido).

Lo destructivo nunca va suelto en una fila de lista: vive dentro del formulario de edición, donde
hay que entrar a propósito.

### 3. El espacio es una escala, no una opinión

`--e1:4px --e2:8px --e3:14px --e4:22px --e5:34px`. Todo margen y todo hueco sale de ahí. Ni
`margin:17px` ni `gap:11px` inventados en cada sitio. Así nada queda amontonado ni separado por
capricho.

### 4. Toda lista larga se pagina

Cualquier lista que pueda pasar de 100 filas lleva `.ui-pag`, con selector de **100 / 200 / 500**
por página y el total siempre a la vista. Y al guardar algo desde una fila, se vuelve a la misma
página, no a la primera.

### 5. El detalle se abre en ventana, no empujando la lista

Ver la ficha de algo usa `.ui-ventana`: se superpone, tiene scroll propio, y se cierra con el
botón, tocando fuera o con Escape. Nunca estira una fila hasta descolocar todo lo demás.

### 6. Lo que se abre se ve desde su primera línea

Ya resuelto en la capa compartida: `scroll-margin-top` en `panel-nav.css` para los anclas, y en
`pieAdmin()` un bloque que desplaza el `<details>` recién abierto hasta debajo de la barra
superior. No hay que volver a resolverlo pantalla por pantalla.

### 7. Se arregla en la capa compartida, no en la pantalla

Si un defecto visual aparece en una pantalla, casi siempre está en todas. Se corrige en
`admin.css`, `panel-nav.css` o `pieAdmin()`, no con un parche local.

---

## Los componentes

Todos viven al final de `admin/admin.css`, con prefijo `ui-`.

| Clase | Para qué |
|---|---|
| `.ui-cab` + `.ui-eyebrow` + `.ui-sub` | Encabezado: categoría, título y acción principal |
| `.ui-kpis` + `.ui-kpi` | Fila de contadores. Si el contador filtra, es un `<a>` y lleva `.on` cuando está activo |
| `.ui-filtros` + `.ui-f` + `.ui-sep` + `.ui-empuje` | Fichas de filtro y buscador. Nunca dos `<select>` grandes |
| `.ui-lista` + `.ui-item` | Filas. Identidad a la izquierda, estado y acción a la derecha |
| `.ui-item-izq` / `.ui-item-der` / `.ui-titulo` / `.ui-avatar` / `.ui-hora` | Piezas de una fila |
| `.ui-chip` (`.ok .mal .aviso .azul`) | Etiquetas de estado |
| `.ui-a` | Acción discreta de fila; se enciende al pasar el cursor |
| `.ui-vacio` | Estado vacío, con borde punteado |
| `.ui-pag` | Paginador |
| `.ui-ventana` + `.ui-tapa` + `.ui-caja` + `.ui-caja-cab` | Ficha en ventana |
| `.ui-seccion` + `h3` | Bloque con rótulo pequeño |

Y lo que ya existía y se sigue usando: `.b .b-1/.b-2/.b-3`, `.tabla`, `.rejilla`, `.ancho`,
`.panel-plegable`, `.pestanas`, `.estado`, `.nota`, `.ok` (`.mal`, `.aviso`), `.tarjeta`.

---

## Esqueleto de una pantalla nueva

```php
<?php
require_once __DIR__ . '/guardia.php';
require_once dirname(__DIR__) . '/includes/loquesea.php';
exigirSesion();

// guardia de instalación si depende de tablas propias
// POST: cada acción termina en header('Location: ...'); exit;

cabeceraAdmin('miseccion');
?>
<div class="ui-cab">
  <div>
    <p class="ui-eyebrow">Grupo del menú</p>
    <h1>Título</h1>
    <p class="ui-sub">Una línea sobre para qué sirve, si no es evidente.</p>
  </div>
  <div><a class="b b-1" href="?nueva=1#formulario">Acción principal</a></div>
</div>

<div class="ui-kpis"> ... contadores, los que filtran son <a> ... </div>
<div class="ui-filtros"> ... fichas .ui-f ... </div>

<div class="ui-lista">
  <div class="ui-item">
    <div class="ui-item-izq">
      <span class="ui-avatar">A</span>
      <span class="ui-titulo"><b>Nombre</b><small>Contexto</small></span>
    </div>
    <div class="ui-item-der">
      <span class="ui-chip ok">Estado</span>
      <a class="b b-2" href="?ver=1">Ficha</a>
    </div>
  </div>
</div>

<div class="ui-pag"> ... si la lista puede crecer ... </div>

<!-- ficha en ventana, solo si hay algo que ver -->
<!-- formulario de alta, plegado, al final -->
<?php pieAdmin(); ?>
```

---

## Lo que no se hace

- No se inventan prefijos nuevos. Si falta un componente, se añade a la capa `ui-` de
  `admin.css` y queda disponible para todos.
- No se usa `<table class="tabla">` para datos que no son tabulares. Una lista de personas o
  marcas es `.ui-lista`; una tabla de tasas o de movimientos sí es tabla.
- No se ponen `style="..."` largos en el HTML. Un ajuste puntual sí; media docena de propiedades
  es una clase que falta.
- No se deja una lista sin paginar "porque ahora son pocas". Las listas solo crecen.

---

## Migración

La capa `ui-` no rompe nada: es aditiva, y las clases viejas siguen funcionando. Las pantallas se
migran cuando se tocan por otro motivo, no todas de golpe. Referencia ya migrada: `admin/marcas.php`.

## Registro de ajustes compartidos

- **2026-08-31 — tablas anchas:** `admin/admin.css` incorpora `.ui-tabla-wrap` para contener
  tablas genuinamente tabulares con muchas columnas. Mantiene el ancho de la página y desplaza
  solamente la tabla cuando no cabe. Se estrenó en `admin/misiones-portal.php`, pestaña Entregas,
  donde Evaluación y Ficha se salían de la zona de contenido. PHP 8.1 pasó lint; la revisión visual
  quedó limitada porque el acceso local documentado respondió “correo o clave incorrectos”.
