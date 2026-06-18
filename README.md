# DEO Tools — Extensión para Trimble Connect 3D Viewer

DEO Tools es una extensión web para el **visor 3D de Trimble Connect** (Trimble Connect for Browser) que añade un panel lateral con herramientas para trabajar con objetos coloreados, exportar e importar colores por GUID, hacer selecciones avanzadas y gestionar vistas.

Está pensada para flujos de revisión basados en IFC, donde los objetos se colorean por estado y se necesita volcar esa información a CSV (por ejemplo, para cruzarla con códigos de RFI), o a la inversa, recuperar un coloreado a partir de un CSV.

> Herramienta creada por **Sergi Artigas (DEO)**. Software libre bajo licencia MIT.
> Versión actual: **v9.5.23** (la versión real la marca `SCRIPT_VERSION` en `index.html` y se muestra en "Código de versión" al conectar).

---

## Características

### Exportar / Importar registro (CSV)

- **Exportar registro**: exporta a CSV los objetos coloreados de la vista activa, con el GUID IFC de cada objeto. Selección de columnas marcables y **reordenables** (el orden de las flechas es el orden del CSV).
- **Detección automática del código de revisión (RFI)** a partir del título de la vista, con modos *Error codes* y *Custom codes*.
- Columna `revisado_deo` (TRUE/FALSE) y columna condicionada `codigo_revision` según el color del objeto.
- **Exportación masiva (ZIP)**: lista con casillas de todas las vistas (orden alfabético, seleccionar/deseleccionar todas, contador `X/total`) y genera **un único ZIP con un CSV por vista seleccionada** más un `_informe.txt`. Incluye **modo permisivo** (omite vistas con fallo y continúa) y **modo estricto** (aborta al primer fallo). Botón Cancelar junto a Exportar y ETA en el Estado. El ZIP se construye con una implementación propia (método STORE), sin dependencias externas.
- **Importar registro (CSV/ZIP)**: aplica colores a partir de uno o varios CSV, o de un **ZIP con CSV dentro**. **Color de aplicación configurable** por código (rojo para `revisado_deo = TRUE`; **magenta por defecto para GUID con código de revisión RFI**, sin prevalecer sobre el rojo DEO), con selector de color y opción para mostrar duplicados. Los GUID que no estén en el modelo se omiten. Botón Cancelar y ETA en el Estado.

### Selección y modelo

- **Selección cercanos**: añade a la selección los elementos del mismo ensamblaje en contacto (aproximado por *bounding box*) con el objeto seleccionado; criterio por defecto: volumen inferior a ~0,001 m³. **Sensibilidad ajustable** y **atajo de teclado configurable** (por defecto `Alt+C`).
- **Excluir de selección**: quita de la selección los objetos por `Name`, por color o por modelo/IFC completo abiertos en el visor (p. ej. ejes de referencia); la exclusión por modelo persiste entre sesiones. Embudo para ordenar por aparición o alfabéticamente.
- **Selección ocultos / Ver ocultos**: gestiona los objetos ocultos marcándolos en blanco translúcido, con rescate de los recoloreados manualmente.
- **Selector de items**: limita la selección por valor de `Name`, por parámetro/valor (p. ej. *Assembly type*, *Bolt standard*) o por color.

### Parámetros, inspección y cantidades

- **Mostrar parámetros**: configura parámetros por tipología (nombres, tornillos, soldaduras) y guarda **etiquetas 3D flotantes** con esos datos (tamaño y distancia ajustables con sliders).
- **Inspector de selección**: lectura rápida de los datos del objeto seleccionado (tipología, marca, dimensiones con unidades) sin abrir el panel de propiedades.
- **Quantity Surveyor**: resumen de cantidades de la selección (peso, volumen, área, longitud, % sobre items visibles) agrupado por soldaduras, tornillos (por estándar + calidad usando `Bolt count`) y perfiles/chapas por material. Formato español con unidades (kg, m³, m², mm), scroll interno y botón de copiar.

### Vistas

- **Configurar vista**: crea o actualiza vistas con nombre generado por campos (código RFI, ubicación, nivel, ejes, capítulo, actuación, fecha, autor), con **validación obligatoria** de campos, autor automático, **"Compartir con"** mediante chips/autocompletado de miembros reales del proyecto y etiquetas. Incluye **buscador global "Vista activa"** con **"Ir a vista"** y modos de orden.
- **Apertura/cambio de vistas robusto**: la activación de una vista se **verifica realmente** (no se da por buena solo porque la API no lance error); prueba distintos métodos/payloads de la Workspace API y confirma por id/nombre, con recuperación de vistas activadas con retardo.

### Color

