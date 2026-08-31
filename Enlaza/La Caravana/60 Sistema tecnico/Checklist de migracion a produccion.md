---
tipo: tecnico
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/prueba/CHECKLIST-MIGRACION.md (2026-08-20)"
---

# Checklist de migración a producción

Lo que hay que hacer **al pasar la zona de pruebas al sitio real**, y que no viaja en los zips porque son claves, credenciales o decisiones de configuración.

Este documento se va ampliando: cada vez que en pruebas se deje algo sin configurar por ser un entorno de ensayo, se anota aquí.

---

## Claves e integraciones

### Turnstile (captcha)
Pendiente desde 2026-08-19.

En pruebas está el código puesto pero **sin claves**, así que el captcha no se dibuja y la comprobación deja pasar. Al migrar:

1. `dash.cloudflare.com` → Turnstile → añadir widget
2. Dominios: `lacaravanave.com` **y** `creadores.lacaravanave.com`
3. Modo: Managed
4. Pegar las dos claves en el panel, **Ajustes**:
   - Turnstile · clave pública
   - Turnstile · clave secreta

Afecta a dos sitios: el buscador de convocados y **la votación pública**. Sin claves, la votación queda sin captcha y solo la protege el tope por conexión.

### SMTP de Brevo
En el config de pruebas está **vacío a propósito**, para que ningún correo pueda salir. En producción ya está configurado y no hay que tocarlo. Lo que sí hay que comprobar es que `MODO_PRUEBAS` **no exista** en el config real: si estuviera, el freno de correo se activaría en producción y no saldría ningún correo.

### Google Calendar y GTM
En la base de pruebas se dejaron vacíos a propósito. En producción ya están puestos.

---

## Configuración de la votación

- **Tope por IP**: cuatro votos por conexión y edición, escrito en `api/voto.php`. Si en producción se quiere otro número, se cambia ahí.
- **Ventana de votación**: hoy depende de que la edición esté `en_curso`. No hay fecha de apertura y cierre propia de la votación. Si se quiere, hay que añadirla.
- **Los pesos 20/20/20/40** están pendientes de construir en la pantalla de cálculo. Cuando se haga, dejarlos editables desde el panel y no escritos en el código.

---

## Archivos que NUNCA deben cruzar a producción

El empaquetador ya lo impide, pero conviene tenerlo escrito:

- `includes/config.php` — apunta a la base de pruebas
- `creadores-app/includes/ruta-local.php` — apunta al sitio de pruebas
- `includes/pruebas-portal.php`, `pruebas-modo.php`, `pruebas-acceso.php` — el conmutador de estados y la entrada automática sin contraseña

Y los archivos de siembra, que solo existen en pruebas:
`instalar-zona-pruebas.php`, `donde-estoy.php`, `admin/sembrar-mundo.php`, `admin/sembrar-sitio.php`, `admin/sembrar-escenario.php`

---

## Lo que sí hay que subir a producción

`parche-produccion-SITIO.zip` y `parche-produccion-PORTAL.zip`. Son inertes: traen `includes/pruebas.php` y los enganches que lo llaman, y como en producción `MODO_PRUEBAS` no existe, no cambian ningún comportamiento.

Incluyen además correcciones reales que aparecieron durante las pruebas:

- `creador.php` — el perfil público reventaba con un aviso de PHP cuando la trayectoria del creador estaba vacía. Le pasaría a cualquier creador sin ese campo relleno.

---

## Plan de reversa (antes de CUALQUIER actualización en producción)

El ritual, en orden, ANTES de extraer ningún zip en cPanel:

1. **Respaldo de archivos**: cPanel → Administrador de archivos → comprimir la carpeta
   completa de `lacaravanave.com` y la de `creadores.lacaravanave.com`. Descargar los
   dos zips con fecha en el nombre. Revertir código = volver a extraer ese zip.
2. **Respaldo de base**: panel → `admin/respaldo.php` → descargar el .sql del día.
   Revertir datos = importarlo por phpMyAdmin. (Las tablas nuevas de los instaladores
   son aditivas: revertir el código basta casi siempre; el .sql es el paracaídas.)
3. Solo entonces: extraer `actualizacion-SITIO.zip` y `actualizacion-PORTAL.zip`.
   Los 9 instaladores se corren desde UNA pantalla: **`admin/actualizador.php`** —
   detecta lo instalado por tabla centinela, un clic por sistema, en orden, con el
   resultado a la vista. (Ya no hace falta abrirlos uno a uno.)
   (generados con `python prueba/empaquetar-actualizacion.py --zip`, que calcula el
   contenido POR DIFERENCIA contra la copia real — las listas a mano quedaron obsoletas
   el 20-08: cubrían 65 de 168 archivos).
