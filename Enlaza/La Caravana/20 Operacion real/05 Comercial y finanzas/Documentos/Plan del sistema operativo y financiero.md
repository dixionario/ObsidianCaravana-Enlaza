---
tipo: plan
estado: PROPUESTO
actualizado: 2026-08-22
fuente: "La Caravana/PLAN-SISTEMA-OPERATIVO-Y-FINANCIERO.md (2026-07-28)"
---

# La Caravana — Plan del sistema operativo, editorial y financiero

Documento de planeación (sin código todavía). Complementa a `PROYECTO-SISTEMA-COMERCIAL.md`.
Última actualización: 28 de julio de 2026.

---

## 1. Visión general

Todo lo que se planifica aquí gira en torno a una sola idea: **cada edición de La Caravana debe
poder cerrarse con un expediente completo** — qué pasó en la ruta, qué se publicó, cuánto costó,
cuánto entró, y qué aprender para la próxima. Hoy esa información existe a medias (en la cabeza de
la Coordinadora, en el feed de Instagram, en una hoja de cálculo aparte) o no existe. El objetivo no
es construir cuatro sistemas sueltos, sino **cuatro módulos que alimentan un mismo informe final**.

Los módulos se apoyan en lo que ya existe (`ediciones`, `retos`, `entregas`, `patrocinantes`,
`propuestas`, el mapa de ruta con kilómetros) — no se duplica nada de eso, se conecta.

```
Oportunidades → Propuestas → Patrocinantes ─┐
Ediciones → Retos → Entregas ────────────────┼──► Informe de Cierre de Edición
Itinerario + Bitácora (nuevo) ───────────────┤        ├─ para el equipo (retrospectiva)
Calendario de Contenido (nuevo) ─────────────┤        ├─ para las marcas (.docx de resultados)
Finanzas: ingresos + egresos (nuevo) ────────┘        └─ para el Director General (rentabilidad)
```

---

## 2. Fundamento: roles y permisos (Super Admin)

Antes de construir los módulos operativos, hay que resolver quién puede ver qué — porque dos de
los módulos nuevos (Finanzas, y en menor medida la Bitácora) manejan información sensible.

**Cambio de arquitectura**: hoy los roles (`admin`, `comercial`, `productor`, `jurado`) son categorías
fijas con una lista de páginas permitidas escrita en el código. Se mantienen como plantillas, pero se
añade un nivel por encima:

- **Super Admin** (el Director General, hoy la misma persona que ya administra el sitio): puede
  ajustar el acceso de **cada usuario individual**, no solo por categoría. Puede darle o quitarle a una
  persona el acceso a una sección puntual sin inventarle un rol nuevo.
- El módulo de **Finanzas** nace visible **solo para Super Admin**. Un Admin normal no lo ve a
  menos que el Super Admin se lo habilite explícitamente persona por persona.
- Los demás módulos (Itinerario/Bitácora, Calendario de Contenido) usan permisos más abiertos,
  pensados para los roles operativos que se describen abajo.

**Roles nuevos a definir** (documentados como manual de cargo, igual que los 7 ya existentes en
`roles/`, y reflejados como roles del panel):

| Rol de panel | Corresponde a | Ve |
|---|---|---|
| `coordinador` | Coordinadora de Expedición | Itinerario, Bitácora, Checklist de entregables |
| `community` | Community Manager | Calendario de Contenido, métricas por pieza |
| `productor` *(ya existe)* | Productor Ejecutivo | Retos *(ya)* + Presupuesto/Costos de su(s) edición(es) |
| `finanzas` / Super Admin | Director General | Todo lo anterior + ingresos, egresos, rentabilidad, nómina |

---

## 3. Módulo — Itinerario y Bitácora de Edición

**Para quién**: Coordinadora de Expedición.
**Qué resuelve**: hoy la logística vive fuera del sistema (memoria, WhatsApp, Excel).

### Piezas

**Itinerario operativo** — un renglón por día de la edición, dentro de la edición ya existente:
- Fecha, parada/ciudad, horario de salida y llegada, hospedaje asignado, transporte, contacto local
  (nombre y teléfono de quien recibe al equipo en ese punto).
- Se apoya en las paradas que ya calcula `mapa-venezuela.php` (distancia, kilómetros) — no se
  inventa la ruta de nuevo, solo se le agrega la capa operativa (horarios, hospedaje, contacto) que
  hoy no tiene porque esa vista es pública, no de gestión.