- **Normalización de color** a `#RRGGBBAA` (admite `#RGB/#RGBA/#RRGGBB/#RRGGBBAA`, `rgb()/rgba()`, objetos `{r,g,b,a}`/`{red,green,blue,alpha}`, arrays, valores 0–1 y 0–255, alfa).
- **Detección de colores nativos IFC/Tekla**: además de los colores de presentación, intenta extraer el color real desde las propiedades del objeto (con diagnóstico). Sigue siendo una aproximación dependiente de lo que exponga la API (ver Limitaciones).

### Productividad y diagnóstico

- **Botón global "Actualizar datos del visor"**: botón sticky en la esquina superior derecha del panel (visible al hacer scroll) que vuelve a leer modelos IFC, colores, parámetros y valores `Name` en una sola acción. Muestra **halo azul parpadeante + giro** durante la carga inicial de datos.
- **Bug Report** (panel Desarrollador): crea reportes de incidencia con **capturas asistidas, pegado de imagen, anotaciones y menciones de la interfaz**; obligatorios título, descripción y zona/herramienta afectada. Se guardan en `localStorage` y se descargan como **ZIP** (TXT + JSON + imágenes + `manifest.json` + `reported_bugs.json/.txt/_summary.json`).
- **Reported bugs**: ventana ligera de seguimiento de incidencias conocidas en el panel Desarrollador (lista activa / histórico, comentarios editables persistentes).
- **Registro de consola** con `recordError` para diagnóstico.
- Interfaz organizada en menús desplegables, con interruptores estilo Trimble y descripciones flotantes al pasar el ratón.

---

## Estructura del repositorio

```
.
├── index.html        # La extensión completa (HTML + CSS + JS en un solo archivo)
├── manifest.json     # Manifiesto que Trimble Connect lee para cargar la extensión
├── assets/
│   ├── icon.svg      # Icono de la extensión (vectorial, referenciado por manifest.json)
│   └── icon.png      # Icono alternativo en PNG
├── icon.svg          # Copia en raíz (compatibilidad con manifests cacheados)
├── icon.png          # Copia en raíz (compatibilidad)
├── old_code/         # Copias históricas versionadas de index.html (index_vX.Y.Z.html)
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── CLAUDE.md         # Reglas de trabajo para asistentes de código (Claude)
├── AGENTS.md         # Reglas equivalentes para otros agentes
└── LICENSE
```

La extensión es **100 % estática**: no necesita servidor, build ni dependencias más allá de la propia Workspace API de Trimble (que se carga por CDN desde el propio `index.html`). Por eso funciona directamente publicada en **GitHub Pages**.

GitHub Pages del proyecto: `https://srgio92.github.io/trimble-export-colors/`

---

## Instalación

### 1. Publica los archivos

La forma más sencilla es **GitHub Pages**:

1. Haz un *fork* o copia este repositorio en tu cuenta.
2. En el repositorio, ve a **Settings → Pages** y activa GitHub Pages sobre la rama `main` (carpeta raíz `/`).
3. Tu extensión quedará publicada en una URL del tipo:
   `https://TU_USUARIO.github.io/NOMBRE_DEL_REPO/`

Cualquier hosting estático con **HTTPS** sirve igual.

### 2. Añade la extensión en Trimble Connect

1. Abre **Trimble Connect for Browser** e inicia sesión.
2. Entra en la zona de **Extensiones** (Settings / Extensions del visor).
3. Añade una extensión personalizada indicando la **URL del manifiesto**:
   `https://TU_USUARIO.github.io/NOMBRE_DEL_REPO/manifest.json`
4. Abre un proyecto con un modelo cargado. El panel **DEO Tools** aparecerá en la barra lateral.

> La ubicación exacta del menú de extensiones puede variar según la versión de Trimble Connect. Consulta la documentación oficial: https://developer.trimble.com/docs/connect/workspace-api/

---

## Uso

### Exportar registro (colores a CSV)

1. Sitúate en la vista cuyo coloreado quieras exportar (su título debe contener el código de revisión, p. ej. `(RFI_18861) MÓDULO B...`).
2. Abre **Exportar registro**.
3. Marca y ordena las columnas que quieras.
4. Pulsa **Exportar registro**.

El archivo se descarga con separador `;`, codificación UTF‑8 (con BOM, para que Excel respete los acentos) y todas las celdas entre comillas.

#### Columnas disponibles

| Columna            | Descripción                                                        | Por defecto |
|--------------------|--------------------------------------------------------------------|:-----------:|
| `GUID_IFC`         | GUID IFC comprimido de 22 caracteres (el GlobalId del IFC)         | ✓           |
| `color`            | Color aplicado al objeto (`#RRGGBBAA`)                             | ✓           |
| `codigo_revision`  | Código RFI (solo en filas cuyo color esté en la lista de revisión) | ✓           |
| `revisado_deo`     | `TRUE`/`FALSE` según el color                                      | ✓           |
| `View_name`        | Nombre completo de la vista de origen                              | ✓           |
| `modelId`          | Identificador del modelo                                           | —           |
| `objectRuntimeId`  | Identificador en tiempo de ejecución del objeto                    | —           |

