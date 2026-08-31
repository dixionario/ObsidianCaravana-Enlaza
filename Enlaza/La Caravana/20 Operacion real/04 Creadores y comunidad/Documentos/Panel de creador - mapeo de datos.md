---
tipo: plan
estado: PROPUESTO
actualizado: 2026-08-22
fuente: "La Caravana/prueba/PLAN-PANEL-CREADOR-EN-PRUEBAS.md (2026-08-19)"
---

# Panel de creador — mapeo de datos

Primera entrega: qué dato de cada maqueta sale de dónde, qué falta, y qué es decorado.
Sin tocar código todavía.

Todo esto se construye y se valida en la **Zona de pruebas**. De migrar al sistema real se habla cuando el trabajo esté verificado.

---

## Sobre qué portal se construye

Se trabaja sobre **`creadores-app`**, el Portal de correo y clave, no sobre `portal.php`.

Los dos documentos de partida (`PLAN-PANEL-CREADOR-MVP.md` y `PROMPT-CLAUDE-PANEL-CREADOR.md`) nombran `portal.php`, `api/entrega.php` y `includes/nav-creador.php`, y dicen "no crear usuario ni contraseña". Eso se escribió para el portal viejo por enlace personal. Las cinco maquetas, en cambio, muestran la barra inferior de cinco secciones y el porcentaje de perfil, que son de `creadores-app`.

Conviene saber que **el Portal nuevo ya toca los dos sistemas**, así que no hay que elegir uno y perder el otro:

| Pantalla | Tablas | Para quién |
|---|---|---|
| `mision.php` | `missions`, `mission_submissions` | Convocado: misión de preselección |
| `retos.php` | `retos`, `entregas`, `iniciativas` | Elegido: retos durante la expedición |

Son dos fases distintas de la misma persona, no dos sistemas que compitan. Las maquetas 2 y 5 ("Misión 01") son la fase de preselección.

---

## Lo que ya existe y no hay que construir

| Dato | De dónde sale |
|---|---|
| Nombre, ciudad, especialidad, redes | `creadores` |
| Porcentaje de perfil | `creadores.profile_completion` |
| Estado del creador (Convocado, Elegido...) | `applications.status` |
| Marca permanente de Clasificado | `creadores.clasificado_en` |
| Edición y sus fechas | `ediciones` |
| Misión: número, nombre, objetivo, instrucciones, fecha límite, video briefing | `missions` |
| Entrega de la misión | `mission_submissions` |
| Retos: día, título, consigna, reglas, criterios, kilómetros, plataformas, apertura y cierre | `retos` |
| Entregas de reto: enlaces de Instagram y TikTok, comentario, estado, kilómetros, si llegó tarde | `entregas` |
| Etiquetas: skills, expertise, estilo, intereses | `tags`, `creador_tags` |
| Experiencia con marcas y exclusividades | `creador_marcas`, `creador_exclusividades` |
| Mensajes del equipo | ya montado en `mensajes.php` |
| Preguntas frecuentes | ya montado en `faq.php` |

Un hallazgo que vale la pena subrayar: `missions` ya trae `requisito_application_status` y `requisito_profile_status`. **El sistema ya sabe bloquear una misión según el estado del creador y lo completo que tenga el perfil.** Eso enlaza directo con el conmutador de modos de la zona de pruebas: cambias de modo y la misión se bloquea o se abre sola.

---

## Maqueta por maqueta

### 4. Inicio

| Elemento | Estado |
|---|---|
| Hero con imagen de la edición | Falta el campo de imagen en `ediciones`, pero ya hay `imagen` y `video` |
| Estado actual (CONVOCADO) | Existe |
| Misión activa + cuenta regresiva | Existe (`missions.fecha_limite`) |
| Ruta con ciudades completadas | Parcial: `inicio.php` ya pinta `pa-camino`. Las ciudades salen de `ediciones.estados` |
| Tu avance (Perfil, Misión, Verificación, Selección) | Existe |
| Próximas acciones | Se compone de lo anterior, no necesita tabla |
| Cifras: postulados, convocados, elegidos | **Calculables** con `COUNT` sobre `applications`. No hace falta inventarlas |
| Recompensa "Orquídea" | Falta. Ver más abajo |

`inicio.php` ya tiene 17 KB con hero, ficha, camino y progreso. Es rediseño, no obra nueva.

### 2. Misión

