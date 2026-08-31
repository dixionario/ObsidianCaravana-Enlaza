---
tipo: decisiones
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/prueba/GOBERNANZA-CREADORES-DECISIONES.md (2026-08-20)"
---

# Gobernanza de creadores — decisiones consolidadas

Fecha: 20-08-2026

## Capas que nunca se mezclan

1. Estado operativo: Convocado, Clasificado, Convocado por inactividad, Suspendido.
2. Cantera: KE y rangos Andariego, Trochero, Rutero y Baquiano.
3. Expedición: KR por edición y rangos Pasajero, Copiloto, Piloto y Campeón.
4. Distinciones: Elegido por edición y Leyenda como corona histórica.

Los KR nunca se suman a los KE. Cada nueva edición empieza en Pasajero, pero el rango de Ruta alcanzado queda en el historial de esa edición.

## Escalera inicial de Cantera

| Rango | KE | Misiones | Semanas activas | Aprobación |
|---|---:|---:|---:|---|
| Andariego | 0 | 0 | 0 | Automática |
| Trochero | 250 | 4 | 4 | Automática |
| Rutero | 900 | 12 | 12 | Automática |
| Baquiano | 2.500 | 30 | 26 | Equipo |

Todo es configurable en Administración → Rangos → Escalera.

## Escalera inicial de Ruta

| Rango | Porcentaje del KR disponible |
|---|---:|
| Pasajero | 0% |
| Copiloto | 25% |
| Piloto | 55% |
| Campeón | 80% |

Los umbrales se calculan sobre el KR disponible en cada edición y se administran en Rangos → Rangos de Ruta.

Campeón es un rango de desempeño; puede haber varios. Leyenda es la corona del resultado final de la edición.

## Recompensas y dificultad

Cada actividad KE tiene base, dificultad D1–D5, tope diario y tope semanal. Los multiplicadores iniciales son 1, 1,5, 2, 3 y 4. Los cambios afectan eventos futuros; el ledger histórico no se reescribe.

Los retos mantienen su recompensa KR configurable por edición. El ledger KR aditivo queda disponible para bonificaciones, penalizaciones y otras fuentes idempotentes.

## Actividad y rachas

- Activo: al menos una actividad válida durante los últimos 7 días.
- 8–29 días: inactivo; rango y privilegios congelados.
- 30–59 días: Convocado por inactividad.
- 60 días: Suspendido por inactividad.

Estos estados son operativos y reversibles. No borran KE, rangos, participaciones, Elegidos ni Leyendas, y no sobrescriben las filas históricas de `cv_applications`.

Una semana cuenta si existe una actividad válida: misión, colaboración, laboratorio, voto o aporte aprobado. Iniciar sesión no basta.

Hitos iniciales: 4 semanas = 20 KE; 8 = 40 KE; 12 = 60 KE. Cada hito es idempotente.

## Implementación

- Motor: `prueba/sitio/includes/gobernanza-creador.php`.
- Instalador idempotente: `prueba/sitio/admin/instalar-gobernanza-creadores.php`.
- Gobierno: pestañas de `prueba/sitio/admin/rangos.php`.
- Portal: `prueba/portal/inicio.php` y `prueba/portal/nivel.php`.