4. Correr los instaladores en el orden del guion de lanzamiento, verificar, y
   descargar un respaldo NUEVO post-actualización.

Regla de oro: si algo sale mal, primero se restaura el zip de archivos del paso 1;
la base solo se restaura si un instalador dejó algo a medias (no ha pasado: todos
son re-ejecutables).

## Ediciones — rutas canónicas (2026-08-20)

Al migrar, corregir A MANO en el panel real (Gestionar ediciones) las rutas, que en producción están desactualizadas (Chichiriviche, Apure, sin Barquisimeto, sin cierres en Caracas):

- Ciudades de Venezuela: `Barquisimeto · Valencia · Maracay · Caracas`
- Playas de Venezuela: `Isla de Margarita · Sucre · Mochima · Pto. La Cruz` (acortada el 20-08)
- Llanos Venezolanos: `Barinas · Portuguesa · Cojedes · Guárico · Caracas`
- Andes Venezolanos: `Táchira · Mérida · Trujillo · Caracas`
- Crear la **Edición Navideña** (`Maracaibo`, orden 5)
- **Imagen de la Navideña**: subir por el panel (Medios) el archivo `edicion-navidena-2026.jpg` (el hero optimizado, 321 KB; el original está en Downloads como `navidad-maracaibo.png`) y asignarla a la edición en Gestionar ediciones. El panel genera su `-mini` solo; si no, subir también `edicion-navidena-2026-mini.jpg`. Los dos archivos listos están en `prueba/sitio/assets/subidas/`.

El orden del texto ES el orden de la ruta.

**Rediseño del catálogo de ediciones (2026-08-20).** El código viaja solo (bloques/ediciones.php + sección `edx` en caravana.css + campos nuevos en esquemas.php). Lo que se replica A MANO en el panel real:

- Color de cada edición (Gestionar ediciones), con la paleta del panel del creador: Ciudades `#3B82F6`, Playas `#FFC629` (texto `#141210`), Llanos `#22C55E`, Andes `#C084FC`, Navideña `#EF4444`.
- Las descripciones tipo cartel de cada edición (los textos están en la base local y en la maqueta del director).
- El bloque en la página `ediciones` toma el diseño solo; en `home` existe pero está oculto (visible=0, estado que ya traía producción): decidir si se muestra. Detalle técnico que viaja con el código: el parser de paradas del portal ahora acepta «·» y coma (portal/inicio.php).

## Datos

**LEY (2026-08-20): los datos solo bajan, nunca suben.** El entorno local trabaja sobre una copia de la base real; al migrar, NADA de esa copia se exporta a producción. Solo viajan código e instaladores, y son los instaladores corridos allá los que reconectan con los datos oficiales (reconstruyen Ke, recalibran rangos) desde el estado real. Cualquier semilla necesaria va dentro de un instalador idempotente, nunca como SQL.


- La base de pruebas **no se migra**. Producción tiene sus propios datos.
- Lo que sí hay que replicar en producción son las **tablas nuevas**: `comunidad_publicaciones`, `comunidad_comentarios`, `comunidad_reacciones`, `recursos`, `votos_creadores`, y la columna `ip_huella` en `votantes`. Se crean con los mismos instaladores: `admin/instalar-comunidad.php` y `admin/instalar-voto-creadores.php`.
- **Kilómetros de Experiencia y niveles (2026-08-20).** Tablas `km_experiencia` y `leyendas`, más las columnas `rangos_creador.kr_min`, `rangos_creador.desbloquea`, `creadores.nivel_visto` y `recursos.nivel_min`. Se crean todas con `admin/instalar-km-creador.php`, que además reconstruye los Ke históricos y corona hacia atrás las ediciones archivadas. Es re-ejecutable.

  Tres cosas que **no viajan** y hay que rehacer a mano en producción:

  1. **Los umbrales de la escalera.** El instalador los recalibra solo si siguen siendo los de fábrica (0/100/250/450/700). Los que se dejen validados en pruebas hay que anotarlos aquí y volver a ponerlos en el panel real, en Rangos → Escalera. Valores de partida: Andariego 0, Trochero 40, Rutero 120, Baquiano 400 con 150 Kr de ruta.
  2. **El catálogo de Ke** (cuánto paga cada acción y su tope diario). Vive en Ajustes con claves `ke_*` y `ke_tope_*`, así que no viaja en los zips.
  3. **El texto de "qué desbloquea" cada rango** y el **rango mínimo de cada recurso**, que son decisiones de contenido.

  Antes de publicar en real, abrir Rangos → Escalera y mirar la vista previa partida por liga: si casi todos los aspirantes quedan en el primer peldaño, el segundo está demasiado lejos para quien no puede ganar Kr.