**Bitácora diaria** — un registro corto por día:
- Qué pasó, incidencias, aprendizajes, pendientes. Texto libre, rápido de llenar desde el celular en
  ruta (nada de formularios largos — la Coordinadora no tiene tiempo para eso en movimiento).
- Esta es la pieza más valiosa de todo el plan para la retrospectiva: sin bitácora, cada edición
  "empieza de cero" en experiencia. Con ella, el Informe de Cierre puede mostrar un resumen real
  de los siete (o quince) días, no solo cifras.

**Checklist de entregables con patrocinantes**:
- Ligado a `retos_patrocinados` (campo que ya existe en `patrocinantes`): una lista de "compromisos"
  por marca (ej. reel 2 de 4, mención en el evento de cierre) con estado cumplido/pendiente.
- Esto es lo que hoy se controla a ojo y se le olvida a cualquiera cuando hay tres marcas a la vez.

---

## 4. Módulo — Calendario de Contenido

**Para quién**: Community Manager.
**Qué resuelve**: no existe un calendario editorial; hoy se publica sobre la marcha.

### Piezas

**Calendario por edición**:
- Qué se publica, qué día, en qué plataforma (Instagram, TikTok, YouTube Shorts), quién lo produce,
  y su estado (planeado → grabado → editado → publicado).
- Se vincula a los **retos y entregas ya existentes** en vez de duplicar información — un reto que ya
  tiene entrega aprobada puede convertirse en una fila del calendario automáticamente, y el
  Community Manager solo añade lo que no viene de un reto (piezas institucionales, detrás de
  cámaras, resúmenes).

**Métricas reales por pieza**:
- Alcance, guardados, comentarios, "me gusta" — capturados manualmente pieza por pieza (no hay
  conexión automática a Instagram/TikTok API en este alcance, es carga manual del Community
  Manager, igual que hoy revisa sus propias métricas).
- Esto es lo que convierte las cifras estáticas del mediakit y de las propuestas comerciales (que hoy
  viven en `propuesta_metricas` como números fijos del mediakit 2026) en **cifras reales por
  edición** — con el tiempo, el catálogo de métricas deja de ser "lo que dice el mediakit" y pasa a
  ser "lo que de verdad logramos edición tras edición", que es un argumento de venta mucho más
  fuerte para Propuestas.

---

## 5. Módulo — Finanzas (Presupuesto, Costos, Rentabilidad)

**Para quién**: Productor Ejecutivo (costos operativos) y Super Admin / Director General
(visión completa, incluida nómina).
**Qué resuelve**: hoy el ingreso ya se captura (Propuestas, Patrocinantes) pero no hay ningún
registro del egreso — sin eso, nadie sabe si una edición dejó ganancia o pérdida.

### Concepto único: movimientos financieros

Cada movimiento (ingreso o egreso) registra:

- **Tipo**: ingreso o egreso.
- **Categoría**: libre, definida por ustedes — ej. patrocinio, operativo, imprevisto, nómina,
  proveedor, producción, transporte, hospedaje.
- **Edición asociada** (opcional): si se asocia, alimenta la rentabilidad de esa edición; si no, es un
  gasto general de la empresa (sueldos fijos, herramientas, hosting del sitio).
- **Monto, moneda y tasa del día**: multi-moneda real. Cada movimiento guarda la tasa vigente
  al momento de registrarlo — no una tasa global del sistema — para que la rentabilidad histórica
  de una edición cerrada no se distorsione si la tasa cambia después. Las tasas se cargan a mano
  (sin conexión automática al BCV: es más seguro y toma segundos hacerlo manual).
- **Comprobante**: foto o archivo de factura, subido con el mismo mecanismo que ya usan Medios y
  los mockups de Propuestas (no es infraestructura nueva, es el mismo patrón aplicado aquí). Un
  egreso sin comprobante no se bloquea, pero queda marcado como "sin soporte" — visibilidad, no
  fricción.
- **Justificación**: una línea de texto obligatoria (para qué fue el gasto), independiente de si el
  comprobante ya está adjunto o llega después.

### Ingresos: se generan solos

Cuando una propuesta pasa a "aceptada" o se confirma un patrocinante, el ingreso correspondiente
ya no hay que teclearlo dos veces — se refleja automáticamente en Finanzas, ligado a su edición.

### Egresos: manuales, libres, con justificación

Transporte, hospedaje, producción, proveedores, imprevistos, nómina — todos se registran a mano,
sin restricción de quién puede registrar (dentro de su rol), pero **siempre con justificación**, y
nómina/sueldos visible únicamente a Super Admin.

### Salida