> El `GUID_IFC` se obtiene convirtiendo el runtime id al GUID externo con la API y **comprimiéndolo** al formato IFC de 22 caracteres.

### Lógica del código de revisión

El valor base del código se determina en este orden:

1. **Custom codes ON** y con texto en el cuadro → se usa ese texto.
2. En otro caso, se intenta **extraer del título** de la vista el patrón `RFI_XXXXX`.
3. Si no se puede extraer:
   - **Error codes ON** → se escribe `ERROR (NOMBRE_DE_LA_VISTA)` (o `ERROR (VIEW_NAME_NOT_FOUND)`).
   - **Error codes OFF** → `codigo_revision` queda vacío (con aviso en el log).

> Importante: aunque exista un código base, **solo se escribe en `codigo_revision` cuando el color del objeto está en la lista de colores de revisión**. En el resto de colores la celda queda vacía.

### Convención de colores

Estos colores están configurados como constantes al inicio de `index.html` y puedes adaptarlos a tu propio criterio. La comparación es exacta sobre el valor `#RRGGBBAA` en mayúsculas (ajusta las constantes o la comparación si tu modelo usa un canal alfa distinto a `FF`).

### Exportación masiva (varias vistas, ZIP)

Activa **Exportación masiva**. Aparece una lista con casillas de todas las vistas del proyecto (seleccionar/deseleccionar todas, orden alfabético, contador `X/total`) y un interruptor **Modo estricto**:

- **Permisivo** (por defecto): si una vista falla, la omite, sigue con las demás y entrega un ZIP parcial con informe.
- **Estricto**: si una vista falla, aborta toda la exportación.

Marca las vistas y pulsa **Exportar registro**. Se genera **un CSV por vista dentro de un único ZIP** (`DEO_export_masiva_AAAA-MM-DD.zip`) con un `_informe.txt` que detalla modo, vistas exportadas/vacías/con fallo y estado final. El Estado muestra progreso y ETA, con botón **Cancelar**.

### Importar registro (CSV/ZIP)

Pulsa **Importar registro** y elige uno o varios CSV, o un **ZIP con CSV dentro**. La extensión convierte cada GUID a runtime id, aplica el color configurado (rojo a `revisado_deo = TRUE`; magenta por defecto a los GUID con código RFI) y **omite** los GUID que no estén en el modelo. El Estado muestra progreso por archivo y ETA, con botón **Cancelar**.

### Selección cercanos

1. Selecciona un objeto en el visor.
2. Activa el interruptor **Selección cercanos** (o usa el atajo de teclado configurable).
3. Ajusta la sensibilidad si lo necesitas.

Añade a la selección los elementos del mismo ensamblaje en contacto con el seleccionado (por ejemplo, tornillería y soldaduras adyacentes a un perfil).

> El contacto es **aproximado mediante cajas envolventes (*bounding boxes*)**, no contacto geométrico exacto.

---

## Limitaciones conocidas

- **Selección cercanos** es una aproximación por *bounding box* y usa la jerarquía del ensamblaje como conjunto de candidatos; puede requerir ajuste según el modelo.
- La **detección de carpetas de vistas** depende de que la API las exponga; si no, solo se ofrece "Todo el modelo".
- La **exportación masiva** se ejecuta en el navegador: para muchos miles de vistas el proceso puede ser largo y el CSV resultante pesado.
- La comparación de colores es **exacta** (incluyendo el canal alfa).
- **Colores nativos IFC/Tekla**: la extensión intenta detectarlos desde las propiedades del objeto, pero la API no garantiza exponerlos en todos los casos; cuando solo existen colores nativos no accesibles, los paneles de color pueden mostrar un aviso técnico.
- **Etiquetas 3D flotantes**: funcionan en modelo directo; sobre vistas guardadas la API crea el markup pero el visor puede no renderizarlo (limitación del visor Trimble). Usa el **Inspector de selección** como vía principal en ese caso.
- La **apertura de vistas** depende de los métodos que exponga la Workspace API de tu entorno; la extensión prueba varios y verifica la activación, pero si ninguno confirma el cambio, lo indica en el log en vez de asumir que funcionó.

---

## Privacidad

La extensión se ejecuta **íntegramente en tu navegador** a través de la Workspace API de Trimble. No envía datos a ningún servidor externo: las exportaciones se descargan localmente y las importaciones se leen localmente. Los reportes de Bug Report y la lista de Reported bugs se guardan solo en el `localStorage` del navegador.

---

## Licencia

Distribuido bajo licencia **MIT**. Consulta el archivo [LICENSE](LICENSE).
