---
tipo: plan
estado: PROPUESTO
actualizado: 2026-08-22
fuente: "La Caravana/La Caravana/PLAN-COMERCIAL-Y-CONTABLE.md (2026-08-13)"
---

# La Caravana — Del prospecto al balance

Plan para cerrar el circuito comercial y financiero. 13 de agosto de 2026.

Este documento parte de un inventario real del código, no de una idea nueva. La conclusión
principal es que **la mayor parte ya está construida**, pero las piezas no se hablan entre sí y hay
un error en el modelo contable que hace que Finanzas hoy diga cosas que no son ciertas.

---

## 1. Lo que ya existe (y probablemente no sabías que existe)

| Pieza | Dónde vive | Estado |
|---|---|---|
| Planes de patrocinio | `cv_planes_patrocinio` | Existe, pero **no tiene pantalla propia**: se editan escondidos dentro de Propuestas |
| Espacios del vehículo | `cv_espacios_vehiculo` + `cv_edicion_espacios` | Inventario por edición, con estados disponible / reservada / confirmada |
| Propuestas | `cv_propuestas` + `cv_propuesta_items` | Completo: enlace público con token, la marca acepta, pide cambios o rechaza, y todo queda en historial |
| Multi-moneda | `cv_tasas_cambio` | Correcto: cada movimiento guarda **la tasa del día**, no una tasa global |
| Movimientos financieros | `cv_movimientos_financieros` | Con `origen_tipo` / `origen_id`, o sea trazables hasta su causa |
| Ingreso automático | `registrarIngresoAutomatico()` | Al aceptar una propuesta ya crea el ingreso |
| Prospecto → propuesta | `cv_propuestas.oportunidad_id` | El campo del vínculo existe |
| Contratos | `cv_documentos_legales` + `cv_contratos_creador` | Solo para creadores. Para marcas no hay nada |

Tu frase "creo que ya tenemos algo así pero todo muy suelto" es exacta. El caso más claro: los
planes de patrocinio, que son el corazón de lo comercial, no tienen pantalla propia.

---

## 2. El error que hay que corregir primero

Hoy, cuando una marca acepta una propuesta, el sistema registra un **ingreso** y ese ingreso cuenta
como dinero disponible. Pero aceptar no es pagar.

Eso significa que **Finanzas ahora mismo puede estar diciendo que tienes dinero que nadie te ha
pagado todavía.** Es justo lo que intuiste al escribir "hasta que su estatus no sea pagado no
comienza a ser un activo".

La lógica contable correcta (contabilidad de devengo, que es el estándar):

- **Cuando la marca acepta**: se reconoce el ingreso (ya te ganaste el derecho a cobrar) **y**
  nace una **cuenta por cobrar**, que es un activo, pero un activo que todavía no puedes gastar.
- **Cuando la marca paga**: la cuenta por cobrar baja y sube **caja o banco**. El ingreso no se
  vuelve a tocar, porque ya se había reconocido.

Con esa separación, cada informe responde una pregunta distinta y ninguna se contamina:

- **Pérdidas y ganancias** responde: ¿este negocio gana o pierde?
- **Balance general** responde: ¿dónde está el dinero, qué tenemos y quién nos debe?

Sin esa separación, las dos preguntas se mezclan y ninguna tiene respuesta confiable.

---

## 3. Lo que hay que construir

### A. Paquetes — pantalla propia (el nombre corto)

Hoy los planes viven escondidos. Necesitan su propia zona, dentro de Comercial.

**Nombre recomendado: "Paquetes".** Corto, comercial, y no se confunde con "planes" (que en el
panel ya suena a planificación de edición). Alternativas si prefieres: *Tarifas* (más frío, más
claro sobre precios) o *Catálogo* (más amplio, si algún día vendes algo que no sea patrocinio).

Qué tendría:

- Crear y editar paquetes con su precio de lista y su moneda.
- **Servicios incluidos como líneas sueltas**, no como un párrafo de texto. Hoy `descripcion` es
  texto libre; hace falta una tabla `cv_plan_items` para que cada servicio sea una cosa con nombre,
  cantidad y precio. Esto es lo que permite lo siguiente.
- Al armar una propuesta, se parte de un paquete y **se modifica libremente**: quitar servicios,
  añadir otros, cambiar precios. La propuesta guarda de qué paquete salió, pero vive su propia vida.
  La estructura para esto ya existe (`propuesta_items.origen_tipo` / `origen_id`); falta el flujo.

Esto responde directamente a tu observación de que todo es negociación y el plan se modifica no
solo en precio sino en servicios.

### B. De propuesta aceptada a cliente

Cuando la marca acepta, hoy pasa poco. Debería pasar todo esto de una vez:

1. Se crea o actualiza su ficha en **Patrocinantes** (la tabla ya existe).
2. Se genera el **contrato de patrocinio** (ver punto C).
3. Se crea el **plan de cobro** con sus cuotas (ver punto D).
4. El prospecto del embudo pasa a **Ganado**.

### C. Contrato de patrocinio

No hay que inventar nada: se copia el patrón que ya funciona con creadores. Una **plantilla
editable** desde el panel (como `documentos_legales`), y al aceptar se genera una **instancia
congelada** con los datos de esa marca, ese paquete y esos montos (como `contratos_creador`).

