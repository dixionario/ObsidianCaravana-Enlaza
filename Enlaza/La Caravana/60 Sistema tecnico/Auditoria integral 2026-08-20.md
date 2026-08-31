---
tipo: tecnico
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/prueba/AUDITORIA-2026-08-20.md (2026-08-20)"
---

# Auditoría integral del sistema — 2026-08-20

Cinco capas + análisis de migración, sobre el árbol de trabajo completo
(lo construido por Claude + lo construido por Codex, ya con las 3
correcciones de la revisión de Codex aplicadas).

## Resultados por capa

| Capa | Alcance | Resultado |
|---|---|---|
| **Developer** | 401 PHP lintados (rama 8.1 del hosting) | **0 errores de sintaxis** |
| | Ley de creadores (instaladores que viajan) | **0 escrituras** sobre `cv_creadores`/`cv_applications` |
| | Duplicados de funciones cargables juntas | 0 reales (falsos positivos: JS y copias muertas) |
| | Idempotencia de instaladores | **Confirmada por efectos**: 2 pasadas consecutivas, delta 0 filas |
| **Visitante** | Crawl del sitio público desde portada (20 páginas enlazadas) | **0 errores, 0 avisos** |
| **Creador** | 21 pantallas × 4 perfiles (elegido/clasificado/convocado sembrado + clasificada real) | **84/84 en verde** |
| **Administrador** | Las 49 pantallas del menú del panel, con sesión | **49/49 en verde** |
| **Diseñador** | Regla editorial en CSS nuevo (line-height < .92) | 0 violaciones nuevas; las 2 restantes son excepciones documentadas (logo, cifra con contención) |
| | Visual desktop+móvil | Verificado en la pasada del mediodía (hero, catálogo, menú móvil, sin scroll horizontal) |

## El hallazgo crítico (y su arreglo)

**Las listas manuales del empaquetador cubrían 65 de los 168 archivos que deben
viajar.** Una actualización empaquetada con `rehacer-paquetes.ps1` habría subido
a cPanel un sistema a medias (sin supervisión, insignias, gobernanza, finanzas...).

**Arreglo construido**: `prueba/empaquetar-actualizacion.py` — calcula el paquete
POR DIFERENCIA contra la copia real de producción (el clon del hosting para el
sitio, el espejo para el portal). No hay lista que envejezca. Lleva doble cerrojo:
rechaza configs/andamiaje/datos, y **aborta** si algún instalador del paquete
escribe sobre creadores/applications. Dry run de hoy: 128 archivos SITIO + 39
PORTAL = 167. Se ejecuta con `--zip` solo cuando el director lo pida.

## Guion de actualización en cPanel (el día que toque)

0. **Refrescar el clon local** con un respaldo fresco de producción y repetir la
   auditoría rápida (la foto local es del 16-08; producción siguió viva).
1. **Plan de reversa** (ya en CHECKLIST-MIGRACION): comprimir y descargar las dos
   carpetas del hosting + descargar respaldo .sql desde `admin/respaldo.php`.
2. Generar y extraer `actualizacion-SITIO.zip` y `actualizacion-PORTAL.zip`.
3. Correr instaladores en orden: comunidad → voto-creadores → km-creador →
   supervision → insignias → gobernanza → participacion → finanzas.
4. Configuración manual del panel (checklist): rutas de ediciones + Navideña con
   su imagen, colores de paleta, umbrales de rangos, valores de Ke, desbloqueos.
5. Lanzamiento de la edición: elegir → **publicar** cada creador → fechas +
   paradas en Edición en vivo → retos.
6. Verificar con el propio guion de la capa visitante/creador/admin y descargar
   respaldo post-actualización.

## Correcciones aplicadas durante la auditoría (sesión de hoy)

- Supervisión restringida a `exigirAdmin()`.
- Debug de envíos condicionado a `MODO_PRUEBAS` (+ log viejo eliminado).
- Mapa de insignias alineado al catálogo real de Ke.
- (Previas) menú móvil, barrido editorial de 16 interlineados, parser de rutas,
  fallback FIELD() del autologin de pruebas, contador espejo por estatus.

## Recomendaciones honestas (pendientes de decisión del director)

1. **Git como máquina de reversa local.** El repo no tiene ni un commit: hoy
   "deshacer" depende de memoria y copias. Un `git init` con `.gitignore` para
   `_local/`, medios y logs daría foto por cambio, diff instantáneo y vuelta
   atrás en segundos. Es la herramienta de reversión más barata que existe.
2. **Refresco del clon antes del empaquetado final** (paso 0 del guion): baja
   respaldo fresco, reimporta, re-audita. 30 minutos que compran certeza.
3. **Ensayo en la zona de pruebas del hosting** antes de producción: extraer los
   mismos zips en prueba.lacaravanave.com y correr el guion completo allá. Es
   exactamente el flujo por etapas ya acordado; no saltárselo por prisa.
4. **Los retos de Ciudades** siguen siendo el único bloque funcional apagado, y
   es contenido: sin ellos no hay Kr, ni tabla, ni corona en producción.
