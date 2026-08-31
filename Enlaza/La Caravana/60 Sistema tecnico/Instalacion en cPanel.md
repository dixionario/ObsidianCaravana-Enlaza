---
tipo: tecnico
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/prueba/LEEME.md (2026-08-19)"
---

# Carpeta `prueba`

Aquí se trabaja MODO PRUEBA. `La Caravana/` no se toca hasta que algo esté validado.

## Qué hay dentro

| Carpeta | Qué es | A dónde sube |
|---|---|---|
| `sitio/` | sitio y panel de pruebas | raíz de **prueba.lacaravanave.com** |
| `portal/` | Portal de creadores de pruebas | raíz de **pruebacreadores.lacaravanave.com** |
| `_paquetes/` | los cuatro zips de instalación y el LEEME largo | ver `LEEME-ZONA-PRUEBAS.md` |
| `_respaldo-original/` | los archivos de `La Caravana/` antes de tocarlos | red de seguridad |

No hay una tercera copia de nada: los zips se arman leyendo directamente de `sitio/` y `portal/`, así que el archivo que editas es siempre el que se sube.

## Lo que NO está aquí

`sitio/` lleva el código, no los medios. Quedaron fuera a propósito:

- `assets/img` (42 MB)
- `assets/video` (17 MB)
- `assets/subidas` (2 MB)
- los PDF, los zips viejos, `graphify-out/`

Esos archivos no cambian cuando se prueba una pantalla, y ya están en el servidor de pruebas porque el paso 3 del montaje copia la carpeta entera de `lacaravanave.com`. Si alguna vez hace falta uno, se toma de `La Caravana/`.

Dicho de otro modo: **`prueba/sitio` no es un espejo completo del sitio.** Es la parte editable.

## El ciclo de trabajo

1. Se edita en `prueba/sitio/` o `prueba/portal/`.
2. Se sube el archivo cambiado al subdominio de pruebas que le toca.
3. Se prueba ahí, con datos inventados y con el correo frenado.
4. Cuando funciona, y solo entonces, se copia el mismo archivo a `La Caravana/` y se sube al sitio real.

El paso 4 es deliberado y manual. Nada pasa de pruebas a producción por accidente.

Para promover un archivo ya validado, por ejemplo `admin/creadores.php`:

```bash
cp "prueba/sitio/admin/creadores.php" "La Caravana/admin/creadores.php"
```

Antes de copiar, conviene ver qué cambia:

```bash
diff "La Caravana/admin/creadores.php" "prueba/sitio/admin/creadores.php"
```

## Los dos archivos que nunca se promueven

- `sitio/includes/config.php` apunta a la base de pruebas y enciende `MODO_PRUEBAS`.
- `portal/includes/ruta-local.php` hace que el Portal de pruebas cruce a su propio sitio.

Si alguno de los dos llegara a `lacaravanave.com` o a `creadores.lacaravanave.com`, el sitio real quedaría leyendo la base de pruebas. Son los únicos dos archivos con esa propiedad.

## La deuda que queda abierta

`La Caravana/` está ahora mismo exactamente como estaba antes de empezar: los siete archivos compartidos volvieron a su versión original y `includes/pruebas.php` no está ahí.

Eso significa que la zona de pruebas y producción corren **código distinto** hasta que subas `parche-produccion-SITIO.zip` y `parche-produccion-PORTAL.zip`. El parche es inerte en producción, no cambia ningún comportamiento, pero mientras no esté aplicado hay que tener cuidado con una cosa:

> Si promueves un archivo desde `prueba/` a `La Caravana/` antes de aplicar el parche, ese archivo llevará consigo su enganche de pruebas. No rompe nada (todos los enganches están guardados con `function_exists`), pero deja `La Caravana/` a medio parchear.

Lo limpio es aplicar el parche a `La Caravana/` y a producción de una sola vez, y a partir de ahí las dos copias quedan alineadas. Los archivos ya parcheados son los que están en `sitio/includes/`, `sitio/admin/` y `portal/includes/`, que es de donde salen los dos zips `parche-produccion-*`.

## Rehacer los zips

Después de cambiar algo del andamiaje de pruebas:

```bash
powershell -ExecutionPolicy Bypass -File "prueba/rehacer-paquetes.ps1"
```

## Paquete 2026-08-28 — perfiles del equipo y experiencia ÉLITE

- Se generaron nuevamente `actualizacion-SITIO.zip` y `actualizacion-PORTAL.zip`.
- El Sitio incluye el instalador idempotente `admin/instalar-perfiles-equipo.php`, que crea únicamente `cv_perfiles_equipo` y `cv_logros_equipo`.
- Después de subir ambos paquetes, ejecutar en `Actualizaciones del sistema` la fila `Perfiles públicos y logros del equipo`.
- El Portal incorpora barra y candado 0/5 de ÉLITE, celebración sonora opcional, perfiles públicos del equipo y 26 tarjetas WebP.
- Auditoría del paquete: no incluye SQL, configuración local, archivos `*-maquina.php` ni escrituras sobre `cv_creadores` o `cv_applications`.

