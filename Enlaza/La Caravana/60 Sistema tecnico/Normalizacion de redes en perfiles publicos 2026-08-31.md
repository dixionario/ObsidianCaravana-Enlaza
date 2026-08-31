# Normalización de redes en perfiles públicos — 2026-08-31

## Incidente

Algunos creadores guardaron la URL completa de Instagram, TikTok o YouTube en vez del `@usuario`. Las fichas públicas imprimían literalmente ese valor y, además, anteponían otro dominio en Instagram y TikTok. El resultado era una etiqueta demasiado larga y un enlace incorrecto, con parámetros de seguimiento como `igsh`, `_t` o `si`.

## Corrección

Se añadió una normalización compartida en `prueba/sitio/includes/funciones.php` que acepta `@usuario`, usuario simple, URL con o sin `www` y URL completa copiada desde una aplicación.

- La etiqueta pública queda siempre como `@usuario` cuando el perfil permite extraerlo.
- La dirección se reconstruye de forma canónica para Instagram, TikTok y YouTube.
- Se eliminan consultas y fragmentos de seguimiento.
- La misma regla se aplica a `creador.php`, `enlaces.php` y las bios públicas.
- No se modifica la información almacenada de los creadores.

## Verificación

Se probaron las variantes observadas en producción y valores simples. Las URLs resultantes apuntan a la cuenta correcta y la ficha del mundo sembrado renderiza sin avisos ni errores PHP.
