# trimble-export-colors

Extensión HTML/JavaScript para Trimble Connect 3D Viewer.

## Archivo principal

El archivo principal es `index.html`. No asumir que existen módulos separados si el código está concentrado en este archivo.

## Reglas de trabajo

- No reescribir el proyecto desde cero.
- No reformatear todo el archivo.
- No modificar funciones que ya funcionan salvo necesidad justificada.
- Trabajar por cambios pequeños y verificables.
- Antes de editar, identificar función o bloque afectado.
- Después de editar, resumir exactamente qué se ha cambiado.
- No hacer commits sin confirmación explícita del usuario.

## Prioridades técnicas

- Estabilidad del viewer.
- Evitar llamadas innecesarias a la API de Trimble.
- Evitar listeners duplicados.
- Evitar fugas de estado global.
- Mantener la UI fluida.
- Mantener compatibilidad con el flujo actual de selección, colores, exportación y herramientas existentes.

## Flujo obligatorio

1. Revisar `git status`.
2. Leer solo los archivos necesarios.
3. Diagnosticar antes de modificar.
4. Proponer cambios por fases.
5. Esperar confirmación antes de aplicar reparaciones grandes.

## Restricciones

- No tocar documentación, README o changelog salvo petición expresa.
- No eliminar código sin explicar impacto.
- No suponer comportamiento de Trimble Connect si no está visible en el código.
- Marcar incertidumbres en vez de inventar.
