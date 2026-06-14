# Changelog — DEO Tools

Registro reconstruido a partir del historial real de `index.html` subido a GitHub.
Cada entrada indica, cuando existe, la versión declarada en `SCRIPT_VERSION`; cuando el archivo no declara versión interna, se identifica por fecha y hash corto del commit.

## [v9.0.0] — 2026-06-14

Versión mayor que consolida la **Fase 5 del Bug Report** (editor de anotaciones, selector visual de elementos de la interfaz y menciones inline) sobre la base estable de la serie v8.14. Recoge el trabajo interno v8.14.1 → v8.14.4. Publicado como incremento mayor `X` por la ampliación funcional del Bug Report. Verificación estática (`node --check` del JS inline, `git diff --check`); **no probado en el visor de Trimble**.

### Añadido — Bug Report avanzado (Fase 5)

* **Selector visual de elementos de la interfaz**: botón con icono de cursor azul en la fila superior del modal. Activa un modo en el que, con `document.elementFromPoint`, se resalta el elemento bajo el cursor (overlay con `pointer-events:none`) y una barra flotante guía la acción. El click identifica el elemento y captura metadatos técnicos (`tagName`, `id`, `className`, `aria-label`, `title`, texto corto, ruta CSS aproximada, breadcrumb de `details/summary` y bounding rect).
* **Menciones/chips de UI dentro de la descripción**: la descripción pasa de `textarea` a un editor `contenteditable` que permite insertar el elemento seleccionado como un chip inline (estilo token gris, con cruz de borrado) en la posición del cursor. Los chips se borran con su cruz o con Backspace/Delete. La descripción se serializa a texto plano con marcadores `[UI: …]` y se guarda además `uiMentions[]` con los metadatos completos.
* **Ctrl+click (y Meta+click) para interactuar sin seleccionar** durante el modo selector: permite abrir/cerrar `details`, desplegar grupos o enfocar campos sin mencionarlos y sin salir del modo selector.
* **Editor de anotaciones de capturas**: las miniaturas son clicables para abrir un editor sobre lienzo con herramientas flecha, rectángulo, círculo, texto, mano libre, deshacer, limpiar, cancelar y guardar. Al guardar, las anotaciones se integran en la imagen y se registran metadatos (`annotated`, `annotationTs`, `annotationCount`, `annotationTypes`, `annotationColors`, `annotationWidths`) conservando metadatos mínimos del original sin duplicar el peso en `localStorage`.
* **Selector de color y grosor de anotación**: paleta compacta inspirada en Recortes de Windows + color personalizado (`input type="color"`) y slider de grosor (1–18 px, por defecto 4) con previsualización de trazo en vivo; rojo como color inicial.
* **Campo "Zona / Herramienta afectada"** gestionado internamente (resumen de menciones), con `affectedArea` compatible.

### Mejorado / Corregido

* Miniaturas clicables para anotar con halo azul al hover/foco y halo persistente cuando la captura ya está anotada; el botón de eliminar miniatura mantiene su comportamiento (no abre el editor).
* Redimensionado vertical del cuadro de descripción restaurado (`resize:vertical`) sobre el nuevo editor enriquecido.
* TXT/JSON/ZIP del Bug Report amplían los metadatos: `uiTarget` (último elemento), `uiMentions[]` (menciones usadas) y datos de anotación por captura; el TXT incluye una sección legible de menciones UI. Compatibilidad con reportes antiguos sin estos campos.

### Organización del repositorio

* `.gitignore` ampliado para no versionar carpetas locales de trabajo (`.claude/`, `gpt_code/`), temporales (`.tmp_*`) ni descargas del Bug Report/Debug (`DEO_bug_reports*.zip`, `DEO_bug_reports*.txt`, `DEO_debug*.txt`).
* Copia histórica versionada en `old_code/index_v9.0.0.html`. Sin mover `index.html` ni `manifest.json` de la raíz; iconos de raíz y de `assets/` conservados según el diseño documentado en `README.md`.

## [v8.14.0] — 2026-06-14

Versión estable que consolida las mejoras de **Bug Report / Debug** y un conjunto de correcciones y ajustes de UI en **Configurar vista** y **Quantity Surveyor**. Publicado como incremento `Y`. Verificación estática (`node --check` del JS inline, `git diff --check`); **no probado en el visor de Trimble**.

### Añadido / Mejorado

* **Bug Report / Debug**: snapshot técnico ampliado (historial de acciones de usuario, última selección no vacía con bbox/propiedades, estado de UI, estado por herramienta, capacidades de la API, diagnóstico de descargas y de almacenamiento, identidad de código con hash del HTML). El TXT incluye un resumen legible y el detalle completo; el Debug grande también incorpora el snapshot común. Captura asistida con temporizador rediseñado (burbujas flotantes mover/cancelar, botón "Pegar ahora") y zona de pegado centrada con el modal atenuado detrás.

### Corregido / UI

* **Configurar vista**: fecha de hoy autocompletada al crear; autor unificado mostrando nombre legible (nunca el correo) en crear y actualizar; chips de Etiquetas y Compartir con borrables por teclado (Backspace/Delete); sugerencias de Compartir con posicionadas bajo el cuadro y mejor ordenadas.
* **Configurar vista — layout responsive de textboxes**: los campos conservan su ancho mínimo y crecen de forma proporcional al ancho del panel; las filas Código/Ubicación/Nivel/Ejes y Fecha/Autor solo saltan de línea cuando no caben; ancho de Nivel suficiente para `N10-N100`.
* **Quantity Surveyor**: toda el área del bloque recalcula al pulsar; copiar/limpiar no recalculan.
* Icono del botón **Actualizar datos del visor**: orientación corregida.

## [v8.13.0] — 2026-06-13

Incorpora la herramienta **Bug Report** en el panel Desarrollador y varios ajustes de UI. Publicado como incremento `Y`. Verificación estática (sin node en el equipo: comprobaciones equivalentes de ids/funciones duplicadas y balance de delimitadores); **no probado en el visor de Trimble**.

### Añadido

