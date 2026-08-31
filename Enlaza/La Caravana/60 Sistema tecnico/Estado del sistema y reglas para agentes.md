---
tipo: tecnico
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/AGENTS.md (2026-08-20)"
---

# La Caravana — Guía para agentes (Codex, Claude, etc.)

Plataforma de creadores de contenido que recorre Venezuela por ediciones
(expediciones). PHP 8.1 + MariaDB, sin framework. Dos caras: sitio público con
panel admin (`lacaravanave.com`) y Portal del creador (`creadores.lacaravanave.com`).

## MAPA DE CARPETAS — léelo antes de tocar nada

| Carpeta | Qué es | ¿Se edita? |
|---|---|---|
| `prueba/sitio/` | **ÁRBOL DE TRABAJO** del sitio + panel. Es el sitio oficial fusionado + parches nuevos | **SÍ — aquí se trabaja** |
| `prueba/portal/` | **ÁRBOL DE TRABAJO** del Portal del creador | **SÍ — aquí se trabaja** |
| `La Caravana/` | Espejo de PRODUCCIÓN real (lo que corre en el hosting) | **NO. Jamás. Solo lectura** |
| Raíz del repo (`admin/`, `includes/`, `api/`...) | Copia VIEJA y desactualizada | **NO. Ni leerla como referencia** |
| `prueba/_local/` | Andamiaje solo-de-esta-máquina (Apache, clon extraído, logs) | Config local; no viaja nunca |

## LAS LEYES (del director; no negociables)

1. **LOS DATOS SOLO BAJAN, NUNCA SUBEN.** A cPanel viaja solo código e
   instaladores idempotentes. Jamás un dump, INSERT o fila de una base local.
   Los instaladores corridos en producción reconstruyen desde los datos reales
   de allá. Si algo necesita semillas, van DENTRO de un instalador.
2. **Los creadores de producción son intocables.** Ningún instalador o código
   nuevo hace UPDATE/DELETE/INSERT sobre `cv_creadores` ni `cv_applications`
   (única excepción auditada: ADD COLUMN aditivas y el `nivel_visto` que cada
   creador actualiza sobre sí mismo). Antes de empaquetar, se repite la
   auditoría con grep. Ningún creador ni estatus viaja desde local.
3. **Flujo local primero.** Todo se desarrolla y prueba aquí. NO se empaquetan
   zips salvo que el director pida "empaqueta para cPanel"
   (`prueba/rehacer-paquetes.ps1`, con cerrojos que rechazan config local,
   `*-maquina.php` y `.sql`).
4. **Lo que quede sin configurar se anota en `prueba/CHECKLIST-MIGRACION.md`
   en el momento en que pasa.**
5. **Calidad editorial**: ningún texto multilínea con line-height < .92
   (display .96); cifras con coma llevan padding que contenga el descendente;
   los defectos se corrigen en la CAPA COMPARTIDA, nunca pantalla por pantalla;
   todo responsive; se recorre la pantalla como humano antes de entregar.
6. **Agrupar, no dispersar**: función nueva = pestaña dentro de una pantalla
   existente del panel, no ítem de menú nuevo. Panel usa la capa `ui-` de
   `admin/admin.css` (ver `La Caravana/GUIA-DE-ESTILO-PANEL.md`); portal nuevo
   usa prefijo `pn-` (`prueba/portal/assets/panel.css`); sitio público, clases
   por bloque en `prueba/sitio/assets/css/caravana.css`.

## ENTORNO LOCAL (Windows + Laragon, ya montado)

- **Arrancar Apache** (única forma correcta; resuelve DLL de PHP):
  `powershell -NoProfile -File "prueba\_local\arrancar-apache.ps1"`
  NO usar la interfaz de Laragon (Start All chocaría con este Apache y con MariaDB).
- **PHP para lint**: `C:\laragon\bin\php\php-8.1.33-Win32-vs16-x64\php.exe -l`
  (8.1 = la rama del hosting). TODO archivo tocado se linta antes de darse por hecho.
- **MariaDB** local como servicio, puerto 3306, usuario `root`, clave `Rafalien_22`.
  Cliente: `C:\laragon\bin\mysql\mysql-8.4.3-winx64\bin\mysql.exe`.

### Los dos mundos (mismo código, base por hostname)

| URL | Base de datos | Para qué |
|---|---|---|
| `http://prueba.localhost` (+ `pruebacreadores.localhost`) | `caravana_clon_local` — copia de la REAL (2.945 creadores; foto del 16-08) | Preparar la versión final |
| `http://sembrado.localhost` (+ `sembradocreadores.localhost`) | `caravana_pruebas_local` — mundo inventado (30 creadores, 10 elegidos, retos, foro) | Terreno desechable |

- Panel admin (ambos mundos): `admin@pruebas.local` / `pruebas-local-2026` en `/admin/login.php`.
- Portal: autologin activo; `?creador=N` cambia de creador; `?sinauto=1` prueba el login real.
- El selector de base vive en `prueba/sitio/includes/config-local-maquina.php`
  (solo existe en esta máquina; SMTP vacío = ningún correo puede salir).
- El panel exige token `csrf` en TODO POST (viene inyectado en el HTML de cada form).

