# Changelog

Todas las versiones notables de DEO Tools. El formato sigue, de forma aproximada,
[Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/).

## [v7] — 2026-06-09

### Añadido
- **Importar CSV**: aplica colores a los objetos del modelo cargado a partir de un CSV con GUID + color (descomprime el GUID IFC al UUID, lo convierte a runtime id y colorea con `setObjectState`). Los GUID ausentes se omiten.
- Descripciones flotantes (tooltips) que aparecen al dejar el ratón quieto 0,4 s sobre cada opción.

### Cambiado
- Los controles ON/OFF pasan a **interruptores deslizantes estilo Trimble**.
- **Columnas reordenables** con flechas ↑/↓ (el orden define el del CSV).
- **Selección cercanos** convertida en interruptor con bloque de *Configuración* (parámetro / condición / valor + unidad).
- El botón de **Debug** pasa a icono negro sobre transparente, arriba a la derecha del menú Desarrollador.
- Se retira el título "DEO Tools" del cuerpo; solo se ven los menús desplegables.
- Placeholders de los desplegables (`parámetro`, `condición`) ocultos de la lista y mostrados en gris.

### Corregido
- El alcance de Extended export muestra siempre la opción "Todo el modelo".

## [v6] — 2026-06-08

### Añadido
- Reorganización en panel **DEO Tools** con tres menús: Herramientas del modelo, Herramientas del archivo y Desarrollador.
- Columna **`codigo_revision` condicionada por color** (constante `REVISION_CODE_COLORS`).
- Columna **`revisado_deo`** TRUE/FALSE (constante `REVIEWED_DEO_COLORS`).
- Columna **`View_name`**.
- Toggles **Error codes** y **Custom codes** para el código de revisión.
- **Extended export** (recorrido de varias vistas con `selectView`) a un único CSV.
- Herramienta **Selección cercanos** (contacto aproximado por *bounding box*).
- Botón de **Debug** que descarga un informe técnico.

## [v5] — 2026-06-08

### Añadido
- **Selección de columnas** mediante casillas.
- Se omite la conversión de GUID si la columna `GUID_IFC` no está marcada (más rápido).

## [v4] — 2026-06-08

### Corregido
- **Detección fiable del código RFI**: `getCurrentView()` se reintenta varias veces y la lectura de objetos coloreados se hace antes (calienta el visor) para evitar el `undefined` por *timing*.

## [v3] — 2026-06-08

### Corregido
- **GUID vacíos**: conversión resiliente (divide y vencerás) que aísla los objetos sin GUID externo en vez de tirar el lote entero.
- **Formato del GUID**: el GUID externo (UUID) se **comprime** al GUID IFC de 22 caracteres.
- Informe de debug ampliado.

## [v1–v2] — inicial

- Conexión con la Workspace API de Trimble.
- Exportación a CSV de los objetos coloreados de la vista activa
  (GUID, color, código de revisión, modelId, objectRuntimeId).