* **Bug Report** (panel Desarrollador): botón de cámara que emerge bajo el botón Debug al pasar el ratón. Abre un modal ligero con título, descripción redimensionable y capturas de pantalla opcionales (`navigator.mediaDevices.getDisplayMedia` → canvas reducido). Las capturas se muestran como miniaturas con eliminación individual. Al enviar, el reporte se acumula en `localStorage` (`deo.bugReports.v1`, máx. 20 reportes / 5 capturas) junto a un *snapshot* técnico saneado (versión, selección, vista activa, modelos, resumen de colores, toggles y diagnósticos existentes). Con reportes pendientes, el botón Debug cambia a icono de descarga rojo y descarga un TXT acumulado (`DEO_bug_reports_vX.Y.Z_…txt`), limpiando los pendientes; sin reportes, el Debug funciona como antes. El modal solo se cierra con **Cancelar** o **Enviar**.
* **Opción "— sin selección —"** en los desplegables de Name (Excluir / Selector de items), resaltada en tono rojizo; no aporta filtro y permite dejar una línea en blanco de forma explícita.

### Corregido / UI

* Icono del botón **Actualizar datos del visor**: orientación y sentido de giro corregidos.
* Pestaña **Clear** del Quantity Surveyor: borde perimetral gris en las cuatro caras del trapecio; el halo azul ya no queda retenido tras pulsar (solo visible en hover).

## [v8.12.0] — 2026-06-13

Estabilización crítica de **Excluir de selección por modelo/IFC** y del manejo de colores (trabajo interno v8.11.1), más auditoría dirigida del archivo. Publicado como incremento `Y` (corrección/estabilización de herramientas existentes, sin herramienta nueva). Verificación estática y de funciones puras en navegador; **no probado en el visor de Trimble**.

### Corregido

* **Excluir de selección por modelo/IFC**: marcar un modelo (p. ej. `Project Axes.trb`) ahora elimina de forma fiable sus objetos de la selección, incluso seleccionando un único objeto. Se añade una **capa de alias** entre los ids de `getModels()`, `getObjects()` y `getSelection()` y el **nombre normalizado** del modelo; la exclusión se resuelve por el `modelId` real de la selección (por id directo o por nombre normalizado), de modo que funciona aunque Trimble devuelva ids distintos para el mismo archivo.
* **Persistencia de exclusiones migrada a formato versionado** `deo.excludedModels.v2` (`{names, ids}`), con lectura tolerante del formato antiguo (solo ids). Los ids persistidos **huérfanos** (de modelos que ya no están cargados) se descartan al refrescar y no contaminan modelos actuales; la intención del usuario se conserva por nombre normalizado.
* **Color "no legible" `#RRGGBBAA…` sobredimensionado**: un hexadecimal con más de 8 dígitos (p. ej. `#50D0CB39C`) ya no se marca como "Color no legible". Si contiene al menos RGB válido se interpreta de forma tolerante (RGB de los 6 primeros dígitos; alfa de los 8 primeros si existen, si no `FF`), conservando la normalización exacta a `#RRGGBBAA` y la comparación por color. El swatch sigue usando color sólido.
* **Estabilización del filtro tras selección**: si Trimble dispara eventos de selección troceados (al seleccionar una capa/modelo) o durante una operación en curso, la herramienta reaplica el filtro con un pequeño *retry* controlado, sin bucles ni llamadas masivas, y `setSelection` solo se llama si hay cambios reales.
* **Inicialización con `API = null`**: `getLoadedModelsList()`, `getModelObjectsMap()` y `safeGetSelection()` degradan sin romper si `API`/`API.viewer` aún no están listos (eliminando los errores `Cannot read properties of null (reading 'viewer')` del arranque). La lista de modelos se repuebla tras la conexión.

### Diagnóstico

* `COLOR_DIAG` añade el caso de hex sobredimensionado (`coerced`, raw, normalized y motivo). `EXCLUDE_MODELS_DIAG` se amplía con el registro de modelos y aliases, ids huérfanos descartados, nombres/ids efectivos, mapa id→nombre y el último resumen de filtrado de selección por modelo.

### Auditoría (sin cambios funcionales)

* Revisión dirigida del archivo: sin ids HTML ni funciones duplicadas; todas las llamadas `JSON.parse`/`localStorage` están protegidas; el arranque no invoca `API.viewer` sin guarda. El flujo activo de **Configurar vista** (escenarios `cvNew`/`cvUpd` con chips de Compartir/Etiquetas) funciona; queda **pendiente** (no abordado en esta versión, por seguridad) la limpieza del clúster de código muerto de "plantillas" previo al rework (`tplCfg_*`, `templateSelect`, `createViewFromTemplate`, etc.), que está protegido con guardas y no se ejecuta.

## [v8.11.0] — 2026-06-13

Rework de **Configurar vista** (incorporado desde versiones internas de trabajo v8.10.38–v8.10.43) y mejoras de la pestaña **Clear** del Quantity Surveyor (trabajo interno v8.10.9–v8.10.37). Publicado como incremento `Y` por estabilización/mejora de herramientas existentes, sin herramienta nueva.

### Configurar vista

* Campos agrupados en **líneas fijas por flujo** (principal, secundaria, actuación, meta, etiquetas, compartir) con autoajuste de ancho (`tpl-autosize`/`tpl-autofit`) y *wrap* interno cuando el panel se estrecha.
* **Prefijos automáticos no seleccionables** para Código (`RFI_`) y Nivel (`N`), separados del valor editable. El parser de código (`codeUserPart`) limpia y normaliza solo la parte de usuario.
* **Fecha secuencial** `dd-mm-aaaa` con guiones automáticos, validación de prefijo por dígitos (`isValidDateDigitsPrefix`) y placeholder no editable.
* **Nivel** con formato `N-N` (dos tramos numéricos) y normalización propia.
* Alineación al extremo derecho del campo Ejes; ancho base alineado con Etiquetas.
* Chips/autocompletado de **Etiquetas** y **Compartir con** integrados en los flujos Crear y Actualizar (`cvNew_*` / `cvUpd_*`).

### Quantity Surveyor

* **Pestaña Clear** rediseñada como pestaña colgante en la esquina inferior derecha del panel: trapecio con esquinas inferiores ligeramente redondeadas, contorno en las 4 caras, halo azul continuo que envuelve panel y pestaña, interior blanco al pasar el ratón por la herramienta y gris oscuro al pasar sobre la propia pestaña; sin desplazamiento al pulsar (solo contrae el texto). Margen inferior solo cuando la pestaña está activa.