### Corrección del actualizador visible en producción

- La instalación quedó registrada también en el actualizador consolidado que usa el panel (`admin/actualizaciones.php`), no solamente en el actualizador auxiliar.
- La versión visible es `2026.08.28.02 · Perfiles públicos y logros del equipo`.
- La creación de tablas se centralizó en `instalarPerfilesEquipo()` para que ambos flujos ejecuten exactamente la misma operación idempotente.
- Prueba local: primera ejecución `true`, segunda ejecución `false`; la versión queda registrada una sola vez.
- Se regeneraron los dos ZIP y se confirmó que el paquete del Sitio contiene `includes/consolidacion.php`, `includes/ecosistema-creador.php` y `admin/instalar-perfiles-equipo.php`, sin archivos prohibidos.

### Paquete 2026-08-29 — check verificado vivo

- Se regeneraron `actualizacion-SITIO.zip` y `actualizacion-PORTAL.zip` después de ajustar únicamente el check azul de creador verificado.
- El ZIP del Portal contiene la regla `.pn-verificacion.creador` actualizada en `assets/panel.css`.
- Auditoría: 0 archivos SQL, configuraciones locales, archivos `*-maquina.php`, `.env` o dumps.

### Paquete 2026-08-29 — familia Destello Caravana

- Se regeneraron los paquetes después de implementar los badges Destello para Creador, Moderador, Consejo, Administrador y Super Admin.
- El ZIP del Portal contiene la familia compartida en `assets/panel.css`, incluida la variante iridiscente del Super Admin.
- Auditoría final: 0 archivos SQL, configuraciones locales, archivos `*-maquina.php`, `.env` o dumps.
- SHA-256 Portal: `39A300530A9BC5039440522930971CB5C5EB7528F3EC9B650A68D24EFEFA0342`.

### Corrección de contraste Destello — 2026-08-29

- Se regeneró el ZIP del Portal tras validar las cinco credenciales sobre el fondo azul real de Comunidad y a 375 px.
- El Super Admin queda en 21 px, con corona blanca sólida, perfil luminoso y halo multicolor de alto contraste.
- Auditoría del ZIP: 169 archivos; 0 SQL, configuraciones locales, `*-maquina.php`, `.env` o dumps.
- SHA-256 Portal corregido: `5FA7F746D65920F9BD06F657BE7A71555E315566349E04517B365B36CE6E0179`.

### Paquete de estados Plata/Oro — 2026-08-29

- El Portal incorpora la bandera aspiracional Plata para Convocado y Oro amarillo para Clasificado, ambas con destello y texto metálico.
- Incluye la corrección del cierre de la tarjeta de estado y sus límites responsive para impedir desplazamientos en móvil.
- Auditoría: 169 archivos; 0 SQL, configuraciones locales, `*-maquina.php`, `.env` o dumps.
- SHA-256 Portal: `D0EB5FA73862C4D05DA45618E868E4EFEFF10E9ABBE8E0445E9E23121EB3064C`.

### Paquete final de estados Plata/Oro/Elegido — 2026-08-29

- Se regeneró el Portal con Convocado Plata, Clasificado Oro y Elegido en esmalte azul cobalto, marco dorado y laureles reales de diez hojas.
- Validación del Elegido real del mundo local: 360 px, sin desbordamiento, símbolo y título dentro de la tarjeta y 0 errores PHP visibles.
- Auditoría: 169 archivos; 0 SQL, configuraciones locales, `*-maquina.php`, `.env` o dumps.
- SHA-256 Portal final: `FB2692A119CD31A446F19569650D17B5FC88F19EE79F276744AD5A40A72C8B63`.

### Regeneración por enlace ausente y limpieza del paquete — 2026-08-29

- El ZIP del Portal dejó de estar presente en `_paquetes`; se regeneró antes de volver a entregarlo.
- El empaquetador ahora excluye `.omc`, además de SQL, configuraciones locales, `*-maquina.php`, `.env` y dumps.
- Paquete limpio: 164 archivos, con `inicio.php` y `assets/panel.css` en la raíz correcta del Portal.
- SHA-256 vigente: `C8E8C6C7F7FCD77BA697F43CD07AB9D5093FDFE5D2D835BE73462B3D37CCA4D2`.

### Acceso directo del Super Admin desde la ficha — 2026-08-29