# Gobernanza de creadores (20-08-2026)

- [ ] Ejecutar `admin/instalar-gobernanza-creadores.php` en producción antes de habilitar el nuevo front del Portal.
- [ ] Revisar en Rangos los umbrales KE, las cuatro bandas KR, dificultades, topes y reglas de actividad.
- [ ] Confirmar que los KR disponibles de cada edición corresponden a la suma de sus retos publicados.
- [ ] No transportar `cv_actividad_creador`, `cv_semanas_actividad` ni `cv_km_ruta` desde local: producción las crea el instalador y las alimentan eventos reales.

# Verificación privada de identidad (20-08-2026)

- [ ] Ejecutar dos veces `admin/instalar-verificacion-identidad.php`; debe crear o conservar `cv_verificaciones_identidad` sin tocar creadores.
- [ ] Confirmar que PHP puede escribir en `_privado_verificaciones`, carpeta hermana del directorio público del sitio; nunca copiar documentos locales a producción.
- [ ] En Rangos → Kilómetros de Experiencia, confirmar el premio `identidad_verificada` (valor inicial recomendado: 50 Ke).
- [ ] Probar con un documento ficticio: envío, revisión en la ficha privada, acceso solo administrativo, aprobación y un único asiento Ke aunque se procese de nuevo.
- [ ] La foto de perfil es obligatoria para llegar al 100%; no ejecutar actualizaciones masivas sobre `cv_creadores`: el porcentaje efectivo se calcula al mostrar y se persiste cuando cada creador guarda su perfil.

# Centro de operaciones de creadores (20-08-2026)

- [ ] Ejecutar dos veces `admin/instalar-supervision-creadores.php`: crea `cv_acceso_creadores`, `cv_pases_supervision`, `cv_operaciones_creadores` y las columnas aditivas de autor administrativo del foro.
- [ ] Confirmar en Usuarios que la cuenta del director tenga `super_admin=1` y el nombre comunitario deseado (por ejemplo, Diccionario).
- [ ] Probar “Ver su Portal con supervisión”: pase de un uso, franja amarilla identificada, publicación con insignia ADMIN y regreso al panel.
- [ ] Probar suspensión temporal, bloqueo permanente y reactivación con una cuenta controlada. El bloqueo entra en vigor en la siguiente petición y conserva perfil, rangos y kilómetros.
- [ ] No transportar pases, auditorías ni publicaciones del mundo sembrado. En producción solo viajan código e instalador; la Zona de pruebas flotante queda restringida a `sembradocreadores.localhost`.

# Consejo y oportunidades de creadores (20-08-2026)

- [ ] Ejecutar dos veces `admin/instalar-participacion-creadores.php`; debe crear solicitudes, consultas y respuestas sin modificar `cv_creadores` ni `cv_applications`.
- [ ] Configurar y publicar una consulta real desde Rangos → Participación antes de anunciar el Consejo.
- [ ] Revisar solicitudes de Formación y Alianzas desde Rangos → Participación y definir responsables operativos.
- [ ] No transportar la consulta de ensayo ni solicitudes locales: en producción solo viaja el instalador.

## Migrador a pruebas (2026-08-20)

Herramienta nueva: `prueba/_paquetes/migrar-a-pruebas.php` (lo genera
`python prueba/generar-migrador.py`, que imprime su clave de acceso).
Clona el sitio real hacia la zona de pruebas DESDE el propio cPanel:

1. Subir el archivo a la RAIZ de lacaravanave.com y abrirlo en el navegador.
2. Entrar con la clave impresa al generarlo, revisar el plan (rutas y base
   destino) y escribir CLONAR A PRUEBAS.
3. Avanza solo por etapas: base (tabla por tabla), vaciar y copiar sitio,
   vaciar y copiar portal, sellos (config de pruebas generado con SMTP
   apagado, freno de correo, franja, noindex, cruce del portal).
4. Al terminar: proteger los subdominios (Directory Privacy), extraer los
   zips de actualizacion encima, correr admin/actualizador.php, y BORRAR
   el migrador con su boton rojo.

Cerrojos: produccion solo se lee; destinos y base deben contener "prueba"
en el nombre; la frase de confirmacion es obligatoria; pruebas-acceso.php
(entrada automatica del portal) NO se instala porque la base clonada trae
creadores reales. Ensayado completo en local el 2026-08-20: 111 tablas y
714 MB clonados, mundo migrado arrancando sin warnings.