## [v8.10.8] — 2026-06-13

Botón global de actualización del visor y auditoría de cambios manuales recientes (revertidos parcialmente y rehechos).

### Corregido / auditoría

* **Botón global "Actualizar datos del visor"** rehecho correctamente. Los cambios manuales previos (externos, no de Claude) eran defectuosos: dos bloques CSS contradictorios (`.global-refresh-btn` circular + `.global-refresh-float` cuadrado, este último CSS muerto sin elemento), botón **circular** y alineado a la **izquierda**, espacios en blanco al final (línea con trailing whitespace) y conversión de todo el archivo de CRLF a LF. Se restauró la base limpia v8.10.7 (CRLF) y se reimplementó el botón.
* El botón es ahora **cuadrado con esquinas redondeadas** (36×36, `border-radius:10px`, no circular), **sticky en la esquina superior derecha** del panel (`top:8px`, margen derecho de 16 px para no solaparse con la barra de scroll), visible al hacer scroll, con icono negro de dos flechas circulares centrado, hover azul coherente y contracción al pulsar. Sin texto ni estados visibles persistentes; solo gira el icono mientras refresca.
* La barra contenedora usa `pointer-events:none` (no tapa títulos ni contenido) y solo el botón recibe clics.
* **Incremento de versión corregido**: el cambio manual había saltado a v8.10.10 (con comentario v8.10.8 incoherente); se fija el incremento limpio **v8.10.8** sobre la última versión buena v8.10.7.

### Cambiado

* **Refrescos locales integrados en el botón global**: `excludeRefresh`, `excludeModelsRefresh`, `includeRefresh`, `qsRefreshAll`, `namesParamRefresh`, `boltsParamRefresh`, `weldsParamRefresh` y `csvParamRefresh` se ocultan (se conservan en el DOM por compatibilidad de listeners; ningún listener queda roto). Se mantienen **Actualizar vistas**, **Debug** y **Cancelar** de export/import.
* Nueva función central `refreshViewerDataGlobal()`: invalida las cachés dependientes del visor (Name, parámetros, índice rápido, propiedades, fallback de color) y vuelve a leer modelos IFC, colores de presentación, valores Name y parámetros; reindexa valores rápidos solo si el Selector de items está en uso; refresca Inspector y motor de selección. Deshabilita el botón y bloquea reentradas mientras trabaja (sin refrescos simultáneos); los errores se registran en debug sin texto visible permanente.
* Textos de ayuda que apuntaban a los botones ↻ locales ahora apuntan al botón global del visor.

## [v8.10.7] — 2026-06-12

Corrección de interfaz y E/S por lotes. Lote de 19 ajustes sobre v8.10.6.

### Corregido

* **Excluir de selección**: la primera fila de la lista de IFC queda alineada con los botones de filtro y actualizar.
* **Colores Tekla/no estándar**: el swatch del color personalizado con alfa < 255 se muestra sobre damero (transparencia legible, nunca blanco falso). La normalización (`#RRGGBBAA`), la comparación por color y `analyzeColor` (objeto `{r,g,b,a}`/`{red,green,blue,alpha}`, array, `#RGB(A)`/`#RRGGBB(AA)`, `rgb()/rgba()`, escalas 0–1 y 0–255) se mantienen.
* **Barras de Tamaño/Distancia de etiqueta**: imán al 100 % (±8) y refresco del porcentaje; los valores afectan a las etiquetas 3D creadas con Agregar (escala y offset).
* **Inspector de selección**: separación respecto al título, primera línea sin negrita y parámetros con sangría. Se retiran los textos "No es un tornillo/soldadura…" y "Dato encontrado en: padre" (el Inspector detecta cualquier objeto). Nota fija: "El Inspector muestra los parámetros configurados en Mostrar parámetros".
* **Acciones rápidas**: nuevo icono de edición tipo lápiz que escribe (animación `pencilWrite`). El botón de añadir color ya no se corta (queda en el flujo responsive, alineado con los círculos). Se elimina el botón "Aceptar" del menú de color: al elegir un color en el selector se añade con la transparencia seleccionada (alfa real aplicado al pintar).
* **Quantity Surveyor**: la longitud (total y por tipo de soldadura) se muestra en metros con formato español (1 m = 1000 mm; punto de miles, coma decimal).
* **Configurar vista / Actualizar vista**: parser de título tolerante que separa código, ubicación, nivel, ejes, elemento, tarea, fecha y autor (ya no vuelca todo en Actuación). Prefijo `RFI_` bloqueado en el campo Código (solo se edita el número).
* **Exportar CSV**: Error codes y Custom codes alineados (misma estructura de fila).
* **Hover azul** coherente en todos los botones pulsables (anillo interior, sin salto de layout, salvo deshabilitados).

### Añadido

* **Exportación masiva con selección de vistas**: lista con scroll de todas las vistas, casillas por vista, orden alfabético, "Seleccionar/Deseleccionar todas" y contador `X/total` junto al interruptor. Exporta solo las vistas marcadas; si no hay ninguna, no inicia y avisa.
* **Exportación masiva a ZIP único**: genera un CSV por vista dentro de un único ZIP (un solo guardado), con informe `_informe.txt`. Implementación de ZIP **propia sin dependencias externas** (método STORE + CRC32). Botón Cancelar junto a Exportar.
* **Importación CSV/ZIP**: acepta un CSV, varios CSV o un ZIP con CSV dentro (lectura STORE directa y DEFLATE vía `DecompressionStream`; ignora lo que no sea CSV). Botón Cancelar junto a Importar.
* **Estado con ETA**: exportación e importación muestran "X/Y · ETA mm:ss" con media móvil del tiempo por vista/archivo y cancelación limpia.

### Notas

* El cambio de vista en la exportación masiva usa el primer método disponible del Workspace API (`setView`/`selectView`/`setCurrentView`/…); si la API no lo expone, se informa y cada vista puede leer el contexto actual.
* El botón Cancelar de la sección Estado se conserva para Quantity Surveyor e indexado de parámetros; export e import usan su Cancelar propio en línea.

## [v8.10.6] — 2026-06-12

Auditoría completa del proyecto y reparación de colores/UI. Esta entrada consolida también los refinamientos de interfaz internos v8.10.3–v8.10.5 (paleta de colores rápidos personalizables, filtro por Name en Excluir de selección, panel pulsable de Quantity Surveyor, barras de tamaño/distancia de etiquetas), que no llegaron a publicarse por separado.

