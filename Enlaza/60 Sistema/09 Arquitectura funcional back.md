---
tipo: enlaza-contexto
estado: PROPUESTO
actualizado: 2026-08-22
fuente: "Documents/ENLAZA_Contexto_Maestro.md (v0.1, 2026-07-29)"
---
## 9. Arquitectura funcional: back

### 9.1 Dominios

| Dominio | Responsabilidad |
|---|---|
| Identidad | Registro, acceso, perfiles, roles y permisos. |
| Organizaciones | Entidades, representantes, equipos y documentación. |
| Comunidad | Grupos, relaciones, intereses e interacciones. |
| Oportunidades | Convocatorias, ofertas, recursos y elegibilidad. |
| Programas | Ciclos, cohortes, actividades y resultados. |
| Eventos | Agenda, espacios, capacidad y asistencia. |
| Contenido | Publicación, versiones, categorías e idiomas. |
| Comercio | Precios, órdenes, pagos, reembolsos y liquidaciones. |
| CRM | Contactos, segmentos, campañas y seguimiento. |
| Alianzas | Pipeline, acuerdos, obligaciones y desempeño. |
| Soporte | Casos, incidentes, escalamiento y resolución. |
| Datos | Eventos, métricas, cohortes y reportes. |

### 9.2 Entidades mínimas

- persona;
- cuenta;
- rol;
- organización;
- representante;
- relación;
- consentimiento;
- programa;
- oportunidad;
- convocatoria;
- solicitud;
- evento;
- sesión;
- reserva;
- asistencia;
- contenido;
- aliado;
- acuerdo;
- producto;
- orden;
- pago;
- interacción;
- ticket;
- incidente;
- métrica;
- experimento.

### 9.3 Reglas técnicas

- fuente de verdad definida por entidad;
- APIs versionadas;
- operaciones críticas idempotentes;
- historial auditable;
- separación entre datos personales y analíticos;
- control de acceso por mínimo privilegio;
- eventos de dominio;
- integraciones desacopladas;
- recuperación ante fallas;
- trazabilidad de cambios.

---

---
Parte de [[00 Enlaza (inicio)]]. Documento original: v0.1, 2026-07-29.