- **Rentabilidad por edición**: ingresos de esa edición (Propuestas + Patrocinantes) menos sus
  egresos asociados.
- **Vista general de la empresa**: incluye también los gastos que no pertenecen a ninguna edición
  puntual (sueldos fijos, herramientas).

---

## 6. La pieza que une todo: Informe de Cierre de Edición

Cuando una edición cambia su estado a **archivada**, el sistema arma automáticamente un informe
único, combinando:

- Kilómetros recorridos y paradas (ya existente, mapa de ruta).
- Retos cumplidos y creadores participantes (ya existente).
- Resumen de la bitácora de esos días (nuevo, módulo 3).
- Contenido publicado vs. planeado, con sus métricas reales (nuevo, módulo 4).
- Cumplimiento del checklist con cada patrocinante (nuevo, módulo 3).
- Rentabilidad: ingresos vs. egresos de esa edición (nuevo, módulo 5).

### Tres versiones del mismo dato, para tres públicos

| Para quién | Qué contiene | Cómo se entrega |
|---|---|---|
| El equipo | Todo lo anterior, sin filtrar — memoria institucional para decidir qué repetir y qué cortar | Vista dentro del panel |
| Las marcas | Solo lo que les compete: cumplimiento de sus entregables, contenido publicado, métricas de alcance de su patrocinio — sin cifras internas de rentabilidad | Documento `.docx` — **reutiliza exactamente el exportador que ya construimos para Propuestas** (mismo motor OOXML vía ZipArchive), ahora como "informe de resultados" en vez de cotización |
| El Director General | Todo lo anterior + rentabilidad neta, para decidir de un vistazo si la edición dejó ganancia | Vista dentro del panel, sección Finanzas |

Esto significa que el trabajo que ya invertimos en el exportador de Propuestas **no fue de un solo
uso** — es el motor de documentos de todo el sistema comercial y ahora también del cierre de
edición.

---

## 7. Cómo se conecta con lo ya construido

| Módulo nuevo | Se apoya en (ya existente) | Qué le agrega |
|---|---|---|
| Itinerario y Bitácora | `ediciones`, `mapa-venezuela.php` (paradas, km) | Capa operativa: horarios, hospedaje, contacto, incidencias |
| Calendario de Contenido | `retos`, `entregas` | Calendario editorial + métricas reales por pieza |
| Checklist de patrocinio | `patrocinantes.retos_patrocinados` | Estado cumplido/pendiente por compromiso |
| Finanzas | `propuestas`, `patrocinantes` (ya dan el ingreso) | El lado del egreso + rentabilidad |
| Informe de Cierre | Todo lo anterior + `propuesta-docx.php` (motor de documentos) | Un documento final por edición, en 3 versiones |
| Super Admin | `admin/guardia.php` (roles actuales) | Permisos finos por usuario, por encima de los roles |

---

## 8. Fases sugeridas (orden de construcción)

No es obligatorio este orden, pero minimiza dependencias:

1. **Super Admin y permisos por usuario** — es la base de seguridad que todo lo demás necesita
   (en particular, Finanzas no debería existir sin esto primero).
2. **Finanzas** (movimientos, multi-moneda, comprobantes) — el de mayor impacto inmediato: por
   primera vez se ve rentabilidad real por edición, sin depender de que los otros módulos existan.
3. **Itinerario y Bitácora** — el más fácil de empezar a usar desde ya, y el que más rápido genera
   contenido para el Informe de Cierre.
4. **Calendario de Contenido** — el que más se beneficia de que ya exista una edición con retos y
   entregas cargadas, así que tiene sentido después de que el ciclo operativo esté rodando.
5. **Informe de Cierre de Edición** — el que amarra todo; solo tiene sentido una vez que al menos
   Finanzas y Bitácora tengan datos reales que mostrar.

---

## 9. Decisiones abiertas (antes de construir)

- ¿Cuántas personas ocuparán los roles de Coordinadora y Community Manager desde ya — se
  crean sus usuarios en el panel de inmediato, o el módulo se construye primero y se asignan
  personas después?
- ¿La moneda de referencia para ver la rentabilidad consolidada es USD, o prefieres Bs?
- ¿Los "imprevistos" (egresos no planeados) necesitan aprobación previa del Director General antes
  de registrarse, o se registran libremente y se revisan después (como se había conversado)?
- ¿El Informe de Cierre para las marcas se genera automático al archivar la edición, o prefieres
  revisarlo y editarlo a mano antes de enviarlo (como ya pasa con las Propuestas)?