- La ficha administrativa de cada creador incorpora `Abrir su Portal`, visible solo para el Super Admin.
- El botón emite el pase de supervisión de un solo uso ya existente, con CSRF, caducidad de cinco minutos, pestaña separada y registro de inicio/finalización.
- La franja amarilla de supervisión añade `Editar perfil` y conserva `Volver al panel`; nunca oculta que se está operando sobre una persona real.
- Cada sección guardada desde `perfil-editar` durante supervisión registra `perfil_editado_superadmin`, el usuario administrativo y la sección afectada.
- No se añadieron tablas ni instaladores: reutiliza el sistema de supervisión ya desplegado.
- El despliegue requiere ambos dominios: Sitio para el botón de la ficha y Portal para la franja, edición y auditoría.
- El empaquetador se reforzó para excluir carpetas `.omc` aunque aparezcan anidadas en `admin`, `includes` u otra ruta.
- ZIP Sitio: 178 archivos, 0 prohibidos, SHA-256 `395048444899A94E7684DE2EC681D23D9CB75E05BAE3F32CBE1F97352C0579FD`.
- ZIP Portal: 164 archivos, 0 prohibidos, SHA-256 `299E03338FD68CC61DB2E76CBB1DC759D1783BB8A506F31A29D379530EFA16BF`.
- Corrección posterior: el botón estaba dentro del acordeón inferior `Acciones del Portal`, por lo que resultaba difícil de descubrir. Se movió a la cabecera visible de la ficha, debajo de los datos de contacto, dentro del bloque dorado `Supervisar Portal`.
- ZIP de corrección del Sitio: 178 archivos, 0 prohibidos, botón visible confirmado; SHA-256 `783C7F2BC0A3B786530FAA695A02A4E0F6E4847398E3D48C8D5B1AA3C8F96D7A`.

## Incidente: `www` redirige al Portal — 2026-08-30

- Síntoma confirmado externamente: abrir `https://www.lacaravanave.com/` termina en `https://creadores.lacaravanave.com/login`.
- `https://lacaravanave.com/` sí alcanza el sitio público y entra en la verificación de seguridad de Cloudflare.
- El código de `prueba/sitio/` y el espejo de producción no contienen una redirección de la portada hacia el Portal. El `index.php` público renderiza `home`.
- DNS observado: `www.lacaravanave.com` es un CNAME correcto hacia `lacaravanave.com`; los nameservers son de Cloudflare.
- Diagnóstico final: el DNS y las redirecciones visibles de cPanel estaban correctos, pero el alias `www` estaba entrando al árbol/vhost del Portal. El paquete de recuperación de multilinks añadió en `includes/arranque.php` un cerrojo que enviaba cualquier ejecución del Portal bajo `www.lacaravanave.com` a `creadores.lacaravanave.com`; luego `index.php` lo llevaba a `/login`. Ese cambio hizo visible la asociación incorrecta del alias.
- Se preparó `CORRECCION-WWW-Y-HTACCESS-2026-08-30.zip` con dos paquetes por raíz. El Sitio recibe el `.htaccess` productivo, sin `noindex`, conservando las URLs limpias. El Portal recupera `www` hacia `https://lacaravanave.com` antes de PHP y el cerrojo PHP ya no convierte un host público en una URL del Portal.
- La reparación usa 302 mientras se verifica el hosting, para no dejar otra decisión equivocada cacheada como 301.
- Pendiente externo: pedir al hosting que asocie `www.lacaravanave.com` al vhost/document root de `lacaravanave.com`, no al de Creadores.

## Cierre diario hacia GitHub — 2026-08-31

- Se creó inicialmente la automatización local `Cierre diario GitHub · La Caravana`, pero se eliminó inmediatamente al precisar el director que no desea pushes programados.
- Decisión vigente: el cierre hacia GitHub es manual. Al terminar la jornada, el director solicita `haz push`; el agente revisa, confirma el alcance, hace commit y ejecuta el push en ese momento.
- Remoto confirmado: `origin` apunta al repositorio `dixionario/lacaravanasite`; rama configurada al crearla: `chore/project-stabilization` con su upstream homónimo.
- Cada cierre manual hace `fetch`, revisa divergencia, estado y diff; linta cada PHP modificado y audita secretos, datos, paquetes y escrituras prohibidas antes de agregar archivos.
- Solo se hace commit y push de cambios seguros. No se crean commits vacíos, no se cambia de rama, no se usa `force push` y el proceso se detiene si el remoto está adelantado, hay conflictos, falla la autenticación o aparece contenido sensible.
- Las credenciales de GitHub permanecen en el mecanismo local de autenticación de Git; no se documentan ni copian al vault.

## Paquetes integrales regenerados — 2026-08-31

- Se regeneraron los paquetes de producción después de contener el desbordamiento de la tabla de entregas en `admin/misiones-portal.php` mediante la clase compartida `.ui-tabla-wrap` de `admin/admin.css`.
- Sitio: 188 archivos, 819.762 bytes, SHA-256 `FBFB2B74E312CCA75E6D1F2E229E5C67BEFA4B3FBA5FB1F4C94BD42366A24074`.
- Portal: 167 archivos, 10.307.958 bytes, SHA-256 `00A5D3FF89D007514EA6374B8A790735B76EE0814F7E9619042EFDC7BE955EA4`.
- El cerrojo del empaquetador no detectó configuraciones locales, archivos `*-maquina.php`, SQL, logs, Markdown ni instaladores con escrituras prohibidas sobre creadores o postulaciones.
- Son paquetes diferenciales integrales contra el espejo de producción; no son un parche aislado de dos archivos.