### Corregido

* **Alineación de las barras de etiquetas flotantes**: el thumb de las barras *Tamaño texto* y *Distancia* queda centrado verticalmente sobre el track (Chrome/Edge/Firefox) mediante `appearance:none` + estilos de thumb. La muesca del 100% coincide con el punto real (47,368 % = (100−10)/(200−10)). No se toca la barra de Sensibilidad de Selección cercanos.
* **Detección y visualización de colores Tekla/no estándar**: los swatches usan un color sólido (`swatchCss`, RGB sin alfa), de modo que un color válido con alfa bajo (típico de Tekla) ya **no se ve lavado ni en blanco**. La comparación/exclusión sigue usando el color normalizado `#RRGGBBAA`. `analyzeColor` acepta objeto `{r,g,b,a}` / `{red,green,blue,alpha}` / `{R,G,B,A}`, array `[r,g,b(,a)]`, `#RRGGBB(AA)`, `rgb()/rgba()` y escalas 0–1 y 0–255; lo no interpretable se muestra como "Color no legible" con patrón, nunca en blanco.
* **Falsos círculos blancos** eliminados en Excluir por color, Selector por color y colores personalizados.

### Cambiado

* **Colores rápidos por defecto**: el gris pasa a `#a8abb4` (gris nativo de la paleta Trimble). El magenta (`#ec00b6`) y el rojo (`#fc0d1b`) se conservan a propósito: son los marcadores exactos que el CSV usa para `codigo_revision` y `revisado_deo`; cambiarlos rompería la exportación.
* `COLOR_DIAG` ampliado: batería de formatos de entrada con su normalización y `swatchCss`, muestras de colores que antes se habrían visto lavados, y regla de swatch sólido.

### Auditoría

* Sin ids HTML duplicados, sin funciones declaradas dos veces, sin listeners duplicados en la inicialización, sin `console.log`, sin botones test/legacy visibles. Todas las herramientas principales (CSV export/import, exportación masiva, selección cercanos, ocultos, selector/exclusión por Name y color, exclusión por IFC, Mostrar parámetros, etiquetas 3D, Inspector, Quantity Surveyor, Configurar vista con chips) verificadas como cableadas. `git diff --check` limpio.

## [v8.10.2] — 2026-06-12

Versión de estabilización que consolida los ciclos internos v8.9.x (diagnóstico de etiquetas 3D) y v8.10.0/v8.10.1 (reorganización de la interfaz) en una versión funcional.

### Corregido

* **Excluir de selección / colores**: si `getColoredObjects` devuelve 0 pero hay objetos accesibles en el visor, ya no se afirma "No hay colores aplicados en el modelo"; se informa de que los colores nativos del IFC/Tekla pueden no estar expuestos por la API. Diagnóstico ampliado en `COLOR_DIAG` (motivo del fallback, sonda de presentación, totales).
* **Lista de IFC/modelos en Excluir de selección**: solo muestra los modelos realmente abiertos en el visor (fuente: `getObjects`, con `getModels` solo para nombres). Antes listaba todo el proyecto.
* **Selección ocultos en modelo IFC directo**: validación de la detección — si los "ocultos" cubren ~todo lo accesible (el visor ignora el filtro `{visible:false}`), se aborta sin tocar visibilidad en lugar de ocultar/blanquear el modelo entero. Diagnóstico en `HIDDEN_DIAG`.
* **Etiquetas 3D en modelo directo**: restauradas. El bloqueo por contexto (falso positivo de vista guardada) dejaba inoperativos Agregar/Eliminar/Eliminar todos; los botones ya no se deshabilitan y el motor de coordenadas puras (`addTextMarkup`, anclaje techo+0,5 m, mm) vuelve a ejecutarse. Warning fijo: "Sobre vistas existentes guardadas esta función puede no estar disponible".
* **Mostrar parámetros**: los switches de Ver nombres / Ver tornillos / Ver soldaduras ya no abren/cierran el desplegable; el nombre sí.

### Añadido

* **Quantity Surveyor**: box con scroll interno, botón copiar (texto limpio) superpuesto y botón Limpiar. Resumen con formato español y unidades (kg, m³, m², mm). Agrupaciones: soldaduras por Assembly type → tipo (unidades, longitud total, gargantas); tornillos por estándar + calidad usando `Bolt count` (M/L con unidades reales); perfiles/chapas por material con peso total y grupo Undefined. Colores sobrios por tipología.
* **Excluir de selección**: botón embudo que alterna orden aparición/alfabético de la lista de modelos; box con scroll (~8 filas).
* **Inspector de soldadura**: unidades (mm/kg/m²/m³) y redondeo a 3 decimales en valores dimensionales; los textos (W10, Site, marcas…) no se alteran.
* Secciones de debug: `HIDDEN_DIAG`, `CONFIG_VIEW_DIAG`; ampliadas `COLOR_DIAG`, `EXCLUDE_MODELS_DIAG`, `FLOATING_LABEL_DIAG` (contexto activo, estado de botones, ids reales vs locales, último error de markup).

### Cambiado

* **Configurar vista**: recuperada la disposición anterior (Código, Ubicación, Nivel, Ejes, Elemento, Actuación, Fecha, Autor) en ambos flujos, con "Compartir con" mediante chips + autocompletado por flujo y campo "Etiquetas". Autoajuste fluido de los campos. Sin bloque de plantillas/"Datos comunes". Al abrir, expande Actualizar vista si hay vista guardada activa (precargando descripción, autor, compartidos y etiquetas) o Crear nueva vista en caso contrario.
* "Eliminar todos" de etiquetas 3D con fondo rojo suave (acción destructiva moderada).
* Iconos copiados a `assets/` y `manifest.json` apuntando a ellos (las copias en raíz se conservan por compatibilidad con manifests cacheados).

## [v8.8.0] — 2026-06-10

### Añadido

