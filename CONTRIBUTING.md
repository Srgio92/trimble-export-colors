# Cómo contribuir

¡Gracias por tu interés en mejorar DEO Tools! Cualquier ayuda es bienvenida.

## Empezar

La extensión es un único archivo estático (`index.html`) con HTML, CSS y JavaScript.
No hay paso de *build* ni dependencias que instalar.

Para probar tus cambios:

1. Publica tu versión en un hosting estático con HTTPS (GitHub Pages sirve perfectamente).
2. Añádela como extensión personalizada en Trimble Connect for Browser mediante la URL de su `manifest.json`.
3. Usa el botón **Debug** (menú Desarrollador) para obtener `DEO_debug.txt`, que vuelca las claves de la API, la vista activa, las vistas del proyecto y el resultado de las conversiones de GUID. Es la mejor base para diagnosticar problemas.

## Estilo

- Mantén todo en `index.html`, sin dependencias externas salvo la Workspace API de Trimble.
- JavaScript claro, con funciones separadas y `try/catch` por bloques.
- No rompas las funciones existentes (exportación de vista activa, etc.).
- CSV: separador `;`, codificación UTF‑8 y celdas entre comillas.
- Actualiza `SCRIPT_VERSION` y `LAST_UPDATED` y añade una entrada en `CHANGELOG.md`.

## Reportar errores

Abre un *issue* describiendo:

- Qué esperabas y qué ocurrió.
- Pasos para reproducirlo.
- El contenido de `DEO_debug.txt` si es relevante (revisa que no contenga datos sensibles antes de pegarlo).

## Ideas y mejoras

Algunas líneas abiertas: comparación de color ignorando el alfa, detección de carpetas de vistas, exportación extendida por lotes/archivos para grandes volúmenes, y afinado de la selección por proximidad.
