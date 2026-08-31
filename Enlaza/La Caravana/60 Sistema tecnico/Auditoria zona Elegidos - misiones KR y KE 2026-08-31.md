---
tipo: auditoria
estado: PROPUESTA
fecha: 2026-08-31
---

# Auditoría zona Elegidos: misiones, KR, KE y selección final

## Resultado ejecutivo

El sistema contiene dos circuitos distintos:

- Misiones de Cantera/plataforma: usan `cv_missions` y `cv_mission_submissions`; entregan KE.
- Misiones de Ruta para Elegidos: usan `cv_retos` y `cv_entregas`; entregan KR después de revisión del equipo.

La selección final vigente no usa KR ni KE. Reparte el resultado entre Audiencia 10%, Cantera 30%, Jurado 20% y Dirección 40%.

## Hallazgos

1. Un Reto activo aparece para los Elegidos de la edición y el Portal valida plataforma, enlaces, plazo y CSRF. La entrega queda pendiente con 0 KR.
2. El equipo debe aprobar o rechazar cada entrega de Ruta. Al aprobar puede mantener o cambiar el KR base. Por tanto, el KR de Ruta no es automático.
3. Activar/publicar una misión o reto no crea actualmente una notificación para todos los destinatarios. Aparece en el Portal, pero no «les llega» por el Centro de notificaciones.
4. Las Misiones de Cantera sí pagan KE automáticamente por entregar. Después de 10 minutos, la evaluación automática marca la entrega como evaluada, le asigna al azar Inspirador/Emocionante/Valioso y paga el mismo KE de misión calificada. Esta etiqueta no mide calidad real.
5. El KE es experiencia y constancia. Si todos cumplen lo mismo, el empate es correcto y no impide distinguir al ganador porque KE no forma parte de la selección final.
6. El KR sí diferencia solo cuando el equipo ajusta la recompensa por calidad. Si se aprueba siempre el KR base, quienes completen todo quedan iguales.
7. El tratamiento de tardías no es uniforme: la tabla pública puede excluir KR tardío, mientras la Liga de Ruta y algunos resúmenes pueden conservarlo. Debe haber una sola regla compartida.
8. `admin/retos.php` procesa POST sin la validación CSRF obligatoria del panel.
9. En el mundo sembrado había 9 applications con estado Elegido, no 10; 11 misiones de video publicadas; el único Reto activo ya tenía fecha de cierre vencida. Un estado `activo` vencido aparece en la lista pero ya no admite entrega.

## Regla recomendada

- KE permanece fuera de la elección final. Mide trayectoria, actividad y desbloqueos.
- KR entra como bloque explícito de desempeño en ruta con 30% del resultado final.
- Pesos propuestos: KR 30%, Jurado 25%, Dirección 25%, Cantera 10%, Audiencia 10%.
- El KR de cada misión se calcula contra una rúbrica de 100 puntos: Narrativa 25, Ejecución 20, Autenticidad 20, Cumplimiento de consigna 20 y Evolución 15.
- Fórmula: `KR otorgado = KR base × nota de rúbrica / 100`.
- Entrega a tiempo es condición, no calidad: a tiempo conserva el resultado; tarde aplica 0 KR salvo excepción manual motivada. El sistema automático puede validar plazo, enlace y existencia, pero nunca inventar una nota creativa.
- La revisión humana es obligatoria antes de acreditar KR. Debe guardar desglose, revisor, fecha y motivo de cualquier excepción.
- La comparación final usa porcentaje de KR obtenido sobre KR disponible, para no favorecer a quien tuvo acceso a más misiones o bonus.
- Si todos logran el mismo porcentaje KR, el bloque empata y deciden Jurado/Dirección/votos; no debe fabricarse una diferencia artificial.

## Trabajo técnico pendiente si se aprueba

- Notificar a los Elegidos al activar un Reto y registrar destinatarios.
- Añadir rúbrica por entrega y auditoría del cálculo de KR.
- Unificar la regla de tardías en todos los rankings y resúmenes.
- Incorporar KR como quinto bloque en Selección final y explicar la fórmula en pantalla.
- Desactivar la calificación creativa aleatoria; conservar solo automatizaciones objetivas.
- Añadir CSRF a todos los POST de `admin/retos.php`.
- Probar el flujo completo con diez Elegidos y una misión abierta vigente.
