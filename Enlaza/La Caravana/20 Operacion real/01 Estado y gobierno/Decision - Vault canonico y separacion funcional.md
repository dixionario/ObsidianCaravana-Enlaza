---
tipo: decision
proyecto: La Caravana
estado: APROBADO
fecha: 2026-08-28
---

# Vault canónico y separación funcional

## Decisión

El vault canónico de La Caravana, Enlaza y Diccionario Venezolano es
`C:\Users\Dixionario\Documents\cerebrodiccionario`.

La memoria de La Caravana se separa por función:

- `20 Operacion real/`: estado, gobierno, equipo, ediciones, rutas, creadores,
  comunidad, comercio y finanzas.
- `30 Experiencia y aprendizajes/`: historia, hitos, narrativa y conocimiento
  reutilizable nacido de la experiencia real.
- `60 Sistema tecnico/`: plataforma, código, datos, seguridad, auditorías,
  despliegues y reglas para agentes.
- `90 Fuentes y archivo/`: documentos originales, anexos y respaldos históricos.

La carpeta `OBSIDIAN/` del repositorio de La Caravana queda como respaldo
histórico y no recibirá memoria nueva.

## Migración

El 28 de agosto de 2026 se reorganizó el vault completo, se actualizaron sus
cuatro índices principales y se verificaron 107 notas Markdown. No quedaron
enlaces rotos en el contenido activo. El único enlace inexistente pertenece al
archivo de bienvenida de ejemplo conservado en `99 Archivo/`.

Antes de la reorganización se creó el respaldo completo
`C:\Users\Dixionario\Documents\cerebrodiccionario-respaldo-2026-08-28`.

## Respaldo remoto acordado

El 31 de agosto de 2026 se eligió el repositorio de GitHub
`dixionario/ObsidianCaravana-Enlaza` como destino del respaldo versionado del
vault. Antes de cada publicación se auditan secretos, credenciales, bases de
datos y archivos efímeros de Obsidian. El repositorio remoto no sustituye la
copia local canónica ni el respaldo completo del 28 de agosto.

El primer respaldo se publicó ese mismo día en la rama `main`: 146 archivos
versionados en el commit inicial `9378ecf`. Se excluyen la sesión visual de
Obsidian, caché, papelera, logs y los `data.json` privados de plugins. El remoto
es privado y la rama local quedó enlazada con `origin/main`.