## QUÉ ESTÁ CONSTRUIDO (2026-08-20)

- **Sistema Kr/Ke**: kilómetros de Ruta (retos/iniciativas aprobadas; ya existían)
  y de Experiencia (libro mayor `cv_km_experiencia`, clave única idempotente).
  Motor en `prueba/sitio/includes/km-creador.php`. Escalera de rangos
  (Andariego→Trochero→Rutero→Baquiano, con `kr_min`), corona de **Leyenda** por
  edición (`cv_leyendas`), ligas separadas (Aspirantes/Clasificados por Ke,
  En ruta por Kr). Los km se empiezan a ganar al ser **Clasificado**.
  Pantallas: `prueba/portal/nivel.php`, tarjeta en `inicio.php`, ranking en
  `comunidad.php`; admin en `admin/rangos.php` (3 pestañas). Instalador:
  `admin/instalar-km-creador.php` (re-ejecutable).
- **Catálogo de ediciones rediseñado** («Cinco rutas, un mismo país»): ficha
  cartel compartida en `prueba/sitio/includes/tarjeta-edicion.php` (usada por
  bloque `ediciones`, `convocatoria.php` y bloque `edicion-activa` — si tocas
  la ficha, tócala AHÍ). CSS sección `edx` en `caravana.css`. 5 ediciones:
  Ciudades(azul #3B82F6)/Playas(oro #FFC629)/Llanos(verde #22C55E)/
  Andes(magenta #C084FC)/Navideña(rojo #EF4444). Rutas canónicas en
  `prueba/EDICIONES-RUTA-MAESTRA.md`.
- **Edición Ciudades encendida en el clon**: 10 elegidos publicados, fechas y
  4 paradas PROVISIONALES (notas lo dicen). Faltan retos (contenido del equipo).
- Barrido editorial hecho (16 line-heights corregidos), menú móvil arreglado
  (backdrop-filter creaba containing block), flotante Postúlate eliminado.

## PENDIENTES CONOCIDOS

- Retos de la edición Ciudades (contenido del director).
- Decidir: retos amarrados a parada; mecánica de la Navideña; qué desbloquea
  cada rango; bono 15 Ke al clasificar (¿se queda?).
- Bloques `edicion-destacada` y `edicion-especial` aún con diseño viejo.
- Auditoría responsive página a página sin terminar (preguntas, bio, marcas interiores).

## CÓMO VERIFICAR (obligatorio antes de dar algo por hecho)

1. `php -l` (8.1) sobre cada archivo tocado.
2. Cargar la pantalla real por HTTP en el mundo que toque y revisar que no
   haya `Warning:|Deprecated:|Fatal error:` en el HTML (display_errors está ON).
3. Si tocaste el sistema de kilómetros: correr dos veces el instalador no debe
   duplicar nada; grep de escrituras sobre `creadores|applications` en lo nuevo.
4. Móvil 375px: sin scroll horizontal; menú hamburguesa abre y cierra.

## GOTCHAS QUE YA MORDIERON

- `cv_ediciones.estados` separa paradas con `·` (el portal parsea `·` y coma).
- `FIELD()` de MySQL da 0 (ordena primero) a valores no listados.
- La sesión de PowerShell no persiste entre comandos: login + acción en UNA llamada.
- Estados de postulación: postulado→convocado→clasificado→elegido (finalista/
  seleccionado/reserva son legacy; Reserva NO es un rango).
- "Elegir" un creador (applications.status) NO lo publica en la web: eso es
  `creadores.estado='publicado'` (acción `estado` del panel de creadores).
- Los medios (assets/img|video|subidas) se sirven por Alias de Apache desde
  `prueba/_local/clon/`; no existen dentro de `prueba/sitio/`.

## LECCIONES DE AUDITORÍA (20-08, corregidas — no repetir)

- Poderes de supervisión/impersonación exigen `exigirAdmin()`, nunca `exigirSesion()`.
- Logging de debug SIEMPRE tras `if (defined('MODO_PRUEBAS') && MODO_PRUEBAS)`.
- Un tipo de Ke nuevo se registra en `catalogoKe()` Y en `categoriaKeInsignia()` a la vez;
  un tipo fantasma en el mapa de insignias es un multiplicador que jamás se aplica.
- Instalación en un hosting: `admin/actualizador.php` corre los 9 instaladores con
  un clic cada uno, en orden, detectando por centinela lo ya instalado.
- Para empaquetar hacia cPanel: `python prueba/empaquetar-actualizacion.py` (diferencia
  contra la copia real). Las listas a mano de rehacer-paquetes.ps1 quedaron obsoletas
  para actualizaciones (siguen valiendo para el andamiaje de la zona de pruebas del hosting).

## Ajuste de navegación del perfil — 2026-08-31

- El CTA azul `Editar perfil` de la ficha moderna del Portal abre directamente `perfil-editar?editar=1`.
- Se eliminó el paso intermedio por la ficha antigua de `perfil-editar.php`, que volvía a mostrar otro botón `Editar mi perfil`.
- Archivo: `prueba/portal/perfil.php`. PHP 8.1 lint correcto; la comprobación HTTP quedó impedida porque el Apache local no logró iniciar en esta sesión.