* Marca visual en el 100% del slider de "Selección cercanos" con efecto imán (±10% se engancha al valor base).
* Botón de refresco de grupos de vistas en "Configurar plantilla" y en "Exportar colores a CSV".
* Caché compartida de grupos de vistas (`buildViewGroupsCache`) con detección de métodos nativos del API y deducción desde `ViewSpec`.
* Función `nativeViewGroups()` para probar `getViewGroups`, `getGroups`, `getViewSets` y `getFolders` cuando existan.
* Detección de namespace de markups mediante `markupApi()` (`API.markup` o `API.viewer.markup`).
* Pick estándar de markup con `markupPick()` y variante sin referencias de objeto con `stripPickRefs()`.
* Detección de tornillería y soldaduras por `Name` y por clase/propiedades IFC con `matchBolt()` y `matchWeld()`.
* Cancelación automática de "Mostrar parámetros" cuando la selección cambia a mitad de proceso (`paramRefreshSeq`).
* Aplicación de estado por lotes con progreso mediante `setStateInBatches()`.
* Detección de blanco normalizado con `isWhiteNormalized()`.
* Separación del registro de ocultos entre blancos y recoloreados con `splitRegistryByColor()`.
* Log de importación CSV con muestra de hasta 10 GUID no encontrados.

### Cambiado

* "Selección cercanos" amplía el rango de sensibilidad de 0–200% a 0–300%.
* El alcance de "Selección cercanos" escala de forma cuadrática por encima del 100% (300% ≈ 9× el radio y el volumen máximo base), de modo que el alcance máximo es muy superior al anterior.
* Textos explicativos de "Selección cercanos" reescritos para describir los tramos 0%, 100% y hasta 300%.
* "Ver ocultos" conserva el registro de objetos ocultos al desactivarse, permitiendo activarlo y desactivarlo de forma repetida sin perder estado.
* "Ver ocultos" usa la escala de opacidad correcta del Workspace API (0–100) en lugar de 0–1.
* "Mostrar parámetros" envía los picks en formato estándar (posiciones en milímetros más `modelId` y `objectRuntimeId`), sin los campos no documentados que provocaban el rechazo del API.
* "Mostrar parámetros" aplica una cascada de fallbacks: lote completo, uno a uno, picks solo con coordenadas y, por último, iconos SVG 3D.
* La importación CSV solo convierte los GUID aún pendientes en cada modelo, evitando reconvertir lotes ya resueltos.
* El campo "Carpeta" de la exportación masiva pasa a denominarse "Grupo de vistas", con "Todas las vistas" como valor por defecto del propio desplegable.
* El filtrado por grupo y los nombres de archivo e informe de la exportación masiva usan el nombre real del grupo.
* "Configurar plantilla" obtiene los grupos de vistas desde la caché compartida en lugar de consultar el detalle de cada vista en serie.

### Corregido

* Corrección de "Ver ocultos", que solo funcionaba una vez: ya puede usarse de forma repetida.
* Corrección de la fuga de estado que ocultaba todos los objetos blancos del modelo al desactivar "Ver ocultos", aunque nunca hubieran estado ocultos.
* Corrección del rescate de ocultos: un objeto cuyo color no consta deja de interpretarse como recoloreado, evitando vaciar el registro por error.
* Corrección de la opacidad de objetos rescatados y mostrados en "Ver ocultos" (antes quedaban prácticamente invisibles).
* Corrección de "Ver nombres", "Ver tornillos" y "Ver soldaduras", que no creaban etiquetas por el formato incorrecto de los picks de markup.
* Corrección del rojo calibrado de la importación CSV, que se aplicaba con opacidad casi nula y provocaba parpadeos.
* Corrección de los grupos de vistas de "Configurar plantillas de vista" y de la exportación masiva para mostrar los grupos reales del proyecto.
* Corrección del estado, que quedaba en "Esperando conexión..." tras conectar: ahora refleja conectado, operación en curso, completada, error y cancelado.
* "Excluir de selección" recalcula la selección también al desactivarse.

### Eliminado

* Funciones obsoletas: `probeColor()`, `importStateForKey()`, `stateKeyFromColor()`, `rgbaFromNormalizedColor()`, `isNumericValue()` y `toNumber()`.
* Uso del Map temporal por fila en la importación CSV (`importBucket(new Map(), "__TMP")`).
* Nota obsoleta sobre el tamaño de fuente de las etiquetas de texto en "Mostrar parámetros".

### Limitaciones conocidas

* Si el visor rechaza incluso el formato estándar de markup de texto, se recurre a coordenadas puras y, después, a iconos SVG 3D; si tampoco hay `addIcon`, se informa en el log y en el Estado.
* Si Trimble no expone grupos de vistas de forma nativa ni en el `ViewSpec` (ni en el detalle de las primeras 150 vistas), el desplegable solo muestra "Todas las vistas" con aviso en el log.
* Los objetos vueltos a ocultar en "Ver ocultos" conservan el color blanco interno; no se restaura el color original del modelo.
* El análisis de detalle de grupos de vistas se limita a 150 vistas para no bloquear el navegador ni saturar el API.

## [v8.7.5] — 2026-06-10

### Añadido

* Autocompletado en el campo “Compartido con” de plantillas de vista, imitando el funcionamiento nativo de Trimble.
* Carga de miembros del proyecto mediante la API de Trimble para sugerir usuarios coincidentes con el texto escrito.
* Sugerencias en formato `nombre | correo`.
* Chips eliminables para los usuarios añadidos en “Compartido con”.
* Contador dinámico de usuarios compartidos.
* Lectura ampliada de grupos de vistas desde vistas existentes y desde los datos disponibles de `ViewSpec`.
* Uso automático del usuario activo de Trimble como autor en “Crear vista nueva”.
* Relleno automático de fecha y autor en “Actualizar vista” cuando una vista existente no contiene esos campos.
* Fallback de visualización de parámetros mediante iconos SVG 3D cuando `API.markup.addTextMarkup()` no devuelve identificadores utilizables.
* Logs específicos para importación CSV, ocultos y visualización de parámetros.

### Cambiado

* La importación CSV queda limitada exclusivamente a filas con `revisado_deo = TRUE`.
* La importación CSV deja de restaurar colores de la columna `color`; ya no importa magentas ni colores no revisados.
* Los GUID revisados se seleccionan y se pintan directamente en rojo calibrado.
* “Selección cercanos” corrige la interpretación de la sensibilidad:

  * `0 %`: mantiene únicamente el objeto seleccionado.
  * `100 %`: aplica el comportamiento base.
  * `200 %`: amplía el alcance sobre el criterio base.
