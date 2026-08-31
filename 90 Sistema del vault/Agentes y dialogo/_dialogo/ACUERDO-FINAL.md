---
tipo: acuerdo
estado: CONFIRMADO
fecha: 2026-08-22
participantes: Claude (Claude Code), Codex (OpenAI Codex CLI)
director: Alejandro Liendo
---

# Acuerdo final Claude - Codex

Diálogo completo en [[ACUERDO-CLAUDE-CODEX]]. Se cerró tras una ronda completa de contraste (Codex corrigió seis afirmaciones de Claude, todas verificadas en disco) y seis decisiones del director que resolvieron lo estructural. La segunda ronda de Codex se interrumpió por lentitud; sus preguntas abiertas (P5-P8) las resolvió Claude verificando las mismas fuentes.

## 1. Jerarquía de los proyectos (CONFIRMADO por el director)

- **Enlaza** es el proyecto grande.
- **La Caravana** es el brazo de Enlaza, y hoy funciona como su MVP.
- **Diccionario Venezolano** es la marca personal del director en redes: canal y voz, no empresa matriz.

Evidencia que lo respalda: `Diccionario-Venezolano-OS/AGENTS.md` línea 5-8 dice que "ENLAZA, La Caravana y otros proyectos de Alejandro no forman parte del OS". La frase del Mediakit "un proyecto de Diccionario Venezolano" se lee como presentación de marca, no como propiedad societaria.

Queda DESCARTADA la hipótesis previa de Claude de que Diccionario fuera el paraguas.

## 2. Relación Enlaza - La Caravana

- Modelo: **laboratorio / canal**. La Caravana descubre talento; Enlaza lo desarrolla y monetiza. `CONFIRMADO` (decisión del director, ECO no aplica: pasa a ser EN-DEC-002).
- `Enlaza/enlaza_roadmap.html` queda como `PROPUESTO`, no actualiza el contexto maestro.
- Implementación parcial ya existente: "Oportunidades élite · Enlaza" y "Alianzas Enlaza" en `La Caravana/prueba/portal/gobernanza.php` líneas 26, 30 y 70. Existencia `CONFIRMADO`; umbrales (ke, kr, misiones, semanas) `PROPUESTO`.
- Las 16 decisiones obligatorias de la sección 7.2 del contexto maestro (propiedad de marcas, dominios, código, datos, facturación, ingresos, responsabilidad legal) siguen `POR DEFINIR`.

## 3. Fuentes canónicas

- Regla general: **el archivo más reciente por fecha de modificación**, sin importar carpeta (decisión del director).
- El estado real del sistema es la **zona de pruebas** (`La Caravana/prueba/sitio` y `La Caravana/prueba/portal`, Laragon local 8801/8802). Producción (`La Caravana/La Caravana/`) es lo publicado y va por detrás.
- `La Caravana/prueba/_local/clon/` nunca es canónico: es un clon fechado de producción.
- Duplicados verificados (Codex, SHA-256 idéntico): PROYECTO-SISTEMA-COMERCIAL 6 copias, LEEME 5, PLAN-SISTEMA-OPERATIVO 3, GUIA-DE-ESTILO 3. Total 51 .md en el repo (35 sin contar GRAPH_REPORT).

## 4. Huecos conocidos

- **Textos institucionales vivos** (Nosotros, ediciones, convocatoria, legales): viven en MySQL y parcialmente en 33 instaladores PHP del repo. Pospuestos por decisión del director. El vault arranca sin ellos.
- **Ficha de Enlaza**: 13 de 14 campos `POR DEFINIR`.
- **Situación societaria**: el borrador nombra participantes fundacionales propuestos (Alejandro Liendo, Carlos Coronado, Ramses Guevara), no socios jurídicamente confirmados, y no fija participaciones.

## 5. Estructura del vault (acordada)

Un solo vault, jerarquía anidada que refleja el punto 1. Sin carpeta `Ecosistema/`: el puente Caravana - Enlaza vive en `40 Creadores` y en el roadmap.

```
cerebrodiccionario/
  00 Inicio.md
  Enlaza/
    00 Enlaza (inicio).md
    Contexto maestro/           26 secciones del documento v0.1
    Roadmap (PROPUESTO).md
    Decisiones.md               EN-DEC-nnn
    La Caravana/
      00 La Caravana (inicio).md
      10 Identidad/  20 Organización, sociedad y equipo/  30 Ediciones y rutas/
      40 Creadores/  50 Comercial y finanzas/  60 Sistema técnico/
      70 Decisiones.md          LC-DEC-nnn
      _Fuentes.md  _anexos/
  Diccionario Venezolano/
    Marca y voz.md
  _dialogo/
```

## 6. Reglas heredadas del OS de Diccionario

- Etiquetas de conocimiento en frontmatter: `CONFIRMADO`, `PROPUESTO`, `HIPOTESIS`, `POR DEFINIR`, `DESCARTADO`.
- Registro de decisiones con ID por proyecto: `EN-DEC-nnn`, `LC-DEC-nnn`.
- DV-DEC-003: ninguna IA es la memoria principal; la memoria vive en archivos.
- Los originales se conservan en `_anexos` junto a su conversión, con ruta y fecha registradas en `_Fuentes.md`.

## 7. Pendiente del director

1. Las 16 decisiones de la sección 7.2 de Enlaza (propiedad, facturación, ingresos, legal).
2. Definir la ficha general de Enlaza (13 campos).
3. Exportación de los textos vivos de MySQL, cuando se decida hacerla.
4. Si `_Fuentes.md` debe crecer a carpeta `80 Fuentes y trazabilidad` (propuesta de Codex, no decidida).
