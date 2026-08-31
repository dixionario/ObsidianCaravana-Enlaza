---
tipo: referencia
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/prueba/EDICIONES-RUTA-MAESTRA.md (2026-08-20)"
---

# Ediciones — la ruta maestra

Definición canónica dada por el director el 2026-08-20. Es la columna vertebral del
calendario y del sistema completo: cada edición es una convocatoria, una selección,
una ruta con paradas ordenadas, una carrera de kilómetros y una corona.

## El catálogo

| # | Edición | Ruta (en orden) | Paradas |
|---|---------|-----------------|---------|
| 1 | **Ciudades** | Barquisimeto → Valencia → Maracay → **Caracas** | 4 |
| 2 | **Playas** | Isla de Margarita → Sucre → Mochima → Pto. La Cruz | 4 |
| 3 | **Llanos** | Barinas → Portuguesa → Cojedes → Guárico → **Caracas** | 5 |
| 4 | **Andes** | Táchira → Mérida → Trujillo → **Caracas** | 4 |
| 5 | **Navideña** | Maracaibo | 1 |

Dos patrones que definen el diseño:

- **Ajuste del 20-08 (tarde):** Playas quedó en 4 paradas sin Caracas en el texto
  visible (decisión del director por largo del texto; Paria se nombra por su
  estado, Sucre). Si el cierre en Caracas sigue vigente para Playas como evento,
  no aparece en la ruta pública.
- **Caracas cierra toda ruta regular.** Es el gran final de cada edición: la caravana
  siempre vuelve a la capital. En el sistema, la última parada es donde se decide la
  corona (los últimos retos) y donde tiene sentido el evento de cierre.
- **La Navideña es especial.** Una sola ciudad (Maracaibo, la capital de la Navidad
  venezolana), cierre del año. No es una ruta: es un destino. Sus mecánicas pueden
  diferir (menos retos, más evento) y el sistema no debe asumir "mínimo 2 paradas"
  en ninguna parte.

## Cómo mueve al sistema completo

El ciclo de una edición, con las piezas ya construidas:

1. **Convocatoria** — la edición pasa a `convocatoria`; los aspirantes se postulan.
2. **La cantera juega** — los Clasificados (hoy 798 reales) suman **Ke** en el portal
   entre ediciones: misiones de plataforma, ideas, ayudar en el foro. Su liga y su
   escalera (Andariego → Trochero → Rutero → Baquiano) corren todo el año, sin
   depender de ninguna edición.
3. **Selección** — el equipo elige ~10 de la cantera (evaluación + finalistas en el
   panel). El nivel no decide la selección, por ley de diseño.
4. **La ruta** — la edición pasa a `en_curso`. Cada parada de la ruta lleva sus retos;
   los retos aprobados pagan **Kr**. La tabla de la edición ordena a los 10 y el
   primero lleva la corona provisional.
5. **El cierre en Caracas** — últimos retos, evento final.
6. **Archivo** — la edición pasa a `archivada` y la corona de **Leyenda** se congela
   para siempre con el nombre de la edición. El título es acumulable.
7. **Siguiente edición** — la cantera sigue creciendo; los Kr ganados se quedan con
   cada creador (suben su escalera: Baquiano exige 150 Kr de ruta).

Un año completo = 5 coronas de Leyenda en disputa + una escalera que nunca se detiene.

## Dónde vive cada cosa en el sistema

- La ruta se guarda en `cv_ediciones.estados`, separada por «·» (formato del panel
  de producción). El portal la pinta en el mapa de Inicio; el parser acepta «·» y
  coma desde el 2026-08-20 (`portal/inicio.php`).
- El orden de las paradas ES el orden del texto. Caracas va de última a propósito.
- Los retos se asocian a la edición (`cv_retos.edicion_id`) con su `dia`; el avance
  del mapa se calcula por proporción de retos aprobados sobre el total.
- La corona: `leyendaProvisional()` en vivo, `cv_leyendas` al archivar
  (`includes/km-creador.php`).

## Qué queda por decidir (para el director)

1. **Retos por parada**: hoy los retos llevan `dia`, no parada. ¿Amarramos cada reto
   a una parada concreta de la ruta (Reto de Barquisimeto, Reto de Valencia...)?
   Haría el mapa exacto en vez de proporcional, y permitiría "kilómetros por ciudad".
2. **La Navideña**: ¿misma mecánica con menos retos, o formato propio (p. ej. un solo
   gran reto + evento)? ¿Tiene corona de Leyenda con una sola parada?
3. **El orden del año**: ¿Ciudades → Playas → Llanos → Andes → Navideña es también el
   orden cronológico de la temporada?

## Ley aplicable

Estas rutas se cargaron en la base LOCAL para probar el sistema. Al subir, las
ediciones se configuran **a mano en el panel de producción** (Gestionar ediciones),
con estos textos exactos. Ningún SQL viaja. Ver la ley en `prueba/CLAUDE.md`.