* “Ver ocultos” ajusta la aplicación de opacidad para intentar representar correctamente el 50 % de transparencia en el visor.
* La lógica de ocultos se reorganiza para comprobar si un objeto marcado en blanco ha sido recoloreado antes de volver a ocultarlo.
* El desplegable de grupo de vistas se alimenta con más fuentes de información disponibles en la API.

### Corregido

* Corrección de importación CSV lenta por procesar filas innecesarias.
* Corrección de importación de colores no deseados desde CSV.
* Corrección de la lógica de sensibilidad al 0 % en “Selección cercanos”.
* Corrección parcial de “Ver ocultos” para no volver a ocultar elementos que hayan sido recoloreados manualmente.
* Mejora de robustez en “Ver nombres”, “Ver tornillos” y “Ver soldaduras” con fallback si los markups nativos no se crean correctamente.

### Limitaciones conocidas

* La API pública de Trimble no documenta de forma completa la escritura de grupo de vistas, etiquetas y compartición al crear una vista desde extensión. El código intenta usar esos datos cuando están disponibles, pero si Trimble los rechaza, la vista se crea con nombre y descripción.
* La visualización de nombres, tornillos y soldaduras depende del soporte real de `API.markup.addTextMarkup()` en el entorno cargado. Si la API no permite crear markups persistentes, se usa un fallback visual.

## [v8.7.4] — 2026-06-10 12:09 — d349af7

### Añadido

* Fecha automática en “Crear vista nueva” cuando el campo Fecha está vacío.
* Autor automático en “Crear vista nueva” cuando el campo Autor está vacío.
* Función `autoAuthorValue()` para intentar obtener el autor desde los datos disponibles del usuario/proyecto.
* Función `ensureCreateAutoFields()` para completar fecha y autor antes de generar la vista.
* Función `buildViewSpec()` para construir la especificación de vista incluyendo nombre, descripción, grupo, etiquetas y compartición cuando la API lo permita.
* Función `parseTemplateList()` para convertir listas de etiquetas o correos introducidas en texto en arrays utilizables.
* Botón único de refresco para acciones rápidas: `qsRefreshAll`.
* Texto de ayuda sobre los círculos del selector por color.
* Función `addTextMarkupsRobust()` para crear markups por lotes y reintentar individualmente si Trimble rechaza una llamada completa.
* Función `normalizeMarkupIds()` para normalizar la respuesta de `API.markup.addTextMarkup`.

### Cambiado

* “Configurar plantilla” queda limitado a:

  * Nombre especial.
  * Grupo de vistas.
  * Etiquetas.
  * Compartido con.
* Se eliminan de “Configurar plantilla” los campos de definición del nombre de vista: código, ubicación, nivel, ejes, elemento, tarea, fecha y autor.
* El campo “Carpeta” pasa a denominarse “Grupo de vistas”.
* “Compartir con” pasa a denominarse “Compartido con”.
* Los inputs y selects de plantillas reducen tamaño de fuente y altura para mostrar más texto en el ancho disponible.
* Acciones rápidas se reorganiza con una cabecera propia, un único refresco superior y filas en grid adaptable.
* Los botones “Seleccionar” de acciones rápidas pasan a ser botones cuadrados con icono, ocupando menos ancho.
* El botón de refresco de “Actualizar vista” usa el mismo icono SVG que el resto de refrescos.
* La creación de markups de “Mostrar parámetros” informa de progreso y de rechazo por permisos/API.

### Corregido

* Mejora de robustez en “Mostrar parámetros” cuando la API de Trimble no devuelve ids de markup de forma uniforme.
* Reducción de cortes visuales en los textboxes de plantillas de vista.

## [v8.7.3] — 2026-06-10 11:01 — 920b669

### Añadido

* Nueva estructura completa de “Plantillas de vista”:

  * `1. Configurar plantilla`.
  * `2. Crear vista nueva`.
  * `3. Actualizar vista`.
* Formato normalizado de nombre de vista:
  `(Código de inspección) Ubicación, Nivel, Ejes, Elemento clave - Tarea (fecha, autor)`.
* Campos separados para código, ubicación, nivel, ejes, elemento clave, tarea, fecha y autor en creación y actualización de vista.
* Preview del nombre final en creación y actualización.
* Selector de vistas existentes para actualización.
* Parseo de nombres existentes mediante `parseViewName()`.
* Carga de datos de una vista existente en el formulario de actualización.
* Migración de plantillas antiguas mediante `migrateOldTemplates()`.
* Carga de la plantilla más reciente mediante `newestTemplate()`.
* Normalización automática de código mediante `normalizeCodeValue()`.
* Normalización de fecha en formato `dd-mm-yyyy` mediante `normalizeDateDash()`.
* Cache de índices de acciones rápidas por parámetro mediante `quickParamIndexCache`.
* Construcción de índice de parámetros mediante `buildQuickParamIndex()`.
* Importación CSV reforzada con candidatos de GUID mediante `addGuidImportCandidate()`.
* Conversión segura runtime/static id mediante `convertRuntimeIdsSafe()` y `staticObjectIdsForRuntime()`.
* Funciones específicas para importación por buckets y estado de objeto:

  * `importBucket()`.
  * `importStateForKey()`.
  * `stateKeyFromColor()`.
  * `rgbaFromNormalizedColor()`.
* Función `isReviewedTrue()` para reconocer valores equivalentes a TRUE en `revisado_deo`.
* Detección adicional de objetos blancos mediante `getWhiteColoredMap()`.

### Cambiado

* Rango de sensibilidad de “Selección cercanos” modificado de 10–300 % a 0–200 %.
* El texto del desplegable “Selector de items” se corrige para que solo el placeholder aparezca en gris; los valores reales se muestran en negro.
* “Acciones rápidas” sustituye botones de color con texto por círculos de color sin texto visible.
* Los círculos de color quedan centrados horizontalmente y con mayor separación.
* Las listas de colores de incluir/excluir se aproximan al estilo nativo de Trimble mediante swatches circulares.
* La opción “Quitar color” se elimina de acciones rápidas.
* La selección rápida por parámetro deja de escanear todo el modelo al pulsar “Seleccionar”; primero se indexa con refresco y después se selecciona desde caché.
* “Mostrar parámetros” adopta el sistema de ordenación de columnas con asa de arrastre.
* El summary de herramientas con ON/OFF deja de abrir/cerrar el panel por pulsación sobre el texto; la interacción queda vinculada al switch.
* “Ver ocultos” pasa a basarse en objetos blancos con opacidad configurada, registro interno y detección de blancos coloreados.
* La importación CSV da prioridad a `revisado_deo = TRUE` frente a cualquier color indicado en la columna `color`.
* La importación de revisados aplica rojo mediante objeto RGBA y no como string hexadecimal.
* El panel “Desarrollador” recupera una flecha visual propia compatible con el layout flex.
* Se elimina el texto: `Configura los parámetros como en "Columnas a exportar".`

