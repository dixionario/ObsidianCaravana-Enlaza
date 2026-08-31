---
tipo: glosario
estado: CONFIRMADO
actualizado: 2026-08-22
---

# Glosario de creadores

Los terminos del ecosistema, en un solo lugar. Antes estaban dispersos entre cinco planes.

| Termino | Que es |
|---|---|
| **Cantera** | La capa de entrada. Creadores que participan sin haber sido seleccionados para la expedicion. Tiene su propia escalera de progreso. |
| **Ruta** | La capa de los seleccionados que viajan con la expedicion. Escalera distinta y mas exigente que la de Cantera. |
| **Kilometros** | La moneda de progreso. Se acumulan por misiones cumplidas, constancia y participacion. Hay kilometros de Cantera (ke) y de Ruta (kr), y no se mezclan. |
| **Misiones** | Los encargos concretos que un creador cumple. Unidad basica de actividad del portal. |
| **Misiones espejo** | Misiones que la comunidad de Cantera ejecuta en paralelo a lo que hace la expedicion en carretera. Es el mecanismo que conecta a quien no viaja con la ruta real. |
| **Orquidea** y **Turpial** | Las dos insignias del sistema de reconocimiento. |
| **Los Elegidos** | Los creadores que la votacion publica selecciona para la ruta. |
| **Consejo** | Organo consultivo de creadores. Representa y propone; no decide presupuestos, contratos, seguridad, seleccion final ni asuntos legales. |
| **Rachas** | Actividad sostenida en el tiempo, que alimenta el progreso. |

## Los dos sistemas de creador, que no se mezclan

Comparten la tabla `creadores` en base de datos, pero son procesos distintos:

1. **Retos y expedicion** — acceso por token, publico, con votacion. Es el sistema de la convocatoria y Los Elegidos.
2. **Portal de creadores** — acceso por email y clave, con evaluacion de preseleccion. Es el sistema del panel y las misiones.

Confundir sus campos de estado rompe los dos. Cualquier cambio a un campo compartido exige buscar todos sus usos antes.

## Donde vive cada cosa

- Reglas y escaleras: [[Gobernanza de creadores - decisiones]]
- La logica completa: [[La logica del ecosistema de creadores]]
- La comunidad: [[El Campamento - MVP de comunidad]]
- El panel: [[Panel de creador - MVP operativo]] y [[Panel de creador - mapeo de datos]]
- El puente con Enlaza: [[Oportunidades elite y el puente con Enlaza]]
