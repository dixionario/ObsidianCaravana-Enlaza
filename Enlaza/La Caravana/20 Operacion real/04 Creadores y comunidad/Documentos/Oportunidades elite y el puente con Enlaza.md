---
tipo: puente
estado: CONFIRMADO
actualizado: 2026-08-22
fuente: "La Caravana/prueba/portal/gobernanza.php (lineas 26, 30, 70)"
---

# Oportunidades elite y el puente con Enlaza

Aqui es donde el modelo **laboratorio / canal** ([[Decisiones|EN-DEC-002]]) deja de ser teoria: ya esta implementado en el portal de pruebas.

Un creador sube por la escalera de La Caravana; al cruzar cierto umbral se le abre la puerta de Enlaza. La puerta abre una **evaluacion**, no un premio automatico.

## Lo que existe hoy en codigo

`CONFIRMADO` — dos oportunidades elite en `prueba/portal/gobernanza.php`:

| Oportunidad | Tipo | Umbral | Que desbloquea |
|---|---|---|---|
| Formacion privada con La Caravana | Desarrollo profesional | 5.000 ke, 300 kr, 50 misiones, 52 semanas | Evaluacion y seleccion del equipo |
| **Alianzas Enlaza** | Oportunidades comerciales | 8.000 ke, 800 kr, 75 misiones, 78 semanas | Validacion comercial y aprobacion del equipo |

La descripcion de Alianzas Enlaza en el propio codigo dice: elegibilidad para proyectos con marcas, hoteles, restaurantes, destinos y experiencias de viaje gestionadas por el ecosistema Enlaza.

El texto que ve el creador aclara que llegar a la meta abre una evaluacion y no garantiza contratos, viajes ni cupos; la afinidad profesional, la reputacion y la disponibilidad tambien cuentan.

## Estado de cada pieza

- Que estas oportunidades existen y estan implementadas: `CONFIRMADO`.
- Los umbrales concretos (ke, kr, misiones, semanas): `PROPUESTO`. Son configurables via `cfg('oportunidades_elite')`.
- Que Enlaza pueda cumplir su parte (marcas, aliados, contratos reales): `POR DEFINIR`. Depende de las 16 decisiones pendientes en [[00 Enlaza (inicio)]].

## Por que importa

Este es el unico punto del sistema donde La Caravana y Enlaza se tocan en produccion. El [[Roadmap (PROPUESTO)]] lo describe como el nucleo que no debe romperse: La Caravana adquiere, Enlaza prueba, la red activa, el desempeno genera reputacion, la reputacion desbloquea mas valor.