### Corregido

* Corrección de importación de GUID revisados para aplicar rojo calibrado `{r:252,g:13,b:27,a:255}`.
* Corrección de restauración de colores `#RRGGBBAA` convirtiéndolos a objeto RGBA antes de aplicarlos.
* Corrección de la lógica de ocultos para volver a ocultar elementos blancos al desactivar “Ver ocultos”.
* Corrección de la lógica de rescate de objetos ocultos recoloreados.
* Corrección parcial de “Mostrar parámetros” usando `API.markup.addTextMarkup` con `start`, `end`, `text`, color y coordenadas en milímetros.

### Eliminado

* Eliminado el bloque anterior “3. Renombrar vista existente”.
* Eliminados los nombres visibles de los colores en acciones rápidas.
* Eliminados placeholders con datos reales en plantillas de vista.

## [v8.6.1] — 2026-06-10 09:52 — c9bdc5a

### Añadido

* Nuevo bloque “Acciones rápidas”.
* Botones rápidos para pintar la selección:

  * Gris neutro.
  * Magenta.
  * Rojo.
  * Quitar color.
* Dos filas de selección rápida por parámetro y valor.
* Desplegables `qsParam1`, `qsValue1`, `qsParam2`, `qsValue2`.
* Botones de refresco por fila para actualizar parámetros y valores.
* Funciones:

  * `applyQuickColor()`.
  * `wireQuickActions()`.
  * `populateQuickParamSelect()`.
  * `populateQuickValueSelect()`.
  * `refreshQuickRow()`.
  * `selectByParamValue()`.
* Apertura automática de apartados de plantilla mediante `applyTemplateDefaultOpen()`.
* Detección de vista guardada actual mediante `detectCurrentSavedView()`.
* Actualización del texto del botón de vista mediante `updateViewButtonLabel()`.

### Cambiado

* La herramienta de plantillas se divide visualmente en:

  * `1. Configurar plantilla`.
  * `2. Actualizar vista`.
  * `3. Renombrar vista existente`.
* La importación CSV aclara que los objetos con `revisado_deo = TRUE` se pintan en rojo.
* Se reorganizan textos y pistas de uso en plantillas y acciones rápidas.

## [v8.6.0] — 2026-06-10 09:00 — 8ee9d58

### Añadido

* Grupo “Mostrar parámetros”.
* Herramientas internas de visualización:

  * “Ver nombres”.
  * “Ver tornillos”.
  * “Ver soldaduras”.
* Selectores de parámetros por herramienta:

  * `namesParamList`.
  * `boltsParamList`.
  * `weldsParamList`.
* Parámetros predefinidos para nombres, tornillos y soldaduras.
* Lectura de parámetros disponibles del modelo mediante `scanModelParameters()`.
* Creación de markups de texto con `API.markup.addTextMarkup`.
* Funciones:

  * `makeParamPicker()`.
  * `buildParamLabel()`.
  * `refreshParamMarkups()`.
  * `scheduleParamMarkups()`.
  * `scheduleAllParamMarkups()`.
  * `removeMarkupIds()`.
* Nuevo bloque “Plantillas de vista”.
* Guardado de plantillas en `localStorage`.
* Selector de plantillas guardadas.
* Eliminación de plantillas.
* Creación de vista desde nombre generado.
* Renombrado de vista existente.
* Funciones de gestión de plantillas:

  * `loadTemplates()`.
  * `saveTemplates()`.
  * `currentTemplateFromForm()`.
  * `saveTemplateFromForm()`.
  * `deleteSelectedTemplate()`.
  * `populateTemplateSelect()`.
  * `populateRenameViews()`.
  * `renameSelectedView()`.
* Constante `HIDDEN_MARK_OPACITY`.
* Funciones para registro y rescate de ocultos:

  * `clearHiddenWhite()`.
  * `setRegistryVisible()`.
  * `rescueRecolored()`.

### Cambiado

* “Mostrar nombres” deja de ser una opción aislada y pasa a integrarse en el nuevo grupo “Mostrar parámetros”.
* El selector de items se mantiene como herramienta independiente.
* Se amplía la estructura de UI con tool bodies, subtítulos y selectores reutilizables.
* Se mejora la gestión de colores y visibilidad con `getCurrentColorMap()`.

## [v8.5.2] — 2026-06-09 20:32 — 579365e

### Añadido

* Barra de sensibilidad en “Selección cercanos”.
* Control porcentual `nearPercent` con etiqueta `nearPercentLabel`.
* Información de calibración de sensibilidad.
* `getNearCalibrationFactor()` y `updateNearPercentLabel()`.
* “Selector de items” como evolución de “Concentrar selección”.
* Modo “Selección por color” dentro del selector de items.
* Lista de colores incluidos `includeColors`.
* Botón de refresco de colores para selector.
* Sistema de columnas CSV ampliable con:

  * Botón `+`.
  * Selector de columnas disponibles.
  * Refresco de parámetros.
  * Columnas predefinidas.
  * Columnas de propiedades del modelo.
* Funciones de columnas:

  * `addPredefinedColumn()`.
  * `addPropertyColumn()`.
  * `addColumnFromPicker()`.
  * `removeColumn()`.
  * `reorderColumn()`.
  * `updateCsvColumnSelect()`.
  * `refreshCsvAvailableParameters()`.
* Reordenación de columnas mediante arrastre.
* Nueva vista desde plantilla.
* Detección de datos de usuario para autor y fecha.
* Creación de nombre de vista desde campos RFI, módulo, nivel, ejes, elemento y tarea.
* Preview y descripción de vista.
* Panel de desarrollador con summary personalizado.
* Funciones para tooltips y textos flotantes refinados.