| Elemento | Estado |
|---|---|
| Número, nombre, objetivo, instrucciones | Existe en `missions` |
| Fecha límite y cuenta regresiva | Existe |
| Imagen del destino | Falta columna en `missions` |
| Requisitos (lista con tildes) | Hoy es texto libre en `instrucciones`. Para pintarlos como lista hace falta separarlos |
| Pasos de la misión (1, 2, 3 con progreso) | Falta. Hoy la entrega es un solo paso |
| Subir enlace | Existe en `mission_submissions` |
| Estado y retroalimentación del equipo | Existe parcialmente. Hay `instalar-calificacion-mision.php` |
| Recompensa "Orquídea" | Falta |

### 5. Perfil

| Elemento | Estado |
|---|---|
| Retrato, nombre, especialidad, ciudad | Existe |
| Insignia de estado (CONVOCADO) | Existe |
| Porcentaje de perfil | Existe |
| Datos personales, redes | Existe |
| Skills con nivel (puntos) | Existe: `creador_tags.nivel` ya guarda nivel |
| Disponibilidad | Existe |
| Áreas de interés | Existe |
| **Equipo de grabación** (cámara, dron, micrófono) | **Falta.** Tabla nueva |
| **Verificación por niveles** | **Falta.** Hoy `profile_status` solo tiene tres valores |
| **Logros** (convocados, misiones, reconocimientos) | Parcial: calculables, salvo "reconocimientos" |
| **Beneficios activos** | **Falta.** Tabla nueva o texto por edición |

`perfil.php` ya tiene 24 KB. Es la pantalla más completa que existe.

### 1. Comunidad

**No existe nada.** Es la sección más nueva de las cinco.

| Elemento | Qué haría falta |
|---|---|
| Novedades | Tabla nueva, o derivarlo de la actividad ya registrada |
| Foro de retos | Tabla nueva de publicaciones y comentarios |
| Actividad reciente | Derivable de `entregas` y `mission_submissions` |
| Ranking de kilómetros | **Calculable ya**: `entregas.kilometros` sumados por creador |
| Próximo encuentro en vivo | Existe `agenda` en el sitio |

El ranking de kilómetros es el que más rendimiento da por menos trabajo: el dato ya está, solo falta la consulta y la pantalla.

### 3. Recursos

**No existe nada.**

| Elemento | Qué haría falta |
|---|---|
| Brief oficial (PDF descargable) | Tabla nueva de recursos por edición, o reusar `medios` |
| Guía de grabación, checklist técnico | Lo mismo |
| Preguntas frecuentes | **Ya existe** (`faq.php`) |
| Calendario de briefings | Existe `agenda` en el sitio |
| Beneficios del creador | Falta |

Buena parte de Recursos es una pantalla que agrupa cosas que ya están dispersas, más una tabla de documentos por edición.

---

## Lo que las maquetas muestran y no debe construirse tal cual

Estos datos son de relleno de la maqueta y no deben pasar a pantalla como cifras fijas:

- "3.000 Convocados", "+15 mil postulados", "10 Elegidos" — se calculan de la base o no se muestran
- "1.250 km" de Carlos M. y los nombres del ranking — el ranking es real, los nombres no
- "Nivel de verificación: Avanzado" — no existe ese sistema de niveles
- "Alejandro Martínez", "@alejandromx", "Sony A7 III" — datos de ejemplo

---

## Orden propuesto

Ordenado por relación entre lo que se gana y lo que cuesta, no por el orden de las maquetas.

**1. Inicio y Misión.** Son las dos pantallas donde el creador entra todos los días, ya existen, y el rediseño se apoya en datos que ya están. Aquí se nota el salto visual entero.

**2. Perfil.** Ya tiene casi todo. Se reorganiza según la maqueta y se deja fuera lo que no tiene respaldo.

**3. Recursos.** Autocontenida y de bajo riesgo. Necesita una tabla de documentos por edición y su pantalla en el panel.

**4. Comunidad.** La más grande y la única sin nada construido. Empezar por el ranking de kilómetros, que ya tiene el dato.

**5. Lo que necesita tabla nueva**: equipo de grabación, beneficios, niveles de verificación, recompensas. Cada uno con su migración y su espejo en el panel.

---

## Decisiones que hacen falta antes de construir

1. **Recompensas ("Orquídea").** Aparece en dos maquetas y no existe en la base. ¿Es un sistema de insignias por misión cumplida, o el nombre de la edición?

2. **Pasos de la misión.** Las maquetas muestran tres pasos con progreso. Hoy la entrega es un solo acto. ¿Se parten en pasos reales que se guardan, o es solo una forma de dibujar el estado actual?

3. **Comunidad.** Es lo más caro de todo. ¿Entra en esta vuelta o se deja para después de validar las otras cuatro?
