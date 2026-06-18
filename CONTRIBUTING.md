# Cómo contribuir

¡Gracias por tu interés en mejorar DEO Tools! Cualquier ayuda es bienvenida.

## Empezar

La extensión es un único archivo estático (`index.html`) con HTML, CSS y JavaScript.
No hay paso de *build* ni dependencias que instalar.

Para probar tus cambios:

1. Publica tu versión en un hosting estático con HTTPS (GitHub Pages sirve perfectamente).
2. Añádela como extensión personalizada en Trimble Connect for Browser mediante la URL de su `manifest.json`.
3. Usa el **Bug Report** (panel Desarrollador) para crear un reporte y descargarlo como ZIP: incluye un TXT/JSON de diagnóstico (claves de la API, vista activa, vistas del proyecto, conversiones de GUID, etc.), las capturas con anotaciones y la lista de Reported bugs. Es la mejor base para diagnosticar problemas.

## Estilo

- Mantén todo en `index.html`, sin dependencias externas salvo la Workspace API de Trimble.
- JavaScript claro, con funciones separadas y `try/catch` por bloques.
- No rompas las funciones existentes (exportación de vista activa, etc.).
- CSV: separador `;`, codificación UTF‑8 y celdas entre comillas.
- **Incrementa `SCRIPT_VERSION` en CADA cambio de código**, por pequeño que sea: es el único modo fiable de verificar en GitHub Pages que el visor carga la versión nueva (el código aparece en "Código de versión" al conectar). Añade la entrada correspondiente en `CHANGELOG.md`.

## Reportar errores

Abre un *issue* describiendo:

- Qué esperabas y qué ocurrió.
- Pasos para reproducirlo.
- El contenido de `DEO_debug.txt` si es relevante (revisa que no contenga datos sensibles antes de pegarlo).

## Ideas y mejoras

Algunas líneas abiertas: comparación de color ignorando el alfa, detección de carpetas de vistas, exportación extendida por lotes/archivos para grandes volúmenes, y afinado de la selección por proximidad.