La marca lo ve y lo acepta desde el mismo enlace con token que ya usa para la propuesta, así que la
aceptación queda registrada con fecha, nombre y comentario. Eso es exactamente tu "que el cliente
vea que todo es serio y todo queda por escrito".

Nueva tabla: `cv_contratos_marca`.

### D. Cobros y pagos, en varias monedas

Aquí está lo que hoy no existe en absoluto. Dos tablas:

- **`cv_cobros`** — las cuotas acordadas. Cada una con su monto, moneda, fecha de vencimiento y de
  qué propuesta viene. Permite "50% al firmar y 50% al terminar la edición".
- **`cv_pagos`** — lo que de verdad entró. Cada pago con su monto, **su moneda y la tasa de ese
  día**, su fecha, su método y su comprobante.

Esto resuelve tu caso literal: una marca paga una parte en divisas y otra en bolívares. Cada pago
guarda su propia tasa, así que la conversión histórica no se distorsiona cuando la tasa cambie
después.

Estado de cada cliente, calculado y no tecleado: **al día**, **debe**, o **vencido**.

### E. Finanzas: los dos estados que pediste

**Pérdidas y ganancias.** Ingresos devengados menos egresos, por periodo y por edición. La base ya
está (`rentabilidadEdicion()`), falta presentarla como un estado de verdad, con sus categorías.

**Balance general.** Lo que no existe:

- **Activos**: caja y banco por moneda, cuentas por cobrar, e inventario.
- **Pasivos**: cuentas por pagar (a proveedores, a creadores).
- **Patrimonio**: la diferencia.

**Inventario de activos** (nueva tabla `cv_activos`): las tres camionetas, cámaras, drones,
equipos. Con su valor de compra, su fecha y su depreciación si la quieren llevar. Es lo que
convierte el balance en algo real y no solo en un saldo de caja.

---

## 4. El hilo completo, cuando esté terminado

```
Formulario de marca
      ↓
Cliente potencial (embudo)
      ↓  ya construido
Reunión agendada  →  Google Calendar + invitación
      ↓
Propuesta armada desde un Paquete, modificada a gusto
      ↓
La marca la abre con su enlace y acepta
      ↓
Patrocinante + Contrato + Cuotas por cobrar + Prospecto en Ganado
      ↓
Pagos (USD, Bs, parciales, cada uno con su tasa)
      ↓
Pérdidas y ganancias  ·  Balance general
```

---

## 5. Orden de construcción

Cada fase deja el sistema utilizable; ninguna depende de que la siguiente exista.

**Fase 1 — Paquetes y el arreglo contable.**
Pantalla propia de Paquetes con servicios por línea. Y separar lo devengado de lo cobrado en
Finanzas. Es la fase más importante porque corrige un dato que hoy es engañoso.

**Fase 2 — Cobros y pagos.**
Cuotas, pagos parciales, multi-moneda con tasa por pago, y el estado de deuda de cada cliente.

**Fase 3 — Contrato de patrocinio.**
Plantilla editable, instancia congelada, aceptación por token.

**Fase 4 — Pérdidas y ganancias, y Balance general.**
Los dos estados, con el inventario de activos.

**Fase 5 — El cierre del circuito.**
Que aceptar una propuesta dispare todo lo anterior en un solo paso.

---

## 6. Decisiones tomadas (13 de agosto de 2026)

**El nombre corto: Paquetes.** Así queda en el menú, en las tablas y en el código.

**La moneda base: el dólar.** Coincide con lo que ya está construido (`movimientos_financieros`
guarda `monto_usd`), así que no hay nada que migrar. Lo que sí hay que corregir es el euro como
moneda por defecto en paquetes y espacios del vehículo, que era la inconsistencia.

**Varias referencias de dólar, tabla propia.** En Venezuela no hay una sola tasa: conviven el
BCV, el Binance y el euro, y cuál se aplica depende del cliente y del acuerdo. El modelo pasa de
*una tasa por moneda* a *una tasa por moneda y referencia*:

- `cv_referencias_tasa` — el catálogo, que ustedes definen. Se siembra con BCV, Binance y Euro,
  pero pueden crear las que hagan falta.
- `cv_tasas_cambio` gana una columna `referencia`. Cada día se puede cargar el BCV y el Binance
  por separado, y ambos conviven.
- Cada movimiento y cada pago guarda **qué referencia usó y con qué valor**, no solo el número.
  Así, meses después, se puede explicar por qué un cobro se convirtió como se convirtió.
- Se elige una referencia por defecto para lo que no especifique otra cosa.

**Cuándo se reconoce el ingreso: al aceptar la propuesta** (devengo). Es lo estándar y es lo que
permite saber cuánto te deben. Si prefieres base caja, hay que decirlo antes de la Fase 2.

---

## 7. Lo que este plan no hace

No convierte el panel en un software de contabilidad completo. No hay libro diario, ni asientos de
partida doble, ni conciliación bancaria, ni declaración de impuestos. Lo que hace es que los
números que ya genera el negocio sean ciertos y estén conectados entre sí.

Si en algún momento hace falta contabilidad formal para un contador externo, lo que se construya
aquí sirve como fuente de datos para exportar, no como reemplazo.