### Cambiado

* “Concentrar selección” se renombra a “Selector de items”.
* La configuración anterior de selección por parámetro se simplifica para trabajar por `Name`.
* La exportación CSV permite añadir parámetros de proyecto como columnas.
* Las columnas de exportación pasan de lista fija a sistema configurable.
* Se refina la UI de colores, selectores y plantillas.

## [v8.5] — 2026-06-09 19:31 — 1284758

### Añadido

* Opción “Concentrar selección”.
* Desplegable basado en `Name` para limitar selección.
* Toggle “Mostrar nombres”.
* Uso inicial de `API.markup.addTextMarkup` para mostrar nombres en el modelo.
* Función `showObjectNames()`.
* Funciones de ocultos:

  * `clearHiddenWhite()`.
  * `setRegistryVisible()`.
* Nueva importación CSV mediante `importCsvFiles()`.

### Cambiado

* “Excluir de la selección” pasa a “Excluir de selección”.
* La selección cercana se orienta más a candidatos de ensamblaje.
* Se simplifican cachés y lógica de filtros de selección.
* La importación de CSV se separa en flujo de múltiples archivos.

### Eliminado

* Se eliminan controles explícitos de refresco de parámetros/valores en “Incluir en la selección”.
* Se elimina parte de la lógica compleja previa de cachés de inclusión.

## [pre-v8.5] — 2026-06-09 18:48 — 01d02cb

### Añadido

* Herramientas con summary-switch integrado:

  * `nearSummarySwitch`.
  * `excludeSummarySwitch`.
  * `hiddenSummarySwitch`.
  * `includeSummarySwitch`.
* Contenedores específicos:

  * `nearTool`.
  * `excludeTool`.
  * `hiddenTool`.
  * `includeTool`.
* Opción “Ver ocultos”.
* Registro de objetos ocultos blancos mediante `hiddenWhiteRegistry`.
* Funciones:

  * `getHiddenObjectEntries()`.
  * `markHiddenObjectsWhite()`.
  * `hiddenWhiteMapStillWhite()`.
  * `setHiddenWhitesVisible()`.
  * `syncHiddenUiState()`.
  * `wireModelSummarySwitch()`.
* Obtención de valores `Name` mediante `getNameValues()`.
* Nueva combinación final de selección con `applyFinalSelection()`.

### Cambiado

* Las herramientas principales pasan a tarjetas desplegables con interruptor en el encabezado.
* La selección de inclusión se orienta progresivamente hacia valores de `Name`.
* Se reorganiza la lógica de vecinos, ocultos, excluidos e incluidos en una selección final combinada.

## [pre-v8.5] — 2026-06-09 18:05 — 79cfb2c

### Añadido

* “Selección ocultos”.
* “Incluir en la selección”.
* Filtros por parámetro, condición y valor.
* Botones para actualizar parámetros, valores y caché de inclusión.
* Caché de propiedades por modelo mediante `propsCache`.
* Lectura de propiedades con `getPropertiesCached()`.
* Análisis robusto de color mediante `analyzeColor()`.
* Funciones de filtrado:

  * `buildIncludeCaches()`.
  * `collectParamValues()`.
  * `computeExcludedRemovals()`.
  * `computeIncludeMatches()`.
  * `customFilterMatches()`.
* Herramientas de selección por mapas:

  * `addToMapSet()`.
  * `mapSetToSelector()`.
  * `mapSetCount()`.
  * `applySelectionMap()`.
* Debug ampliado con historial de errores.
* Gestión de errores con `recordError()` y `safeJson()`.

### Cambiado

* El sistema pasa de selección cercana simple a motor de selección con varias capas:

  * cercanos,
  * excluidos,
  * ocultos,
  * incluidos.
* Se introduce una arquitectura de caché para reducir llamadas repetidas a propiedades.
* Se refuerza la detección de tornillería y soldaduras con `isFastenerOrWeld()`.

## [v8.2] — 2026-06-09 15:05 — 10caada

### Añadido

* Selector `folderSelect` para carpetas/grupos de vistas.
* Botón `folderRefresh`.
* Botón de cancelar operación `cancelOp`.
* Estado de proceso `progress`.
* Función `populateFolders()`.
* Detección de carpeta/grupo mediante `detectFolder()`.
* Filtro de vistas por carpeta mediante `viewMatchesFolder()`.
* Cálculo de vecinos mediante `computeNeighbors()`.
* Procesado de selección mediante `processSelection()`.
* Función `showCancel()`.

### Cambiado

* `extendedFilter` se sustituye por `folderSelect`.
* El estado inicial muestra “Vista cargada con éxito”.
* La exportación masiva empieza a trabajar con carpetas reales si la API las expone.
* Se reestructura el control de cancelación para operaciones largas.
* Se mejora el procesado de selección y de exclusiones.

## [v8] — 2026-06-09 13:58 — d4725c6

### Añadido

* Herramienta “Excluir de selección”.
* Lista de colores detectados para excluir.
* Botón para actualizar colores del modelo.
* `excludeToggle`, `excludeColors`, `excludeRefresh`.
* Caja de estado/progreso.
* Cancelación de exportación masiva.
* Exportación masiva renombrada visualmente a “Exportación masiva”.
* Función `populateExcludeColors()`.
* Función `formatETA()` para tiempo estimado.
* Función `setProgress()`.

### Cambiado

* `exportExtended()` pasa a `exportMassive()`.
* El texto “Extended export” pasa a “Exportación masiva”.
* Se incorpora UI específica para exclusión automática por color.

## [v7.2] — 2026-06-09 13:50 — de139b6

### Añadido

* Configuración personalizada para “Selección cercanos”.
* Toggle `nearCustom`.
* Texto explicativo del criterio por defecto.
* Constante `CONTACT_VOLUME_MAX`.
* Función `augmentSelection()`.
* Eventos de selección con `handleWorkspaceEvent()`.
* Función `onSelectionChanged()`.
* Activación/desactivación de configuración cercana mediante `setNearConfigEnabled()`.

### Cambiado

* La selección cercana pasa a ejecutarse automáticamente a partir de cambios de selección.
* La lógica considera volumen máximo de candidato.
* Se sustituye el selector de alcance anterior por `extendedFilter`.
* Se mejora el control para no reentrar en eventos de selección.

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
