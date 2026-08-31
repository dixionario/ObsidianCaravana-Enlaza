---
tipo: historia-de-proyecto
estado: SINTESIS_VERIFICADA
actualizado: 2026-08-23
tags:
  - la-caravana
  - hitos
  - enlaza
---

# Historia e hitos de La Caravana

## Qué está naciendo

La Caravana es una plataforma itinerante de contenido, cultura, turismo y entretenimiento que recorre Venezuela con creadores seleccionados. Su promesa pública es:

> **Venezuela sobre ruedas, contada por quienes la viven.**

Nació como expedición y plataforma de contenido, pero el trabajo de 2026 la convirtió también en un sistema de convocatoria, formación, comunidad, evaluación, producción, gobernanza y operación comercial. Ese cambio explica por qué hoy funciona como MVP de [[01 Ecosistema ENLAZA - visión ejecutiva|ENLAZA]].

## Hitos de definición y producto

### Julio de 2026 — identidad comercial y arquitectura

- Se documentó La Caravana como recorrido a bordo de tres camionetas Maxus D90, con creadores, retos, rutas e integración orgánica de marcas.
- Se estructuraron cinco familias de ediciones: Ciudades, Playas, Llanos, Andes y una edición especial; después se incorporó la Navideña como quinta identidad canónica del catálogo.
- Se construyó el sistema comercial de propuestas: embudo, enlaces privados por token, constructor de líneas, respuestas de la marca, métricas de vistas, exportación Word y rotulador visual de los vehículos.
- Se diseñó la página de ruta personalizada para ediciones de marca.
- Se formularon roles de producción y un borrador societario, todavía sin participaciones definitivas.

### Primera mitad de agosto — operación y portal

- Se consolidaron panel administrativo, sitio público y Portal del creador.
- El panel se reorganizó por intención: Resumen, Comercial, Producción, Contenido y Sistema.
- Se creó una capa visual compartida `ui-` para reducir inconsistencias y corregir defectos en componentes comunes.
- Se diseñaron planes comercial, contable, financiero y operativo.
- Se propuso El Campamento como comunidad privada vinculada a la expedición.

### 19–20 de agosto — el sistema de creadores toma forma

- Se formalizó el camino postulado → convocado → clasificado → elegido.
- Se diseñaron misiones espejo para que la comunidad participe desde cada territorio sin competir con los diez Elegidos.
- Se construyó el sistema Kr/Ke: experiencia verificable, ruta, escalera de rangos, ligas y corona de Leyenda por edición.
- Se añadieron gobernanza, participación, verificación privada de identidad, supervisión administrativa y trazabilidad.
- El catálogo de ediciones adoptó el concepto “Cinco rutas, un mismo país”.
- La edición Ciudades quedó encendida en el clon con diez Elegidos, cuatro paradas provisionales y fechas; los retos continuaban pendientes de contenido.

### 20 de agosto — auditoría integral

- 401 archivos PHP pasaron lint de PHP 8.1 sin errores.
- 84 recorridos del Portal —21 pantallas por cuatro perfiles— quedaron en verde.
- 49 pantallas del panel fueron verificadas con sesión.
- El crawl público de 20 páginas no mostró errores ni avisos.
- Dos pasadas de instaladores confirmaron idempotencia sin duplicar filas.
- La auditoría detectó que el empaquetado manual cubría solo 65 de 168 archivos necesarios. Se creó un empaquetador por diferencias con cerrojos contra configuraciones locales, datos y escrituras prohibidas sobre creadores.

### 22 de agosto — preparación de actualización

- Se generó un manifiesto de actualización con 167 archivos: 128 del sitio y 39 del Portal.
- Se consolidó `admin/actualizador.php` para ejecutar instaladores en orden y detectar lo ya instalado.
- Se documentaron acceso a pruebas online, coherencia de edición activa, fechas de llegada y salida e iconos por parada.

El manifiesto prueba preparación de paquete, no despliegue en producción.

### 23 de agosto — experiencia pública más viva

- El reloj dejó de morir al llegar a cero: ahora pasa automáticamente de cuenta regresiva a “Convoy en movimiento” y cuenta el tiempo en ruta hasta el regreso.
- Se compactó la votación de escritorio a dos filas de cinco creadores para mantener selección y confirmación en una pantalla.
- El hero de la expedición eliminó el mapa y conservó una banda de ciudades, decisión tomada por legibilidad móvil y claridad de ruta.
- La variante sin mapa fue verificada en móvil y varios tamaños de escritorio sin solapamientos ni scroll horizontal.

## El cambio más importante

El verdadero hito no es una sola pantalla. Es el paso de una experiencia episódica a una infraestructura repetible:

```text
antes: convocatoria → viaje → contenido

ahora: convocatoria → expediente → formación → selección
      → viaje → desempeño → reputación → oportunidad
```

## Principios que ya gobiernan el proyecto

- Los datos reales solo bajan a pruebas; nunca se suben desde local.
- Los registros de creadores de producción son intocables.
- Todo se prueba localmente antes de empaquetar.
- Los instaladores deben ser idempotentes.
- Los diez Elegidos protagonizan la expedición; la comunidad amplía la ruta.
- La colaboración y el desempeño valen más que la popularidad aislada.
- Una misión debe producir aprendizaje, historia u oportunidad.
- La reputación debe basarse en acciones verificables.
- Las mejoras visuales se resuelven en capas compartidas.

## Hito pendiente que desbloquea la ruta

Los retos de la edición Ciudades siguen siendo el bloque funcional decisivo: sin retos publicados no hay Kr efectivos, competencia de ruta ni coronación basada en desempeño real.

